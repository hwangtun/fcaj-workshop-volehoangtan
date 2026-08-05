---
title: "Kiểm tra quá trình upload và xử lý ảnh"
date: 2026-08-04
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

Sau khi hoàn thành việc tạo S3 Bucket và cấu hình S3 Trigger cho AWS Lambda, bước tiếp theo là kiểm tra toàn bộ quy trình upload và xử lý ảnh của hệ thống nhằm đảm bảo các thành phần hoạt động đúng theo thiết kế.

Quá trình kiểm tra bao gồm:

1. Upload ảnh thông qua Backend.
2. Kiểm tra ảnh được lưu vào Input S3 Bucket.
3. Kiểm tra Backend tạo metadata ban đầu trong DynamoDB với trạng thái `PROCESSING`.
4. Kiểm tra S3 Event Notification kích hoạt AWS Lambda.
5. Kiểm tra Lambda xử lý ảnh và lưu kết quả vào Output S3 Bucket.
6. Kiểm tra Lambda cập nhật metadata trong DynamoDB với trạng thái `SUCCESS` hoặc `FAILED`.

Luồng kiểm tra:

```text
Client
   |
   | POST /api/image/upload
   v
Spring Boot Backend
   |
   +-------------------------------+
   |                               |
   | Upload image to S3            | Create metadata
   |                               | status = PROCESSING
   v                               v
Input S3 Bucket               DynamoDB
      |
      v
S3 Event Notification
      |
      v
AWS Lambda Image Processing
      |
      +-------------------------------+
      |                               |
      | Save optimized image          | Update metadata
      v                               |
Output S3 Bucket                      |
                                      |
                                      v
                               DynamoDB
                         status = SUCCESS/FAILED
```

---

### 1. Upload ảnh vào hệ thống

Người dùng upload ảnh thông qua API của Backend:

```
POST /api/image/upload
```

Request bao gồm:

- Danh sách ảnh cần upload.
- `userId`.
- Cấu hình tối ưu ảnh (`configReq`).
- Định dạng đầu ra (`format`).

Ví dụ phản hồi sau khi upload thành công:

```json
{
  "batchId": "11320000-0919-4d79-9a3c-94d15318ce87",
  "status": "PROCESSING"
}
```

Điều này cho thấy Backend đã tiếp nhận yêu cầu và bắt đầu quá trình xử lý ảnh.

![upload-image](/images/5-Workshop/5.4-Lambda-deployment/upload-image.png)

---

### 2. Kiểm tra dữ liệu sau khi upload

Sau khi Backend nhận yêu cầu upload, hệ thống thực hiện đồng thời hai thao tác:

- Upload ảnh lên **Input S3 Bucket**.
- Tạo bản ghi metadata ban đầu trong **DynamoDB**.

Ảnh được lưu trong Input Bucket theo cấu trúc:

```
uploads/
 └── user001/
      └── test-image.jpg
```

<!-- Đồng thời Backend đính kèm metadata cho object trên S3:

```
batchId
userId
originalName
```

Các metadata này được Lambda sử dụng trong quá trình xử lý ảnh. -->

![test-upload-image](/images/5-Workshop/5.4-Lambda-deployment/test-upload-image.png)

---

### 3. Kiểm tra metadata ban đầu trong DynamoDB

Backend tạo một bản ghi trong bảng:

```
ImageMetadata
```

với trạng thái:

```
PROCESSING
```

Ví dụ:

```json
{
  "batchId": "11320000-0919-4d79-9a3c-94d15318ce87",
  "processingId": "uuid-processing-id",
  "userId": "user001",
  "originalName": "test-image.jpg",
  "status": "PROCESSING",
  "uploadedAt": "2026-08-04T10:30:00Z"
}
```

Bản ghi này giúp hệ thống theo dõi tiến trình xử lý của từng ảnh ngay sau khi upload.

---

### 4. Kiểm tra S3 Trigger hoạt động

Khi ảnh được lưu vào Input Bucket, Amazon S3 phát sinh sự kiện:

```
Object Created
```

Sự kiện này sẽ kích hoạt Lambda Function:

```
autoImageProcessing
```

Có thể kiểm tra quá trình thực thi tại:

```
AWS Lambda
 → Monitor
 → View CloudWatch Logs
```

![lambda-log](/images/5-Workshop/5.4-Lambda-deployment/lambda-log.png)

Nếu Lambda hoạt động thành công, CloudWatch Logs sẽ hiện thị quá trình xử lý và log cuối cùng là:
![success-log](/images/5-Workshop/5.4-Lambda-deployment/success-log.png)

---

### 5. Kiểm tra ảnh sau khi xử lý

Sau khi Lambda hoàn thành xử lý, truy cập Output Bucket:

```
auto-images-output-bucket
```

Kết quả mong đợi:

```
optimized/
 └── user001/
      └── batchId/
            └── test-image.webp

thumbnails/
 └── user001/
      └── batchId/
            └── test-image.webp
```

![output-image-1](/images/5-Workshop/5.4-Lambda-deployment/output-image-1.png)
![output-image-2](/images/5-Workshop/5.4-Lambda-deployment/output-image-2.png)

Kiểm tra:

- Ảnh đã được tối ưu theo cấu hình.
- Dung lượng ảnh giảm so với ảnh gốc.
- Thumbnail được tạo thành công.
- Ảnh được lưu đúng theo `userId` và `batchId`.

---

### 6. Kiểm tra metadata sau khi xử lý

Sau khi Lambda xử lý hoàn tất, bản ghi trong DynamoDB sẽ được cập nhật.

Truy cập:

```
Amazon DynamoDB
 → Tables
 → ImageMetadata
```

Ví dụ:

```json
{
  "batchId": "11320000-0919-4d79-9a3c-94d15318ce87",
  "processingId": "uuid-processing-id",
  "userId": "user001",
  "originalName": "test-image.jpg",
  "status": "SUCCESS",
  "inputKey": "uploads/user001/test-image.jpg",
  "outputKey": "optimized/user001/11320000-0919-4d79-9a3c-94d15318ce87/test-image.webp",
  "thumbnailKey": "thumbnails/user001/11320000-0919-4d79-9a3c-94d15318ce87/test-image.webp",
  "originalSize": 5242880,
  "processedSize": 1048576,
  "compressionRatio": 80,
  "processingTimeMs": 2500,
  "processedAt": "2026-08-04T10:30:03Z"
}
```

Lambda sẽ cập nhật các thông tin như:

- `status`
- `outputKey`
- `thumbnailKey`
- `processedSize`
- `compressionRatio`
- `processingTimeMs`
- `processedAt`

để phản ánh kết quả xử lý cuối cùng của ảnh.

---

### 7. Kiểm tra trạng thái xử lý lỗi

Trong trường hợp xảy ra lỗi, Lambda sẽ cập nhật metadata trong DynamoDB với trạng thái:

```
FAILED
```

đồng thời lưu nguyên nhân lỗi vào trường:

```
errorMessage
```

Ví dụ:

```json
{
  "status": "FAILED",
  "errorMessage": "Unsupported image format"
}
```

Một số tình huống lỗi có thể xảy ra:

- File không đúng định dạng được hỗ trợ.
- File ảnh bị hỏng hoặc không thể đọc.
- Lambda không có quyền truy cập S3.
- Không thể ghi hoặc cập nhật dữ liệu trong DynamoDB.

---

### 8. Kết quả kiểm thử

Sau khi hoàn thành kiểm thử, hệ thống đạt được kết quả như sau:

| Thành phần                                | Kết quả    |
| ----------------------------------------- | ---------- |
| Upload ảnh thông qua Backend              | Thành công |
| Upload ảnh lên Input S3 Bucket            | Thành công |
| Backend tạo metadata ban đầu (PROCESSING) | Thành công |
| S3 Event Notification kích hoạt Lambda    | Thành công |
| Lambda xử lý ảnh                          | Thành công |
| Ảnh tối ưu lưu vào Output S3 Bucket       | Thành công |
| Thumbnail được tạo                        | Thành công |
| Lambda cập nhật metadata trong DynamoDB   | Thành công |
| Log xử lý được lưu trên CloudWatch        | Thành công |

Quá trình kiểm thử cho thấy hệ thống hoạt động đúng theo kiến trúc đã thiết kế. Sau khi người dùng upload ảnh, Backend lưu ảnh vào Input S3 Bucket và tạo metadata ban đầu trong DynamoDB. S3 Event Notification kích hoạt AWS Lambda để xử lý ảnh, lưu kết quả vào Output S3 Bucket và cập nhật metadata trong DynamoDB. Quy trình này đảm bảo việc xử lý ảnh được thực hiện tự động, đồng thời cho phép theo dõi trạng thái xử lý của từng ảnh thông qua cơ sở dữ liệu.
