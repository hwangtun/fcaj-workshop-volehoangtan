---
title: "Cấu hình SNS Notification gửi cảnh báo"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.6.2 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, Amazon SNS được sử dụng để gửi thông báo cảnh báo đến quản trị viên khi quá trình xử lý ảnh xảy ra lỗi.

SNS giúp hệ thống có khả năng thông báo tự động thay vì yêu cầu quản trị viên phải liên tục kiểm tra log trên CloudWatch.

Các trường hợp cần gửi cảnh báo:

- AWS Lambda xử lý ảnh thất bại.
- Không thể đọc ảnh từ S3.
- Không thể ghi metadata vào DynamoDB.
- Xảy ra lỗi trong quá trình tối ưu hóa ảnh.

---

### 1. Tổng quan luồng gửi Notification

Luồng hoạt động của SNS:

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
Email Notification
      |
      |
Administrator
```

Khi hệ thống phát hiện lỗi:

1. Lambda ghi nhận lỗi trong quá trình xử lý.
2. CloudWatch theo dõi trạng thái thực thi.
3. SNS gửi cảnh báo đến quản trị viên.

---

### 2. Truy cập Amazon SNS Console

Mở:

```
AWS Management Console
```

![AWSConsole](/images/5-Workshop/5.6-Monitoring/sns-notification/aws_console.jpg)
Tìm kiếm dịch vụ:

```
Simple Notification Service (SNS)
```

![AWSConsole](/images/5-Workshop/5.6-Monitoring/sns-notification/search_sns.jpg)
Trong thanh điều hướng bên trái chọn:

```
Topics
```

![SNSConsole](/images/5-Workshop/5.6-Monitoring/sns-notification/sns_step1.jpg)
Sau đó chọn:

```
Create topic
```

để tạo SNS Topic mới.

![SNSConsole](/images/5-Workshop/5.6-Monitoring/sns-notification/sns_step2.jpg)

---

### 3. Tạo SNS Topic

Trong giao diện:

```
Create topic
```

Chọn loại:

```
Standard
```

Cấu hình:

#### Name

Nhập:

```
image-processing-alerts
```

Topic này được sử dụng để quản lý các thông báo cảnh báo của hệ thống.

---

#### Display name

Nhập:

```
Image Processing Alert
```

Giúp dễ dàng nhận biết mục đích của Topic.

![create-topic](/images/5-Workshop/5.6-Monitoring/sns-notification/create_topic.jpg)

---

### 4. Tạo Subscription nhận thông báo

Sau khi tạo Topic, chọn:

```
Create subscription
```

Cấu hình:

## Protocol

Chọn:

```
Email
```

---

## Endpoint

Nhập email của quản trị viên nhận cảnh báo.

Ví dụ:

```
admin@example.com
```

Sau đó chọn:

```
Create subscription
```

![create-subscription](/images/5-Workshop/5.6-Monitoring/sns-notification/create_sub.jpg)

---

### 5. Xác nhận Email Subscription

AWS SNS sẽ gửi email xác nhận đến địa chỉ đã đăng ký.

Mở email và chọn:

```
Confirm subscription
```

![Mail](/images/5-Workshop/5.6-Monitoring/sns-notification/mail_confirm.jpg)

Sau khi xác nhận, trạng thái Subscription sẽ chuyển thành:

```
Confirmed
```

![confirm-subscription](/images/5-Workshop/5.6-Monitoring/sns-notification/sub_confirm.jpg)

---

### 6. Kiểm tra SNS Topic

Sau khi cấu hình hoàn tất, SNS Topic hiển thị:

```
Topic name:

image-processing-alerts
```

Subscription:

```
Protocol:

Email

Status:

Confirmed
```

![topic-created](/images/5-Workshop/5.6-Monitoring/sns-notification/sub_status.jpg)

---

### 7. Cấu hình gửi cảnh báo từ CloudWatch

Để hệ thống tự động gửi thông báo khi Lambda Function xảy ra lỗi, tiến hành tạo **CloudWatch Alarm** dựa trên metric **Errors** của Lambda và cấu hình gửi thông báo đến **Amazon SNS**.

#### Bước 1. Truy cập CloudWatch

Truy cập:

```text
Amazon CloudWatch
```

![loudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/cw_console.jpg)

Chọn:

```text
Alarms
    └── Create alarm
```

![CloudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/create_alarm.jpg)

---

#### Bước 2. Chọn Metric

Chọn Metric theo đường dẫn:

```text
Lambda
    └── By Function Name
            └── image-optimizer-lambda
                    └── Errors
```

![CloudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/metric_lambda.jpg)

---

#### Bước 3. Cấu hình điều kiện

Thiết lập điều kiện cảnh báo:

```text
Metric

Errors

Condition

Errors >= 1
```

![CloudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/alarm_condition.jpg)

---

#### Bước 4. Chọn SNS Topic

Trong phần **Notifications**, chọn SNS Topic đã tạo trước đó:

```text
image-processing-alerts
```

![CloudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/cw_sns_select.jpg)

---

#### Bước 5. Hoàn tất tạo Alarm

Đặt tên Alarm, xem lại cấu hình và chọn **Create alarm**.

Sau khi tạo thành công, Alarm sẽ ở trạng thái:

```text
OK
```

![CloudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/alarm_name.jpg)

Khi Lambda Function phát sinh lỗi, quy trình xử lý sẽ diễn ra như sau:

```text
Lambda Error
      │
      ▼
CloudWatch Metric (Errors)
      │
      ▼
CloudWatch Alarm
      │
      ▼
Amazon SNS
      │
      ▼
Administrator Email
```

---

### 8. Kiểm tra gửi Notification

Sau khi hoàn thành cấu hình CloudWatch Alarm và Amazon SNS, tiến hành kiểm tra khả năng gửi thông báo của hệ thống.

#### Bước 1. Tạo lỗi

Có thể tạo lỗi bằng một trong các cách sau:

- Upload tệp không đúng định dạng.
- Lambda Function không có quyền truy cập Amazon S3.
- Chủ động tạo Exception trong mã nguồn Lambda để kiểm thử.

---

#### Bước 2. Kiểm tra CloudWatch Logs

Sau khi Lambda gặp lỗi, truy cập:

```text
Amazon CloudWatch

Logs

Log groups

/aws/lambda/image-optimizer-lambda
```

![CloudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/cw_log.jpg)

CloudWatch sẽ ghi nhận thông tin lỗi, ví dụ:

```text
ERROR

Image processing failed
```

![CloudWatch](/images/5-Workshop/5.6-Monitoring/sns-notification/cw_log_error.jpg)

---

#### Bước 3. Kiểm tra trạng thái Alarm

Sau khi CloudWatch thu thập Metric, Alarm sẽ chuyển từ trạng thái:

```text
OK
```

sang

```text
In alarm
```

![CloudWatch](images/5-Workshop/5.6-Monitoring/sns-notification/alarm_trigger.jpg)

---

#### Bước 4. Kiểm tra Email Notification

Amazon SNS sẽ tự động gửi email thông báo đến địa chỉ đã đăng ký.

Ví dụ:

```text
Subject

AWS Notification
```

```text
Message

Image processing failed.

Function:
image-optimizer-lambda

Status:
FAILED
```

![SNS](/images/5-Workshop/5.6-Monitoring/sns-email.jpg)

Kết quả cho thấy hệ thống đã tự động phát hiện lỗi của Lambda Function và gửi thông báo thành công thông qua Amazon SNS.

---

### 9. Kết quả

Sau khi hoàn thành cấu hình Amazon SNS, hệ thống có khả năng gửi cảnh báo tự động khi xảy ra lỗi.

Kết quả đạt được:

- SNS Topic được tạo thành công.
- Email quản trị viên được đăng ký nhận thông báo.
- Có thể kết nối với CloudWatch để gửi cảnh báo.
- Hỗ trợ phát hiện và xử lý sự cố nhanh chóng.

Luồng Monitoring hoàn chỉnh:

```
                AWS Lambda
                     |
                     |
              Image Processing
                     |
          +----------+----------+
          |                     |
          v                     v
   CloudWatch Logs        DynamoDB
          |
          |
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

Sau bước này, hệ thống **Automatic Image Optimization System on AWS** đã hoàn thiện khả năng giám sát và gửi cảnh báo tự động trong quá trình xử lý ảnh.
