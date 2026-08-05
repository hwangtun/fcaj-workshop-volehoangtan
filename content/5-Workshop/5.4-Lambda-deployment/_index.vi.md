---
title: "Triển khai AWS Lambda xử lý ảnh"
date: 2026-08-04
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

Trong hệ thống **Automatic Image Optimization System on AWS**, AWS Lambda đóng vai trò là thành phần xử lý chính, chịu trách nhiệm tự động tối ưu hóa hình ảnh sau khi người dùng tải ảnh lên hệ thống.

Lambda được sử dụng theo mô hình **serverless**, giúp hệ thống có thể xử lý ảnh mà không cần quản lý máy chủ hoặc hạ tầng chạy ứng dụng.

Quy trình hoạt động của Lambda:

```
User Upload Image
        |
        v
Amazon S3 Input Bucket
        |
        v
S3 Trigger Event
        |
        v
AWS Lambda
        |
        +----------------+
        |                |
        v                v
S3 Output Bucket    DynamoDB
(Optimized Image)   (Metadata)
```

AWS Lambda thực hiện các nhiệm vụ:

- Nhận sự kiện upload ảnh từ Amazon S3.
- Đọc file ảnh từ Input Bucket.
- Kiểm tra định dạng và thông tin ảnh.
- Tối ưu kích thước, dung lượng ảnh.
- Chuyển đổi định dạng ảnh theo cấu hình.
- Tạo thumbnail phục vụ hiển thị nhanh.
- Upload kết quả vào Output Bucket.
- Lưu thông tin quá trình xử lý vào DynamoDB.

---

### Các bước triển khai Lambda

Quá trình triển khai AWS Lambda được thực hiện qua các bước:

#### 5.4.1. Tạo Lambda Function

Tạo Lambda Function mới trên AWS Console, lựa chọn runtime phù hợp và cấu hình quyền thực thi cho Lambda.

Lambda sử dụng IAM Role để truy cập các dịch vụ AWS cần thiết:

- Amazon S3: đọc ảnh đầu vào và ghi ảnh kết quả.
- Amazon DynamoDB: lưu metadata xử lý.
- CloudWatch Logs: ghi log và theo dõi hoạt động.

---

#### 5.4.2. Cấu hình Lambda Function

Sau khi tạo Lambda, tiến hành cấu hình:

- Upload source code xử lý ảnh.
- Cấu hình biến môi trường.
- Thiết lập timeout và memory phù hợp.
- Cấu hình quyền truy cập AWS Resource.

Các biến môi trường được sử dụng:

```
INPUT_BUCKET
OUTPUT_BUCKET
METADATA_TABLE
MAX_WIDTH
JPEG_QUALITY
THUMB_SIZE
```

Những thông tin này giúp Lambda có thể hoạt động linh hoạt mà không cần thay đổi code khi thay đổi cấu hình hệ thống.

---

#### 5.4.3. Kiểm thử quá trình xử lý

Sau khi hoàn thành cấu hình, tiến hành kiểm thử toàn bộ quy trình:

1. Upload ảnh vào Input Bucket.
2. Kiểm tra Lambda được kích hoạt.
3. Kiểm tra log trong CloudWatch.
4. Kiểm tra ảnh tối ưu trong Output Bucket.
5. Kiểm tra metadata trong DynamoDB.

Kết quả mong đợi:

- Lambda xử lý ảnh thành công.
- Ảnh đầu ra được tạo đúng cấu trúc.
- Metadata được lưu đầy đủ.
- Trạng thái xử lý được cập nhật:

```
status = SUCCESS
```

---

### Vai trò của Lambda trong hệ thống

Việc sử dụng AWS Lambda giúp hệ thống:

- Tự động hóa quy trình xử lý ảnh.
- Có khả năng mở rộng khi số lượng ảnh tăng.
- Không cần duy trì server xử lý riêng.
- Kết hợp dễ dàng với Amazon S3 và DynamoDB.
- Giảm chi phí vận hành nhờ mô hình tính phí theo số lần thực thi.

Sau khi hoàn thành triển khai Lambda, hệ thống có khả năng tự động nhận ảnh từ người dùng và thực hiện toàn bộ quá trình tối ưu hóa ảnh trên nền tảng AWS.

#### Nội dung

- [Tạo AWS Lambda Function](5.4.1-create-lambda/)
- [Cấu hình AWS Lambda Function](5.4.2-configure-lambda/)
- [Kiểm tra quá trình xử lý ảnh bằng AWS Lambda](5.4.3-test-upload/)
