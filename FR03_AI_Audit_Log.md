# AI AUDIT REPORT (DỮ LIỆU KIỂM TOÁN HỆ THỐNG)

### AI tool name
* GPT-5.4 mini

### Date and time
* 2026-07-07 01:05:55

### Prompt
* **System Core Blueprint (Full Verbatim Content):**
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
  Giữ nguyên cấu trúc cột bảng đã dùng ở Step 4/Step 6; chỉ được thêm `<br>` bên trong ô, không đổi số cột.
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

* **Input Feature Specification Used:**
  Follow instructions in #prompt:SKILL.md with these arguments: ### FR-03: Quên mật khẩu & Đặt lại mật khẩu (2 bước)
  [](https://github.com/ttbhanh/eshop-sut/tree/main#fr-03-qu%C3%AAn-m%E1%BA%ADt-kh%E1%BA%A9u--%C4%91%E1%BA%B7t-l%E1%BA%A1i-m%E1%BA%ADt-kh%E1%BA%A9u-2-b%C6%B0%E1%BB%9Bc)
  **Bước 1 — Lấy mã OTP:**

  - Người dùng nhập địa chỉ Email đã đăng ký.
  - Hệ thống sinh mã OTP **6 chữ số ngẫu nhiên** và gửi qua Email (trong môi trường demo: hiển thị trực tiếp trên màn hình).
  - Giao diện phải hiển thị **chỉ báo bước (Step Indicator)** — ví dụ: "Bước 1 / 2".
  - Có nút **Quay lại đăng nhập**.
  **Bước 2 — Đặt lại mật khẩu:**

  - Người dùng nhập OTP, Mật khẩu mới, và **Xác nhận mật khẩu mới**.
  - Mật khẩu mới phải tuân thủ điều kiện như FR-01.
  - Hai trường mật khẩu phải khớp nhau.
  - OTP chỉ hợp lệ cho email đã yêu cầu, không thể dùng cho email khác. - Người dùng phải cung cấp: **Họ Tên**, **Email**, **Mật khẩu**.
  - Email phải có định dạng hợp lệ (`user@domain.com`) và là duy nhất trong hệ thống.
  - **Yêu cầu mật khẩu mạnh**: Tối thiểu 8 ký tự, có ít nhất 1 chữ hoa, 1 chữ thường, 1 chữ số và 1 ký tự đặc biệt (`@`, `$`, `!`, `%`, `*`, `?`, `&`).
  - Phải có trường **Xác nhận mật khẩu** — hệ thống từ chối nếu hai trường không khớp.
  - Sau khi đăng ký thành công, người dùng được chuyển tới trang Đăng nhập.

### The AI output
#### Step 1 — Phân tích nghiệp vụ (Business Analyst) cho FR-03: Quên mật khẩu & Đặt lại mật khẩu (2 bước)

| Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Nguồn |
|---|---|---|---|---|
| Email đã đăng ký | Input | Chuỗi | Bắt buộc; phải là email hợp lệ; phải tồn tại trong hệ thống | spec |
| OTP | Input | Chuỗi số | Bắt buộc ở Bước 2; đúng 6 chữ số ngẫu nhiên; chỉ hợp lệ cho email đã yêu cầu; không dùng cho email khác | spec |
| Mật khẩu mới | Input | Chuỗi | Bắt buộc ở Bước 2; phải tuân thủ policy mật khẩu mạnh: tối thiểu 8 ký tự, có ít nhất 1 chữ hoa, 1 chữ thường, 1 chữ số và 1 ký tự đặc biệt trong tập `@`, `$`, `!`, `%`, `*`, `?`, `&` | spec |
| Xác nhận mật khẩu mới | Input | Chuỗi | Bắt buộc ở Bước 2; phải khớp chính xác với mật khẩu mới | spec |
| Chỉ báo bước (Step Indicator) | Output | Thành phần giao diện | Phải hiển thị tiến trình 2 bước, ví dụ “Bước 1 / 2” | spec |
| Nút Quay lại đăng nhập | Output | Thành phần giao diện / điều hướng | Phải cho phép người dùng quay lại màn hình đăng nhập | spec |
| Mã OTP hiển thị / gửi email | Output | Chuỗi số | Hệ thống sinh OTP 6 chữ số ngẫu nhiên; trong môi trường demo có thể hiển thị trực tiếp trên màn hình thay vì gửi email thật | spec |
| Thông báo lỗi email không hợp lệ | Output | Thông báo | [inferred] Hiển thị khi email nhập vào sai định dạng | inferred |
| Thông báo lỗi email không tồn tại | Output | Thông báo | [inferred] Hiển thị khi email hợp lệ nhưng không thuộc tài khoản đã đăng ký | inferred |
| Thông báo lỗi OTP sai / không hợp lệ | Output | Thông báo | [inferred] Hiển thị khi OTP sai giá trị, sai định dạng, hoặc không khớp email đã yêu cầu | inferred |
| Thông báo lỗi OTP không khớp email | Output | Thông báo | Phải hiển thị khi OTP được nhập nhưng không thuộc email đã yêu cầu | spec |
| Thông báo lỗi mật khẩu không đạt policy | Output | Thông báo | Phải hiển thị khi mật khẩu mới không đáp ứng điều kiện như FR-01 | spec |
| Thông báo lỗi xác nhận mật khẩu không khớp | Output | Thông báo | Phải hiển thị khi mật khẩu mới và xác nhận mật khẩu mới không trùng nhau | spec |
| Trạng thái đặt lại mật khẩu thành công | Output | Thông báo / điều hướng | [inferred] Chỉ xuất hiện khi OTP hợp lệ, mật khẩu hợp lệ và hai mật khẩu khớp nhau | inferred |
| Liên kết OTP với email yêu cầu | Ràng buộc nghiệp vụ | Quan hệ dữ liệu | OTP chỉ dùng cho đúng email đã yêu cầu reset | spec |
| Luồng 2 bước | Ràng buộc nghiệp vụ | Quy trình | Bước 1 lấy OTP, Bước 2 dùng OTP để đặt lại mật khẩu | spec |
| Thời gian hiệu lực OTP | Ràng buộc nghiệp vụ | Thời gian | [inferred] Nên có thời gian hết hạn OTP để tránh tái sử dụng kéo dài; đặc tả chưa nêu rõ | inferred |
| Số lần nhập sai OTP tối đa | Ràng buộc nghiệp vụ | Số nguyên | [inferred] Nên giới hạn số lần nhập sai OTP; đặc tả chưa nêu rõ | inferred |

#### Step 2 — Xác định Input & Output cho Domain Testing

| Biến | Vai trò | Dùng cho Domain Testing? | Quan hệ / ghi chú |
|---|---|---|---|
| Email đã đăng ký | Input | Có | Là input chính của Bước 1; quyết định OTP nào được sinh và gắn với tài khoản nào |
| OTP | Input | Có | Là input chính của Bước 2; phải đúng 6 chữ số và phải khớp với email đã yêu cầu |
| Mật khẩu mới | Input | Có | Là input chính của Bước 2; chịu policy mật khẩu mạnh từ FR-01 |
| Xác nhận mật khẩu mới | Input | Có | Phụ thuộc trực tiếp vào “Mật khẩu mới”; dùng để kiểm tra 2 giá trị trùng nhau |
| Chỉ báo bước (Step Indicator) | Output | Không | Là output giao diện, không ảnh hưởng cách phân lớp tương đương của dữ liệu |
| Nút Quay lại đăng nhập | Output | Không | Là output điều hướng, không tạo domain test riêng cho logic dữ liệu |
| Mã OTP hiển thị / gửi email | Output | Có, nhưng chỉ ở mức xác nhận luồng | Là kết quả sinh ra từ email; không phải input phân lớp chính, nhưng cần kiểm tra trong test case luồng hợp lệ |
| Thông báo lỗi / thành công | Output | Không | Là kết quả mong đợi của từng miền, không phải đối tượng phân lớp đầu vào |

### Quan hệ giữa các input
- Email và OTP có quan hệ ràng buộc chặt: OTP chỉ hợp lệ với đúng email đã yêu cầu.
- Mật khẩu mới và Xác nhận mật khẩu mới có quan hệ phụ thuộc: giá trị phải khớp nhau.
- Mật khẩu mới độc lập với Email/OTP về mặt domain, nhưng chỉ được kiểm tra ở Bước 2 sau khi OTP hợp lệ.
- Domain Testing nên tập trung vào 4 input logic chính: Email, OTP, Mật khẩu mới, Xác nhận mật khẩu mới.

### Kết luận thu gọn cho Domain Testing
- Input độc lập chính: Email, OTP, Mật khẩu mới.
- Input phụ thuộc: Xác nhận mật khẩu mới.
- Output giao diện và thông báo chỉ dùng làm expected result, không dùng để chia miền.

#### Step 3 — Xác định Equivalence Classes (Valid/Invalid)

| EC ID | Input/Output | Điều kiện | Loại | Mô tả |
|---|---|---|---|---|
| EC-01 | Email | Email hợp lệ, đúng định dạng, tồn tại trong hệ thống | Valid | Email có thể dùng để yêu cầu reset mật khẩu |
| EC-02 | Email | Email sai định dạng | Invalid | Chuỗi không theo mẫu email hợp lệ |
| EC-03 | Email | Email hợp lệ nhưng không tồn tại trong hệ thống | Invalid | Email không thuộc tài khoản đã đăng ký |
| EC-04 | OTP | OTP đúng 6 chữ số và khớp với email đã yêu cầu | Valid | OTP hợp lệ để sang bước đặt lại mật khẩu |
| EC-05 | OTP | OTP không đúng 6 chữ số | Invalid | OTP quá ngắn, quá dài, chứa ký tự không phải số |
| EC-06 | OTP | OTP đúng định dạng nhưng sai giá trị | Invalid | OTP không khớp với mã đã sinh |
| EC-07 | OTP | OTP thuộc email khác | Invalid | OTP hợp lệ về mặt định dạng nhưng không gắn với email đang reset |
| EC-08 | Mật khẩu mới | Mật khẩu dài tối thiểu 8 ký tự, có chữ hoa, chữ thường, chữ số, ký tự đặc biệt hợp lệ | Valid | Mật khẩu đạt policy FR-01 |
| EC-09 | Mật khẩu mới | Mật khẩu ngắn hơn 8 ký tự | Invalid | Không đạt độ dài tối thiểu |
| EC-10 | Mật khẩu mới | Mật khẩu thiếu chữ hoa | Invalid | Không đạt yêu cầu phức tạp mật khẩu |
| EC-11 | Mật khẩu mới | Mật khẩu thiếu chữ thường | Invalid | Không đạt yêu cầu phức tạp mật khẩu |
| EC-12 | Mật khẩu mới | Mật khẩu thiếu chữ số | Invalid | Không đạt yêu cầu phức tạp mật khẩu |
| EC-13 | Mật khẩu mới | Mật khẩu thiếu ký tự đặc biệt hợp lệ | Invalid | Không đạt yêu cầu phức tạp mật khẩu |
| EC-14 | Xác nhận mật khẩu mới | Xác nhận khớp chính xác với mật khẩu mới | Valid | Chấp nhận để hoàn tất reset |
| EC-15 | Xác nhận mật khẩu mới | Xác nhận không khớp với mật khẩu mới | Invalid | Hệ thống phải từ chối |

### Ghi chú phân lớp
- Email có 3 miền logic rõ ràng: hợp lệ và tồn tại, sai định dạng, hợp lệ nhưng không tồn tại.
- OTP có 4 miền cần tách riêng vì định dạng và tính liên kết với email là 2 kiểm tra khác nhau.
- Mật khẩu mới có nhiều điều kiện độc lập theo policy, nên mỗi thiếu sót chính được tách thành một invalid EC riêng.
- Xác nhận mật khẩu mới chỉ có 2 miền: khớp và không khớp.
- Các EC valid của mật khẩu mới vẫn cần giữ nguyên tất cả điều kiện policy, không được hiểu là chỉ đúng một điều kiện là đủ.

#### Step 4 — Test Case Report (Domain Testing)

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|
| TC-01 | Reset mật khẩu thành công với dữ liệu hợp lệ | Người dùng đã có email đăng ký hợp lệ; đang ở màn hình quên mật khẩu | Email: `nguyenvana@gmail.com`;<br>OTP: `482913` (lấy từ hiển thị demo);<br>Mật khẩu mới: `Passw0rd@1`;<br>Xác nhận: `Passw0rd@1` | 1. Nhập email `nguyenvana@gmail.com` ở Bước 1<br>2. Nhấn lấy OTP<br>3. Quan sát OTP hiển thị trên demo và ghi lại giá trị `482913`<br>4. Nhập OTP `482913` ở Bước 2<br>5. Nhập mật khẩu mới `Passw0rd@1`<br>6. Nhập xác nhận `Passw0rd@1`<br>7. Gửi form | Hệ thống chấp nhận yêu cầu reset mật khẩu và hiển thị thành công |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-02 | Kiểm tra email hợp lệ và tồn tại khi lấy OTP | Người dùng đang ở Bước 1 | Email: `nguyenvana@gmail.com`; OTP: `482913` (lấy từ hiển thị demo) | 1. Nhập email `nguyenvana@gmail.com`<br>2. Nhấn lấy OTP<br>3. Ghi nhận OTP hiển thị `482913` | Hệ thống sinh OTP 6 chữ số và hiển thị/gửi OTP theo môi trường demo |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-03 | Email sai định dạng | Người dùng đang ở Bước 1 | Email: `nguyenvana@` | 1. Nhập email `nguyenvana@`<br>2. Nhấn lấy OTP | Hệ thống từ chối và hiển thị thông báo lỗi email không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-04 | Email hợp lệ nhưng không tồn tại | Người dùng đang ở Bước 1 | Email: `khongtontai@gmail.com` | 1. Nhập email `khongtontai@gmail.com`<br>2. Nhấn lấy OTP | Hệ thống từ chối và hiển thị thông báo email không tồn tại |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-05 | OTP không đúng 6 chữ số | Người dùng đã lấy OTP ở Bước 1 | Email: `nguyenvana@gmail.com`;<br>OTP: `48291A` | 1. Nhập email `nguyenvana@gmail.com`<br>2. Sang Bước 2<br>3. Nhập OTP `48291A` | Hệ thống từ chối và báo OTP không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-06 | OTP đúng định dạng nhưng sai giá trị | Người dùng đã lấy OTP ở Bước 1 | Email: `nguyenvana@gmail.com`;<br>OTP: `123456` | 1. Nhập email `nguyenvana@gmail.com`<br>2. Sang Bước 2<br>3. Nhập OTP `123456` | Hệ thống từ chối và báo OTP không hợp lệ hoặc không khớp |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-07 | OTP hợp lệ nhưng thuộc email khác | Có ít nhất 2 email đã đăng ký | Email A: `nguyenvana@gmail.com`;<br>Email B: `tranthib@gmail.com`;<br>OTP: `482913` (lấy từ email A) | 1. Lấy OTP cho email A `nguyenvana@gmail.com`<br>2. Sang Bước 2 của email B `tranthib@gmail.com`<br>3. Nhập OTP `482913` của email A | Hệ thống từ chối vì OTP không khớp với email đang yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-08 | Mật khẩu mới ngắn hơn 8 ký tự | Người dùng đã qua OTP hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `482913`;<br>Mật khẩu mới: `Pw0rd@1`;<br>Xác nhận: `Pw0rd@1` | 1. Nhập OTP `482913`<br>2. Nhập mật khẩu mới `Pw0rd@1`<br>3. Nhập xác nhận `Pw0rd@1`<br>4. Gửi form | Hệ thống từ chối và báo mật khẩu không đạt policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-09 | Mật khẩu mới thiếu ký tự đặc biệt | Người dùng đã qua OTP hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `482913`;<br>Mật khẩu mới: `Passw0rd1`;<br>Xác nhận: `Passw0rd1` | 1. Nhập OTP `482913`<br>2. Nhập mật khẩu mới `Passw0rd1`<br>3. Nhập xác nhận `Passw0rd1`<br>4. Gửi form | Hệ thống từ chối và báo mật khẩu không đạt policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-10 | Xác nhận mật khẩu không khớp | Người dùng đã qua OTP hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `482913`;<br>Mật khẩu mới: `Passw0rd@1`;<br>Xác nhận: `Passw0rd@2` | 1. Nhập OTP `482913`<br>2. Nhập mật khẩu mới `Passw0rd@1`<br>3. Nhập xác nhận `Passw0rd@2`<br>4. Gửi form | Hệ thống từ chối và báo hai mật khẩu không khớp |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |

#### Step 5 — BVA Thought Process

Mình xác định các biên thực sự có thể dùng từ đặc tả hiện tại như sau:

- OTP: độ dài cố định 6 chữ số, nên có biên theo miền giá trị hợp lệ của mã số 6 chữ số.
- Mật khẩu mới: có biên rõ ràng ở độ dài tối thiểu 8 ký tự.
- Xác nhận mật khẩu mới: không có biên số học, nhưng có biên logic theo quan hệ “khớp / không khớp”.
- Email: đặc tả có yêu cầu hợp lệ và duy nhất, nhưng không có biên số học hoặc range rõ ràng nên chưa phù hợp để dựng BVA theo kiểu min/max chuẩn; phần này phù hợp hơn với Domain Testing và kiểm tra định dạng.

| Input | Biên (min/max) | Giá trị test (min-1, min, min+1, max-1, max, max+1) | Ghi chú |
|---|---|---|---|
| OTP | 6 chữ số, giá trị số từ 000000 đến 999999 | 1. 6 chữ số hợp lệ: 000000, 000001, 999998, 999999<br>2. Dưới biên: 5 chữ số<br>3. Trên biên: 7 chữ số | Vì OTP là chuỗi 6 chữ số ngẫu nhiên, biên chính là độ dài và miền giá trị 6 chữ số. Với dữ liệu thực tế, cần giữ đúng định dạng chuỗi để không mất số 0 ở đầu. |
| Mật khẩu mới | Độ dài tối thiểu 8 ký tự | 1. min-1: 7 ký tự<br>2. min: 8 ký tự<br>3. min+1: 9 ký tự | Đặc tả hiện chỉ nêu min, chưa nêu max nên chưa thể xác định max-1 / max / max+1 một cách chắc chắn. Ngoài độ dài, mật khẩu còn phải thỏa điều kiện chữ hoa, chữ thường, số, ký tự đặc biệt. |
| Xác nhận mật khẩu mới | Quan hệ logic: khớp với mật khẩu mới / không khớp | 1. Giá trị khớp chính xác<br>2. Giá trị sai lệch 1 ký tự<br>3. Giá trị hoàn toàn khác | Đây không phải biên số học, nên mình coi là biên logic để phục vụ BVA theo quan hệ phụ thuộc. |
| Email | Định dạng email hợp lệ và tồn tại trong hệ thống | Không có min/max số học rõ ràng | Email phù hợp hơn với Domain Testing vì đặc tả đang kiểm tra theo định dạng và tính tồn tại, chưa có range để áp BVA chuẩn. |

#### Step 6 — Kết xuất Bảng Test Case BVA

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|
| TC-BVA-01 | OTP dưới biên độ dài: 5 chữ số | Người dùng đã lấy OTP cho email hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `12345` | 1. Nhập email `nguyenvana@gmail.com` ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP `12345`<br>4. Nhập mật khẩu mới `Passw0rd@1`<br>5. Nhập xác nhận `Passw0rd@1`<br>6. Gửi form | Hệ thống từ chối vì OTP không đạt biên 6 chữ số |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-02 | OTP tại biên dưới: 6 chữ số nhỏ nhất hợp lệ | Có OTP được kiểm soát/seed trong môi trường test | Email: `nguyenvana@gmail.com`;<br>OTP: `000000` (lấy từ seed/demo) | 1. Nhập email `nguyenvana@gmail.com` ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP `000000`<br>4. Nhập mật khẩu mới `Passw0rd@1`<br>5. Nhập xác nhận `Passw0rd@1`<br>6. Gửi form | Hệ thống chấp nhận OTP nếu nó đúng với email đã yêu cầu và cho phép reset mật khẩu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-03 | OTP ngay trên biên dưới trong miền 6 chữ số | Có OTP được kiểm soát/seed trong môi trường test | Email: `nguyenvana@gmail.com`;<br>OTP: `000001` (lấy từ seed/demo) | 1. Nhập email `nguyenvana@gmail.com` ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP `000001`<br>4. Nhập mật khẩu mới `Passw0rd@1`<br>5. Nhập xác nhận `Passw0rd@1`<br>6. Gửi form | Hệ thống chấp nhận OTP nếu đúng với email đã yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-04 | OTP ngay dưới biên trên trong miền 6 chữ số | Có OTP được kiểm soát/seed trong môi trường test | Email: `nguyenvana@gmail.com`;<br>OTP: `999998` (lấy từ seed/demo) | 1. Nhập email `nguyenvana@gmail.com` ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP `999998`<br>4. Nhập mật khẩu mới `Passw0rd@1`<br>5. Nhập xác nhận `Passw0rd@1`<br>6. Gửi form | Hệ thống chấp nhận OTP nếu đúng với email đã yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-05 | OTP tại biên trên: 6 chữ số lớn nhất hợp lệ | Có OTP được kiểm soát/seed trong môi trường test | Email: `nguyenvana@gmail.com`;<br>OTP: `999999` (lấy từ seed/demo) | 1. Nhập email `nguyenvana@gmail.com` ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP `999999`<br>4. Nhập mật khẩu mới `Passw0rd@1`<br>5. Nhập xác nhận `Passw0rd@1`<br>6. Gửi form | Hệ thống chấp nhận OTP nếu đúng với email đã yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-06 | OTP trên biên độ dài: 7 chữ số | Người dùng đã lấy OTP cho email hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `1234567` | 1. Nhập email `nguyenvana@gmail.com` ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP `1234567`<br>4. Nhập mật khẩu mới `Passw0rd@1`<br>5. Nhập xác nhận `Passw0rd@1`<br>6. Gửi form | Hệ thống từ chối vì OTP vượt biên độ dài cho phép |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-07 | Mật khẩu mới dưới biên: 7 ký tự | Người dùng đã qua OTP hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `482913`;<br>Mật khẩu mới: `Pw0rd@1`;<br>Xác nhận: `Pw0rd@1` | 1. Nhập OTP `482913`<br>2. Nhập mật khẩu mới `Pw0rd@1`<br>3. Nhập xác nhận `Pw0rd@1`<br>4. Gửi form | Hệ thống từ chối vì mật khẩu dưới độ dài tối thiểu 8 ký tự |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-08 | Mật khẩu mới tại biên: 8 ký tự hợp lệ | Người dùng đã qua OTP hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `482913`;<br>Mật khẩu mới: `Passw0rd@1`;<br>Xác nhận: `Passw0rd@1` | 1. Nhập OTP `482913`<br>2. Nhập mật khẩu mới `Passw0rd@1`<br>3. Nhập xác nhận `Passw0rd@1`<br>4. Gửi form | Hệ thống chấp nhận mật khẩu mới nếu thỏa toàn bộ policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-09 | Mật khẩu mới trên biên: 9 ký tự hợp lệ | Người dùng đã qua OTP hợp lệ | Email: `nguyenvana@gmail.com`;<br>OTP: `482913`;<br>Mật khẩu mới: `Passw0rd@12`;<br>Xác nhận: `Passw0rd@12` | 1. Nhập OTP `482913`<br>2. Nhập mật khẩu mới `Passw0rd@12`<br>3. Nhập xác nhận `Passw0rd@12`<br>4. Gửi form | Hệ thống chấp nhận mật khẩu mới nếu thỏa toàn bộ policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |

Ghi chú trùng lặp:
- TC-BVA-08 và TC-BVA-09 trùng một phần mục tiêu với TC-01 ở Domain Testing nhưng vẫn giữ lại vì đại diện các điểm biên của mật khẩu.
- TC-BVA-02 đến TC-BVA-05 có thể trùng logic với TC-01 nếu cùng dữ liệu hợp lệ, nhưng vẫn giữ lại vì đại diện biên của OTP.
- TC-BVA-07 có thể trùng với TC-08 ở Domain Testing nhưng vẫn giữ lại vì đại diện giá trị ngay dưới biên độ dài mật khẩu.