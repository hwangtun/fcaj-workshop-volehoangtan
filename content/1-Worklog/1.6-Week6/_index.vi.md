---
title: "Worklog Tuần 6"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6

* Tích hợp Frontend với Backend J2EE.
* Xây dựng các Service để giao tiếp với REST API.
* Hoàn thiện chức năng Upload ảnh từ Frontend.
* Triển khai chức năng xác thực người dùng bằng Google OAuth.

### Các công việc cần triển khai trong tuần này

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Phân tích các REST API do Backend cung cấp.<br>- Thiết lập Axios Instance dùng chung cho toàn bộ dự án.<br>- Xây dựng cấu trúc Service cho từng nhóm chức năng. | 20/07/2026 | 20/07/2026 | Axios Documentation <br> Spring Boot REST API |
| 3 | - Tích hợp API Upload Image.<br>- Sử dụng FormData để gửi nhiều ảnh đến Backend.<br>- Xử lý dữ liệu trả về sau khi tải ảnh thành công. | 21/07/2026 | 21/07/2026 | MDN FormData |
| 4 | - Tích hợp các API lấy thông tin Batch và danh sách ảnh.<br>- Hiển thị dữ liệu trên Dashboard và History.<br>- Xử lý trạng thái Loading và thông báo lỗi khi gọi API. | 22/07/2026 | 23/07/2026 | Spring Boot Documentation |
| 5 | - Tích hợp chức năng đăng nhập bằng Google OAuth.<br>- Cấu hình Google OAuth Client.<br>- Lưu JWT Token và thông tin người dùng sau khi đăng nhập thành công. | 24/07/2026 | 24/07/2026 | Google Identity Documentation |
| 6 | - Hoàn thiện AuthContext và cơ chế xác thực người dùng.<br>- Kiểm thử toàn bộ luồng đăng nhập và Upload ảnh.<br>- Khắc phục các lỗi phát sinh trong quá trình tích hợp Backend. | 25/07/2026 | 26/07/2026 | React Context API Documentation |

### Kết quả đạt được tuần 6

* Hoàn thành việc tích hợp Frontend với Backend J2EE thông qua REST API.
* Xây dựng thành công lớp Service sử dụng Axios để giao tiếp với Backend.
* Hoàn thiện chức năng Upload nhiều ảnh bằng FormData.
* Hiển thị dữ liệu Batch và lịch sử xử lý ảnh từ Backend lên giao diện.
* Tích hợp thành công chức năng đăng nhập bằng Google OAuth.
* Xây dựng AuthContext để quản lý JWT Token và thông tin người dùng.
* Hoàn thiện quy trình xác thực người dùng và chuẩn bị cho giai đoạn kiểm thử hệ thống.