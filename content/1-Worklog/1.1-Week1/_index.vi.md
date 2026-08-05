---
title: "Worklog Tuần 1"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu tuần 1

* Làm quen với môi trường thực tập và quy trình làm việc.
* Tìm hiểu yêu cầu của dự án **Automatic Image Optimization System**.
* Nghiên cứu kiến trúc tổng quan và các dịch vụ AWS sẽ sử dụng.
* Chuẩn bị môi trường phát triển cho dự án.

### Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Làm quen với môi trường thực tập và quy trình phát triển phần mềm.<br>- Trao đổi với người hướng dẫn về mục tiêu và phạm vi dự án.<br>- Cài đặt các công cụ phục vụ phát triển (Git, VS Code, AWS CLI). | 15/06/2026 | 15/06/2026 | Tài liệu nội bộ |
| 3 | - Tìm hiểu kiến trúc tổng quan của hệ thống tối ưu hóa ảnh.<br>- Xác định vai trò của Frontend, Backend J2EE và AWS trong hệ thống.<br>- Phân tích luồng xử lý dữ liệu từ người dùng đến AWS. | 16/06/2026 | 16/06/2026 | Tài liệu dự án |
| 4 | - Nghiên cứu các dịch vụ AWS sẽ sử dụng gồm Amazon S3, AWS Lambda và CloudWatch.<br>- Tìm hiểu nguyên lý hoạt động của Event Trigger trên Amazon S3. | 17/06/2026 | 17/06/2026 | https://docs.aws.amazon.com |
| 5 | - Tạo và cấu hình tài khoản AWS.<br>- Làm quen với AWS Management Console.<br>- Tìm hiểu cách quản lý tài nguyên trên AWS. | 18/06/2026 | 18/06/2026 | https://docs.aws.amazon.com |
| 6 | - Thiết kế sơ bộ kiến trúc hệ thống.<br>- Xác định quy trình xử lý ảnh: Upload → Amazon S3 → AWS Lambda → Output Bucket.<br>- Thảo luận các yêu cầu kỹ thuật của dự án. | 19/06/2026 | 20/06/2026 | AWS Architecture Center |

### Kết quả đạt được tuần 1

* Hiểu được mục tiêu, phạm vi và quy trình thực hiện của dự án **Automatic Image Optimization System**.
* Nắm được kiến trúc tổng quan của hệ thống bao gồm:
  * Frontend React.
  * Backend J2EE.
  * Amazon S3.
  * AWS Lambda.
  * CloudWatch.
* Hoàn thành việc tạo và cấu hình tài khoản AWS.
* Làm quen với AWS Management Console và AWS CLI.
* Hiểu được cơ chế kích hoạt AWS Lambda thông qua sự kiện tải ảnh lên Amazon S3.
* Chuẩn bị đầy đủ môi trường phát triển để bắt đầu triển khai các chức năng của hệ thống trong các tuần tiếp theo.