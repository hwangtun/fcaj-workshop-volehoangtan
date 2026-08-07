---
title: "Workshop"
date: 2026-08-04
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

### Triển khai hệ thống Automatic Image Optimization System trên AWS

#### Tổng quan

Hệ thống **Automatic Image Optimization System** được triển khai trên nền tảng AWS nhằm tự động hóa quá trình xử lý và tối ưu hóa hình ảnh. Giải pháp sử dụng kiến trúc serverless, giúp giảm thiểu việc quản lý hạ tầng, tăng khả năng mở rộng và đảm bảo khả năng xử lý ổn định khi số lượng hình ảnh tăng lên.

Trong hệ thống này, người dùng có thể tải hình ảnh lên hệ thống. Hình ảnh sau đó được lưu trữ trên **Amazon S3** và kích hoạt quá trình xử lý tự động thông qua **AWS Lambda**. Lambda thực hiện các thao tác tối ưu như nén dung lượng, thay đổi kích thước và tạo phiên bản hình ảnh đã xử lý.

Kết quả xử lý được lưu trữ trên S3 Output Bucket, đồng thời thông tin metadata của quá trình xử lý được lưu vào **Amazon DynamoDB** để phục vụ việc tra cứu lịch sử, theo dõi trạng thái và quản lý dữ liệu.

Sau quá trình xây dựng và kiểm thử các thành phần của hệ thống, nhóm sử dụng **AWS CDK (Cloud Development Kit)** để tự động hóa quá trình triển khai hạ tầng dưới dạng **Infrastructure as Code (IaC)**. Bên cạnh đó, quy trình triển khai được tích hợp với **CI/CD Pipeline** nhằm tự động hóa việc cập nhật và triển khai hệ thống, giúp đảm bảo tính nhất quán và giảm thiểu thao tác cấu hình thủ công.

Hệ thống cũng tích hợp các cơ chế giám sát và bảo mật:

- **CloudWatch:** Theo dõi log, trạng thái hoạt động và hiệu suất của Lambda.
- **SNS:** Gửi thông báo cảnh báo đến quản trị viên khi xảy ra lỗi trong quá trình xử lý.
- **IAM:** Quản lý quyền truy cập giữa các dịch vụ AWS theo nguyên tắc Least Privilege.
- **KMS:** Mã hóa dữ liệu nhằm tăng cường khả năng bảo mật trong quá trình lưu trữ.

#### Nội dung

1. [Tổng quan kiến trúc hệ thống](5.1-Workshop-overview/)
2. [Chuẩn bị môi trường AWS](5.2-Prerequiste/)
3. [Triển khai Amazon S3 lưu trữ hình ảnh](5.3-S3-setup/)
4. [Triển khai AWS Lambda xử lý ảnh](5.4-Lambda-deployment/)
5. [Triển khai Amazon DynamoDB lưu trữ Metadata](5.5-DynamoDB/)
6. [Monitoring hệ thống với CloudWatch và SNS](5.6-Monitoring/)
7. [Triển khai hệ thống tự động bằng AWS CDK và CI/CD](5.7-CDK-deployment/)
8. [Dọn dẹp tài nguyên AWS sau triển khai](5.8-Cleanup/)
