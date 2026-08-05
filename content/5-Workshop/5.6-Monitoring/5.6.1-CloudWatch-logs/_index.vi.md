---
title: "Theo dõi Lambda Logs bằng Amazon CloudWatch"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.6.1 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, Amazon CloudWatch được sử dụng để theo dõi quá trình thực thi của AWS Lambda và lưu trữ các log phát sinh trong quá trình xử lý ảnh.

CloudWatch Logs giúp quản trị viên:

- Kiểm tra Lambda có được kích hoạt thành công hay không.
- Theo dõi từng bước trong quá trình xử lý ảnh.
- Phát hiện và phân tích lỗi khi xảy ra sự cố.
- Đánh giá hiệu năng xử lý của Lambda.

---

### 1. Truy cập CloudWatch Console

Mở:

```
AWS Management Console
```

![AWSConsole](/images/5-Workshop/5.6-Monitoring/sns-notification/aws_console.jpg)
Tìm kiếm dịch vụ:

```
CloudWatch
```

![AWSConsole](/images/5-Workshop/5.6-Monitoring/cloudwatch-logs/aws_search_cw.jpg)
Trong thanh điều hướng bên trái, chọn:

```
Logs
    |
    +--> Log groups
```

![cloudwatch-console](/images/5-Workshop/5.6-Monitoring/cloudwatch-logs/aws_console.jpg)

---

### 2. Kiểm tra Log Group của Lambda

Khi AWS Lambda được tạo, AWS tự động tạo một Log Group tương ứng trong CloudWatch.

Chọn Log Group:

```
/aws/lambda/image-optimizer-lambda
```

Log Group này chứa toàn bộ log được sinh ra trong quá trình Lambda thực thi.

![log-group](/images/5-Workshop/5.6-Monitoring/sns-notification/cw_log.jpg)

---

### 3. Kiểm tra Log Stream

Trong Log Group:

Chọn:

```
Log streams
```

Mỗi lần Lambda được gọi sẽ tạo một Log Stream mới.

Log Stream chứa thông tin của từng lần thực thi.

Ví dụ:

```
2026/08/04/[$LATEST]xxxxxxxx
```

![log-stream](/images/5-Workshop/5.6-Monitoring/cloudwatch-logs/log_stream.jpg)

---

### 4. Kiểm tra Lambda Execution Logs

Mở một Log Stream để xem chi tiết quá trình xử lý.

Ví dụ log:

```
START RequestId: xxxx

Received S3 Event

Processing image:
sample-image.jpg

Downloading image from S3...

Optimizing image...

Generating thumbnail...

Uploading result to S3...

Saving metadata to DynamoDB...

END RequestId: xxxx
```

Các thông tin quan trọng:

| Log              | Ý nghĩa                   |
| ---------------- | ------------------------- |
| START            | Lambda bắt đầu thực thi   |
| RequestId        | ID của lần gọi Lambda     |
| Processing image | File ảnh đang được xử lý  |
| Uploading result | Upload ảnh kết quả lên S3 |
| Saving metadata  | Lưu dữ liệu vào DynamoDB  |
| END              | Lambda hoàn thành         |

![execution-log](/images/5-Workshop/5.6-Monitoring/cloudwatch-logs/log_event.jpg)

---

### 5. Kiểm tra lỗi trong quá trình xử lý

Trong trường hợp Lambda gặp lỗi, CloudWatch sẽ ghi nhận thông tin lỗi.

Ví dụ:

```
ERROR

Image processing failed

Exception:
Unsupported image format
```

![Log-Error](/images/5-Workshop/5.6-Monitoring/cloudwatch-logs/cw_log_error.jpg)
Các lỗi có thể phát hiện:

- File ảnh không hợp lệ.
- Không có quyền truy cập S3.
- Không thể ghi dữ liệu vào DynamoDB.
- Lỗi trong quá trình xử lý bằng Pillow.

CloudWatch giúp xác định nguyên nhân để thực hiện xử lý sự cố.

---

### 6. Kiểm tra Lambda Metrics

Ngoài Logs, CloudWatch còn cung cấp các Metrics để theo dõi Lambda.

Các chỉ số quan trọng:

```
Invocations
Errors
Duration
Throttles
```

Ý nghĩa:

| Metric      | Mô tả                        |
| ----------- | ---------------------------- |
| Invocations | Số lần Lambda được gọi       |
| Errors      | Số lần Lambda xử lý thất bại |
| Duration    | Thời gian thực thi           |
| Throttles   | Số lần request bị giới hạn   |

Các chỉ số này giúp đánh giá hiệu năng và khả năng mở rộng của hệ thống.

---

### 7. Kiểm tra kết quả Monitoring

Sau khi upload ảnh và Lambda xử lý thành công:

CloudWatch hiển thị log:

```
START RequestId

Processing image

Image optimization completed

Saving metadata to DynamoDB

END RequestId
```

![Log](/images/5-Workshop/5.6-Monitoring/cloudwatch-logs/log_success.jpg)
Điều này xác nhận:

- S3 Trigger gọi Lambda thành công.
- Lambda thực hiện xử lý ảnh.
- Metadata được ghi vào DynamoDB.
- Quá trình xử lý hoàn tất.

---

### 8. Kết quả

Sau bước này, hệ thống đã được cấu hình và kiểm tra Monitoring bằng Amazon CloudWatch.

Kết quả đạt được:

- Lambda tự động tạo log sau mỗi lần thực thi.
- Có thể theo dõi quá trình xử lý ảnh.
- Có thể phát hiện và phân tích lỗi.
- Có dữ liệu phục vụ việc giám sát hệ thống.

Luồng Monitoring:

```
Amazon S3
     |
     |
S3 Event Trigger
     |
     v
AWS Lambda
     |
     |
CloudWatch Logs
     |
     |
Administrator
```

Bước tiếp theo sẽ cấu hình **Amazon SNS Notification** để gửi cảnh báo tự động khi hệ thống phát sinh lỗi.
