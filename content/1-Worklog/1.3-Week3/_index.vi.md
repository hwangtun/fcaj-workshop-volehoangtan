---
title: "Worklog Tuần 3"
date: 2026-06-29
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

### Mục tiêu tuần 3

* Hoàn thiện chức năng xử lý ảnh trên AWS Lambda.
* Khắc phục các lỗi phát sinh khi triển khai thư viện Pillow.
* Kiểm thử quy trình xử lý ảnh tự động.
* Chuẩn bị dữ liệu đầu ra phục vụ các chức năng mở rộng.

### Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Kiểm thử AWS Lambda với nhiều loại ảnh khác nhau.<br>- Kiểm tra hoạt động của Resize và Compression.<br>- Đánh giá chất lượng ảnh sau xử lý. | 29/06/2026 | 29/06/2026 | AWS Lambda Documentation |
| 3 | - Khắc phục lỗi Lambda Layer khi tích hợp Pillow.<br>- Tìm hiểu sự khác biệt giữa môi trường phát triển và môi trường Lambda.<br>- Thử nghiệm nhiều phương pháp đóng gói thư viện. | 30/06/2026 | 30/06/2026 | Pillow Documentation |
| 4 | - Sử dụng Docker để xây dựng Lambda Layer tương thích với Runtime Python.<br>- Kiểm tra khả năng import thư viện Pillow trên Lambda. | 01/07/2026 | 01/07/2026 | AWS Lambda Layers |
| 5 | - Hoàn thiện chức năng sinh Thumbnail.<br>- Lưu ảnh đã xử lý vào Output Bucket.<br>- Kiểm tra cấu trúc thư mục lưu trữ ảnh trên Amazon S3. | 02/07/2026 | 03/07/2026 | Amazon S3 Documentation |
| 6 | - Thiết kế cấu trúc metadata phục vụ lưu trữ thông tin xử lý ảnh.<br>- Xác định các trường dữ liệu như Batch ID, Processing ID, trạng thái xử lý và kích thước ảnh.<br>- Tổng hợp kết quả kiểm thử hệ thống. | 04/07/2026 | 05/07/2026 | AWS Best Practices |

### Kết quả đạt được tuần 3

* Khắc phục thành công các lỗi phát sinh khi triển khai Pillow trên AWS Lambda.
* Xây dựng Lambda Layer tương thích với môi trường Runtime của AWS.
* Hoàn thiện các chức năng xử lý ảnh:
  * Resize ảnh.
  * Nén ảnh.
  * Sinh Thumbnail.
* Hoàn thiện quy trình lưu ảnh sau xử lý vào Output Bucket.
* Kiểm thử thành công luồng xử lý tự động từ Amazon S3 đến AWS Lambda.
* Thiết kế bộ metadata phục vụ cho việc lưu lịch sử xử lý ảnh ở các giai đoạn phát triển tiếp theo.