---
title: "Worklog Tuần 5"
date: 2026-07-13
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

### Mục tiêu tuần 5

* Hoàn thiện cấu trúc giao diện chính của hệ thống.
* Xây dựng Dashboard và Sidebar.
* Hoàn thiện hệ thống điều hướng (Routing).
* Chuẩn bị kết nối giữa Frontend và Backend J2EE.

### Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Thiết kế MainLayout dùng chung cho toàn bộ hệ thống.<br>- Xây dựng Sidebar và Navigation Menu.<br>- Hoàn thiện Footer và bố cục giao diện. | 13/07/2026 | 13/07/2026 | https://reactrouter.com |
| 3 | - Cấu hình React Router.<br>- Phân chia các Layout cho Landing Page, Authentication và Dashboard.<br>- Thiết lập Protected Route cho các trang yêu cầu đăng nhập. | 14/07/2026 | 14/07/2026 | React Router Documentation |
| 4 | - Xây dựng giao diện Dashboard.<br>- Thiết kế các thẻ thống kê (Statistics Cards).<br>- Xây dựng Recent Items và Recent Processing để hiển thị lịch sử xử lý ảnh. | 15/07/2026 | 16/07/2026 | React Documentation |
| 5 | - Hoàn thiện giao diện Sidebar.<br>- Điều chỉnh CSS, xử lý lỗi chồng lấn giữa Sidebar và nội dung chính.<br>- Đồng bộ giao diện giữa các trang. | 17/07/2026 | 17/07/2026 | CSS Flexbox Guide |
| 6 | - Chuẩn bị tích hợp Backend J2EE.<br>- Xây dựng cấu trúc Service để giao tiếp với API.<br>- Thiết kế các kiểu dữ liệu (TypeScript Interfaces) phục vụ việc nhận dữ liệu từ Backend. | 18/07/2026 | 19/07/2026 | Spring Boot Documentation |

### Kết quả đạt được tuần 5

* Hoàn thiện MainLayout và Sidebar cho toàn bộ hệ thống.
* Xây dựng thành công hệ thống điều hướng bằng React Router.
* Hoàn thiện Dashboard với các thành phần:
  * Statistics Cards.
  * Recent Items.
  * Recent Processing.
* Áp dụng Protected Route nhằm giới hạn quyền truy cập vào các trang yêu cầu xác thực.
* Khắc phục các lỗi về bố cục và CSS giữa Sidebar và khu vực hiển thị nội dung.
* Chuẩn bị cấu trúc Frontend để tích hợp với Backend J2EE thông qua các Service và Interface.