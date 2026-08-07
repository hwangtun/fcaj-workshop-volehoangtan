---
title: "Các bước chuẩn bị"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### IAM Permissions

Trước khi triển khai hệ thống **Automatic Image Optimization System**, cần chuẩn bị một tài khoản AWS hoặc IAM User có đầy đủ quyền để tạo và quản lý các dịch vụ được sử dụng trong dự án.

Trong quá trình phát triển, nhóm sử dụng IAM User với các quyền đối với những dịch vụ sau:

- Amazon S3
- AWS Lambda
- Amazon DynamoDB
- Amazon CloudWatch
- Amazon SNS
- AWS IAM
- AWS KMS

Đối với môi trường Production, nên áp dụng nguyên tắc **Least Privilege**, chỉ cấp những quyền cần thiết cho từng dịch vụ nhằm tăng cường bảo mật hệ thống.

![iam](images/5-Workshop/5.2-Prerequisite/iam_policy.png)

<!-- <center>
Hình 5.2.1: IAM Policy tự thiết lập với các quyền truy cập cần thiết theo nguyên tắc Least Privilege
</center> -->

---

#### Chuẩn bị các tài nguyên AWS

Trong workshop này, nhóm sử dụng **AWS Region N. Virginia (us-east-1)** để triển khai toàn bộ hệ thống.

Trước khi bắt đầu, tiến hành tạo các tài nguyên sau:

- Input S3 Bucket
- Output S3 Bucket
- DynamoDB Table
- Lambda Function
- IAM Role cho Lambda
- SNS Topic
- KMS Key

---

#### Tạo S3 Buckets

Tạo hai Amazon S3 Bucket để phục vụ quá trình xử lý hình ảnh:

- **Input Bucket:** lưu trữ hình ảnh do người dùng tải lên.
- **Output Bucket:** lưu trữ hình ảnh sau khi được tối ưu hóa.

Sau khi hoàn tất, kiểm tra danh sách Bucket trong Amazon S3 Console.

![s3](images/5-Workshop/5.2-Prerequisite/s3-buckets.png)

---

#### Tạo bảng DynamoDB

Tiếp theo, tạo bảng **ImageMetadata** để lưu trữ thông tin về quá trình xử lý hình ảnh.

Bảng bao gồm:

- Partition Key: `batchId`
- Sort Key: `processingId`

Ngoài ra, bảng còn lưu các thông tin như:

- userId
- originalName
- status
- originalSize
- processedSize
- compressionRatio
- uploadedAt
- processedAt

![dynamodb](images/5-Workshop/5.2-Prerequisite/dynamodb.png)

---

#### Tạo IAM Role cho Lambda

Tạo IAM Role dành cho AWS Lambda và cấp các quyền cần thiết để Lambda có thể tương tác với các dịch vụ AWS trong quá trình xử lý ảnh. IAM Role bao gồm quyền đọc/ghi Amazon S3, ghi metadata vào Amazon DynamoDB và ghi log lên Amazon CloudWatch. Trong trường hợp sử dụng mã hóa bằng AWS KMS hoặc gửi thông báo trực tiếp qua SNS, các quyền tương ứng sẽ được bổ sung vào Role. Sau khi tạo hoàn tất, IAM Role được gán làm Execution Role cho Lambda Function.

![iam-role](images/5-Workshop/5.2-Prerequisite/iam-role-for-lambda.png)

---

#### Kiểm tra môi trường

Sau khi hoàn thành các bước chuẩn bị, kiểm tra lại các tài nguyên đã được tạo thành công.

Môi trường triển khai cần có:

- 02 Amazon S3 Buckets.
- 01 AWS Lambda Function.
- 01 Amazon DynamoDB Table.
- 01 IAM Role.
- 01 SNS Topic.
- 01 AWS KMS Key.

Sau khi hoàn tất bước chuẩn bị, môi trường AWS đã sẵn sàng để triển khai hệ thống Automatic Image Optimization System trong các phần tiếp theo.
