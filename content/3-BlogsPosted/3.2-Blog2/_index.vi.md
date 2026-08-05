---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# Event-Driven Architecture – Nền tảng cho các hệ thống Serverless trên AWS

Trong quá trình thực tập và tìm hiểu về các kiến trúc Serverless trên AWS, em đã thực hiện một bài chia sẻ về Event-Driven Architecture (EDA) trên cộng đồng AWS Study Group. Nội dung bài viết giới thiệu nguyên lý hoạt động của kiến trúc hướng sự kiện và vai trò của mô hình này trong việc xây dựng các ứng dụng hiện đại có khả năng tự động hóa, mở rộng và phản hồi theo thời gian thực.

Các nội dung chính của bài viết gồm:

* Giới thiệu khái niệm Event-Driven Architecture (EDA) và nguyên lý hoạt động của kiến trúc hướng sự kiện.
* Phân tích cơ chế phát sinh, truyền và xử lý sự kiện giữa các dịch vụ AWS thông qua Amazon EventBridge.
* Trình bày các lợi ích của Event-Driven Architecture như giảm sự phụ thuộc giữa các thành phần (Loose Coupling), tăng khả năng mở rộng (Scalability), xử lý gần như theo thời gian thực và tối ưu chi phí khi triển khai mô hình Serverless.
* Giới thiệu một số Best Practices khi thiết kế hệ thống Event-Driven, bao gồm xây dựng các dịch vụ độc lập, thiết kế AWS Lambda theo nguyên tắc Idempotent, sử dụng Dead Letter Queue (DLQ) để xử lý lỗi và theo dõi hệ thống bằng Amazon CloudWatch.
* Minh họa kiến trúc hướng sự kiện thông qua sơ đồ kết hợp các dịch vụ Amazon S3, Amazon EventBridge, AWS Lambda, Amazon SNS và Amazon SQS.

Bài viết giúp em củng cố kiến thức về Event-Driven Architecture, đồng thời hiểu rõ hơn cách các dịch vụ trong hệ sinh thái AWS phối hợp với nhau để xây dựng các hệ thống Serverless có khả năng mở rộng, tự động hóa và dễ bảo trì.

## Hình minh họa

![overview](/images/3-BlogsPosted/blog_2.png)
## Link bài viết

[Xem bài viết trên AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2234610517303908&hoisted_section_header_type=recently_seen&__cft__[0]=AZbyhAyUCRN5R-so0NeOjpWZ6P1Gq8ep_R31jElXlmFrUF1zkq8VjubmawbC2b3ysSHBCTVgzddnmyNLL0vE4WY0SfFNP_SXVKj00pJeUBhYksXEE4SzzRESwWhReqqlhWJX9OA9jXDoP3IJPMw97Ut-tKIqCEHL-MNOzNqmEfNae2cZRlZtsy6PpRjq8fpc8PA&__tn__=%2CO%2CP-R)