# Tổng quan
Xuyên suốt bài tập, AI (Gemini, Claude Sonnet 5, GPT-5.4 mini) được sử dụng ở hai vai trò khác nhau: (1) giải thích lý thuyết và thiết kế quy trình/Skill cho Domain Testing & BVA, và (2) trực tiếp áp dụng quy trình đó để sinh test case cho 4 tính năng FR-03, FR-11, FR-17, FR-06.
Qua đó em thấy chất lượng output không chỉ phụ thuộc vào năng lực suy luận của model, mà phụ thuộc rất nhiều vào chất lượng của chính bản Skill/prompt đã được tinh chỉnh qua nhiều vòng. Nhìn chung, AI thể hiện tốt ở việc tuân thủ quy trình lý thuyết (EC, BVA, checkpoint) nhưng có xu hướng mắc lỗi ở phần phân tích ra dữ liệu thật, tức là biến lý thuyết miền/biên thành dữ liệu test cụ thể đúng với những gì người dùng thực sự nhìn thấy trên giao diện. Vì vậy để có thể sinh bộ test case sát với dữ liệu, người dùng cần cung cấp thông tin (định dạng/ràng buộc,...) sát nhất có thể thì sẽ không cần chỉnh sửa quá nhiều sau khi sinh test case.

# Đánh giá từng tính năng
## FR-03: Quên mật khẩu & Đặt lại mật khẩu
### Điểm mạnh: 
- Tự suy luận đúng và quan trọng nhất một ràng buộc chéo mà đặc tả không nêu trực tiếp, ví dụ OTP phải gắn với đúng email đã yêu cầu (EC-06/EC-07 trong Step 3). Khi thực thi thật, đây là lỗi bảo mật nghiêm trọng nhất được phát hiện (BUG-FR03-006), hệ thống không kiểm tra ràng buộc OTP–email.
- Việc tách riêng từng điều kiện AND của password policy (thiếu hoa/thường/số/ký tự đặc biệt) thành các test case độc lập đã giúp phát hiện ra một lỗi hệ thống rất nghiêm trọng: toàn bộ 13/16 test case Domain+BVA đều Failed
### Hạn chế
- Ở lần chạy đầu tiên bảng Test Case Report tại Step 4 dùng dữ liệu rất trừu tượng ("Email hợp lệ", "OTP đúng 6 chữ số và khớp email", "Mật khẩu mới hợp lệ") thay vì giá trị cụ thể có thể copy-paste chạy ngay. Sau đó phải chỉnh lại Skill để ép buộc "Giá trị đại diện cụ thể", cho thấy AI có xu hướng mặc định viết mô tả trừu tượng nếu không bị ép rõ ràng bằng rule.

## FR-11: Xem lịch sử đơn hàng (User)
### Điểm mạnh
Đây là tính năng có tỷ lệ Pass cao nhất trong toàn bộ 4 feature, không phát hiện bug thật sự nào ngoài 1 hạn chế tính năng (thiếu tìm kiếm theo mã đơn, dẫn đến 2 test case Blocked). Khi đặc tả rõ ràng và ít điều kiện chéo phức tạp, AI có thể tạo ra bộ test case rất sát với hành vi thực tế của hệ thống.
### Điểm yếu
AI đã tự bịa ra dữ liệu backend làm Input Data, cụ thể là current_user_id: usr_1042 và order_code: DH20260708-001, trong khi đây là kiểm thử hộp đen qua giao diện người dùng, người dùng thực tế không bao giờ nhìn thấy usr_1042, chỉ thấy email test@eshop.com; và mã đơn hiển thị trên UI thực tế là #1. AI không phân biệt được dữ liệu nội bộ (backend/DB) và dữ liệu hiển thị (frontend/UI) khi làm black-box test case.

## FR-17: Quản lý Mã Giảm Giá
### Điểm mạnh
- Đây là tính năng AI làm tốt nhất, giá trị cụ thể khớp gần như hoàn toàn với dữ liệu dùng khi thực thi thật (SUMMER2026, admin@eshop.com, discount_value: 101, min_order_amount: -1...) mà không cần sửa lại nhiều, một phần vì chỗ này em cung cấp thông tin format dữ liệu.
### Điểm yếu
- TC-BVA-04 (percent = 101, tức max+1) và TC-10 ở Domain Testing (percent = 101) là cùng một bộ dữ liệu test, đây cũng không phải lỗi của AI nhưng cho thấy khi range quá hẹp (chỉ 1–100), Domain Testing và BVA có xu hướng tất yếu trùng lặp giá trị biên trên.

## FR-06: Xem chi tiết sản phẩm (User) - Mobile
### Điểm mạnh
- Tuân thủ quy trình checkpoint nghiêm ngặt: AI dừng đúng sau mỗi Step, không tự gộp bước, không tự suy diễn khi thiếu xác nhận 
- Tư duy phân lớp EC chặt chẽ, tách case có lý do rõ ràng: việc tách product_status thành EC riêng thay vì kết hợp với quantity, hay tách EC-QTY-02/EC-QTY-08 (0 vs âm) dù cùng "nhỏ hơn min" nhưng khác cơ chế validate, thể hiện đúng nguyên tắc "nếu nghi ngờ các phần tử trong 1 EC xử lý khác nhau → tách nhỏ".
- Chủ động lấp khoảng trống đặc tả và gắn nhãn minh bạch: các ràng buộc suy luận (max=99, case hết hàng, case NULL) đều được đánh dấu [inferred] rõ ràng, không đánh lận với dữ liệu từ spec gốc — giúp người review dễ kiểm tra lại giả định.
- Test data cụ thể, không mơ hồ: không có test case nào dùng cụm chung chung kiểu "giá trị bất kỳ"; mọi input đều là giá trị copy-paste chạy ngay được (0, "abc", 2.5, "!@#$%", -5...).

### Điểm yếu

Thiết kế đầy đủ nhưng không đảm bảo được thực thi đầy đủ — rủi ro nằm ở khâu chuyển giao. Đây là gap lớn nhất: AI tạo ra bộ test case rất kỹ (10 domain + 6 BVA, cover cả 2 phía biên và toàn bộ trạng thái sản phẩm), nhưng bản thực thi chỉ chạy 10/16 case, bỏ đúng cụm case khó/ít trực quan hơn (trạng thái Hết hàng, NULL, biên trên). Đây không hẳn là lỗi của AI — nhưng AI (hoặc quy trình audit) không có bước nào đối chiếu "test case đã thiết kế" với "test case đã thực thi" để cảnh báo thiếu sót, dẫn đến rủi ro cho là "đã test đầy đủ" trong khi chỉ mới cover phân nửa.