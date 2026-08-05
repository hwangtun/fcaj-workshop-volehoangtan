---
title: "Bản đề xuất"
date: 2026-08-04
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

---

# Automatic Image Optimization System on AWS

## Giải pháp tối ưu hóa hình ảnh tự động sử dụng kiến trúc AWS Serverless

### 1. Tóm tắt điều hành

Automatic Image Optimization System là một nền tảng giúp tự động tối ưu hóa hình ảnh sau khi người dùng tải lên. Hệ thống hướng đến các cá nhân hoặc doanh nghiệp có nhu cầu lưu trữ số lượng lớn hình ảnh nhưng vẫn đảm bảo chất lượng hiển thị và tiết kiệm dung lượng lưu trữ.

Người dùng chỉ cần tải ảnh lên thông qua giao diện web. Sau đó, AWS Lambda sẽ tự động xử lý, nén và tối ưu ảnh, đồng thời tạo thumbnail và lưu kết quả vào Amazon S3. Toàn bộ thông tin về quá trình xử lý sẽ được lưu trong Amazon DynamoDB để phục vụ tra cứu lịch sử. Hệ thống sử dụng AWS KMS để mã hóa dữ liệu, Amazon CloudWatch để giám sát và Amazon SNS để gửi thông báo khi xảy ra lỗi.

Kiến trúc Serverless giúp hệ thống có khả năng mở rộng linh hoạt, giảm chi phí vận hành và hạn chế việc quản lý máy chủ.

---

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Nhiều cá nhân và doanh nghiệp cần tối ưu hình ảnh trước khi lưu trữ hoặc sử dụng trên website nhằm giảm dung lượng nhưng vẫn giữ chất lượng. Tuy nhiên, quá trình này thường được thực hiện thủ công bằng các phần mềm chỉnh sửa hoặc dịch vụ trực tuyến, gây mất thời gian và khó quản lý khi số lượng ảnh lớn.

Ngoài ra, nhiều hệ thống chỉ tập trung vào việc nén ảnh mà chưa cung cấp khả năng theo dõi lịch sử xử lý, quản lý metadata hay giám sát quá trình xử lý.

#### Giải pháp

Nhóm đề xuất xây dựng một hệ thống tối ưu hóa hình ảnh tự động trên nền tảng AWS.

Người dùng tải ảnh lên thông qua ứng dụng web. Backend Spring Boot lưu ảnh vào Amazon S3 Input Bucket và ghi nhận metadata ban đầu. Khi có ảnh mới, Amazon S3 sẽ kích hoạt AWS Lambda để thực hiện tối ưu hóa ảnh theo cấu hình người dùng lựa chọn.

Ảnh sau khi xử lý sẽ được lưu vào Amazon S3 Output Bucket cùng với thumbnail. Metadata sẽ được cập nhật vào Amazon DynamoDB để phục vụ truy vấn lịch sử xử lý. AWS CloudWatch thu thập log và giám sát hoạt động của hệ thống, trong khi Amazon SNS gửi cảnh báo khi quá trình xử lý gặp lỗi. AWS KMS được sử dụng để mã hóa dữ liệu lưu trữ trên S3 nhằm tăng cường bảo mật.

Hệ thống cung cấp giao diện web cho phép người dùng:

- Đăng ký và đăng nhập.
- Tải lên nhiều ảnh cùng lúc.
- Theo dõi trạng thái xử lý.
- Xem lịch sử tối ưu hóa.
- Tải xuống ảnh đã xử lý.

Ngoài ra, quản trị viên có thể theo dõi thống kê sử dụng và quản lý tài khoản người dùng.

#### Lợi ích và hoàn vốn đầu tư (ROI)

Hệ thống giúp giảm đáng kể thời gian tối ưu hóa hình ảnh thủ công, đồng thời tiết kiệm dung lượng lưu trữ và băng thông truyền tải. Việc tự động hóa toàn bộ quy trình giúp giảm công sức quản lý, nâng cao hiệu quả làm việc và dễ dàng mở rộng khi số lượng người dùng tăng lên.

Nhờ sử dụng các dịch vụ Serverless của AWS, chi phí vận hành chỉ phát sinh khi có yêu cầu xử lý, giúp tối ưu ngân sách cho các hệ thống vừa và nhỏ.

---

### 3. Kiến trúc giải pháp

Hệ thống áp dụng kiến trúc AWS Serverless kết hợp với ứng dụng Backend Spring Boot và Frontend React.

Luồng xử lý chính:

1. Người dùng tải ảnh lên thông qua giao diện web.
2. Backend lưu ảnh vào Amazon S3 Input Bucket.
3. Amazon S3 phát sinh sự kiện ObjectCreated.
4. AWS Lambda được kích hoạt để xử lý ảnh.
5. Lambda tối ưu hóa ảnh và tạo thumbnail.
6. Kết quả được lưu vào Amazon S3 Output Bucket.
7. Metadata được cập nhật vào Amazon DynamoDB.
8. CloudWatch ghi log và theo dõi hoạt động.
9. SNS gửi thông báo nếu xảy ra lỗi.
10. Người dùng xem lịch sử và tải ảnh đã xử lý.

_(Thêm sơ đồ kiến trúc của nhóm tại đây.)_

#### Dịch vụ AWS sử dụng

- Amazon S3: lưu trữ ảnh gốc và ảnh sau khi tối ưu.
- AWS Lambda: xử lý và tối ưu hóa hình ảnh.
- Amazon DynamoDB: lưu metadata và lịch sử xử lý.
- AWS KMS: mã hóa dữ liệu trên Amazon S3.
- Amazon CloudWatch: giám sát và ghi log.
- Amazon SNS: gửi thông báo lỗi.
- AWS IAM: quản lý quyền truy cập các dịch vụ AWS.

#### Thiết kế thành phần

- **Frontend (React):** giao diện người dùng.
- **Backend (Spring Boot):** xác thực, upload ảnh, truy vấn lịch sử.
- **Amazon S3:** lưu trữ ảnh đầu vào và đầu ra.
- **AWS Lambda:** xử lý ảnh bằng Pillow.
- **Amazon DynamoDB:** lưu trạng thái xử lý và metadata.
- **AWS KMS:** mã hóa dữ liệu lưu trữ.
- **CloudWatch & SNS:** giám sát và cảnh báo hệ thống.

---

### 4. Triển khai kỹ thuật

#### Các giai đoạn triển khai

1. Phân tích yêu cầu và thiết kế kiến trúc hệ thống.
2. Xây dựng Backend Spring Boot và API.
3. Xây dựng giao diện người dùng bằng React.
4. Triển khai AWS S3, Lambda, DynamoDB và IAM.
5. Tích hợp các thành phần AWS với Backend.
6. Kiểm thử chức năng và triển khai hoàn chỉnh.

#### Yêu cầu kỹ thuật

**Frontend**

- React
- TypeScript
- Tailwind CSS
- Ant Design

**Backend**

- Spring Boot
- Spring Security
- JWT Authentication
- AWS SDK for Java

**AWS**

- Amazon S3
- AWS Lambda (Python + Pillow)
- Amazon DynamoDB
- AWS IAM
- AWS KMS
- Amazon CloudWatch
- Amazon SNS

---

### 5. Lộ trình và mốc triển khai

- **Tuần 1:** Phân tích yêu cầu và thiết kế kiến trúc.
- **Tuần 2:** Xây dựng Backend và Frontend.
- **Tuần 3:** Triển khai các dịch vụ AWS.
- **Tuần 4:** Tích hợp hệ thống, kiểm thử và hoàn thiện báo cáo.

---

### 6. Ước tính ngân sách

Chi phí chủ yếu đến từ các dịch vụ AWS.

**Hạ tầng dự kiến**

- Amazon S3
- AWS Lambda
- Amazon DynamoDB
- AWS KMS
- Amazon CloudWatch
- Amazon SNS

Đối với quy mô thử nghiệm và học tập, tổng chi phí ước tính chỉ vài USD mỗi tháng nhờ tận dụng AWS Free Tier và mô hình thanh toán theo mức sử dụng (Pay-as-you-go).

---

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

- Lambda xử lý thất bại.
- Upload ảnh không thành công.
- Vượt giới hạn Free Tier.
- Người dùng tải lên ảnh không hợp lệ.
- Mất kết nối với dịch vụ AWS.

#### Chiến lược giảm thiểu

- Sử dụng Amazon CloudWatch Logs để theo dõi hoạt động của AWS Lambda và hỗ trợ phân tích lỗi trong quá trình xử lý ảnh.
- Sử dụng Amazon SNS để gửi cảnh báo đến quản trị viên khi quá trình tối ưu hóa ảnh xảy ra lỗi.
- Kiểm tra định dạng và kích thước file ảnh trước khi đưa vào quá trình xử lý nhằm hạn chế lỗi từ dữ liệu đầu vào.
- Áp dụng IAM Role với quyền truy cập tối thiểu cho các dịch vụ AWS nhằm tăng cường bảo mật.
- Sử dụng AWS KMS để mã hóa dữ liệu hình ảnh lưu trữ trên Amazon S3.

#### Kế hoạch dự phòng

- Lưu trữ metadata và trạng thái xử lý trong Amazon DynamoDB giúp quản trị viên theo dõi lịch sử và xác định nguyên nhân khi xảy ra lỗi.
- Duy trì log xử lý trên Amazon CloudWatch để hỗ trợ kiểm tra và khắc phục sự cố.
- Sao lưu thông tin cấu hình quan trọng của hệ thống AWS nhằm hỗ trợ khôi phục khi cần thiết.

---

### 8. Kết quả kỳ vọng

#### Cải tiến kỹ thuật

- Tự động hóa hoàn toàn quy trình tối ưu hóa hình ảnh.
- Giảm dung lượng lưu trữ nhưng vẫn duy trì chất lượng hình ảnh.
- Theo dõi trạng thái xử lý theo thời gian thực.
- Quản lý lịch sử xử lý tập trung.

#### Giá trị dài hạn

Hệ thống là nền tảng để phát triển các ứng dụng xử lý đa phương tiện trên nền tảng đám mây. Trong tương lai có thể mở rộng thêm các chức năng như chuyển đổi định dạng ảnh, watermark, AI Image Enhancement, nhận diện nội dung bằng Amazon Rekognition hoặc tích hợp CDN để tối ưu phân phối hình ảnh.
