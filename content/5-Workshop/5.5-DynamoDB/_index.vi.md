---
title: "Lưu trữ Metadata bằng Amazon DynamoDB"
date: 2026-08-04
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, Amazon DynamoDB được sử dụng để lưu trữ metadata của các hình ảnh trong quá trình xử lý.

Sau khi AWS Lambda hoàn thành quá trình tối ưu hóa ảnh, thông tin xử lý sẽ được ghi vào DynamoDB nhằm phục vụ việc:

- Theo dõi trạng thái xử lý ảnh.
- Lưu lịch sử xử lý.
- Tra cứu thông tin ảnh đã tối ưu.
- Hiển thị dữ liệu trên giao diện người dùng.
- Quản lý kết quả xử lý theo từng batch upload.

---

### 1. Tổng quan thiết kế DynamoDB

Hệ thống sử dụng DynamoDB Table:

```
ImageMetadata
```

Bảng này lưu thông tin chi tiết của từng ảnh sau khi được Lambda xử lý.

Cấu trúc khóa chính:

```
ImageMetadata

Partition Key:
batchId

Sort Key:
processingId
```

Trong đó:

| Attribute        | Description                                       |
| ---------------- | ------------------------------------------------- |
| batchId          | ID dùng để nhóm các ảnh trong cùng một lần upload |
| processingId     | ID duy nhất của từng quá trình xử lý ảnh          |
| userId           | Người dùng thực hiện upload                       |
| originalName     | Tên file ảnh ban đầu                              |
| inputBucket      | S3 Bucket chứa ảnh gốc                            |
| outputBucket     | S3 Bucket chứa ảnh đã tối ưu                      |
| inputKey         | Đường dẫn ảnh gốc trong S3                        |
| outputKey        | Đường dẫn ảnh sau khi xử lý                       |
| thumbnailKey     | Đường dẫn ảnh thumbnail                           |
| format           | Định dạng ảnh sau khi tối ưu                      |
| uploadedAt       | Thời gian upload ảnh                              |
| processedAt      | Thời gian hoàn thành xử lý                        |
| status           | Trạng thái xử lý                                  |
| errorMessage     | Thông tin lỗi nếu xử lý thất bại                  |
| lambdaRequestId  | ID của Lambda execution                           |
| originalSize     | Kích thước ảnh ban đầu                            |
| processedSize    | Kích thước ảnh sau tối ưu                         |
| compressionRatio | Tỷ lệ giảm dung lượng                             |
| processingTimeMs | Thời gian xử lý                                   |

---

### 2. Luồng lưu trữ Metadata

Quá trình lưu dữ liệu vào DynamoDB:

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
 +----------------+
 |
 v
DynamoDB ImageMetadata
```

Sau khi Lambda hoàn thành quá trình xử lý:

- Ảnh tối ưu được lưu vào Output Bucket.
- Thumbnail được tạo.
- Metadata của ảnh được ghi vào DynamoDB.

---

### 3. Triển khai DynamoDB

Quá trình triển khai DynamoDB gồm hai bước:

```
5.5-DynamoDB
│
├── 5.5.1-create-dynamodb
│
└── 5.5.2-verify-metadata
```

---

#### 5.5.1-create-dynamodb

Trong bước này, thực hiện:

- Tạo DynamoDB Table.
- Cấu hình Partition Key.
- Cấu hình Sort Key.
- Thiết lập chế độ Capacity.
- Kiểm tra Encryption.

Bảng được tạo:

```
Table name:

ImageMetadata
```

Khóa chính:

```
Partition Key:

batchId


Sort Key:

processingId
```

---

#### 5.5.2-verify-metadata

Sau khi Lambda xử lý ảnh thành công, tiến hành kiểm tra dữ liệu được ghi vào DynamoDB.

Kiểm tra:

- Item được tạo trong bảng.
- Trạng thái xử lý.
- Thông tin file ảnh.
- Kích thước trước và sau tối ưu.
- Đường dẫn ảnh trong S3.

Ví dụ dữ liệu:

```json
{
  "batchId": "batch-001",
  "processingId": "image-001",
  "originalName": "sample-image.jpg",
  "status": "SUCCESS",
  "format": "WEBP",
  "originalSize": 204800,
  "processedSize": 51200,
  "compressionRatio": 75
}
```

---

### 4. Kết quả

Sau khi hoàn thành triển khai DynamoDB, hệ thống có khả năng lưu trữ và quản lý metadata của các ảnh được xử lý.

Kết quả đạt được:

- DynamoDB Table `ImageMetadata` được tạo thành công.
- Lambda có thể ghi dữ liệu xử lý vào DynamoDB.
- Metadata được liên kết với ảnh trong S3.
- Hỗ trợ truy vấn lịch sử xử lý ảnh.

Kiến trúc lưu trữ metadata:

```
                 AWS Lambda
                      |
                      |
              Process Image Result
                      |
                      v
              Amazon DynamoDB
                      |
                      |
              ImageMetadata Table
```

Sau bước này, hệ thống đã hoàn thiện phần lưu trữ metadata cho quy trình tối ưu hóa ảnh tự động.

#### Nội dung

- [Tạo DynamoDB Table lưu trữ Metadata](5.5.1-create-dynamodb/)
- [Kiểm tra Metadata trong DynamoDB](5.5.2-verify-metadata/)
