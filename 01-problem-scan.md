# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinhomes** | Tốn thời gian | Soạn phản hồi đánh giá 1-sao của cư dân thủ công (mất 8-15 phút/review). |
| 2 | **Vinhomes** | AI-upgrade | Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi rập khuôn, phản hồi chậm. |
| 3 | **Xanh SM** | Pain từ người khác | Chưa có AI scoring hành vi lái xe nguy hiểm — phát hiện vi phạm chậm 48-72h. |
| 4 | **Vinmec** | Tốn thời gian | Bác sĩ tóm tắt thủ công hồ sơ bệnh án trước khi tiếp nhận (mất 8-20 phút/bệnh nhân). |
| 5 | **VinFast** | AI-upgrade | Sales consultant tra thông số kỹ thuật chậm tại showroom (mất 1-2 phút/câu hỏi). |
| 6 | **VinFast** | Tốn thời gian | Forecast linh kiện/tồn kho bằng Excel thủ công, tỉ lệ lỗi stockout ~25%. |

---

## 🗳️ Quyết định lựa chọn:
Nhóm quyết định chọn **3 bài toán: #5 (VinFast Sales Assistant), #1 (Vinhomes Review Response), #3 (Xanh SM Driver Safety Scoring)** để thực hiện Quick-Assess.

### Lý do lựa chọn và loại bỏ:
* **#6 (VinFast Forecast):** Bài toán cần tích hợp sâu với dữ liệu ERP/supply chain từ 20+ nhà cung cấp, phạm vi kỹ thuật quá lớn cho prototype 30 phút. Nên dùng ML truyền thống (time-series) thay vì LLM.
* **#2 (Vinhomes Chatbot):** Rủi ro AI trả lời sai về giá vé/khuyến mãi có thể gây tranh chấp thương mại với Vinpearl. Cần gom dữ liệu chính sách sạch và test kỹ hơn trước khi prototype.
* **#4 (Vinmec EMR):** Dữ liệu hồ sơ bệnh án thuộc diện thông tin y tế nhạy cảm, cần review tuân thủ pháp lý (HIPAA-equivalent) trước khi đưa vào LLM — chưa phù hợp để prototype trong Lab.

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn top 3 từ danh sách SCAN: **#5 (VinFast Sales Assistant), #1 (Vinhomes Review Response), #3 (Xanh SM Driver Safety Scoring).**

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Sales consultant tra thông số kỹ thuật    |
│                     chậm tại showroom                       |
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên sales tại showroom           │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hỏi thông số kỹ thuật                            |
│   2. Nhân viên search trên hệ thống nội bộ / hỏi đồng       |
│      nghiệp                                                 |
│   3. Nhân viên trả lời khách hàng                           |
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? 2. Nhân viên search trên    |
│   hệ thống nội bộ / hỏi đồng nghiệp (⏱ 1-2 phút/lượt)       |
│ AI có thể nhảy vào hỗ trợ ở bước nào? 2. AI Answer Chatbot  |
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian search thông số kỹ thuật từ 1-2 phút ──> 30s |
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent |
└─────────────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Soạn phản hồi đánh giá 1-sao của cư dân thủ công  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên chăm sóc khách hàng Vinhomes │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Đọc nội dung đánh giá ──> 2. Soạn phản hồi thủ công    |
│   ──> 3. Gửi phản hồi ──> 4. Theo dõi xử lý                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? 2. Soạn phản hồi (⏱ 8-15 phút/review) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: LLM draft    │
│   phản hồi; Bước 3: human review & gửi                      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm thời gian soạn phản hồi từ 8-15 phút ──> dưới 2 phút│
│   - Tỉ lệ review được phản hồi trong 24h: 65% ──> 95%       │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                     │
│                                                             │
│ Bài toán (1 câu): Chưa có AI scoring hành vi lái xe nguy hiểm  │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Quản lý vận hành đội xe & tài xế      │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Hệ thống GPS/app thu dữ liệu tốc độ, phanh, tăng ga    │
│   ──> 2. Nhân viên xuất báo cáo Excel cuối tuần             │
│   ──> 3. Xem thủ công, đánh dấu tài xế vi phạm nổi bật      │
│   ──> 4. Gọi điện nhắc nhở / xếp lịch đào tạo lại           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? 2-3: Review Excel thủ công │
│   (⏱ 2-3 ngày/tuần trễ, bỏ sót ~40% vi phạm nhỏ)           │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│   Bước 2-3: AI tự động tính Driver Safety Score theo thời   │
│   gian thực, cảnh báo ngay khi phát hiện hành vi nguy hiểm  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm tỉ lệ tai nạn/sự cố liên quan lái ẩu: -30% sau 6T  │
│   - Thời gian phát hiện hành vi nguy hiểm: 48h ──> real-time │
│   - Độ bao phủ tài xế được monitor: 20% ──> 100%            │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*