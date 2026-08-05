---
title: "Cấu hình S3 Trigger cho AWS Lambda"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, AWS Lambda được sử dụng để tự động xử lý ảnh sau khi người dùng tải ảnh lên hệ thống.

Để tự động kích hoạt Lambda, hệ thống sử dụng **Amazon S3 Event Notification**. Khi một ảnh mới được upload vào Input Bucket, S3 sẽ gửi sự kiện đến Lambda để thực hiện quá trình tối ưu hóa.

Luồng xử lý:

1. Người dùng tải ảnh lên **Input Bucket**.
2. Amazon S3 phát sinh sự kiện **Object Created**.
3. S3 Trigger gọi AWS Lambda.
4. Lambda thực hiện:
   - Đọc ảnh từ Input Bucket.
   - Tối ưu kích thước và dung lượng ảnh.
   - Chuyển đổi định dạng ảnh theo cấu hình.
   - Tạo thumbnail.
   - Lưu ảnh kết quả vào Output Bucket.
   - Lưu thông tin xử lý vào DynamoDB.

---

### 1. Mở AWS Lambda Console

Truy cập [AWS Lambda Console](https://console.aws.amazon.com/lambda/)

Trong danh sách Lambda function, chọn function xử lý ảnh:

![lambda-function](/images/5-Workshop/5.3-S3-setup/lambda-function.png)

---

### 2. Thêm Trigger cho Lambda

Trong giao diện Lambda, chọn Function overview, sau đó click Add trigger:

![add-trigger](/images/5-Workshop/5.3-S3-setup/add-trigger.png)

---

### 3. Chọn Amazon S3 làm Trigger

Trong phần **Trigger configuration**, chọn nguồn trigger Amazon S3:

![select-s3-trigger](/images/5-Workshop/5.3-S3-setup/select-s3-trigger.png)

---

### 4. Cấu hình S3 Trigger

Trong mục **Bucket**, chọn Input Bucket đã được tạo ở bước trước:

```
auto-images-input-bucket
```

Trong mục **Event type**, chọn:

```
All object create events
```

Sự kiện này sẽ được kích hoạt mỗi khi có một object mới được tạo trong bucket.

![s3-trigger-config](/images/5-Workshop/5.3-S3-setup/s3-trigger-config.png)

---

### 5. Cấu hình bộ lọc file

Để giới hạn các file được Lambda xử lý, có thể cấu hình filter theo đường dẫn hoặc định dạng file.

Ví dụ:

#### Prefix

```
uploads/
```

Chỉ các file nằm trong thư mục `uploads` được xử lý.

Ví dụ:

```
uploads/user001/image01.jpg
```

#### Suffix

```
.jpg
```

Có thể áp dụng cho các định dạng ảnh được hỗ trợ:

```
.jpg
.jpeg
.png
.webp
```

{{% notice note %}}
Ngoài việc sử dụng filter của S3 Trigger, Lambda vẫn thực hiện kiểm tra định dạng và kích thước ảnh trước khi xử lý để đảm bảo dữ liệu đầu vào hợp lệ.
{{% /notice %}}

---

### 6. Cấp quyền cho S3 gọi Lambda

Khi tạo S3 Trigger, AWS tự động tạo quyền cho phép S3 gọi Lambda function.

Kiểm tra tại:

```
Lambda
 → Configuration
 → Permissions
 → Resource-based policy statements
```

Policy sẽ chứa quyền:

```
s3.amazonaws.com
    lambda:InvokeFunction
```

![lambda-permission](/images/5-Workshop/5.3-S3-setup/lambda-permission.png)

---

### 7. Kiểm tra Trigger đã được tạo

Sau khi cấu hình thành công, trong phần **Function overview** của Lambda sẽ hiển thị kết nối:

![trigger-complete](/images/5-Workshop/5.3-S3-setup/trigger-complete.png)

---

### 8. Kiểm tra quá trình xử lý

Upload thử một ảnh vào Input Bucket:

Ví dụ:

```
uploads/user001/test-image.jpg
```

Sau khi upload, hệ thống thực hiện:

1. S3 phát sinh sự kiện Object Created.
2. Lambda được kích hoạt.
3. Lambda tải ảnh từ Input Bucket.
4. Ảnh được tối ưu hóa.
5. Kết quả được lưu vào Output Bucket.
6. Metadata xử lý được lưu vào DynamoDB.

Cấu trúc dữ liệu sau xử lý:

```
auto-images-output-bucket

optimized/
 └── user001/
      └── batchId/
            └── test-image.webp

thumbnails/
 └── user001/
      └── batchId/
            └── test-image.webp
```

Thông tin xử lý được lưu trong DynamoDB:

```
ImageMetadata

├── batchId
├── processingId
├── originalName
├── processedSize
├── compressionRatio
├── processingTimeMs
└── status = SUCCESS
```

Như vậy, S3 Trigger giúp hệ thống tự động gọi Lambda ngay sau khi người dùng hoàn tất quá trình upload ảnh, đảm bảo quy trình tối ưu hóa ảnh diễn ra tự động và không cần thao tác thủ công.
