---
title: "Worklog Tuần 6"
date: 2026-07-27
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6

* Tích hợp Frontend với Backend thông qua REST API.
* Hoàn thiện chức năng xác thực người dùng bằng Google OAuth.
* Xây dựng cơ chế quản lý phiên đăng nhập.
* Hoàn thiện chức năng tải ảnh lên hệ thống.

### Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Phân tích các REST API do Backend cung cấp.<br>- Xây dựng các Service để giao tiếp với Backend bằng Axios.<br>- Chuẩn hóa cấu trúc gọi API trong toàn bộ dự án. | 27/07/2026 | 27/07/2026 | Spring Boot Documentation <br> Axios Documentation |
| 3 | - Tích hợp chức năng đăng nhập bằng Google OAuth.<br>- Cấu hình Google Identity Services.<br>- Nhận ID Token và gửi đến Backend để xác thực người dùng. | 28/07/2026 | 28/07/2026 | Google Identity Documentation |
| 4 | - Xây dựng AuthContext để quản lý trạng thái đăng nhập.<br>- Lưu JWT Token và thông tin người dùng sau khi đăng nhập thành công.<br>- Thiết lập Protected Route cho các trang yêu cầu xác thực. | 29/07/2026 | 30/07/2026 | React Context API |
| 5 | - Tích hợp chức năng Upload Image.<br>- Gửi nhiều ảnh bằng FormData.<br>- Truyền thông tin người dùng vào yêu cầu tải ảnh.<br>- Kiểm tra dữ liệu phản hồi từ Backend. | 31/07/2026 | 01/08/2026 | MDN FormData <br> Spring Boot Documentation |
| 6 | - Kiểm thử toàn bộ quy trình đăng nhập và tải ảnh.<br>- Khắc phục các lỗi phát sinh trong quá trình tích hợp Frontend và Backend.<br>- Hoàn thiện luồng xử lý và xác thực người dùng. | 02/08/2026 | 02/08/2026 | Tài liệu dự án |

### Kết quả đạt được tuần 6

* Hoàn thành việc tích hợp Frontend với Backend thông qua REST API.
* Xây dựng thành công các Service sử dụng Axios để giao tiếp với Backend.
* Tích hợp thành công chức năng đăng nhập bằng Google OAuth.
* Hoàn thiện AuthContext để quản lý JWT Token và thông tin người dùng.
* Áp dụng Protected Route nhằm giới hạn quyền truy cập vào các trang yêu cầu đăng nhập.
* Hoàn thiện chức năng tải nhiều ảnh bằng FormData.
* Kiểm thử thành công quy trình xác thực người dùng và tải ảnh lên hệ thống.
* Khắc phục các lỗi phát sinh trong quá trình tích hợp giữa Frontend và Backend.