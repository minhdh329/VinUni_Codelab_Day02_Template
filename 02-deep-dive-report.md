# Deep Dive Report — Vin Smart Future

---

## Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow Mapping
**Bài toán được chọn: Hệ thống hỗ trợ tìm trạm sạc và cảnh báo pin nguy cấp cho lái xe VinFast.**

#### Workflow hiện tại
1. Lái xe EV nhận cảnh báo pin thấp hoặc quay lại chủ xe và báo cần tìm trạm sạc.  
2. Nhân viên chăm sóc khách hàng hoặc trung tâm hỗ trợ xem bản đồ trạm sạc và dữ liệu pin gần nhất.  
3. Hệ thống/nhân viên tính khoảng cách, thời gian đến, trạng thái trạm, và mức độ đầu hàng đang đợi.  
4. Nhân viên soạn tin nhắn hoặc gọi điện hướng dẫn khách hàng đến trạm phù hợp.  
5. Khách hàng phản hồi lại nếu trạm quá xa, quá đầy, hoặc cần điều hướng lại.  
6. Nếu pin đã rất thấp, bộ phận hỗ trợ phải gọi lại và tính toán phương án an toàn, có thể kêu gọi trợ xe sạc di động.  

#### Điểm tắc nghẽn (Bottleneck)
* 🔴 **Bottleneck chính:** Bước 2–4, nơi nhân viên phải xử lý nhiều dữ liệu thủ công: khoảng cách, trạng thái trạm, mức pin, và mối nguy hiểm nếu pin < 5%.
* 🔄 **Handoff:** Từ dữ liệu do xe/ứng dụng cung cấp → nhân viên hỗ trợ → họ soạn tin nhắn hoặc hướng dẫn → khách hàng tiếp nhận.
* **Thời gian vận hành trung bình:** khoảng **8–12 phút/lượt** cho trường hợp pin thấp và **15–20 phút/lượt** nếu cần xử lý khẩn cấp hoặc kéo dài về trạm.
* **Tổng cộng = 10 phút/lượt trung bình**.

### 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Lái xe VinFast, nhân viên chăm sóc khách hàng, đội ngũ hỗ trợ điều phối trạm sạc và bộ phận vận hành mạng lưới sạc. |
| **2. Current Workflow** | Trước khi xuất phát hoặc khi pin xuống thấp, khách hàng hoặc nhân viên phải tự kiểm tra trạm gần nhất, so sánh khoảng cách, thời gian đến, trạng thái trạm, và mức độ an toàn. Quy trình chủ yếu dựa vào thao tác thủ công trong app/hottline và xử lý bằng người. |
| **3. Bottleneck** | Bước gợi ý trạm và đánh giá mức độ an toàn khi pin rất thấp là chậm và dễ sai. Nhân viên phải xử lý nhiều tham số đồng thời, đặc biệt khi pin dưới 5% thì không được recommend trạm cách xa hơn 5km. |
| **4. Business Impact** | Tổn thất về thời gian chờ, nguy cơ xe bị hết pin giữa đường, giảm trải nghiệm khách hàng, tăng tải cuộc gọi hỗ trợ, và có thể gây tổn hại uy tín thương hiệu VinFast trong phân khúc xe điện. |
| **5. Success Metric** | AI/Rule engine giúp giảm thời gian tìm trạm từ **8 phút xuống dưới 2 phút**, tăng tỷ lệ gợi ý an toàn đúng **> 95%**, giảm số trường hợp pin cực thấp không được cảnh báo kịp thời **> 30%**. |
| **6. Operational Boundary** | AI chỉ được đề xuất trạm sạc an toàn, không bao giờ khuyến nghị trạm xa hơn 5km khi pin < 5%; nếu pin dưới ngưỡng an toàn thì phải trigger `dispatch_mobile_charger` và luôn bắt đầu tin nhắn với thẻ `[DRAFT_ONLY]`, không được gửi trực tiếp mà không có người review. |

### 3.3. Future-State Flow & AI Fit
* **AI Fit:** [x] Rule / State-Machine  [x] LLM Feature  [ ] Agentic Loop

#### Future-State Flow
1. **📡 Thu thập dữ liệu:** Xe gửi trạng thái pin, vị trí GPS, tốc độ, lịch trình, và dữ liệu trạm sạc xung quanh.  
2. **🔧 Rule Engine:** Hệ thống kiểm tra ngưỡng pin: nếu pin < 5% thì không đề xuất trạm > 5km; ngay lập tức phát đi lệnh `dispatch_mobile_charger`.  
3. **🔵 AI Step (LLM):** Tạo bản nháp tin nhắn hướng dẫn cho khách hàng: ưu tiên trạm gần nhất, chỉ ra thời gian đến, cảnh báo pin nguy hiểm, giữ nguyên tag `[DRAFT_ONLY]` cho mục đích review.  
4. **🟢 Human-in-the-loop:** Nhân viên vận hành hoặc CSKH xem lại và xác nhận tin nhắn gửi đến khách hàng.  
5. **↩️ Fallback:** Nếu dữ liệu trạm không đầy đủ, hệ thống chuyển sang quy trình an toàn: ưu tiên trạm sạc gần nhất có thể xác minh, hoặc yêu cầu điều xe sạc di động và dừng khuyến nghị đến trạm xa.  

#### Vì sao giải pháp này fit?
* Đây là bài toán có nhiều quy tắc nền tảng rõ ràng (pin, khoảng cách, cảnh báo an toàn) nên **Rule / State-Machine** rất hiệu quả.  
* LLM chỉ nên đóng vai trò **drafting và explanation**, không tự động gửi tin nhắn.  
* Do tính rủi ro cao của an toàn trạm sạc và khách hàng đang di chuyển, **HITL** là bắt buộc.  
* Bài toán này không cần AI agent phức tạp vì mục tiêu chủ đạo là **an toàn + tốc độ + khống chế rủi ro**, không phải tự hành động độc lập trên toàn hệ thống.

---

## Phase 5 — EVALUATE

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> Bài toán này có tính rủi ro cao nhưng lại rất rõ về quy tắc vận hành: pin < 5% phải dừng khuyến nghị trạm xa, ưu tiên dispatch mobile charger, và bắt buộc có review trước khi gửi. Đây là trường hợp phù hợp nhất để dùng mô hình AI dưới dạng hỗ trợ draft không tự động gửi, kết hợp với rule-based logic và human-in-the-loop.  
> Về chi phí, bài toán không cần xây dựng hệ thống AI phức tạp hay agentic automation; chỉ cần một rule engine + prompt guardrail + hành vi review rõ ràng. Về kỹ thuật, ranh giới an toàn rất dễ định nghĩa và kiểm tra bằng test case, nên dễ đo lường thành công và giảm rủi ro sai.  
> Do đó, quyết định là **GO** với scope hẹp: ép ràng buộc an toàn, test trên các adversarial prompts, và chỉ cho phép AI đóng vai trò draft nội dung chứ không tự động quyết định hướng đi cho người lái.

---
