# 🏗️ Phase 3 — DEEP-DIVE

## 3.1. Current-State Workflow
Quy trình phát hiện và xử lý hành vi lái xe nguy hiểm hiện tại của Xanh SM:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Hệ thống GPS │     │ Nhân viên    │     │ Review thủ   │     │ Gọi điện /   │
│ & app thu    │ ──→ │ xuất Excel   │ ──→ │ công, đánh   │ ──→ │ xếp lịch đào │
│ dữ liệu lái  │     │ báo cáo cuối │     │ dấu vi phạm  │     │ tạo lại tài  │
│ xe (tốc độ,  │     │ tuần         │     │ nổi bật      │     │ xế           │
│ phanh, tăng) │     │              │     │              │     │              │
│ Ai: Hệ thống │     │ Ai: Vận hành │     │ Ai: Vận hành │     │ Ai: Vận hành │
│ ⏱ auto       │     │ ⏱ 3-4 giờ/  │     │ ⏱ 2-3 ngày  │     │ ⏱ 30 phút/  │
│              │     │   tuần       │     │   trễ 🔴     │     │   tài xế 🔴  │
│ Out: Raw log │     │ Out: Excel   │     │ Out: Danh    │     │ Out: Lịch    │
│              │     │   thô        │     │   sách vi    │     │   đào tạo    │
│              │     │              │     │   phạm       │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘

🔴 = Bottlenecks
⏱ Tổng thời gian phát hiện vi phạm: 48–72 giờ sau sự kiện xảy ra.
📊 Độ bao phủ tài xế được monitor: chỉ ~20% (những trường hợp nổi bật nhất).
```

---

## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên vận hành đội xe (Fleet Operations) và Quản lý an toàn (Safety Manager) tại Xanh SM. |
| **2. Current Workflow** | Hệ thống GPS và ứng dụng tài xế tự động thu dữ liệu chuyển động xe (tốc độ, phanh gấp, tăng ga đột ngột, timestamp). Nhân viên vận hành xuất toàn bộ log sang Excel cuối tuần, dò thủ công để phát hiện vi phạm nổi bật, sau đó gọi điện nhắc nhở hoặc xếp lịch tái đào tạo. Toàn bộ quy trình mất 48–72 giờ từ khi sự kiện xảy ra đến khi được phát hiện. |
| **3. Bottleneck** | Bước 2–3: Phân tích log thủ công trên Excel không thể bao phủ toàn bộ 100% tài xế — chỉ ~20% trường hợp nổi bật được xem xét; bỏ sót ~40% vi phạm nhỏ nhưng tích lũy (phanh gấp lặp lại, vượt tốc độ giới hạn nội đô liên tục). Không có cơ chế cảnh báo real-time. |
| **4. Business Impact** | Xanh SM vận hành >15,000 xe điện toàn quốc. Tỉ lệ tai nạn/sự cố do lái ẩu ở fleet không có safety scoring cao hơn 2.3× so với fleet có AI monitoring (benchmark Grab/Gojek). Chi phí sửa chữa xe, bồi thường khách hàng và tổn hại thương hiệu ước tính ~20–30 tỷ VNĐ/năm. Nhân sự review thủ công tiêu tốn ~3–4 giờ/tuần/người. |
| **5. Success Metric** | 1. Giảm tỉ lệ tai nạn/sự cố liên quan đến lái ẩu **-30% sau 6 tháng** triển khai.<br>2. Thời gian phát hiện hành vi nguy hiểm: từ **48–72 giờ → dưới 30 giây (real-time)**.<br>3. Độ bao phủ tài xế được monitor tự động: từ **~20% → 100%**. |
| **6. Operational Boundary** | **AI được phép:** Tự động tính Driver Safety Score theo thời gian thực từ dữ liệu GPS/sensor; phân loại mức nguy hiểm (Low / Medium / High); đẩy cảnh báo lên dashboard Safety Manager; gửi thông báo nhắc nhở in-app tài xế **sau khi Safety Manager phê duyệt**. **TUYỆT ĐỐI CẤM:** AI không được tự ý đình chỉ tài khoản tài xế, trừ điểm thưởng, hoặc gửi thông báo kỷ luật mà chưa có con người phê duyệt (bắt buộc HITL cho mọi hành động ảnh hưởng đến thu nhập tài xế). |

---

## 3.3. Future-State Flow & AI Fit

* **AI Fit:** Chọn **Agentic Loop** — cần monitor liên tục theo thời gian thực, tự động tính điểm từ nhiều sự kiện tích lũy, và trigger cảnh báo không đồng bộ (khác với LLM Feature là request-response đơn lẻ).
* **Quy trình tương lai (Future-State):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Hệ thống GPS │     │ 🔵 AI Agent  │     │ 🔵 Dashboard │     │ 🟢 Safety    │
│ & sensor thu │ ──→ │ tự động tính │ ──→ │ cảnh báo     │ ──→ │ Manager      │
│ dữ liệu lái  │     │ Driver Safety│     │ real-time    │     │ review &     │
│ xe liên tục  │     │ Score        │     │ tài xế       │     │ phê duyệt    │
│              │     │ (real-time)  │     │ High-Risk    │     │ hành động    │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ 🟢 Gửi thông │
                                                               │ báo nhắc nhở │
                                                               │ tài xế (sau  │
                                                               │ khi duyệt)   │
                                                               └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI score bất
                                                               thường / sensor lỗi,
                                                               flag để nhân viên
                                                               review thủ công.
```

---

# 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
   → **Có.** Xanh SM đã thu thập dữ liệu GPS và sensor từ toàn bộ đội xe. Log có cấu trúc rõ ràng (timestamp, vehicle_id, speed, acceleration, brake_force, location). Dữ liệu sạch và đủ lớn (>15,000 xe × nhiều tháng vận hành).

2. [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
   → **Có.** AI chỉ tính điểm và cảnh báo — không có quyền hành động trực tiếp lên tài khoản tài xế. Mọi hành động kỷ luật bắt buộc qua Safety Manager phê duyệt (HITL). Khi sensor lỗi, hệ thống tự động flag để review thủ công (Fallback rõ ràng).

3. [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
   → **Có.** Safety Manager được giảm tải từ dò Excel sang chỉ cần review dashboard cảnh báo. Tài xế được nhắc nhở sớm thay vì bị xử lý sau khi tai nạn đã xảy ra — win-win cho cả hai phía.

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán đạt **GO** vì ba lý do cốt lõi:
>
> 1. **Dữ liệu sẵn sàng:** Xanh SM đã có đủ dữ liệu GPS/sensor có cấu trúc từ >15,000 xe — không cần giai đoạn thu thập tốn kém. Có thể bắt đầu prototype ngay lập tức.
>
> 2. **ROI rõ ràng và đo được:** Tiết kiệm ~20–30 tỷ VNĐ/năm từ chi phí tai nạn, sửa chữa, bồi thường — cộng với tiết kiệm nhân sự review thủ công. Payback period ước tính dưới 6 tháng sau triển khai.
>
> 3. **Rủi ro thấp, kiểm soát được:** Ranh giới vận hành rõ ràng (AI chỉ cảnh báo, không hành động trực tiếp lên tài xế). HITL bắt buộc trước mọi quyết định ảnh hưởng đến thu nhập tài xế. Fallback về quy trình thủ công khi sensor lỗi.
>
> **Scope prototype đề xuất:** Bắt đầu với 500 xe tại Hà Nội, đo giảm tỉ lệ sự cố trong 3 tháng đầu trước khi rollout toàn quốc.


