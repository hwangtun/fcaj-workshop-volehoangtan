---
title: "Kiểm tra hệ thống sau triển khai"
date: 2026-08-07
weight: 6
chapter: false
pre: " <b> 5.7.6. </b> "
---

---

Sau khi quá trình **CI/CD Pipeline** hoàn tất thành công, cần kiểm tra lại toàn bộ hệ thống để đảm bảo Frontend, Backend và quy trình xử lý hình ảnh hoạt động đúng trên môi trường AWS.

Quá trình kiểm tra được thực hiện theo luồng:

```text id="4a7g2k"
Frontend
   │
   ▼
Đăng nhập
   │
   ▼
Upload Image
   │
   ▼
S3 Original Bucket
   │
   ▼
Lambda xử lý ảnh
   │
   ├──► S3 Optimized Bucket
   │
   └──► CloudWatch Logs
```

### Bước 1: Kiểm tra Frontend trên AWS Amplify

Truy cập **AWS Management Console** và mở dịch vụ **AWS Amplify**.

Chọn ứng dụng đã được triển khai và nhánh `main`.

Lấy **URL public** của ứng dụng và mở URL trên trình duyệt.

#### Kết quả mong đợi

Giao diện **Automatic Image Optimization System** được hiển thị bình thường và có thể truy cập từ Internet.

---

### Bước 2: Kiểm tra chức năng đăng nhập

Trên giao diện ứng dụng, thực hiện đăng nhập bằng **Google**.

Kiểm tra xem quá trình xác thực có hoàn tất thành công và người dùng có thể truy cập vào các chức năng yêu cầu đăng nhập hay không.

#### Kết quả mong đợi

Người dùng đăng nhập thành công và có thể truy cập vào giao diện chính của hệ thống.

---

### Bước 3: Kiểm tra chức năng Upload

Truy cập chức năng **Upload** trên hệ thống.

Tải lên một hình ảnh có kích thước lớn hơn **1 MB** để kiểm tra quá trình xử lý.

Sau khi upload, kiểm tra trạng thái của hình ảnh trên giao diện nếu hệ thống có hỗ trợ hiển thị trạng thái xử lý.

#### Kết quả mong đợi

Hình ảnh được upload thành công và hệ thống bắt đầu quá trình xử lý.

---

### Bước 4: Kiểm tra S3 Original Bucket

Truy cập **Amazon S3** và mở bucket dùng để lưu trữ hình ảnh gốc (**Original Bucket**).

Kiểm tra xem file hình ảnh vừa upload đã được lưu trữ trong bucket hay chưa.

#### Kết quả mong đợi

Hình ảnh gốc xuất hiện trong Original Bucket với thông tin và đường dẫn tương ứng với file đã upload.

---

### Bước 5: Kiểm tra S3 Optimized Bucket

Sau khi Lambda hoàn tất quá trình xử lý, truy cập bucket lưu trữ hình ảnh đã tối ưu (**Optimized Bucket**).

Kiểm tra file hình ảnh được tạo ra sau quá trình xử lý.

Có thể kiểm tra các thông tin:

- Hình ảnh đã được tạo thành công.
- Kích thước file sau xử lý giảm so với hình ảnh gốc.
- Định dạng hình ảnh được chuyển sang **WebP** nếu cấu hình tối ưu yêu cầu.
- File được lưu đúng vị trí trong Output Bucket.

#### Kết quả mong đợi

Hình ảnh tối ưu xuất hiện trong Optimized Bucket và đáp ứng cấu hình xử lý đã thiết lập.

---

### Bước 6: Kiểm tra Lambda Logs trên CloudWatch

Để kiểm tra chi tiết quá trình xử lý, truy cập **Amazon CloudWatch** và mở Log Group của Lambda xử lý hình ảnh.

Tìm log tương ứng với lần upload vừa thực hiện và kiểm tra quá trình xử lý.

Có thể tham khảo phần [CloudWatch Logs](../../5.6-Monitoring/5.6.1-CloudWatch-logs/) để biết cách truy cập và theo dõi log của Lambda.

Kiểm tra xem Lambda có phát sinh lỗi trong quá trình xử lý hay không và xác nhận quá trình xử lý hoàn tất thành công.

#### Kết quả mong đợi

CloudWatch ghi nhận log của Lambda và không xuất hiện lỗi trong quá trình xử lý. Lambda hoàn tất việc tối ưu hình ảnh thành công.

---

### Kết quả kiểm tra

Sau khi hoàn thành các bước trên, có thể xác nhận toàn bộ luồng xử lý của hệ thống:

| Thành phần          | Kết quả mong đợi                 |
| ------------------- | -------------------------------- |
| AWS Amplify         | Frontend truy cập được           |
| Google Login        | Đăng nhập thành công             |
| S3 Original Bucket  | Nhận được hình ảnh gốc           |
| AWS Lambda          | Được kích hoạt và xử lý hình ảnh |
| S3 Optimized Bucket | Lưu được hình ảnh đã tối ưu      |
| CloudWatch          | Ghi nhận log xử lý thành công    |

Nếu các bước trên đều đạt kết quả mong đợi, hệ thống đã được triển khai thành công và sẵn sàng phục vụ quá trình sử dụng thực tế.
