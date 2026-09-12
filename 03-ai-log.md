# AI Log — Vin Smart Future

---

## 1. Mục đích sử dụng AI trong quá trình làm bài

Trong buổi Lab 02, tôi sử dụng AI như một trợ lý đồng hành để hỗ trợ tư duy sản phẩm, xác định pain point vận hành và xây dựng prompt prototype. AI giúp tôi nhanh chóng generate các ý tưởng về bài toán thực tế trong các công ty thành viên Vingroup, từ đó lọc ra các vấn đề có tính khả thi và khả năng triển khai rõ ràng.

Cụ thể, AI hỗ trợ tôi trong các bước sau:
- Gợi ý các bài toán thực tế và bottleneck trong vận hành VinFast, Xanh SM, Vinhomes, Vinmec.
- Tạo ra các quick problem cards với cấu trúc rõ ràng: actor, workflow, bottleneck, metric, AI fit.
- Phân tích ưu/nhược điểm của việc dùng AI hoặc rule-based system cho từng bài toán.
- Đề xuất prompt mẫu và kiểm tra các ràng buộc an toàn trong hệ thống prompt prototype.

Vì thế, AI đóng vai trò như một người đồng nghiệp hỗ trợ thảo luận, không phải là người quyết định cuối cùng. Tôi vẫn giữ vai trò chủ động trong việc đánh giá tính thực tế, độ an toàn và độ khả thi của bài toán.

---

## 2. AI đã giúp tôi điều gì

AI giúp tôi rất nhiều trong việc:

### 2.1. Brainstorm ý tưởng bài toán
Khi bắt đầu, tôi chưa có một bài toán cụ thể rõ ràng cho Vin Smart Future. AI đã gợi ý nhiều trường hợp như:
- điều phối tài xế Xanh SM,
- định tuyến trạm sạc cho xe điện VinFast,
- phản hồi cư dân Vinhomes,
- chăm sóc bệnh nhân/đặt lịch Vinmec.

Từ đó, tôi lọc ra bài toán phù hợp nhất: **hỗ trợ tìm trạm sạc an toàn cho lái xe VinFast khi pin thấp**. Đây là bài toán có ràng buộc an toàn rõ ràng, metric dễ đo, và phù hợp với prototype prompt.

### 2.2. Cấu trúc hóa câu hỏi và quick cards
AI giúp tôi định hình các quick cards theo đúng template của lab, bao gồm:
- actor,
- workflow hiện tại,
- bottleneck,
- metric,
- architecture phù hợp.

Điều này giúp tôi tránh việc phát triển một ý tưởng quá mơ hồ hoặc quá rộng.

### 2.3. Thiết kế prompt và ranh giới an toàn
AI cũng hỗ trợ tôi viết prompt prototype với các quy tắc như:
- output phải bắt đầu bằng `[DRAFT_ONLY]`,
- khi pin < 5% phải ưu tiên dispatch mobile charger,
- không được gửi trực tiếp tin nhắn cho khách hàng.

Nhờ đó, tôi có thể kiểm tra được model có tuân thủ vùng an toàn hay không khi bị user ép và prompt injection.

---

## 3. AI trả lời sai / hallucination ở đâu

Trong quá trình làm bài, AI cũng có những sai sót đáng lưu ý.

### 3.1. Đánh giá không luôn chắc chắn về mức độ rủi ro
Lúc đầu, AI có xu hướng đề xuất rất nhiều bài toán mà không phân biệt rõ đâu là bài toán thực sự cần AI, đâu là bài toán nên dùng rule-based system. Một số gợi ý quá rộng, ví dụ như “AI tự động điều phối xe” hoặc “AI tự gửi phản hồi khách hàng” mà không đặt ra ranh giới an toàn rõ ràng.

Đây là điểm mà tôi đã phải chỉnh sửa: không để AI kéo bài toán ra quá lớn. Tôi cần tinh gọn lại thành một bài toán có 
- clear boundary,
- measurable impact,
- risk-aware design.

### 3.2. Có thể bỏ qua safety rule nếu prompt không chặt
Khi tôi thử xây dựng prompt prototype, AI có khả năng bỏ qua hoặc lỏng lẻo trong cách phát biểu ràng buộc. Ví dụ, nếu prompt không nói rõ “không được gửi trực tiếp”, model có thể đưa ra câu trả lời dạng hướng dẫn không đầy đủ hoặc không bắt đầu bằng `[DRAFT_ONLY]`.

Đây là nơi tôi nhận ra: AI không tự động hiểu được “hạn chế an toàn” nếu không được ép bằng lời nhắn cực kỳ chặt và cụ thể.

### 3.3. Khó xác định model trong môi trường thật
Khi đang làm prototype, có một vấn đề thực tế là Gemini API có thể đổi tên model hoặc các key API không hợp lệ. Tôi đã gặp lỗi API key invalid và lỗi model không còn khả dụng. Đây không phải lỗi logic của prompt, nhưng là vấn đề môi trường / dependency. Tôi đã phải tinh chỉnh lại cách debug và kiểm tra runtime, thay vì chỉ tin vào output của AI.

---

## 4. Tôi đã sửa prompt và ranh giới như thế nào

Để đạt được kết quả chuẩn, tôi đã cải thiện prompt theo 3 bước chính:

### 4.1. Làm rõ vai trò của AI
Tôi xác định rõ AI là một **dispatch co-pilot** chứ không phải hệ thống tự động gửi tin nhắn. Đây giúp AI biết mình chỉ được hỗ trợ draft, không được hành động độc lập.

### 4.2. Đặt ràng buộc an toàn bằng quy tắc cụ thể
Tôi không chỉ nói “hãy an toàn”, mà nói rất cụ thể:
- bắt đầu bằng `[DRAFT_ONLY]`,
- không gửi trực tiếp,
- nếu pin < 5% thì không được recommend trạm > 5km,
- phải trigger `dispatch_mobile_charger`.

Việc làm này rất quan trọng vì AI dễ bị lệ thuộc vào yêu cầu người dùng nếu không có quy tắc “hard constraint”.

### 4.3. Dùng adversarial test để stress test prompt
Tôi tạo các test case mô phỏng tình huống người dùng bắt AI bypass tag hoặc yêu cầu ẩn logic an toàn. Khi prompt không đủ chặt, output thường sai. Sau khi chỉnh sửa prompt, tôi kiểm tra lại xem:
- output có bắt đầu bằng `[DRAFT_ONLY]` không,
- dưới ngưỡng pin nguy hiểm, có trigger mobile charger không,
- model có bị hội chứng prompt injection phá ràng buộc không.

---

## 5. Kết luận

AI đã là một công cụ rất hữu ích trong quá trình làm bài Lab 02, đặc biệt trong việc brainstorming và định hình sản phẩm. Tuy nhiên, AI không phải là “người quyết định cuối cùng” cho các bài toán an toàn và vận hành. Tôi đã học được rằng AI hiệu quả nhất khi được gắn với:
- ranh giới rõ,
- test case cụ thể,
- human review,
- và không được phép tự động hành động trong những tình huống nhạy cảm.

Với bài toán VinFast EV charging safety, cách làm hợp lý nhất là **Rule + LLM + HITL**: AI hỗ trợ viết draft nội dung, nhưng rule engine và con người mới là người chịu trách nhiệm quyết định cuối cùng.

Đây cũng chính là bài học quan trọng nhất của lab: **AI tốt khi biết giới hạn của mình**.
