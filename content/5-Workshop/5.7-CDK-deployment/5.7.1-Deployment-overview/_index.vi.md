---
title: "Tìm hiểu tổng quan quá trình triển khai bằng AWS CDK"
date: 2026-08-07
weight: 1
chapter: false
pre: " <b> 5.7.1. </b> "
---

---

Trong các phần trước, hệ thống **Automatic Image Optimization System** đã được xây dựng và cấu hình các thành phần cần thiết trên nền tảng AWS. Phần này trình bày quy trình triển khai toàn bộ hệ thống lên môi trường AWS thực tế bằng **AWS CDK**, **AWS Amplify** và **GitHub Actions**.

Quy trình triển khai được thực hiện theo hướng **IaC** kết hợp với **CI/CD**, giúp giảm thiểu các thao tác cấu hình thủ công, đảm bảo tính nhất quán và thuận tiện khi triển khai lại hệ thống.

Hệ thống triển khai gồm ba thành phần chính:

### 1. Backend Infrastructure

Hạ tầng Backend được định nghĩa dưới dạng **IaC** bằng **AWS CDK sử dụng Python**. CDK cho phép mô tả các tài nguyên AWS bằng mã nguồn và tự động tạo, cấu hình các tài nguyên cần thiết trong quá trình triển khai.

Các dịch vụ chính được sử dụng trong Backend Infrastructure gồm:

- **Amazon API Gateway:** Cung cấp các API để Frontend giao tiếp với Backend.
- **AWS Lambda:** Thực thi các chức năng xử lý của hệ thống.
- **Amazon S3:** Lưu trữ hình ảnh đầu vào và kết quả xử lý.
- **Amazon DynamoDB:** Lưu trữ metadata và thông tin trạng thái xử lý hình ảnh.
- **Amazon EventBridge:** Hỗ trợ cơ chế xử lý và điều phối các sự kiện trong hệ thống.
- **AWS IAM:** Quản lý quyền truy cập giữa các dịch vụ AWS.

Các tài nguyên trên được định nghĩa trong các CDK Stack của dự án và có thể được triển khai tự động thông qua AWS CDK.

### 2. Frontend Application

Frontend của hệ thống được xây dựng bằng **React và Vite**. Sau khi mã nguồn được build thành các file tĩnh, ứng dụng được triển khai và hosting trên **AWS Amplify**.

AWS Amplify cung cấp môi trường hosting để người dùng có thể truy cập ứng dụng thông qua Internet. Frontend sử dụng API được cung cấp bởi API Gateway để giao tiếp với Backend.

Quy trình triển khai Frontend gồm:

```text
React/Vite Source Code
        │
        ▼
     npm build
        │
        ▼
    Build Output
        │
        ▼
  AWS Amplify Hosting
```

### 3. CI/CD Pipeline

Quá trình triển khai được tự động hóa bằng **GitHub Actions**. Workflow CI/CD được cấu hình trong repository của dự án và thực hiện các bước cần thiết để cập nhật Backend và Frontend.

Quy trình tổng quát:

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions
    │
    ├──────────────► AWS CDK
    │                    │
    │                    ▼
    │             Backend Infrastructure
    │
    └──────────────► Build Frontend
                         │
                         ▼
                    AWS Amplify
```

Khi mã nguồn mới được push lên nhánh được cấu hình trong workflow, GitHub Actions sẽ tự động thực hiện quy trình triển khai theo cấu hình của dự án. Điều này giúp giảm thao tác thủ công và đảm bảo hệ thống được cập nhật theo phiên bản mã nguồn mới nhất.

### Tổng kết

Thông qua việc kết hợp **AWS CDK**, **AWS Amplify** và **GitHub Actions**, hệ thống có thể được triển khai theo quy trình tự động và nhất quán. AWS CDK chịu trách nhiệm định nghĩa và triển khai hạ tầng Backend, AWS Amplify cung cấp môi trường hosting cho Frontend, trong khi GitHub Actions đảm nhiệm việc tự động hóa quy trình CI/CD.

Các bước triển khai chi tiết sẽ được trình bày trong các phần tiếp theo.
