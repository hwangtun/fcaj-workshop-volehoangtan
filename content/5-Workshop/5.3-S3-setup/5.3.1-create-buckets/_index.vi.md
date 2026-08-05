---
title: "Tạo các S3 Bucket lưu trữ ảnh"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, Amazon S3 được sử dụng để lưu trữ ảnh trong suốt quá trình xử lý.

Hệ thống sử dụng hai S3 Bucket riêng biệt:

- **Input Bucket**: Lưu trữ ảnh gốc do người dùng tải lên.
- **Output Bucket**: Lưu trữ ảnh sau khi được AWS Lambda tối ưu hóa và các ảnh thumbnail được tạo ra.

Việc phân tách hai bucket giúp quản lý dữ liệu rõ ràng, kiểm soát quyền truy cập bằng IAM và đảm bảo dữ liệu được bảo vệ trong quá trình xử lý.

Luồng xử lý dữ liệu:

```
                 User
                  |
                  v
          Upload Image
                  |
                  v
        Amazon S3 Input Bucket
                  |
                  |
        Create Metadata Record
                  |
                  v
          DynamoDB ImageMetadata
             (PENDING)
                  |
                  |
        S3 Object Created Event
                  |
                  v
             AWS Lambda
                  |
        +---------+---------+
        |                   |
        v                   v
 Output S3 Bucket     Update DynamoDB
(Optimized Image)     (SUCCESS/FAILED)
```

---

### 1. Truy cập Amazon S3 Console

Mở [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/)

Trong thanh điều hướng bên trái, chọn:

```
Buckets
```

Sau đó chọn:

```
Create bucket
```

để bắt đầu tạo bucket lưu trữ ảnh.

![s3-console](/images/5-Workshop/5.3-S3-setup/s3-dashboard.png)

---

### 2. Tạo Input Bucket

Trong giao diện **Create bucket**, cấu hình các thông tin:

#### Bucket name

Nhập tên bucket:

```
auto-images-input-bucket
```

{{% notice warning %}}
Tên S3 Bucket phải là duy nhất trên toàn bộ AWS. Nếu tên bucket đã tồn tại, cần thay đổi sang một tên khác.
{{% /notice %}}

![s3-create-name-bucket](/images/5-Workshop/5.3-S3-setup/s3-create-name-bucket.png)

---

### 3. Cấu hình quyền truy cập Input Bucket

Trong phần **Object Ownership**, chọn:

```
ACLs disabled (recommended)
```

Hệ thống sử dụng IAM Policy để quản lý quyền truy cập thay vì ACL.

Trong phần **Block Public Access**, giữ cấu hình mặc định:

```
Block all public access
```

{{% notice note %}}
Ảnh người dùng tải lên không được public trực tiếp. Việc truy cập ảnh được kiểm soát thông qua Backend API và quyền IAM.
{{% /notice %}}

![input-bucket-access](/images/5-Workshop/5.3-S3-setup/s3-access.png)

---

### 4. Cấu hình mã hóa cho Input Bucket

Trong phần **Default encryption**, bật mã hóa dữ liệu:

Chọn:

```
Server-side encryption with AWS KMS keys (SSE-KMS)
```

Sau đó chọn KMS Key được tạo cho hệ thống.

Việc sử dụng AWS KMS giúp bảo vệ ảnh gốc khi lưu trữ trên Amazon S3.

![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/open-kms-dashboard.png)
![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/kms-2.png)
Nhập tên alias:

```
image-optimization-kms
```

![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/kms-3.png)

Chọn IAM user quản lý hệ thống:
![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/kms-4.png)

Thêm IAM Role của Lambda:
![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/kms-5.png)
![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/kms-6.png)
![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/kms-7.png)

Chọn KMS Key vừa tạo và nhấn Create bucket:
![s3-kms-encryption](/images/5-Workshop/5.3-S3-setup/kms-8.png)

Vậy là ta đã có Input bucket:
![create-input-bucket](/images/5-Workshop/5.3-S3-setup/create-input-bucket.png)

---

### 5. Tạo Output Bucket

Thực hiện tương tự các bước tạo Input Bucket.

Đặt tên:

```
auto-images-output-bucket
```

Output Bucket dùng để lưu:

- Ảnh đã được tối ưu dung lượng.
- Ảnh thumbnail phục vụ hiển thị nhanh trên giao diện.

Cấu trúc lưu trữ:

```
auto-images-output-bucket

optimized/
 └── {userId}/
      └── {batchId}/
            └── image.webp

thumbnails/
 └── {userId}/
      └── {batchId}/
            └── image.webp
```

Vậy là ta đã có Output bucket:
![create-output-bucket](/images/5-Workshop/5.3-S3-setup/create-output-bucket.png)

---

### 6. Kiểm tra danh sách Bucket

Sau khi hoàn tất, trong danh sách Amazon S3 Bucket sẽ hiển thị:

| Bucket                    | Chức năng                          |
| ------------------------- | ---------------------------------- |
| auto-images-input-bucket  | Lưu trữ ảnh gốc người dùng tải lên |
| auto-images-output-bucket | Lưu trữ ảnh đã tối ưu và thumbnail |

![bucket-list](/images/5-Workshop/5.3-S3-setup/s3-dashboard.png)

---

# 7. Kiểm tra hoạt động của S3 Bucket

Sau khi tạo thành công, hệ thống sẽ sử dụng các bucket này trong quy trình xử lý ảnh:

1. Backend nhận ảnh từ người dùng.
2. Ảnh được upload vào **Input Bucket**.
3. S3 Trigger kích hoạt AWS Lambda.
4. Lambda thực hiện tối ưu hóa ảnh.
5. Ảnh sau xử lý được lưu vào **Output Bucket**.
6. Thông tin xử lý được lưu vào DynamoDB.

Metadata được lưu trong DynamoDB bao gồm:

```
ImageMetadata

├── batchId
├── processingId
├── userId
├── originalName
├── inputBucket
├── outputBucket
├── inputKey
├── outputKey
├── uploadedAt
├── processedAt
├── status
├── originalSize
├── processedSize
├── compressionRatio
└── processingTimeMs
```

Việc sử dụng Amazon S3 giúp hệ thống có khả năng lưu trữ ảnh với dung lượng lớn, kết hợp cùng AWS Lambda để xây dựng quy trình xử lý ảnh tự động và có khả năng mở rộng.
