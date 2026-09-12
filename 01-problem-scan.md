# Problem Scan — Vin Smart Future

---

## Phase 1 — SCAN

Hãy sử dụng 4 lenses dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại ít nhất 5 bài toán/bottleneck thực tế.

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
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | Xanh SM | Lặp lại + Pain từ người khác | Tài xế và điều phối viên mất nhiều thời gian phối hợp lộ trình, điểm đón/điểm trả khách và xử lý yêu cầu khẩn cấp do hệ thống gợi ý chưa chính xác, gây trễ và phàn nàn. |
| 2 | VinFast | Tốn thời gian | Hệ thống cần soạn gợi ý trạm sạc phù hợp cho lái xe EV dựa trên pin, vị trí và khoảng cách, nhưng hiện nay xử lý thủ công/không đồng bộ khiến khách hàng mất thời gian tìm trạm, chờ đợi và lo lắng khi pin thấp. |
| 3 | Vinhomes | AI có thể tốt hơn | Nhân viên quản lý cư dân và CSKH phải soạn nhiều phản hồi định kỳ về khiếu nại, bảo trì, và thông tin tiện ích, trong khi câu trả lời mang tính lặp lại và cần phản hồi nhanh trong giờ cao điểm. |
| 4 | Vinmec | Pain từ người khác | Bệnh nhân và nhân viên gặp khó khăn trong việc xác định lịch hẹn/khu vực khám, nhận thông tin chăm sóc hậu khám theo từng trường hợp; quy trình thủ công khiến hồ sơ dày và chậm xử lý. |
| 5 | Vinpearl / VinWonders | Tốn thời gian | Đội ngũ hỗ trợ du lịch và lễ tân phải trả lời nhiều câu hỏi lặp lại về đặt phòng, lịch trình, chính sách khuyến mãi, trễ tiếng và chậm phản hồi trong giờ cao điểm. |

---

## Phase 2 — QUICK-ASSESS

Chọn top 3 bài toán từ danh sách trên và hoàn thiện 3 Quick Problem Cards dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                        │
│                                                             │
│ Bài toán (1 câu): Tài xế Xanh SM mất thời gian do hệ thống  │
│ điều phối lộ trình và điểm đón không tối ưu, gây trễ và sai  │
│ lệch trong giờ cao điểm.                                    │
│ Công ty thành viên: [x] Xanh SM  [ ] VinFast  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế, điều phối viên, khách hàng đặt  │
│ xe và bộ phận vận hành Xanh SM.                             │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận yêu cầu khách hàng ──> 2. So khớp tài xế gần nhất │
│   3. Gợi ý lộ trình thủ công ──> 4. Điều chỉnh theo lưu lượng │
│   5. Thông báo lại cho khách hàng                            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Gợi ý lộ trình và điều chỉnh │
│ theo thời gian thực (⏱ 8-12 phút/lượt)                       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động đề xuất tuyến   │
│ tối ưu, điểm đón/điểm trả và cảnh báo thời tiết/tắc nghẽn    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian xử lý   │
│ từ 10 phút xuống còn dưới 3 phút, tăng tỷ lệ giao xe đúng  │
│ giờ > 90%, giảm số cuộc gọi điều phối > 20%.                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                        │
│                                                             │
│ Bài toán (1 câu): Lái xe VinFast mất thời gian tìm trạm sạc  │
│ phù hợp khi pin thấp và hệ thống hiện chưa cảnh báo/đề xuất   │
│ được lộ trình an toàn.                                       │
│ Công ty thành viên: [ ] Xanh SM  [x] VinFast  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Lái xe EV, bộ phận chăm sóc khách hàng,  │
│ trung tâm hỗ trợ VinFast.                                     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách hàng báo pin thấp ──> 2. Nhân viên xem trạm gần  │
│   3. Tính khoảng cách và trạng thái trạm ──> 4. Gửi khuyến  │
│   nghị thủ công ──> 5. Theo dõi lại nếu khách hàng không phản │
│   hồi                                                        │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? So sánh và gợi ý trạm phù  │
│ hợp dưới điều kiện pin cực thấp (⏱ 6-10 phút/lượt)          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Dự đoán mức ưu tiên   │
│ trạm, cảnh báo pin nguy hiểm và yêu cầu điều xe sạc di động  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian tìm    │
│ trạm từ 8 phút xuống còn dưới 2 phút; giảm tỷ lệ khách hàng │
│ phải dừng đột ngột do pin hết > 30%; tăng độ tin cậy lời khuyên │
│ an toàn.                                                     │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                        │
│                                                             │
│ Bài toán (1 câu): Cư dân Vinhomes nhận phản hồi chậm và lặp  │
│ lại về khiếu nại ban quản lý, bảo trì, và thông tin tiện ích. │
│ Công ty thành viên: [ ] Xanh SM  [ ] VinFast  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Cư dân, nhân viên CSKH, quản lý tòa nhà, │
│ bộ phận vận hành Vinhomes.                                   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi khiếu nại qua app/chat ──> 2. Nhân viên đọc  │
│   3. Phân loại yêu cầu bằng thủ công ──> 4. Soạn câu trả lời  │
│   5. Chuyển cho bộ phận xử lý                              │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Phân loại và soạn phản hồi  │
│ lặp lại cho nhiều yêu cầu tương tự (⏱ 5-8 phút/lượt)         │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tạo bản nháp phản hồi,│
│ phân loại mức ưu tiên và gợi ý lời nhắn chuẩn hoá cho từng  │
│ nhóm sự cố.                                                 │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian phản    │
│ hồi từ 7 phút xuống dưới 2 phút, tăng tỷ lệ xử lý đúng  │
│ chuyên mục lên > 90%, giảm số phản hồi nhầm kiểu.           │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---
