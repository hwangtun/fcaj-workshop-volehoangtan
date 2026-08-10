---
title: "Agent Force - Deepdive Day 2"
date: 2026-08-08
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “Agent Force - Deepdive Day 2 - Personalization, Evaluation & Optimization”

## Mục Đích Của Sự Kiện

-Tìm hiểu các tính năng nâng cao của Amazon Bedrock AgentCore.
-Hiểu cách xây dựng AI Agent có khả năng ghi nhớ ngữ cảnh và cá nhân hóa trải nghiệm người dùng.
-Nắm được các công cụ đánh giá, giám sát và tối ưu hóa AI Agent trong quá trình vận hành.
-Thực hành sử dụng các thành phần của AgentCore để xây dựng và đánh giá một AI Agent.

## Danh Sách Diễn Giả

- **Giuseppe Marazzotta** - Head of Technology, AWS
- Các team đã tham gia AABW.

## Nội Dung Nổi Bật

### Amazon Bedrock AgentCore L300
Buổi học tập trung vào các tính năng nâng cao của AgentCore nhằm giúp AI Agent hoạt động hiệu quả hơn trong môi trường thực tế.
Memory cho phép AI Agent lưu trữ và sử dụng thông tin từ các cuộc hội thoại trước để tạo ra trải nghiệm mang tính cá nhân hóa.

Thông qua Memory, Agent có thể:

- Ghi nhớ sở thích và thông tin của người dùng.
- Duy trì ngữ cảnh trong các cuộc hội thoại dài.
- Đưa ra phản hồi phù hợp hơn dựa trên lịch sử tương tác.
- Giảm việc người dùng phải lặp lại cùng một thông tin nhiều lần.

Tính năng này đặc biệt hữu ích đối với các chatbot chăm sóc khách hàng, trợ lý cá nhân hoặc hệ thống hỗ trợ nội bộ doanh nghiệp.

### Evaluations

AgentCore Evaluations hỗ trợ đánh giá chất lượng hoạt động của AI Agent thông qua nhiều tiêu chí khác nhau.

Một số nội dung được giới thiệu gồm:

- Đánh giá độ chính xác của câu trả lời.
- Kiểm tra mức độ hoàn thành yêu cầu của người dùng.
- So sánh kết quả giữa nhiều phiên bản Prompt hoặc Agent.
- Hỗ trợ xác định những điểm cần cải thiện trước khi triển khai vào môi trường Production.

Nhờ đó, quá trình phát triển AI Agent trở nên có cơ sở dữ liệu thay vì chỉ đánh giá bằng cảm nhận.

### Observability

Observability giúp theo dõi toàn bộ quá trình xử lý của AI Agent.

Các thông tin có thể quan sát bao gồm:

- Luồng thực hiện của Agent.
- Thời gian xử lý từng bước.
- Các Tool được Agent gọi.
- Token sử dụng.
- Lỗi phát sinh trong quá trình thực thi.

Thông tin này giúp việc phân tích, gỡ lỗi và tối ưu hiệu năng của Agent trở nên dễ dàng hơn.

---

# AgentCore Components

Ngoài các tính năng chính, buổi học còn giới thiệu các thành phần quan trọng trong AgentCore.

## Registry

Registry đóng vai trò quản lý các Agent, Tool và tài nguyên được sử dụng trong hệ thống.

Nhờ Registry, việc tái sử dụng các thành phần và quản lý phiên bản của Agent trở nên thuận tiện hơn.

## Harness

Harness là môi trường dùng để kiểm thử AI Agent trước khi đưa vào triển khai thực tế.

Thông qua Harness có thể:

- Chạy nhiều bộ dữ liệu kiểm thử.
- So sánh kết quả giữa các phiên bản Agent.
- Đánh giá chất lượng sau khi thay đổi Prompt hoặc cấu hình.

Điều này giúp giảm rủi ro khi triển khai hệ thống AI vào môi trường Production.

## Tools

AgentCore hỗ trợ tích hợp nhiều Tool để mở rộng khả năng của AI Agent.

Ví dụ:

- Truy vấn cơ sở dữ liệu.
- Gọi API bên ngoài.
- Thực hiện các phép tính.
- Đọc tài liệu hoặc dữ liệu doanh nghiệp.

Nhờ đó Agent không chỉ tạo văn bản mà còn có thể thực hiện các tác vụ thực tế.

## Payments

Payments hỗ trợ quản lý việc sử dụng tài nguyên và theo dõi chi phí khi Agent gọi các dịch vụ AI hoặc Tool bên ngoài.

Việc theo dõi chi phí giúp doanh nghiệp kiểm soát ngân sách và tối ưu quá trình vận hành.

## Optimization

Optimization cung cấp các cơ chế tối ưu hiệu suất AI Agent.

Bao gồm:

- Giảm độ trễ phản hồi.
- Tối ưu Prompt.
- Giảm chi phí sử dụng mô hình.
- Cải thiện chất lượng câu trả lời.

## Policy

Policy giúp thiết lập các quy tắc quản trị AI Agent.

Ví dụ:

- Kiểm soát quyền truy cập.
- Giới hạn hành vi của Agent.
- Thiết lập các quy định về bảo mật.
- Đảm bảo Agent tuân thủ chính sách của doanh nghiệp.

---

# Hands-on Lab

Trong phần thực hành, tôi đã được trải nghiệm các tính năng của AgentCore.

## Thêm Memory cho AI Agent

Thực hiện cấu hình Memory để Agent có khả năng ghi nhớ thông tin giữa các phiên làm việc.

Qua đó có thể quan sát sự khác biệt giữa Agent có Memory và Agent không sử dụng Memory.

## Khám phá Agent Observability

Theo dõi quá trình xử lý của Agent thông qua công cụ Observability.

Có thể xem:

- Các bước Agent thực hiện.
- Thời gian xử lý.
- Tool được sử dụng.
- Thông tin phục vụ việc phân tích và gỡ lỗi.

## Đánh giá Agent bằng AgentCore Evaluations

Sử dụng Evaluations để đánh giá chất lượng phản hồi của AI Agent.

Việc đánh giá giúp:

- Xác định các trường hợp Agent trả lời chưa chính xác.
- So sánh nhiều phiên bản Prompt.
- Đo lường mức độ cải thiện sau khi tối ưu.

## Thực hành với AgentCore Harness

Sử dụng Harness để chạy các bộ kiểm thử và đánh giá hiệu quả hoạt động của Agent trước khi triển khai.

Thông qua đó hiểu rõ hơn quy trình kiểm thử AI Agent trong môi trường phát triển.

---

# Những Gì Học Được

## Về Amazon Bedrock AgentCore

- Hiểu cách sử dụng Memory để xây dựng AI Agent có khả năng cá nhân hóa trải nghiệm người dùng.
- Biết cách theo dõi hoạt động của Agent thông qua Observability.
- Hiểu quy trình đánh giá AI Agent bằng AgentCore Evaluations.
- Nắm được vai trò của Harness trong quá trình kiểm thử và cải tiến Agent.

## Về phát triển AI Agent

- AI Agent không chỉ cần tạo câu trả lời chính xác mà còn phải được đánh giá và theo dõi trong suốt vòng đời phát triển.
- Memory giúp nâng cao trải nghiệm người dùng bằng cách duy trì ngữ cảnh hội thoại.
- Việc kiểm thử và đánh giá thường xuyên giúp cải thiện chất lượng và độ tin cậy của hệ thống.

## Về triển khai thực tế

- Một AI Agent hoàn chỉnh cần có khả năng ghi nhớ, quan sát, đánh giá và tối ưu liên tục.
- Các thành phần như Registry, Harness và Policy giúp hệ thống dễ quản lý, mở rộng và vận hành an toàn hơn.
- Việc theo dõi chi phí và hiệu năng là yếu tố quan trọng khi triển khai AI ở quy mô doanh nghiệp.

---

# Ứng Dụng Vào Công Việc

- Áp dụng Memory để xây dựng chatbot hoặc trợ lý AI có khả năng ghi nhớ thông tin người dùng.
- Sử dụng Evaluations để đánh giá chất lượng phản hồi trước khi triển khai hệ thống.
- Theo dõi hoạt động của Agent bằng Observability nhằm phát hiện và xử lý lỗi nhanh hơn.
- Tận dụng Harness để kiểm thử và cải thiện AI Agent trước khi đưa vào môi trường Production.
- Thiết lập Policy nhằm đảm bảo Agent hoạt động an toàn và tuân thủ các yêu cầu của doanh nghiệp.

---

# Trải nghiệm trong buổi học

Buổi học về **Advanced Amazon Bedrock AgentCore** giúp tôi hiểu rõ hơn những thành phần quan trọng cần có để xây dựng một AI Agent có thể vận hành trong môi trường thực tế, không chỉ dừng lại ở việc tạo câu trả lời.

## Học hỏi về quy trình phát triển AI Agent

- Tôi hiểu rõ vai trò của **Memory** trong việc cá nhân hóa trải nghiệm người dùng và duy trì ngữ cảnh giữa các cuộc hội thoại.
- Tôi biết cách sử dụng **Observability** để theo dõi toàn bộ quá trình hoạt động của Agent và hỗ trợ phân tích, gỡ lỗi.
- Tôi nhận thấy **Evaluations** và **Harness** là những công cụ quan trọng để đánh giá, kiểm thử và tối ưu chất lượng AI Agent trước khi triển khai.

## Hiểu thêm về vận hành AI trong doanh nghiệp

- Buổi học giúp tôi nhận thức rằng việc xây dựng AI Agent không chỉ tập trung vào mô hình ngôn ngữ mà còn cần chú trọng đến khả năng quan sát, đánh giá, tối ưu hiệu năng và quản trị hệ thống.
- Các thành phần như **Registry**, **Policy** và **Optimization** góp phần giúp AI Agent dễ quản lý, an toàn và có khả năng mở rộng trong môi trường doanh nghiệp.

## Bài học rút ra

- Một AI Agent chất lượng cần được đánh giá và cải tiến liên tục trong suốt vòng đời phát triển.
- Việc kết hợp Memory, Observability và Evaluations giúp nâng cao trải nghiệm người dùng cũng như tăng độ tin cậy của hệ thống.
- Kiểm thử và giám sát là những bước không thể thiếu để triển khai AI Agent hiệu quả trong thực tế.

## Một số hình ảnh trong buổi học

![Hình ảnh tham gia sự kiện Agent Force Deepdive day 2](/images/event-img/AFDD2_1.jpg)
![Hình ảnh tham gia sự kiện Agent Force Deepdive day 2](/images/event-img/AFDD2_2.jpg)
![Hình ảnh tham gia sự kiện Agent Force Deepdive day 2](/images/event-img/AFDD2_3.jpg)

> Tổng thể, buổi học giúp tôi hiểu sâu hơn về các khả năng nâng cao của Amazon Bedrock AgentCore, từ việc xây dựng AI Agent có khả năng ghi nhớ và cá nhân hóa, đến đánh giá, giám sát và tối ưu hóa hiệu suất. Những kiến thức và trải nghiệm thực hành này là nền tảng quan trọng để phát triển các hệ thống Agentic AI đáng tin cậy và sẵn sàng triển khai trong môi trường doanh nghiệp.



