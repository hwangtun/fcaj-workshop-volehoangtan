---
title: "Giới thiệu"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Giới thiệu về hệ thống Automatic Image Optimization System trên AWS

Hệ thống **Automatic Image Optimization System** được xây dựng nhằm tự động hóa quá trình tối ưu hóa hình ảnh trên nền tảng AWS. Hệ thống cho phép người dùng tải lên hình ảnh, tự động xử lý để giảm dung lượng, tối ưu kích thước và lưu trữ kết quả đã xử lý.

Giải pháp sử dụng kiến trúc serverless của AWS nhằm giảm thiểu việc quản lý hạ tầng, tăng khả năng mở rộng và đảm bảo tính ổn định trong quá trình xử lý số lượng lớn hình ảnh.

Các dịch vụ AWS chính được sử dụng trong hệ thống:

- **Amazon S3:** Lưu trữ hình ảnh đầu vào và hình ảnh sau khi tối ưu hóa.
- **AWS Lambda:** Tự động thực thi quá trình xử lý hình ảnh khi có hình ảnh mới được tải lên S3.
- **Amazon DynamoDB:** Lưu trữ metadata của quá trình xử lý như thông tin file, trạng thái xử lý, thời gian thực hiện và thông tin lỗi.
- **Amazon CloudWatch:** Theo dõi log, giám sát hoạt động của Lambda và hỗ trợ phát hiện lỗi trong hệ thống.
- **Amazon SNS:** Gửi thông báo cảnh báo đến quản trị viên khi xảy ra lỗi hoặc sự kiện quan trọng.
- **AWS IAM:** Quản lý quyền truy cập giữa các dịch vụ theo nguyên tắc cấp quyền tối thiểu.
- **AWS KMS:** Bảo vệ dữ liệu bằng cơ chế mã hóa và quản lý khóa bảo mật.

#### Tổng quan về kiến trúc hệ thống

Trong hệ thống này, quy trình xử lý hình ảnh được triển khai theo mô hình hướng sự kiện (event-driven architecture).

- **Input S3 Bucket:** Nhận hình ảnh do người dùng tải lên. Khi có file mới được tạo, sự kiện S3 Trigger sẽ kích hoạt AWS Lambda.

- **AWS Lambda Image Processing:** Lambda thực hiện các tác vụ tối ưu hóa hình ảnh như thay đổi kích thước, nén dung lượng và chuyển đổi định dạng theo cấu hình được lựa chọn.

- **Output S3 Bucket:** Lưu trữ các hình ảnh đã được xử lý và sẵn sàng để người dùng tải xuống.

- **Amazon DynamoDB:** Lưu lại thông tin metadata theo từng batch xử lý, giúp hệ thống có thể truy vấn lịch sử xử lý hình ảnh.

- **CloudWatch và SNS:** Theo dõi trạng thái hoạt động của hệ thống và gửi cảnh báo đến quản trị viên khi có lỗi xảy ra.

- **IAM và KMS:** Đảm bảo các dịch vụ AWS chỉ có quyền truy cập cần thiết và dữ liệu được bảo vệ trong quá trình lưu trữ.

![overview](/images/5-Workshop/5.1-Workshop-overview/kientruc.jpg)
