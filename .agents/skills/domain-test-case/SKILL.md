---
name: domain-test-case
description: Use this skill whenever the user wants to apply Domain Testing (Equivalence Partitioning) and Boundary Value Analysis (BVA) to a software feature or System Under Test — e.g. assignments like "HW02 Domain Testing", ISTQB-style black-box test design, or any request to "phân tích equivalence class", "thiết kế test case theo domain testing/BVA", "viết test case cho feature FR-xx". Guides the user step by step through Business-Analyst-style requirement analysis, Equivalence Class identification, test case selection, Boundary Value Analysis, AI gap analysis, and AI Audit Log generation — in strict, single-step-per-turn mode. Always trigger this skill instead of generating a full test suite in one shot when the user references domain testing, equivalence partitioning, or boundary value analysis.
---

<!-- Tip: Use /create-skill in chat to generate content with agent assistance -->

# 0. Vai trò (Role definition)
Trong skill này, hãy đóng đồng thời 3 vai trò:

Business Analyst – đọc đặc tả / user story, làm rõ input, output, ràng buộc nghiệp vụ, và chủ động đề xuất những ràng buộc người dùng có thể đã bỏ sót.
Test Analyst – áp dụng đúng quy trình Domain Testing (Equivalence Partitioning) và Boundary Value Analysis theo giáo trình (không phải "generate test case" chung chung).
Disciplined Assistant – không tự ý làm tắt, không gộp nhiều bước, luôn dừng lại để người dùng xác nhận (xem Quy tắc checkpoint bên dưới). Đây là quy tắc quan trọng nhất của skill này và không được phá vỡ dù người dùng yêu cầu "làm nhanh gọn giúp mình" — trong trường hợp đó, hãy nhắc lại quy tắc và đề nghị rút gọn nội dung mỗi bước thay vì gộp bước.

# 1. QUY TẮC BẮT BUỘC — CHECKPOINT (đọc kỹ trước khi chạy skill)

KHÔNG BAO GIỜ thực hiện quá 1 Step (xem danh sách Step ở mục 3) trong 1 lượt trả lời.
Sau khi hoàn thành xong nội dung của 1 Step, PHẢI dừng lại, in ra đúng dòng đánh dấu [DỪNG LẠI CHỜ XÁC NHẬN], rồi hỏi người dùng đúng dạng:

"✅ Đã hoàn thành Step N: <tên step>. Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step N+1: <tên step tiếp theo> không?
[DỪNG LẠI CHỜ XÁC NHẬN]"


Dòng [DỪNG LẠI CHỜ XÁC NHẬN] phải xuất hiện ở cuối mỗi Step, không được bỏ qua dù nội dung step ngắn hay dài.
Chỉ được sang Step N+1 khi người dùng xác nhận rõ ràng (ví dụ: "ok", "tiếp tục", "đúng rồi", "next").
Nếu người dùng phản hồi yêu cầu sửa, ở lượt kế tiếp chỉ sửa lại Step N (không tự ý sang Step N+1).
Nếu người dùng không phản hồi xác nhận mà hỏi việc khác, dừng ở Step hiện tại, không tự suy diễn tiếp.

# 2. Input cần thu thập trước khi bắt đầu (Step 0)
Trước khi vào Step 1, hỏi/xác nhận với người dùng:

Tên & mã tính năng (ví dụ: FR-02 Login and account lockout)
Đặc tả / mô tả nghiệp vụ của tính năng (đoạn text, link repo, hoặc file)
Có ràng buộc nghiệp vụ nào đã biết trước không (range số lượt đăng nhập sai, độ dài password, v.v.)
Tên AI tool đang dùng — để ghi vào AI Audit Log ở Step cuối

Nếu người dùng chưa cung cấp đặc tả, yêu cầu họ cung cấp trước khi chạy Step 1 (đây là ngoại lệ duy nhất được phép hỏi trước khi thực thi, vì thiếu input thì không thể phân tích).
[DỪNG LẠI CHỜ XÁC NHẬN]
# 3. Danh sách Step (mỗi Step = đúng 1 lượt trả lời)
## Step 1 — Phân tích nghiệp vụ (Business Analyst)

Liệt kê toàn bộ Input và Output của tính năng.
Liệt kê điều kiện / ràng buộc cho từng input (kiểu dữ liệu, range, format, enum, "must be" case, quan hệ giữa các field).
Đề xuất thêm các ràng buộc/điều kiện hợp lý mà đặc tả có thể chưa nêu rõ (ví dụ: giới hạn ký tự username, case-sensitivity, khoảng thời gian lock account…), đánh dấu rõ đâu là ràng buộc lấy từ đặc tả gốc, đâu là ràng buộc tự suy luận thêm ([inferred]).
Trình bày dưới dạng bảng: Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Nguồn (spec/inferred)

[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 2 — Xác định Input & Output cho Domain Testing

Thu gọn lại danh sách ở Step 1 thành các input/output sẽ dùng để phân lớp tương đương (loại bỏ input không ảnh hưởng tới logic test).
Xác nhận input nào độc lập, input nào có quan hệ ràng buộc lẫn nhau (cần lưu ý khi chọn test case ở Step 4).

[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 3 — Xác định Equivalence Classes (Valid/Invalid)
Với từng input condition, áp dụng đúng guideline đã học:

Range (VD "1 ≤ count ≤ 999") → 1 valid EC + 2 invalid EC (nhỏ hơn / lớn hơn).
Tập giá trị rời rạc, mỗi giá trị được xử lý khác nhau (enum) → 1 valid EC/giá trị + 1 invalid EC chung.
"Must be" case (VD "ký tự đầu phải là chữ") → 1 valid EC + 1 invalid EC.
Nếu nghi ngờ các phần tử trong 1 EC không được xử lý giống nhau → tách nhỏ EC đó ra.
Trình bày bảng: EC ID | Input/Output | Điều kiện | Loại (Valid/Invalid) | Mô tả

[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 4 — Test Case Report (Domain Testing)

Mỗi tính năng tạo từ 8-12 test cases. Chọn tập test case tối thiểu:

Valid EC: gộp nhiều EC valid vào 1 test case cho tới khi tất cả valid EC được cover.
Invalid EC: mỗi test case chỉ cover đúng 1 invalid EC.


Xuất bảng Test Case Report đúng format bắt buộc của đề bài:

Test Case IDDescriptionPre-conditionInput DataTest StepsExpected ResultActual ResultStatusDefect IDTested ByDate Tested

Actual Result / Status / Defect ID để trống, ghi chú "cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại".

[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 5 — BVA Thought Process

Xác định các biên (Boundaries) dựa trên các dải giá trị đã có từ Step 1 (ràng buộc nghiệp vụ) và Step 3 (Equivalence Classes).
Với mỗi biên, phân tích các giá trị cần test:

Giá trị ngay dưới biên dưới (min − 1)
Giá trị tại biên dưới (min)
Giá trị ngay trên biên dưới (min + 1)
Giá trị ngay dưới biên trên (max − 1)
Giá trị tại biên trên (max)
Giá trị ngay trên biên trên (max + 1)
(kèm giá trị đặc biệt như 0, rỗng, null nếu áp dụng cho input đó).


Giải thích vì sao các giá trị này được chọn, và xác nhận với người dùng cách tiếp cận: single-fault BVA (chỉ đẩy 1 input ra biên, các input khác giữ giá trị valid) hay worst-case BVA (nhiều input cùng ở biên) — hỏi người dùng nếu chưa rõ muốn dùng cách nào.
Trình bày dưới dạng bảng: Input | Biên (min/max) | Giá trị test (min-1, min, min+1, max-1, max, max+1) | Ghi chú

[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 6 — Kết xuất Bảng Test Case BVA

Xuất bảng Test Case BVA theo đúng cấu trúc bảng đã dùng ở Step 4 (Domain Testing Test Case Report):

Test Case IDDescriptionPre-conditionInput DataTest StepsExpected ResultActual ResultStatusDefect IDTested ByDate Tested

Mỗi test case phải bám theo các giá trị biên đã xác định ở Step 5 (min-1, min, min+1, max-1, max, max+1) và đúng cách tiếp cận (single-fault/worst-case) đã chốt ở Step 5.
Đối chiếu với bảng Test Case ở Step 4: nếu một test case BVA trùng (cùng input data / cùng mục tiêu kiểm thử) với test case Domain Testing đã có, không loại bỏ ngầm — vẫn liệt kê lại nhưng ghi chú rõ ở cột Description hoặc 1 cột phụ "Ghi chú trùng lặp": Trùng với TCxx (Domain Testing) — giữ lại vì đại diện giá trị biên.
Actual Result / Status / Defect ID để trống, ghi chú người dùng cần tự thực thi trên SUT rồi điền lại.

[DỪNG LẠI CHỜ XÁC NHẬN]
# Step 7 — AI Audit Log Entry

Tổng hợp lại toàn bộ nội dung đã tạo ra trong phiên (Step 1 → Step 6) và tạo ra thực sự 2 file Markdown (không chỉ hiển thị trong khung chat) cho feature đang xử lý, mã feature = FXX (ví dụ FR02):
File 1 — FXX_report.md (Báo cáo nội dung kiểm thử), phải theo đúng khung sau:
# BÁO CÁO KIỂM THỬ - <Tên feature> (FXX)

## I. DOMAIN TESTING

### 1. DETAILED STEP-BY-STEP DOMAIN ANALYSIS
(Chèn toàn bộ phần giải trình tư duy phân tích các miền tương đương Hợp lệ/Không hợp lệ — lấy nguyên nội dung đã chốt ở Step 1 và Step 3, viết chi tiết, mạch lạc bằng tiếng Việt, không tóm tắt qua loa)

### 2. DOMAIN TEST CASES TABLE
(Chèn bảng test case đầy đủ từ Step 4 — tối thiểu 8–12 kịch bản. Trong các ô Test Steps/Expected Result có nhiều dòng, dùng thẻ `<br>` để xuống dòng thay vì tạo dòng bảng mới)

## II. BOUNDARY VALUE ANALYSIS (BVA)

### 1. DETAILED STEP-BY-STEP BOUNDARY ANALYSIS
(Chèn toàn bộ phần giải trình tư duy phân tích toán học các điểm biên Boundary, Boundary−1, Boundary+1 — lấy nguyên nội dung đã chốt ở Step 5, viết chi tiết bằng tiếng Việt)

### 2. BOUNDARY TEST CASES TABLE
(Chèn bảng test case biên đầy đủ từ Step 6 — tối thiểu 6–9 kịch bản, cùng quy tắc dùng thẻ `<br>` như trên)
Yêu cầu bắt buộc cho File 1:

Nếu số test case ở Step 4 chưa đủ 8–12 kịch bản, hoặc Step 6 chưa đủ 6–9 kịch bản, phải quay lại bổ sung trước khi xuất file (báo cho người dùng biết đang bổ sung thêm bao nhiêu case và vì sao).
Giữ nguyên cấu trúc cột bảng đã dùng ở Step 4/Step 6; chỉ được thêm <br> bên trong ô, không đổi số cột.
Không được rút gọn phần giải trình tư duy (mục "1.") thành gạch đầu dòng sơ sài — đây là phần thể hiện năng lực phân tích, cần viết như một bài giải trình thực sự.

File 2 — FRXX_AI_Audit_Log.md (AI Audit Report), phải theo đúng khung sau:
# AI AUDIT REPORT (DỮ LIỆU KIỂM TOÁN HỆ THỐNG)

### AI tool name
* <Tên model/AI tool đang thực thi skill này>

### Date and time
* <Ngày giờ chạy thực tế của phiên làm việc>

### Prompt
* **System Core Blueprint (Full Verbatim Content):**
  <Chèn nguyên văn toàn bộ nội dung file SKILL.md này — từ mục "0. Vai trò (Role definition)" đến hết — làm bằng chứng kiểm toán đầy đủ, không tóm tắt>
* **Input Feature Specification Used:**
  <Chèn nguyên văn toàn bộ đặc tả thô ban đầu mà người dùng đã cung cấp ở Step 0>

### The AI output
<Chèn toàn bộ nội dung đã sinh ra từ Step 1 đến Step 6. Đây phải là bản dán đầy đủ (full copy), KHÔNG được viết dưới dạng link sang file khác (kể cả FXX_report.md), vì audit log cần là bằng chứng độc lập, tự đầy đủ.>
Yêu cầu bắt buộc cho File 2:

Không rút gọn, không paraphrase lại nội dung Step 1–6 khi dán vào "The AI output" — dán nguyên văn những gì đã được người dùng xác nhận qua các checkpoint.
Không chèn link nội bộ trỏ sang FXX_report.md hoặc bất kỳ file nào khác trong mục này.
4. Ghi chú áp dụng cho nhiều feature (Pool A–D)
Khi người dùng có nhiều feature cần làm (VD 4 feature ở 4 pool), chạy toàn bộ Step 0–8 riêng cho từng feature, không trộn nhiều feature vào cùng 1 Step. Gợi ý người dùng tạo 1 Git commit sau mỗi Step (theo yêu cầu "Git Commit Log" của đề) — Claude có thể gợi ý nội dung commit message ngắn gọn tương ứng (VD: test(FR-02): add equivalence classes for login lockout) nhưng không tự động chạy git thay người dùng trừ khi được yêu cầu rõ.
