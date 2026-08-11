---
title: "Tạo DynamoDB Table lưu trữ Metadata"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.5.1 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, Amazon DynamoDB được sử dụng để lưu trữ dữ liệu metadata của hệ thống.

DynamoDB bao gồm hai bảng chính:

- **UserMetadata**: lưu thông tin người dùng, phục vụ quản lý user và liên kết dữ liệu ảnh với tài khoản.
- **ImageMetadata**: lưu thông tin quá trình upload, xử lý và tối ưu hóa hình ảnh bởi AWS Lambda.

Trong đó, bảng **ImageMetadata** được sử dụng để theo dõi:

- Tên file ảnh.
- User sở hữu ảnh.
- Thông tin bucket nguồn và bucket kết quả.
- Đường dẫn ảnh gốc, ảnh đã tối ưu và thumbnail.
- Trạng thái xử lý.
- Kích thước ảnh trước và sau tối ưu.
- Thời gian xử lý.
- Thông tin lỗi nếu quá trình xử lý thất bại.

---

### 1. Tổng quan DynamoDB trong hệ thống

Kiến trúc lưu trữ metadata:

```

User
|
|
v
UserMetadata
|
|
userId
|
|
v
Image Upload
|
|
v
ImageMetadata
|
|
AWS Lambda Processing

```

Hai bảng DynamoDB được sử dụng:

| Table         | Mục đích                           |
| ------------- | ---------------------------------- |
| UserMetadata  | Lưu thông tin tài khoản người dùng |
| ImageMetadata | Lưu thông tin xử lý hình ảnh       |

---

### 2. Tạo bảng ImageMetadata

#### Truy cập Amazon DynamoDB Console

Mở:

```

AWS Management Console

```

Tìm kiếm dịch vụ:

```

DynamoDB

```

Trong thanh điều hướng bên trái, chọn:

```

Tables

```

Sau đó chọn:

```

Create table

```

để tạo DynamoDB Table mới.

![dynamodb-console](/images/5-Workshop/5.5-DynamoDB/dynamodb-console.png)

---

### 3. Cấu hình thông tin Table

Trong giao diện:

```

Create table

```

Cấu hình:

#### Table name

Nhập:

```

ImageMetadata

```

Đây là bảng lưu metadata của các hình ảnh được xử lý trong hệ thống.

---

#### Partition Key

Thiết lập:

```

batchId

```

Kiểu dữ liệu:

```

String

```

`batchId` dùng để nhóm các ảnh được upload trong cùng một lần xử lý.

Ví dụ:

```

batchId:
8f7c9e12-xxxx-xxxx

```

Một batch có thể chứa nhiều ảnh.

---

#### Sort Key

Bật:

```

Add sort key

```

Nhập:

```

processingId

```

Kiểu dữ liệu:

```

String

```

`processingId` giúp phân biệt từng ảnh trong cùng một batch.

Ví dụ:

```

batchId:
batch-001

processingId:
image-001

```

Cấu trúc khóa:

```

ImageMetadata

Partition Key:
batchId

Sort Key:
processingId

```

![table-key](/images/5-Workshop/5.5-DynamoDB/table-key.png)

---

### 4. Tạo DynamoDB Table

Sau khi hoàn thành cấu hình:

Nhấn:

```

Create table

```

AWS tiến hành tạo bảng.

![create-table](/images/5-Workshop/5.5-DynamoDB/create-table.png)

---

### 5. Kiểm tra cấu trúc dữ liệu

Chọn:

```

ImageMetadata

```

Vào:

```

Explore table items

```

Ví dụ dữ liệu:

```json
{
  "batchId": "batch-001",
  "processingId": "processing-001",
  "userId": "user-001",
  "originalName": "image.jpg",
  "status": "SUCCESS",
  "format": "WEBP",
  "originalSize": 2048000,
  "processedSize": 512000
}
```

Trong đó:

```
userId
```

được dùng để liên kết dữ liệu ảnh với bảng:

```
UserMetadata
```

---

### 6. UserMetadata Table

Ngoài ImageMetadata, hệ thống sử dụng thêm bảng:

```
UserMetadata
```

Bảng này lưu thông tin người dùng phục vụ:

- Quản lý tài khoản.
- Phân quyền truy cập.
- Liên kết user với các ảnh đã upload.

Cấu trúc khóa:

```
UserMetadata

Partition Key:

userId
```

Ví dụ:

```json
{
  "userId": "user-001",
  "email": "user@gmail.com",
  "role": "USER",
  "createdAt": "2026-08-04T10:00:00Z"
}
```

Quan hệ giữa hai bảng:

```
UserMetadata

userId
   |
   |
   v

ImageMetadata

userId
```

---

### 7. Kết quả

Sau bước này, hệ thống đã có các DynamoDB Table cần thiết:

```
DynamoDB

|
|-- UserMetadata
|
|-- ImageMetadata
```

Thông tin ImageMetadata:

```
Table:

ImageMetadata


Primary Key:

batchId


Sort Key:

processingId


Capacity Mode:

On-demand


Encryption:

Enabled
```

Luồng xử lý metadata:

```
User
 |
 |
Upload Image
 |
 v
Amazon S3
 |
 |
AWS Lambda
 |
 |
Process Image
 |
 |
 v

ImageMetadata
```

DynamoDB đã sẵn sàng lưu trữ thông tin người dùng và metadata của quá trình tối ưu hóa hình ảnh.

Bước tiếp theo sẽ kiểm tra dữ liệu metadata được AWS Lambda ghi vào DynamoDB trong [5.5.2. Kiểm tra Metadata trong DynamoDB](5.5.2-verify-metadata/).
