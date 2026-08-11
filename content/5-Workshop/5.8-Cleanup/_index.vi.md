---
title: "Dọn dẹp tài nguyên AWS sau triển khai"
date: 2026-08-04
weight: 8
chapter: false
pre: " <b> 5.8 </b> "
---

Sau khi hoàn thành quá trình triển khai hệ thống **Automatic Image Optimization System on AWS**, cần tiến hành dọn dẹp các tài nguyên AWS không còn sử dụng nhằm tránh phát sinh chi phí ngoài mong muốn và đảm bảo môi trường AWS luôn được quản lý hiệu quả.

Việc dọn dẹp mang lại các lợi ích sau:

- Xóa các tài nguyên được tạo trong quá trình thử nghiệm.
- Giảm chi phí vận hành trên AWS.
- Tránh để các dịch vụ chạy nền không cần thiết.
- Đảm bảo tài khoản AWS luôn được quản lý gọn gàng và an toàn.

---

### Các tài nguyên cần kiểm tra trước khi xóa

Trong hệ thống này, các tài nguyên AWS đã được triển khai bao gồm:

```text
AWS Resources

├── Amazon S3
│   ├── Input Bucket
│   └── Output Bucket
│
├── AWS Lambda
│   └── image-optimizer-lambda
│
├── Amazon DynamoDB
│   └── ImageMetadata
│
├── Amazon CloudWatch
│   └── Lambda Logs
│
├── Amazon SNS
│   └── Notification Topic
│
└── IAM
    └── Lambda Execution Role
```

---

### 1. Xóa dữ liệu trong Amazon S3 Bucket

Trước khi xóa một S3 Bucket, cần xóa toàn bộ các đối tượng bên trong Bucket.

Các Bucket cần kiểm tra:

```text
auto-images-input-bucket

auto-images-output-bucket
```

Thực hiện theo các bước sau:

1. Truy cập **Amazon S3 Console**.

![s3](/images/5-Workshop/5.8-Cleanup/s3_console.jpg)

2. Chọn Bucket cần xóa.

![s3](/images/5-Workshop/5.8-Cleanup/s3_bucket_select.jpg)

3. Chọn toàn bộ các Object trong Bucket.

![s3](/images/5-Workshop/5.8-Cleanup/s3_object.jpg)

4. Chọn:

```text
Delete
```

5. Xác nhận xóa toàn bộ dữ liệu.

![s3](/images/5-Workshop/5.8-Cleanup/s3_delete_confirm.jpg)

{{% notice warning %}}
Hãy kiểm tra kỹ dữ liệu trước khi xóa. Sau khi Object trong Amazon S3 bị xóa, dữ liệu có thể không thể khôi phục.
{{% /notice %}}

---

### 2. Xóa Amazon S3 Bucket

Sau khi đã xóa toàn bộ dữ liệu trong Bucket, tiến hành xóa Bucket.

Chọn:

```text
Delete bucket
```

![s3](/images/5-Workshop/5.8-Cleanup/s3_delete_bucket.jpg)

Nhập tên Bucket để xác nhận thao tác xóa.

Ví dụ:

```text
auto-images-input-bucket

auto-images-output-bucket
```

---

### 3. Xóa AWS Lambda Function

Truy cập:

```text
AWS Lambda Console
```

![Lambda](/images/5-Workshop/5.8-Cleanup/s3_lambda_console.jpg)

Chọn Function:

```text
image-optimizer-lambda
```

![Lambda](/images/5-Workshop/5.8-Cleanup/s3_lambda_select.jpg)

Tiếp theo chọn:

```text
Actions
    └── Delete function
```

![Lambda](/images/5-Workshop/5.8-Cleanup/s3_lambda_delete_confirm.jpg)

Sau khi xác nhận, Lambda Function sẽ được xóa khỏi hệ thống.

---

### 4. Xóa bảng Amazon DynamoDB

Truy cập:

```text
Amazon DynamoDB
```

![DynamoDB](/images/5-Workshop/5.8-Cleanup/db_console.jpg)

Chọn bảng:

```text
ImageMetadata
```

![DynamoDB](/images/5-Workshop/5.8-Cleanup/db_select.jpg)

Chọn:

```text
Delete table
```

Xác nhận thao tác:

![DynamoDB](/images/5-Workshop/5.8-Cleanup/db_delete_confirm.jpg)

```text
Delete
```

Sau khi hoàn tất, toàn bộ metadata được lưu trong bảng sẽ bị xóa.

---

### 5. Xóa CloudWatch Logs

Ngay cả khi Lambda Function đã bị xóa, các Log Group trong Amazon CloudWatch vẫn có thể tiếp tục được lưu trữ và phát sinh chi phí.

Truy cập:

```text
Amazon CloudWatch
```

![CloudWatch](/images/5-Workshop/5.8-Cleanup/cw_console.jpg)

Chọn:

```text
Logs
    └── Log groups
```

![CloudWatch](/images/5-Workshop/5.8-Cleanup/cw_select.jpg)

Xóa Log Group:

![CloudWatch](/images/5-Workshop/5.8-Cleanup/cw_log_delete.jpg)

```text
/aws/lambda/image-optimizer-lambda
```

---

### 6. Xóa Amazon SNS Topic

Truy cập:

```text
Amazon SNS
```

![SNS](/images/5-Workshop/5.8-Cleanup/sns_console.jpg)

Chọn Topic:

```text
image-processing-alerts
```

![SNS](/images/5-Workshop/5.8-Cleanup/sns_select.jpg)

Chọn:

```text
Delete topic
```

![SNS](/images/5-Workshop/5.8-Cleanup/sns_delete_confirm.jpg)

Sau khi Topic bị xóa, các Subscription liên kết với Topic này cũng sẽ bị xóa.

---

### 7. Kiểm tra IAM Role

Sau khi xóa Lambda Function, kiểm tra IAM Role:

```text
image-optimizer-lambda-role
```

Nếu Role không còn được sử dụng bởi bất kỳ Lambda Function hoặc dịch vụ AWS nào khác, có thể tiến hành xóa để tránh tồn tại các quyền truy cập không cần thiết.

{{% notice note %}}
Không nên xóa IAM Role nếu Role đó vẫn đang được sử dụng bởi các Lambda Function hoặc dịch vụ AWS khác.
{{% /notice %}}

---

### 8. Kiểm tra AWS Billing

Sau khi hoàn tất quá trình Cleanup, cần kiểm tra chi phí phát sinh trên AWS.

Truy cập:

```text
AWS Billing Console
```

![Billing](/images/5-Workshop/5.8-Cleanup/aws_bill.jpg)

Kiểm tra các dịch vụ sau:

- Amazon S3 Storage
- AWS Lambda Requests
- Amazon DynamoDB Usage
- Amazon CloudWatch Logs
- Amazon SNS Requests

Đảm bảo rằng không còn tài nguyên nào tiếp tục phát sinh chi phí ngoài mong muốn.

---

### Kết quả

Sau khi hoàn thành quá trình dọn dẹp tài nguyên:

- Các tài nguyên được tạo trong quá trình thử nghiệm đã được xóa.
- Không còn các dịch vụ AWS chạy ngoài mục đích sử dụng.
- Giảm nguy cơ phát sinh chi phí không mong muốn.
- Môi trường AWS được trả về trạng thái sạch, sẵn sàng cho các lần triển khai tiếp theo.

Toàn bộ quy trình triển khai của hệ thống được thực hiện theo trình tự sau:

```text
5.3 S3 Deployment
        │
        ▼
5.4 Lambda Deployment
        │
        ▼
5.5 DynamoDB Deployment
        │
        ▼
5.6 Monitoring
        │
        ▼
5.7 Clean Up
```

Như vậy, hệ thống **Automatic Image Optimization System on AWS** đã hoàn thành toàn bộ quy trình từ triển khai, kiểm thử, giám sát đến dọn dẹp tài nguyên trên nền tảng AWS.
