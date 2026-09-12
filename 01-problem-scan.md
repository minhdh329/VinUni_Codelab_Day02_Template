# 01 - Problem Scan & Quick Cards

## Bối cảnh

Nhóm tập trung vào các bài toán vận hành có tính lặp lại, tốn thời gian hoặc gây khó khăn cho nhân viên và khách hàng của Xanh SM, Vinmec và Vinhomes. Các con số trong tài liệu là **baseline giả định dùng cho prototype**, cần được kiểm chứng bằng log thực tế trước khi triển khai.

## Phase 1 - SCAN

| # | Công ty | Lens | Bài toán |
|---|---|---|---|
| 1 | Xanh SM | Repetitive | Kiểm tra tính minh bạch của giao dịch và phát hiện giao dịch chuyển tiền giả. |
| 2 | Xanh SM | Stakeholder Pain | Gợi ý điểm đón khách chưa chính xác, khiến tài xế và khách phải gọi lại. |
| 3 | Vinmec | Repetitive, AI-upgrade | Xử lý và trả lời câu hỏi thường gặp về lịch khám, chuẩn bị xét nghiệm, thuốc và thông tin bác sĩ. |
| 4 | Vinmec | Repetitive, Time-consuming | Đặt lịch và tối ưu lịch khám bác sĩ. |
| 5 | Vinhomes | AI-upgrade, Stakeholder Pain | Dự đoán nhu cầu bảo trì thiết bị trong khu đô thị. |

## Phase 2 - Quick Problem Cards

### Card #1 - Kiểm tra giao dịch Xanh SM

- **Bài toán:** Phát hiện giao dịch đáng ngờ hoặc giao dịch chuyển tiền giả trước khi hoàn tất đối soát.
- **Công ty:** Xanh SM.
- **Actor:** Nhân viên đối soát, bộ phận vận hành và tài xế.
- **Workflow:** Ghi nhận giao dịch -> kiểm tra thông tin giao dịch và lịch sử chuyến đi -> đối chiếu biên lai/tài khoản thanh toán -> liên hệ xác minh -> quyết định xử lý.
- **Bottleneck:** Đối chiếu thủ công ở bước 2-3, khoảng 10 phút/giao dịch trong baseline giả định.
- **AI hỗ trợ:** Phân tích dữ liệu giao dịch, đánh dấu dấu hiệu bất thường và ưu tiên hồ sơ cần nhân viên kiểm tra.
- **Metric:** Phát hiện ít nhất 95% giao dịch đáng ngờ và giảm thời gian kiểm tra từ 10 phút xuống dưới 2 phút/giao dịch.
- **Architecture:** LLM Feature kết hợp rule-based validation và human review.

### Card #2 - Gợi ý điểm đón khách

- **Bài toán:** Đề xuất điểm đón chính xác hơn khi địa chỉ khách nhập không rõ hoặc không trùng với lối vào thực tế.
- **Công ty:** Xanh SM.
- **Actor:** Tài xế và khách hàng.
- **Workflow:** Khách nhập địa chỉ -> hệ thống xác định vị trí trên bản đồ -> tài xế gọi khách xác nhận -> hai bên tìm vị trí đón phù hợp -> bắt đầu chuyến.
- **Bottleneck:** Xác định vị trí và gọi xác nhận ở bước 2-3, khoảng 5 phút/lượt trong baseline giả định.
- **AI hỗ trợ:** Phân tích địa chỉ, lịch sử điểm đón và ngữ cảnh địa điểm để đề xuất tọa độ/lối vào gần nhất.
- **Metric:** Giảm tỷ lệ phải gọi lại từ 20% xuống dưới 5%; giảm thời gian xác nhận từ 5 phút xuống dưới 2 phút/lượt.
- **Architecture:** LLM Feature kết hợp dữ liệu bản đồ và rule kiểm tra khoảng cách.

### Card #3 - Hỗ trợ câu hỏi thường gặp tại Vinmec

- **Bài toán:** Trả lời nhanh các câu hỏi hành chính và hướng dẫn chuẩn bị khám của bệnh nhân.
- **Công ty:** Vinmec.
- **Actor:** Bệnh nhân và nhân viên tổng đài.
- **Workflow:** Bệnh nhân gửi câu hỏi -> nhân viên tra cứu lịch/tài liệu được duyệt -> soạn câu trả lời -> gửi phản hồi -> chuyển bác sĩ nếu câu hỏi phức tạp.
- **Bottleneck:** Tra cứu và soạn câu trả lời ở bước 2-3, khoảng 8 phút/lượt trong baseline giả định.
- **AI hỗ trợ:** Tìm thông tin trong kho tài liệu được phê duyệt và soạn bản nháp cho nhân viên kiểm tra.
- **Metric:** Trả lời 85% câu hỏi thường gặp trong dưới 10 giây; giảm thời gian xử lý từ 8 phút xuống dưới 2 phút/lượt.
- **Architecture:** LLM Feature có retrieval từ nguồn được phê duyệt và bắt buộc human-in-the-loop.

## Lựa chọn cho Deep-Dive

Nhóm chọn **Card #1 - Kiểm tra giao dịch Xanh SM** vì bài toán có quy trình lặp lại, có thể đo bằng thời gian xử lý và tỷ lệ phát hiện, đồng thời vẫn kiểm soát được rủi ro bằng việc không cho AI tự quyết định khóa tài khoản hoặc hoàn tiền.
