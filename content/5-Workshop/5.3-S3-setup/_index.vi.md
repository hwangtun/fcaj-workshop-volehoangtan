---
title: "Cấu hình Amazon S3"
date: 2026-08-04
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Cấu hình Amazon S3

Trong phần này, nhóm tiến hành cấu hình **Amazon S3** để lưu trữ hình ảnh của hệ thống. Hai S3 Bucket được sử dụng gồm **Input Bucket** và **Output Bucket**.

- **Input Bucket** dùng để lưu trữ hình ảnh do người dùng tải lên. Khi có hình ảnh mới, Amazon S3 sẽ phát sinh sự kiện **ObjectCreated** để kích hoạt AWS Lambda thực hiện quá trình xử lý.
- **Output Bucket** dùng để lưu trữ hình ảnh sau khi được tối ưu hóa và các thumbnail được tạo trong quá trình xử lý.

Việc sử dụng Amazon S3 giúp hệ thống lưu trữ dữ liệu với độ bền cao, khả năng mở rộng linh hoạt và dễ dàng tích hợp với các dịch vụ khác của AWS.

#### Nội dung

- [Tạo các S3 Bucket lưu trữ ảnh](5.3.1-create-buckets/)
- [Cấu hình S3 Trigger cho AWS Lambda](5.3.2-configure-trigger/)
