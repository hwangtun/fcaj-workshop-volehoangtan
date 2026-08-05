---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# AWS CDK – Infrastructure as Code cho hạ tầng đám mây hiện đại

Trong quá trình thực tập và tìm hiểu về Infrastructure as Code (IaC) trên AWS, em đã thực hiện một bài chia sẻ về AWS Cloud Development Kit (AWS CDK) trên cộng đồng AWS Study Group. Nội dung bài viết giới thiệu AWS CDK như một công cụ giúp xây dựng, quản lý và triển khai hạ tầng đám mây bằng mã nguồn, góp phần nâng cao khả năng tự động hóa và quản lý hạ tầng trong các dự án AWS.

Các nội dung chính của bài viết gồm:

* Giới thiệu khái niệm Infrastructure as Code (IaC) và vai trò của AWS CDK trong việc quản lý hạ tầng trên AWS.
* Giải thích quy trình triển khai hạ tầng bằng AWS CDK thông qua các lệnh cdk synth, cdk diff và cdk deploy, đồng thời làm rõ mối quan hệ giữa AWS CDK và AWS CloudFormation.
* Phân tích những ưu điểm của AWS CDK như quản lý hạ tầng bằng mã nguồn, hỗ trợ kiểm soát phiên bản với Git, dễ dàng tái tạo môi trường triển khai và tích hợp với các quy trình CI/CD.
* Trình bày một số Best Practices khi sử dụng AWS CDK, bao gồm hạn chế thay đổi trực tiếp trên AWS Management Console để tránh Infrastructure Drift, sử dụng cdk diff trước khi triển khai và tổ chức hạ tầng thành nhiều Stack hoặc Construct nhằm tăng khả năng bảo trì và tái sử dụng.
* Minh họa quy trình triển khai hạ tầng bằng AWS CDK từ mã nguồn đến AWS CloudFormation và các tài nguyên AWS.

Bài viết giúp em hiểu rõ hơn về phương pháp triển khai hạ tầng theo mô hình Infrastructure as Code, đồng thời nâng cao kiến thức về AWS CDK và quy trình tự động hóa trong việc quản lý tài nguyên trên nền tảng AWS.


## Link bài viết

[Xem bài viết trên AWS Study Group](https://www.facebook.com/groups/660548818043427/?multi_permalinks=2234080517356908&hoisted_section_header_type=recently_seen&__cft__[0]=AZb29XuCsS2ASBvBcZMVpC3a3WWPu8ZhDL59LYPyV088Y1iiE9eqRqLg-cts8T4BBiyrYnDCgNw2OVnDT3Ym1IWIxLF6hwBlhSLZ33VUJtRWhyXWj0qZI4UO0erzPDquhlcFOI1MRZ0useuMx_1O2dP7u_HGzt6YWlHoFVd2MWol0g&__tn__=%2CO%2CP-R)
