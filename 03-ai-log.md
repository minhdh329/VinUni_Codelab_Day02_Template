# 📝 AI Log — Nhật ký tương tác AI (Lab 02: AI Product Scoping)

**Học viên:** Hieu  
**Mảng kinh doanh đã chọn:** Xanh SM (GSM) — Driver Safety Scoring  
**Công cụ AI sử dụng:** Antigravity (Claude Sonnet 4.6 Thinking)  
**Ngày thực hiện:** 12/09/2026

---

## 1. AI Giúp Được Gì

### 🔍 Phase 1 — SCAN: Brainstorm Pain Points

**Prompt tôi dùng:**
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

**AI giúp ích:** Trong vòng dưới 30 giây, AI gợi ý được **~15 pain point** trải đều 4 mảng kinh doanh, kèm con số ước tính tổn thất (ví dụ: ~20–30 tỷ VNĐ/năm từ tai nạn xe Xanh SM, ~8–10 tỷ VNĐ từ delay phản hồi review Vinhomes). Tôi đã cần 20 phút để tự brainstorm nhưng chỉ nghĩ ra ~3-4 bài toán chung chung, thiếu số liệu. AI đã rút ngắn giai đoạn này xuống còn ~5 phút và cho output đa dạng hơn nhiều.

**Tôi đã tự chọn lọc:** Không dùng toàn bộ danh sách AI đưa ra — tôi đã loại bỏ các bài toán quá generic hoặc không có tính khả thi kỹ thuật cao (VD: bỏ Forecast supply chain vì cần ML time-series, không phải LLM).

---

### 🃏 Phase 2 — QUICK-ASSESS: Điền Quick Problem Cards

**AI giúp ích:** Tôi đã điền thủ công Card #1 và #2, nhưng Card #3 (Xanh SM Driver Safety Scoring) còn trống nhiều trường. AI đã điền đầy đủ workflow 4 bước, số liệu bottleneck (trễ 2-3 ngày, bỏ sót ~40% vi phạm nhỏ), và 3 metrics có số.

**Điểm tôi chủ động sửa lại:** Card #2 ban đầu tôi để metric là "Tốc độ phản hồi" (quá mơ hồ). AI đã gợi ý cụ thể hơn: *"Giảm thời gian soạn phản hồi từ 8-15 phút → dưới 2 phút, tỉ lệ phản hồi trong 24h: 65% → 95%"*. Tôi chấp nhận sửa vì metric này đo được thực tế.

---

### 🏗️ Phase 3 — DEEP-DIVE: Hoàn thiện 6-field Problem Statement

**AI giúp ích nhiều nhất ở đây.** Tôi cung cấp bài toán đã chọn, AI tự động:
- Điền đủ 6 trường với ngôn ngữ chính xác và có số liệu dẫn chứng
- Vẽ current-state workflow dạng ASCII diagram với ký hiệu ⏱ và 🔴
- Xây dựng future-state flow với 3 ký hiệu 🔵 AI Step / 🟢 HITL / ↩️ Fallback
- Giải thích lý do chọn **Agentic Loop** thay vì LLM Feature một cách thuyết phục

**Tôi kiểm tra và đồng ý** với reasoning của AI: bài toán Driver Safety Scoring cần monitor liên tục, trigger không đồng bộ → Agentic Loop đúng hơn LLM Feature.

---

### 💻 Phase 4 — Prompt Prototype

**AI giúp ích:** Implement `evaluate_prompt()` sử dụng `google-genai` SDK với `temperature=0.0` cho deterministic testing. AI cũng tự viết `SYSTEM_PROMPT` với 3 quy tắc operational boundary rõ ràng và 3 adversarial test cases bám sát bài toán của tôi.

**Tôi kiểm tra ranh giới:** AI đã chủ động thay thế test cases mẫu (về sạc pin xe) thành test cases phù hợp với bài toán Driver Safety Scoring. Đây là điểm AI thể hiện khả năng **adapt context** tốt — không copy-paste mù quáng từ deliverable-example.

---

## 2. AI Trả Lời Sai / Không Chính Xác Ở Đâu

### ❌ Vấn đề 1: Actor trong Card #3 không chính xác

Ban đầu AI điền `Actor = "Nhân viên HR Xanh SM"` cho bài toán Driver Safety Scoring. Đây là sai về mặt vận hành — HR không phải người vận hành hệ thống theo dõi lái xe hằng ngày.

**Tôi sửa lại:** Actor đúng là `"Quản lý vận hành đội xe (Fleet Operations) và Safety Manager"`.

**Nhận xét:** AI đã đưa ra assumption hợp lý nhưng thiếu domain knowledge cụ thể. Đây là lỗi **hallucination-light** — không sai hoàn toàn nhưng thiếu chính xác về role thực tế trong fleet management.

---

### ❌ Vấn đề 2: Số liệu ước tính chưa có nguồn xác minh

Các con số AI đưa ra (VD: "tỉ lệ tai nạn cao hơn 2.3× benchmark Grab/Gojek", "~40% vi phạm nhỏ bị bỏ sót") là **ước tính hợp lý** dựa trên ngữ cảnh ngành, nhưng chưa có nguồn dữ liệu xác minh thực tế từ Xanh SM.

**Cách xử lý:** Tôi giữ các số liệu này làm *working assumption* trong Lab và ghi chú rõ cần validate với dữ liệu thực khi triển khai. Trong bài nộp, tôi đã thêm chú thích "(benchmark Grab/Gojek)" để minh bạch nguồn so sánh.

---

### ❌ Vấn đề 3: Deliverable format không khớp ban đầu

Khi AI lần đầu viết `02-deep-dive-report.md`, tôi phát hiện một số điểm không khớp format với `02-deliverable-example.md` (thiếu section "Quyết định lựa chọn", bảng SCAN dùng tiếng Anh thay vì tiếng Việt nhất quán).

**Tôi đã sửa:** Yêu cầu AI đọc lại deliverable-example và align lại format. AI sửa đúng ngay sau khi nhận feedback cụ thể. Bài học: **AI cần context đầy đủ** — không nên giả định AI tự biết format cần theo nếu chưa cung cấp file mẫu.

---

## 3. Prompt/Ranh Giới Tôi Đã Tinh Chỉnh

### 🔧 Tinh chỉnh 1: Thêm ràng buộc "theo format deliverable-example"

**Trước (kết quả chưa tốt):**
> *"Hoàn thiện file deep-dive-report."*

**Sau (kết quả chuẩn):**
> *"Hoàn thiện file deep-dive-report, quy tắc trình bày theo mẫu trong deliverable-example (phase 3+5), cuối cùng check lại problem scan xem trình bày có giống trong deliverable-example không & sửa lại nếu cần."*

**Kết quả:** AI không chỉ fill nội dung mà còn tự so sánh với mẫu và chỉ ra 3 điểm cần sửa trong `01-problem-scan.md` mà tôi chưa nhận ra.

---

### 🔧 Tinh chỉnh 2: Thêm ràng buộc "bám sát bài toán đã chọn" cho prompt prototype

Khi yêu cầu "hoàn thiện phần TODO trong prompt_prototype.py", AI ban đầu có xu hướng giữ nguyên test cases về sạc pin (từ deliverable-example). Tôi không cần chỉnh thêm vì AI đã tự nhận ra và adapt sang Driver Safety Scoring — nhưng đây là điểm cần lưu ý: **khi context chuyển sang bài toán mới, cần kiểm tra AI có carry-over assumption cũ hay không.**

---

## 4. Kết Luận: AI Như Một Thought Partner

| Vai trò | Đánh giá |
|---|---|
| Brainstorm nhanh pain points | ⭐⭐⭐⭐⭐ — Xuất sắc, tiết kiệm 80% thời gian |
| Điền template/form có cấu trúc | ⭐⭐⭐⭐ — Tốt, cần verify domain-specific details |
| Implement code SDK | ⭐⭐⭐⭐⭐ — Chính xác, chọn đúng pattern |
| Tự sáng tạo thay người dùng | ⭐⭐ — Không nên, AI thiếu context thực địa |
| Kiểm tra logic/ranh giới | ⭐⭐⭐ — Cần stress-test kỹ, AI có thể bỏ sót edge cases |

**Nhận xét tổng quan:** AI hoạt động tốt nhất như một **"first draft engine"** — tạo ra 80% bản nháp chất lượng rất nhanh, để tôi tập trung 20% thời gian vào kiểm tra domain accuracy, tinh chỉnh metric, và validate logic nghiệp vụ. Không nên dùng AI như một "black box" nhận output mà không review — đặc biệt với các số liệu tổn thất tài chính và ranh giới vận hành (operational boundary) vì đây là thông tin nhạy cảm ảnh hưởng trực tiếp đến quyết định kinh doanh.
