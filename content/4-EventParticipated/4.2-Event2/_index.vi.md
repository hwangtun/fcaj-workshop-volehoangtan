---
title: "Agent Force - Deepdive Day 2"
date: 2026-08-08
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

# Bài thu hoạch “Agent Force - Deepdive Day 2 - Personalization, Evaluation & Optimization”

## Mục Đích Của Sự Kiện

- Tìm hiểu các tính năng nâng cao của **Amazon Bedrock AgentCore**.
- Xây dựng AI Agent có khả năng ghi nhớ ngữ cảnh và cá nhân hóa trải nghiệm người dùng.
- Nắm vững các công cụ đánh giá, giám sát và tối ưu hóa AI Agent trong môi trường thực tế.
- Thực hành sử dụng các thành phần cốt lõi của **AgentCore** để phát triển và triển khai AI Agent.

## Danh Sách Diễn Giả

- **Nghia Tran** – Agentic SA
- **Anh Pham** – Cloud Consultant, G-Asia Pacific Vietnam

---

# Nội Dung Nổi Bật

## Amazon Bedrock AgentCore L300

Buổi học tập trung vào những khả năng quan trọng giúp đưa AI Agent từ giai đoạn phát triển sang môi trường Production.

### Memory

Memory giúp AI Agent lưu trữ ngữ cảnh của các cuộc hội thoại, bao gồm bộ nhớ ngắn hạn và dài hạn, từ đó tạo ra trải nghiệm mang tính cá nhân hóa.

Thông qua Memory, Agent có thể:

- Ghi nhớ thông tin và sở thích của người dùng.
- Duy trì ngữ cảnh giữa nhiều cuộc hội thoại.
- Hạn chế việc người dùng phải cung cấp lại cùng một thông tin.
- Đưa ra phản hồi phù hợp hơn dựa trên lịch sử tương tác.

Đây là thành phần quan trọng đối với các chatbot chăm sóc khách hàng, trợ lý ảo và các hệ thống AI trong doanh nghiệp.

### Evaluations

AgentCore Evaluations cung cấp cơ chế đánh giá chất lượng hoạt động của AI Agent dựa trên nhiều tiêu chí khác nhau.

Một số khả năng nổi bật gồm:

- Đánh giá độ chính xác của câu trả lời.
- Đánh giá mức độ hữu ích (Helpfulness).
- So sánh nhiều phiên bản Prompt hoặc Agent.
- Sử dụng Judge Model để tự động chấm điểm kết quả.

Nhờ đó, việc cải tiến AI Agent được thực hiện dựa trên dữ liệu thay vì chỉ dựa vào đánh giá chủ quan.

### Observability

Observability giúp theo dõi toàn bộ quá trình hoạt động của AI Agent.

Các thông tin có thể quan sát gồm:

- Log của Agent.
- Trace quá trình xử lý.
- Metrics về hiệu năng.
- Luồng thực thi của Agent.
- Các lỗi phát sinh trong quá trình xử lý.

Điều này giúp việc phân tích, gỡ lỗi và tối ưu hiệu năng của hệ thống trở nên dễ dàng hơn.

---

# AgentCore Components

Ngoài ba tính năng chính, buổi học còn giới thiệu nhiều thành phần quan trọng của AgentCore.

## Registry

Registry là nơi quản lý tập trung các AI Agent, Tool và Skill.

Nhờ Registry, các thành phần có thể được tái sử dụng giữa nhiều dự án, giúp giảm thời gian phát triển và dễ dàng quản lý phiên bản.

## Harness

Harness là môi trường hỗ trợ việc xây dựng và kiểm thử AI Agent.

Thông qua Harness, nhà phát triển có thể:

- Thiết lập nhanh môi trường thử nghiệm.
- Chạy các bộ kiểm thử.
- So sánh kết quả giữa nhiều phiên bản Agent.

Điều này giúp giảm rủi ro trước khi triển khai vào môi trường Production.

## Tools

AgentCore hỗ trợ tích hợp nhiều công cụ mở rộng để tăng khả năng của AI Agent.

Ví dụ:

- Browser Tool.
- Code Interpreter.
- Payment Tool.
- Các API và dịch vụ bên ngoài.

Nhờ đó Agent không chỉ tạo văn bản mà còn có thể thực hiện các tác vụ thực tế.

## Policy

Policy cho phép thiết lập các quy tắc quản trị bằng ngôn ngữ **Seda**.

Các Policy giúp:

- Kiểm soát quyền truy cập.
- Giới hạn hành vi của Agent.
- Đảm bảo yêu cầu về bảo mật.
- Tuân thủ quy định của doanh nghiệp.

## Optimization

Optimization tập trung vào việc cải thiện hiệu năng của AI Agent.

Bao gồm:

- Giảm độ trễ phản hồi.
- Tối ưu Prompt.
- Giảm chi phí sử dụng mô hình.
- Nâng cao chất lượng phản hồi.

---

# Hands-on Lab

Trong phần thực hành, người tham gia được trải nghiệm các tính năng quan trọng của AgentCore.

## Cấu hình Memory

Thực hiện tích hợp Memory để AI Agent có thể ghi nhớ thông tin giữa nhiều phiên làm việc.

Qua đó có thể quan sát sự khác biệt giữa Agent có Memory và Agent không sử dụng Memory.

## Quan sát Agent

Sử dụng công cụ Observability để theo dõi quá trình **Invoke** của Agent.

Có thể quan sát:

- Luồng xử lý.
- Các bước Reasoning.
- Log và Trace.
- Thời gian thực thi.

## Kiểm thử AI Agent

Sử dụng các Template có sẵn để xây dựng Agent xử lý các tình huống như hoàn tiền (Refund) hoặc trả hàng (Return).

Thông qua bài thực hành này, người học hiểu rõ hơn quy trình phát triển và kiểm thử AI Agent trong thực tế.

---

# Những Gì Học Được

## Về Amazon Bedrock AgentCore

- Hiểu cách AgentCore trừu tượng hóa (abstract) nhiều quy trình phức tạp thành các công cụ dễ sử dụng.
- Nắm được vai trò của Memory, Evaluations và Observability trong toàn bộ vòng đời của AI Agent.

## Về phát triển AI Agent

- Nhận thấy rằng phần lớn công việc xây dựng AI Agent thực tế nằm ở Software Engineering.
- Memory, Tool Integration, Observability và Evaluation quan trọng không kém bản thân mô hình AI.
- AI Model chỉ là một thành phần trong toàn bộ hệ thống Agentic AI.

## Về triển khai thực tế

- Hiểu được tầm quan trọng của việc kiểm soát chi phí và tối ưu hiệu năng.
- Nhận thức được vai trò của Policy trong việc đảm bảo bảo mật và tuân thủ.
- Biết cách xây dựng AI Agent có khả năng vận hành ổn định trong môi trường doanh nghiệp.

---

# Ứng Dụng Vào Công Việc

- Áp dụng quy trình **Design → Evaluate → Deploy** trong các dự án AI.
- Sử dụng Observability để theo dõi hiệu năng và tối ưu chi phí token.
- Xây dựng thư viện Agent và Tool dùng chung thông qua Registry.
- Thiết kế AI Agent có khả năng ghi nhớ ngữ cảnh nhằm nâng cao trải nghiệm người dùng.
- Áp dụng các Policy để đảm bảo hệ thống AI tuân thủ yêu cầu bảo mật của doanh nghiệp.

---

# Trải nghiệm trong buổi học

Buổi học giúp tôi thay đổi cách nhìn về việc phát triển AI Agent.

Trước đây, tôi thường tập trung vào việc xây dựng một chatbot hoặc một Agent có thể trả lời câu hỏi. Tuy nhiên, sau buổi học, tôi nhận thấy rằng việc phát triển một AI Agent trong môi trường doanh nghiệp không chỉ dừng lại ở khả năng sinh nội dung mà còn bao gồm rất nhiều yếu tố khác như quản lý bộ nhớ, giám sát hệ thống, đánh giá chất lượng, tối ưu hiệu năng và đảm bảo tính bảo mật.

## Học hỏi từ buổi đào tạo

- Hiểu rõ vai trò của Memory trong việc cá nhân hóa trải nghiệm người dùng.
- Biết cách sử dụng Observability để theo dõi toàn bộ quá trình hoạt động của Agent.
- Hiểu quy trình đánh giá AI Agent thông qua AgentCore Evaluations.
- Làm quen với các thành phần như Registry, Harness, Policy và Optimization.

## Bài học rút ra

- Xây dựng AI Agent là sự kết hợp giữa AI và Software Engineering.
- Khả năng giám sát, đánh giá và tối ưu hóa quan trọng không kém việc lựa chọn mô hình AI.
- Một AI Agent chất lượng cần được thiết kế để có thể mở rộng, bảo trì và vận hành lâu dài trong môi trường doanh nghiệp.


## Một số hình ảnh trong buổi học

![Hình ảnh tham gia sự kiện Agent Force Deepdive day 2](/images/event-img/AFDD2_1.jpg)
![Hình ảnh tham gia sự kiện Agent Force Deepdive day 2](/images/event-img/AFDD2_2.jpg)
![Hình ảnh tham gia sự kiện Agent Force Deepdive day 2](/images/event-img/AFDD2_3.jpg)

> Tổng thể, buổi học giúp tôi hiểu sâu hơn về các khả năng nâng cao của Amazon Bedrock AgentCore, từ việc xây dựng AI Agent có khả năng ghi nhớ và cá nhân hóa, đến đánh giá, giám sát và tối ưu hóa hiệu suất. Những kiến thức và trải nghiệm thực hành này là nền tảng quan trọng để phát triển các hệ thống Agentic AI đáng tin cậy và sẵn sàng triển khai trong môi trường doanh nghiệp.



