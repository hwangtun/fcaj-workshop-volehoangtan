---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Amazon SQS – Chìa khóa cho kiến trúc xử lý bất đồng bộ (Asynchronous Architecture) trên AWS
Trong quá trình thực tập và tìm hiểu về các dịch vụ Serverless trên AWS, em đã thực hiện một bài chia sẻ về Amazon Simple Queue Service (Amazon SQS) trên cộng đồng AWS Study Group. Nội dung bài viết giới thiệu vai trò của Amazon SQS trong việc xây dựng kiến trúc xử lý bất đồng bộ (Asynchronous Architecture), giúp tăng khả năng mở rộng, giảm tải hệ thống và nâng cao độ tin cậy của ứng dụng.

Các nội dung chính của bài viết gồm:

* Giới thiệu Amazon SQS và cơ chế hoạt động của Message Queue.
* Phân tích mô hình xử lý bất đồng bộ (Asynchronous Processing) khi kết hợp Amazon API Gateway, Amazon SQS và AWS Lambda.
* Trình bày những lợi ích của Amazon SQS như giảm sự phụ thuộc giữa các thành phần (Loose Coupling), tăng khả năng mở rộng (Scalability) và hỗ trợ Retry khi xảy ra lỗi.
* Giới thiệu một số Best Practices khi triển khai Amazon SQS, bao gồm Dead Letter Queue (DLQ), Visibility Timeout và thiết kế Lambda theo nguyên tắc Idempotent.
* Minh họa kiến trúc xử lý bất đồng bộ bằng sơ đồ sử dụng Amazon API Gateway, Amazon SQS, AWS Lambda, Amazon S3 và Amazon DynamoDB.

Bài viết giúp em củng cố kiến thức về Amazon SQS cũng như hiểu rõ hơn cách xây dựng các hệ thống Serverless có khả năng mở rộng và chịu tải cao trên nền tảng AWS.

## Hình minh họa

![overview](/images/3-BlogsPosted/blog_1.png)

## Link bài viết

[Xem bài viết trên AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2234076147357345&hoisted_section_header_type=recently_seen&__cft__[0]=AZbn73yW8KNKYQgwxHrpiV3agA4I1MOTX3p8eQ7Sj0V-jexe8G1iLBdySt-X0l7HN4mNckwnvLGcs2ttzAa0OoZ-Zr0OO51kfS2EaKSoOkRsbjxodd2ztOzZVpeqhruT95_EuG5pssrW3WjsCjhjKhlt6qNfKYial3JlxO6VesbUg8RUNPmOy3wq100eG2LrZjE&__tn__=%2CO%2CP-R)