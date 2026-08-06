---
title: "Worklog Tuần 3"
date: 2026-07-06
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
| 2 | - Kiểm thử AWS Lambda với nhiều loại ảnh khác nhau.<br>- Kiểm tra hoạt động của Resize và Compression.<br>- Đánh giá chất lượng ảnh sau xử lý. | 06/07/2026 | 06/07/2026 | AWS Lambda Documentation |
| 3 | - Khắc phục lỗi Lambda Layer khi tích hợp Pillow.<br>- Tìm hiểu sự khác biệt giữa môi trường phát triển và môi trường Lambda.<br>- Thử nghiệm nhiều phương pháp đóng gói thư viện. | 07/07/2026 | 07/07/2026 | Pillow Documentation |
| 4 | - Sử dụng Docker để xây dựng Lambda Layer tương thích với Python Runtime.<br>- Kiểm tra khả năng import thư viện Pillow trên AWS Lambda. | 08/07/2026 | 08/07/2026 | AWS Lambda Layers Documentation |
| 5 | - Hoàn thiện chức năng sinh Thumbnail.<br>- Lưu ảnh đã xử lý vào Output Bucket.<br>- Kiểm tra cấu trúc thư mục lưu trữ trên Amazon S3. | 09/07/2026 | 10/07/2026 | Amazon S3 Documentation |
| 6 | - Thiết kế cấu trúc metadata phục vụ lưu trữ thông tin xử lý ảnh.<br>- Xác định các trường dữ liệu như Batch ID, Processing ID, trạng thái xử lý và kích thước ảnh.<br>- Tổng hợp kết quả kiểm thử hệ thống. | 11/07/2026 | 12/07/2026 | AWS Well-Architected Framework |

### Kết quả đạt được tuần 3

* Khắc phục thành công các lỗi phát sinh khi triển khai thư viện Pillow trên AWS Lambda.
* Xây dựng Lambda Layer tương thích với môi trường Runtime của AWS.
* Hoàn thiện các chức năng xử lý ảnh:
  * Resize ảnh.
  * Nén ảnh.
  * Sinh Thumbnail.
* Hoàn thiện quy trình lưu ảnh sau xử lý vào Output Bucket.
* Kiểm thử thành công luồng xử lý tự động từ Amazon S3 đến AWS Lambda.
* Thiết kế bộ metadata phục vụ việc lưu lịch sử xử lý ảnh và hỗ trợ tích hợp với DynamoDB trong các giai đoạn phát triển tiếp theo.