---
title: "Monitoring hệ thống với CloudWatch và SNS"
date: 2026-08-04
weight: 6
chapter: false
pre: " <b> 5.6 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, Monitoring được triển khai nhằm theo dõi hoạt động của hệ thống, kiểm tra quá trình xử lý ảnh và gửi cảnh báo khi xảy ra lỗi.

Hệ thống sử dụng hai dịch vụ AWS chính:

- **Amazon CloudWatch Logs**: lưu trữ và theo dõi log thực thi của AWS Lambda.
- **Amazon SNS**: gửi thông báo cảnh báo đến quản trị viên khi phát hiện lỗi.

Việc triển khai Monitoring giúp hệ thống dễ dàng phát hiện sự cố, hỗ trợ quá trình debug và đảm bảo quá trình xử lý ảnh diễn ra ổn định.

---

### Tổng quan kiến trúc Monitoring

Quy trình giám sát hệ thống:

```
User Upload Image
        |
        v
Amazon S3 Input Bucket
        |
        v
S3 Event Trigger
        |
        v
AWS Lambda
        |
        +----------------+
        |                |
        v                v
Amazon S3 Output    DynamoDB
        |
        |
        v
CloudWatch Logs
        |
        |
        v
CloudWatch Alarm
        |
        |
        v
Amazon SNS
        |
        |
        v
Administrator
```

---

### Amazon CloudWatch Logs

Amazon CloudWatch Logs được sử dụng để ghi nhận toàn bộ quá trình thực thi của AWS Lambda.

Mỗi khi Lambda được kích hoạt để xử lý ảnh, hệ thống tự động tạo log chứa các thông tin:

- Thời gian bắt đầu và kết thúc xử lý.
- Request ID của Lambda.
- Thông tin file ảnh đang xử lý.
- Kết quả xử lý.
- Thông tin lỗi nếu xảy ra.

Ví dụ log:

```
START RequestId: xxxx

Processing image:
sample-image.jpg

Optimizing image...

Uploading result to S3...

Saving metadata to DynamoDB...

END RequestId: xxxx
```

CloudWatch Logs giúp:

- Kiểm tra Lambda có được kích hoạt hay không.
- Theo dõi quá trình xử lý ảnh.
- Phân tích lỗi khi hệ thống gặp sự cố.

---

### Amazon SNS Notification

Amazon SNS được sử dụng để gửi thông báo tự động đến quản trị viên khi hệ thống phát sinh lỗi.

Luồng cảnh báo:

```
AWS Lambda
      |
      |
CloudWatch Logs
      |
      |
CloudWatch Alarm
      |
      |
Amazon SNS Topic
      |
      |
Administrator
```

Các trường hợp có thể gửi cảnh báo:

- Lambda execution thất bại.
- Quá trình xử lý ảnh bị lỗi.
- Không thể truy cập tài nguyên AWS.
- Số lượng lỗi vượt quá giới hạn cho phép.

Ví dụ nội dung thông báo:

```
Alert:

Image processing failed.

Function:
image-optimizer-lambda

Status:
FAILED
```

---

### Các bước triển khai Monitoring

Quá trình triển khai Monitoring gồm hai bước:

#### 5.6.1. Theo dõi Lambda Logs bằng CloudWatch

Cấu hình CloudWatch Logs để:

- Kiểm tra log của Lambda Function.
- Theo dõi trạng thái xử lý ảnh.
- Xem chi tiết lỗi khi Lambda thất bại.

Kết quả mong đợi:

- Lambda tự động ghi log sau mỗi lần thực thi.
- Có thể truy xuất lịch sử xử lý.

---

#### 5.6.2. Cấu hình SNS Notification

Thiết lập Amazon SNS để:

- Tạo SNS Topic.
- Đăng ký email nhận thông báo.
- Kết nối cảnh báo với hệ thống Monitoring.
- Kiểm tra khả năng gửi notification.

Kết quả mong đợi:

- Quản trị viên nhận được cảnh báo khi Lambda xảy ra lỗi.

---

### Vai trò của Monitoring trong hệ thống

Việc triển khai CloudWatch và SNS giúp hệ thống:

- Giám sát hoạt động của AWS Lambda.
- Theo dõi quá trình xử lý ảnh.
- Phát hiện lỗi nhanh chóng.
- Hỗ trợ bảo trì và xử lý sự cố.
- Nâng cao độ tin cậy của hệ thống.

Sau khi hoàn thành Monitoring, hệ thống **Automatic Image Optimization System on AWS** có khả năng tự động xử lý ảnh, lưu trữ metadata và giám sát toàn bộ quá trình vận hành.

---

#### Nội dung

- [Theo dõi Lambda Logs bằng Amazon CloudWatch](5.6.1-cloudwatch-logs/)
- [Cấu hình SNS Notification gửi cảnh báo](5.6.2-sns-notification/)
