---
title: "Cấu hình AWS Lambda Function"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

Sau khi tạo Lambda Function, bước tiếp theo là cấu hình các thành phần cần thiết để Lambda có thể thực hiện quá trình tối ưu hóa ảnh trong hệ thống **Automatic Image Optimization System on AWS**.

Các cấu hình bao gồm:

- Upload source code xử lý ảnh.
- Cấu hình Handler.
- Cấu hình Environment Variables.
- Thiết lập tài nguyên thực thi.
- Kiểm tra quyền truy cập AWS Service thông qua Execution Role.
- Gắn Lambda Layer chứa thư viện Pillow.

---

### 1. Upload source code cho Lambda

Truy cập Lambda Function:

```

autoImageProcessing

```

Trong tab:

```

Code

```

Chọn:

```

Upload from
|
+--> .zip file

```

Upload file source code chứa chương trình xử lý ảnh.

Cấu trúc source code:

```

lambda-function/
│
├── lambda_function.py
└── requirements.txt

```

Trong đó:

- `lambda_function.py`: chứa logic xử lý sự kiện từ Amazon S3.
- `boto3`: thư viện dùng để tương tác với các dịch vụ AWS.
- `Pillow`: được cung cấp thông qua Lambda Layer.

{{% notice note %}}
Thư viện Pillow không được đóng gói trực tiếp cùng source code. Lambda sử dụng Lambda Layer riêng để cung cấp thư viện xử lý ảnh, đảm bảo môi trường chạy tương thích với AWS Lambda Linux.
{{% /notice %}}

---

### 2. Cấu hình Handler

Trong phần:

```

Runtime settings

```

Chọn:

```

Edit

```

Cấu hình Handler:

```

lambda_function.lambda_handler

```

Giải thích:

- `lambda_function`: tên file Python chứa source code.
- `lambda_handler`: tên function entry point được AWS Lambda gọi khi nhận sự kiện từ S3.

Ví dụ:

```python
def lambda_handler(event, context):
    # Process S3 event
```

---

### 3. Gắn Lambda Layer cho Pillow

Do Lambda sử dụng thư viện Pillow để xử lý ảnh, cần gắn Lambda Layer chứa thư viện này.

Truy cập:

```
Lambda Function
    |
    +--> Code
    |
    +--> Layers
```

Chọn:

```
Add a layer
```

Chọn:

```
Custom layers
```

Chọn layer:

```
pillow-layer
```

Runtime tương thích:

```
Python 3.14
```

Sau khi thêm layer, Lambda có thể sử dụng:

```python
from PIL import Image
```

để thực hiện:

- Resize ảnh.
- Nén ảnh.
- Tạo thumbnail.

---

### 4. Cấu hình Environment Variables

Lambda sử dụng Environment Variables để lưu các thông tin cấu hình thay vì ghi trực tiếp trong source code.

Truy cập:

```
Configuration
    |
    +--> Environment variables
```

Chọn:

```
Edit
```

Thêm biến môi trường:

| Key            | Value         |
| -------------- | ------------- |
| METADATA_TABLE | ImageMetadata |

Biến này được Lambda sử dụng trong quá trình:

<!-- - Xác định Output Bucket lưu ảnh tối ưu. -->

- Lưu metadata vào DynamoDB.
  <!-- - Thiết lập kích thước resize. -->
  <!-- - Cấu hình chất lượng nén ảnh. -->
  <!-- - Tạo thumbnail. -->

![environment-variable](/images/5-Workshop/5.4-Lambda-deployment/environment-variable.png)

{{% notice note %}}
Việc sử dụng Environment Variables giúp tách biệt cấu hình và source code, giúp dễ dàng thay đổi AWS Resource mà không cần chỉnh sửa chương trình.
{{% /notice %}}

---

### 5. Cấu hình tài nguyên thực thi

Quá trình xử lý ảnh yêu cầu Lambda thực hiện:

- Download ảnh từ Input Bucket.
- Resize ảnh.
- Tạo thumbnail.
- Upload ảnh kết quả.
- Ghi metadata vào DynamoDB.

Do đó Lambda cần được cấu hình tài nguyên phù hợp.

Truy cập:

```
Configuration
    |
    +--> General configuration
```

Chọn:

```
Edit
```

---

#### Memory

Thiết lập:

```
512 MB
```

Memory cao hơn giúp cải thiện hiệu suất xử lý ảnh bằng thư viện Pillow.

---

#### Timeout

Thiết lập:

```
30 sec
```

Lambda có đủ thời gian để:

- Đọc ảnh từ S3.
- Thực hiện tối ưu hóa.
- Tạo thumbnail.
- Upload kết quả.
- Lưu metadata.

![lambda-resource](/images/5-Workshop/5.4-Lambda-deployment/resource.png)

---

### 6. Kiểm tra IAM Permission cho Lambda

Lambda sử dụng Execution Role để truy cập các AWS Service trong quá trình xử lý.

Truy cập:

```
Configuration
    |
    +--> Permissions
```

Kiểm tra Execution Role:

```
LambdaExecutionRole
```

IAM Role cần chứa các quyền:

---

#### Amazon S3

Các quyền:

```

s3:GetObject
s3:PutObject

```

Mục đích:

- Đọc ảnh gốc từ Input Bucket.
- Upload ảnh tối ưu và thumbnail vào Output Bucket.

---

#### Amazon DynamoDB

Các quyền:

```

dynamodb:PutItem
dynamodb:UpdateItem

```

Mục đích:

- Lưu metadata xử lý ảnh.
- Cập nhật trạng thái:

```

PROCESSING
SUCCESS
FAILED

```

---

#### AWS KMS

Các quyền:

```

kms:Decrypt
kms:Encrypt
kms:GenerateDataKey

```

Mục đích:

- Cho phép Lambda truy cập dữ liệu được mã hóa bằng KMS.
- Hỗ trợ quá trình đọc và ghi object trên S3 sử dụng Server-Side Encryption.

---

#### CloudWatch Logs

Quyền:

```

AWSLambdaBasicExecutionRole

```

Mục đích:

- Ghi log khi Lambda thực thi.
- Theo dõi lỗi trong quá trình xử lý.

{{% notice warning %}}
Trong môi trường thực tế, không nên sử dụng các policy Full Access. IAM Policy nên giới hạn đúng các quyền mà Lambda cần sử dụng theo nguyên tắc Least Privilege.
{{% /notice %}}

---

### 7. Kiểm tra cấu hình mã hóa

Hệ thống sử dụng AWS KMS để bảo vệ dữ liệu lưu trữ trên Amazon S3.

Các Bucket:

```

Input Bucket
Output Bucket

```

được cấu hình sử dụng KMS Key.

Lambda truy cập các dữ liệu này thông qua Execution Role:

```

LambdaExecutionRole

```

---

### 8. Lưu cấu hình Lambda

Sau khi hoàn tất các thay đổi:

Click:

```

Save

```

AWS Lambda sẽ cập nhật cấu hình mới cho function.

---

### 9. Kiểm tra cấu hình hoàn tất

Sau khi cấu hình thành công:

```

Function name:
autoImageProcessing

Runtime:
Python 3.14

Handler:
lambda_function.lambda_handler

Memory:
512 MB

Timeout:
30 sec

Environment:
Configured

Layer:
pillow-layer

Execution Role:
image-optimizer-role

```

![lambda-configured](/images/5-Workshop/5.4-Lambda-deployment/resource.png)

---

### 10. Kết quả

Sau bước này, Lambda Function đã được chuẩn bị đầy đủ để xử lý ảnh tự động.

Kiến trúc hiện tại:

```

                Backend API
                     |
                     |
                     v
              Input S3 Bucket
                     |
                     |
            S3 Object Created Event
                     |
                     v
          autoImageProcessing Lambda
                     |
        +------------+------------+
        |                         |
        v                         v

Optimize Image Generate Thumbnail
| |
+------------+------------+
|
v
Output S3 Bucket
|
v
DynamoDB Metadata
|
v
CloudWatch Logs

```

Lambda Function sẽ tiếp tục được cấu hình **S3 Event Trigger** trong bước tiếp theo.
