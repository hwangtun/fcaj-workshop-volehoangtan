---
title: "Worklog Tuần 2"
date: 2026-06-29
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

### Mục tiêu tuần 2

* Xây dựng môi trường lưu trữ ảnh trên Amazon S3.
* Triển khai AWS Lambda để xử lý ảnh tự động.
* Nghiên cứu thư viện Pillow phục vụ việc tối ưu hóa ảnh.
* Kiểm thử luồng xử lý ảnh tự động trên AWS.

### Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo Amazon S3 Input Bucket và Output Bucket.<br>- Kiểm tra quyền truy cập và cấu hình Bucket.<br>- Thực hiện Upload và Download ảnh để kiểm thử. | 29/06/2026 | 29/06/2026 | AWS S3 Documentation |
| 3 | - Tạo AWS Lambda Function sử dụng Python.<br>- Thực hiện các bài kiểm thử Lambda cơ bản.<br>- Làm quen với CloudWatch Logs để theo dõi quá trình thực thi. | 30/06/2026 | 30/06/2026 | AWS Lambda Documentation |
| 4 | - Tìm hiểu thư viện Pillow.<br>- Nghiên cứu cách đóng gói thư viện bằng Lambda Layer.<br>- Chuẩn bị môi trường xử lý ảnh trên Lambda. | 01/07/2026 | 01/07/2026 | Pillow Documentation |
| 5 | - Xây dựng chức năng Resize ảnh.<br>- Thực hiện nén ảnh JPEG.<br>- Thiết lập kích thước ảnh đầu ra theo yêu cầu hệ thống. | 02/07/2026 | 03/07/2026 | AWS Lambda Developer Guide |
| 6 | - Cấu hình Amazon S3 Event Trigger.<br>- Liên kết S3 với AWS Lambda.<br>- Kiểm thử quy trình Upload → Lambda → Output Bucket. | 04/07/2026 | 05/07/2026 | AWS Documentation |

### Kết quả đạt được tuần 2

* Hoàn thành việc triển khai Amazon S3 Input Bucket và Output Bucket.
* Xây dựng thành công AWS Lambda Function sử dụng Python.
* Làm quen với CloudWatch Logs để theo dõi và gỡ lỗi quá trình thực thi Lambda.
* Tích hợp thư viện Pillow để xử lý ảnh trên AWS Lambda.
* Hoàn thành các chức năng xử lý ảnh cơ bản:
  * Resize ảnh.
  * Nén ảnh JPEG.
  * Chuẩn bị sinh Thumbnail ở các giai đoạn tiếp theo.
* Cấu hình thành công S3 Event Trigger giúp Lambda tự động xử lý ảnh sau khi người dùng tải lên.
* Kiểm thử thành công quy trình xử lý ảnh từ Input Bucket đến Output Bucket.