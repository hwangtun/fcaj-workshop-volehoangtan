---
title: "Kiểm tra Metadata trong DynamoDB"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.5.2 </b> "
---

Sau khi tạo DynamoDB Table và cấu hình AWS Lambda có quyền ghi dữ liệu, bước này thực hiện kiểm tra metadata được lưu trữ sau khi quá trình xử lý ảnh hoàn tất.

Mục tiêu kiểm tra:

- Xác nhận Lambda ghi dữ liệu thành công vào DynamoDB.
- Kiểm tra thông tin ảnh sau khi xử lý.
- Đảm bảo dữ liệu liên kết đúng với S3 Input Bucket và Output Bucket.
- Kiểm tra trạng thái xử lý của ảnh.

---

### 1. Chuẩn bị quá trình kiểm tra

Để tạo dữ liệu metadata trong DynamoDB, thực hiện upload một ảnh mẫu vào Input Bucket.

Ví dụ:

```
sample-image.jpg
```

Luồng xử lý:

```
User
 |
 |
Upload Image
 |
 v
Amazon S3 Input Bucket
 |
 |
Object Created Event
 |
 v
AWS Lambda
 |
 |
Image Optimization
 |
 v
DynamoDB ImageMetadata
```

Sau khi Lambda hoàn thành xử lý, một item mới sẽ được tạo trong DynamoDB.

---

### 2. Truy cập DynamoDB Table

Mở:

```
AWS Management Console
```

Chọn:

```
DynamoDB
```

Trong danh sách Tables, chọn:

```
ImageMetadata
```

![dynamodb-table](/images/5-Workshop/5.5-DynamoDB/dynamodb-table.png)

---

### 3. Truy cập dữ liệu trong Table

Trong giao diện DynamoDB Table:

Chọn:

```
Explore table items
```

hoặc:

```
Scan
```

để xem các item đang được lưu trữ.

![explore-items](/images/5-Workshop/5.5-DynamoDB/press-explore.png)
![explore-items](/images/5-Workshop/5.5-DynamoDB/explore-items.png)

---

### 4. Kiểm tra Metadata Item

Sau khi Lambda xử lý thành công, DynamoDB sẽ chứa một item tương ứng với ảnh đã upload.

Ví dụ:

```json
{
  "batchId": "batch-001",
  "processingId": "processing-001",
  "userId": "user001",
  "originalName": "sample-image.jpg",
  "inputBucket": "auto-images-input-bucket",
  "outputBucket": "auto-images-output-bucket",
  "status": "SUCCESS",
  "format": "WEBP",
  "originalSize": 204800,
  "processedSize": 51200,
  "compressionRatio": 75
}
```

---

### 5. Kiểm tra các trường dữ liệu quan trọng

#### Batch Information

Kiểm tra:

```
batchId
processingId
```

Mục đích:

- Xác định nhóm ảnh được upload.
- Phân biệt từng lần xử lý ảnh.

Ví dụ:

```
batchId:
8f7c9e12-xxxx


processingId:
11320000-xxxx
```

---

#### File Information

Kiểm tra:

```
originalName
format
```

Ví dụ:

```
originalName:
sample-image.jpg


format:
WEBP
```

Xác nhận ảnh đã được chuyển đổi sang định dạng tối ưu.

---

#### Processing Status

Kiểm tra:

```
status
```

Các trạng thái:

```
PROCESSING
SUCCESS
FAILED
```

Ví dụ:

```
status:

SUCCESS
```

cho biết quá trình xử lý hoàn thành thành công.

---

#### Storage Information

Kiểm tra:

```
inputBucket
outputBucket
inputKey
outputKey
thumbnailKey
```

Ví dụ:

```
inputBucket:

auto-images-input-bucket


outputBucket:

auto-images-output-bucket
```

Các thông tin này giúp liên kết metadata với file thực tế trong S3.

---

### 6. Kiểm tra thông tin tối ưu hóa ảnh

DynamoDB lưu lại các thông số trước và sau khi xử lý:

```
originalSize

processedSize

compressionRatio

processingTimeMs
```

Ví dụ:

```
Original size:

204800 bytes


Processed size:

51200 bytes


Compression ratio:

75%
```

Điều này giúp đánh giá hiệu quả của quá trình tối ưu hóa ảnh.

---

### 7. Kiểm tra trường hợp lỗi

Trong trường hợp Lambda xử lý thất bại, DynamoDB sẽ lưu trạng thái:

```
status:

FAILED
```

và thông tin lỗi:

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

Dữ liệu lỗi giúp hệ thống dễ dàng theo dõi và xử lý sự cố.

---

### 8. Kiểm tra dữ liệu bằng AWS CLI (Optional)

Có thể kiểm tra DynamoDB bằng AWS CLI:

```bash
aws dynamodb scan \
--table-name ImageMetadata
```

Kết quả trả về danh sách metadata:

```json
{
  "Items": [
    {
      "batchId": {
        "S": "batch-001"
      },
      "status": {
        "S": "SUCCESS"
      }
    }
  ]
}
```

---

### 9. Kết quả

Sau khi kiểm tra, DynamoDB đã lưu thành công metadata của ảnh được xử lý.

Kết quả đạt được:

- Lambda ghi dữ liệu thành công vào DynamoDB.
- Metadata được lưu đầy đủ.
- Trạng thái xử lý được cập nhật chính xác.
- Có thể truy vấn lịch sử xử lý ảnh.

Luồng hoàn chỉnh:

```
             User
              |
              |
        Upload Image
              |
              v
     Amazon S3 Input Bucket
              |
              |
       S3 Object Created
              |
              v
        AWS Lambda
              |
      +-------+-------+
      |               |
      v               v
 Output S3       DynamoDB
                     |
                     |
             ImageMetadata
```

Sau bước này, thành phần lưu trữ metadata của hệ thống **Automatic Image Optimization System on AWS** đã được kiểm tra và hoạt động thành công.
