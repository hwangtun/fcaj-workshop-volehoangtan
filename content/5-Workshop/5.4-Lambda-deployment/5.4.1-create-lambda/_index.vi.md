---
title: "Tạo AWS Lambda Function"
date: 2026-08-04
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, AWS Lambda được sử dụng để thực hiện quá trình xử lý ảnh tự động sau khi người dùng tải ảnh lên Amazon S3.

Lambda function có nhiệm vụ:

- Nhận sự kiện từ Amazon S3.
- Đọc ảnh từ Input Bucket.
- Thực hiện tối ưu hóa ảnh bằng thư viện xử lý ảnh.
- Tạo ảnh thumbnail.
- Lưu ảnh kết quả vào Output Bucket.
- Lưu metadata của quá trình xử lý vào DynamoDB.

---

### 1. Truy cập AWS Lambda Console

Mở [AWS Lambda Console](https://console.aws.amazon.com/lambda/)

Trong thanh điều hướng bên trái, chọn:

```

Functions

```

Sau đó chọn:

```

Create function

```

để tạo Lambda Function mới.

![lambda-console](/images/5-Workshop/5.4-Lambda-deployment/lambda-console.png)

---

### 2. Chọn phương thức tạo Lambda Function

Trong giao diện **Create function**, chọn:

```

Author from scratch

```

Đây là phương thức tạo Lambda function từ source code được phát triển riêng cho hệ thống.

![author-from-scratch](/images/5-Workshop/5.4-Lambda-deployment/author-from-scratch.png)

---

### 3. Cấu hình thông tin Lambda Function

Trong phần **Basic information**, cấu hình:

#### Function name

Nhập tên Lambda:

```

autoImageProcessing

```

Tên này được sử dụng để định danh function xử lý ảnh trong hệ thống.

![function-name](/images/5-Workshop/5.4-Lambda-deployment/function-name.png)

---

#### Runtime

Chọn runtime:

```

Python 3.14

```

Lambda sử dụng Python để xử lý ảnh với các thư viện:

- boto3: tương tác với các dịch vụ AWS.
- Pillow (PIL): xử lý và tối ưu hình ảnh.

![lambda-runtime](/images/5-Workshop/5.4-Lambda-deployment/lambda-runtime.png)

![create-function](/images/5-Workshop/5.4-Lambda-deployment/create-function.png)

---

### 4. Cấu hình Execution Role cho Lambda

Sau khi Lambda Function được tạo thành công, truy cập:

```

Configuration
→ Permissions

```

Trong phần:

```

Execution role

```

chọn:

```

Edit

```

Sau đó chọn:

```

Use existing role

```

Chọn IAM Role đã được tạo trước đó:

```

LambdaExecutionRole

```

Role này chứa các quyền cần thiết cho quá trình xử lý ảnh:

- Amazon S3:
  - Đọc ảnh từ Input Bucket.
  - Ghi ảnh tối ưu và thumbnail vào Output Bucket.

- Amazon DynamoDB:
  - Lưu metadata của quá trình xử lý.

- AWS KMS:
  - Truy cập các tài nguyên được mã hóa bằng KMS.

- CloudWatch Logs:
  - Ghi log hoạt động của Lambda.

{{% notice note %}}
Lambda sử dụng Execution Role để truy cập các AWS Service khác trong quá trình xử lý. Việc cấp quyền được thực hiện theo nguyên tắc Least Privilege, chỉ cấp những quyền cần thiết cho chức năng xử lý ảnh.
{{% /notice %}}

---

### 5. Kiểm tra Lambda Function

Sau khi tạo và cấu hình Execution Role thành công, giao diện Lambda sẽ hiển thị:

```

Function name:

autoImageProcessing

```

Các thông tin quan trọng:

```

Runtime:
Python 3.14

Architecture:
x86_64

Execution role:
LambdaExecutionRole

```

---

### 6. Kết quả

Sau bước này, hệ thống đã có Lambda Function sẵn sàng để thực hiện xử lý ảnh.

Cấu trúc triển khai:

```

Input S3 Bucket
|
| Object Created Event
|
v
autoImageProcessing Lambda
|
+--> Download Original Image
|
+--> Optimize Image
|
+--> Generate Thumbnail
|
+--> Upload Optimized Image
|
+--> Upload Thumbnail
|
+--> Save Metadata To DynamoDB
|
+--> Write Logs To CloudWatch

```

Lambda Function sẽ tiếp tục được cấu hình source code, biến môi trường và S3 Event Trigger trong các bước tiếp theo.
