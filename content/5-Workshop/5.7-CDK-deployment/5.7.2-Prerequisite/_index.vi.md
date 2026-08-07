---
title: "Chuẩn bị môi trường triển khai"
date: 2026-08-07
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

---

Trước khi bắt đầu triển khai hệ thống bằng **AWS CDK**, cần chuẩn bị môi trường trên máy tính cá nhân hoặc môi trường **AWS Cloud9**. Các công cụ dưới đây được sử dụng để triển khai Backend Infrastructure, build Frontend và thực hiện quá trình deployment.

### 1. AWS CLI

**AWS CLI (Command Line Interface)** được sử dụng để tương tác với các dịch vụ AWS thông qua dòng lệnh.

Kiểm tra AWS CLI đã được cài đặt:

```bash
aws --version
```

Nếu chưa cấu hình thông tin xác thực, thực hiện:

```bash
aws configure
```

Sau đó nhập các thông tin được yêu cầu:

```text
AWS Access Key ID:
AWS Secret Access Key:
Default region name:
Default output format:
```

Tài khoản AWS sử dụng để triển khai cần có đủ quyền tạo và cấu hình các tài nguyên cần thiết cho hệ thống, chẳng hạn như **IAM, S3, Lambda, DynamoDB, API Gateway** và các dịch vụ liên quan.

> **Lưu ý:** Không chia sẻ hoặc commit Access Key và Secret Access Key lên repository.

---

### 2. Node.js

**Node.js phiên bản 18 trở lên** được sử dụng để cài đặt AWS CDK CLI và build ứng dụng Frontend React/Vite.

Kiểm tra phiên bản Node.js:

```bash
node --version
```

Kiểm tra npm:

```bash
npm --version
```

Nếu kết quả hiển thị phiên bản Node.js từ 18 trở lên, môi trường đã đáp ứng yêu cầu.

---

### 3. AWS CDK CLI

AWS CDK CLI được sử dụng để thực hiện các thao tác như bootstrap, synthesize và deploy các CDK Stack.

Cài đặt AWS CDK CLI thông qua npm:

```bash
npm install -g aws-cdk
```

Sau khi cài đặt, kiểm tra phiên bản:

```bash
cdk --version
```

Nếu lệnh trả về phiên bản AWS CDK, quá trình cài đặt đã hoàn tất.

---

### 4. Python

Dự án sử dụng **Python 3.13** cho các CDK Stack và các thành phần Python liên quan.

Kiểm tra phiên bản Python:

```bash
python --version
```

Kết quả mong đợi có dạng:

```text
Python 3.13.x
```

Python sẽ được sử dụng trong các bước tiếp theo để tạo môi trường ảo và cài đặt các thư viện cần thiết cho dự án.

---

### 5. Docker

**Docker** được sử dụng trong quá trình triển khai Backend. Một số thành phần Lambda của hệ thống được đóng gói dưới dạng Docker Image thông qua cấu hình `DockerImageFunction` trong AWS CDK.

Do đó, Docker cần được cài đặt và đang ở trạng thái **running** trước khi thực hiện `cdk deploy`.

Kiểm tra Docker:

```bash
docker --version
```

Có thể kiểm tra Docker đang hoạt động bằng:

```bash
docker ps
```

Nếu Docker đang hoạt động, lệnh trên sẽ trả về danh sách các container đang chạy hoặc danh sách rỗng nếu chưa có container nào.

> **Lưu ý:** Khi thực hiện triển khai, AWS CDK có thể sử dụng Docker để build Docker Image cho các Lambda được định nghĩa trong CDK Stack. Vì vậy, Docker Desktop cần được khởi động trước khi thực hiện quá trình deployment.

---

### 6. Kiểm tra môi trường

Sau khi hoàn thành các bước trên, có thể kiểm tra nhanh các công cụ cần thiết bằng các lệnh:

```bash
aws --version
node --version
npm --version
cdk --version
python --version
docker --version
```

Nếu các lệnh đều trả về thông tin phiên bản tương ứng và AWS CLI đã được cấu hình thành công, môi trường đã sẵn sàng để thực hiện các bước triển khai hệ thống bằng AWS CDK.
