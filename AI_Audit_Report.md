## Prompt 1

### AI tool name
* Gemini (Adaptive AI Collaborator)

### Date and time
* 2026-07-03 15:30:00

### Prompt

```text
giải thích cho tôi toàn bộ lí thuyết domain testing và bva
```

### The AI output

```text
Lý thuyết về Domain Testing và Boundary Value Analysis (BVA) đôi khi nghe có vẻ học thuật, nhưng thực chất nó là cách hệ thống hóa tư duy phân tích business logic và "bắt bug" mà các lập trình viên, QA, hay BA vẫn dùng hàng ngày.

Để dễ hình dung nhất cho bài tập trên hệ thống EShop, chúng ta sẽ đi từ định nghĩa đến ví dụ thực tế.

1. Domain Testing (Kiểm thử miền / Phân vùng tương đương)
Khái niệm cốt lõi:
Bạn không thể kiểm thử tất cả các giá trị đầu vào có thể có (ví dụ nhập tất cả các số từ 1 đến 1 tỷ vào ô số lượng hàng). Việc này là bất khả thi.

Thay vì thế, bạn chia các đầu vào thành các "miền" (domains) hay "vùng". Nguyên tắc của kỹ thuật này là: Nếu một hệ thống xử lý đúng (hoặc báo lỗi đúng) với một giá trị trong miền, thì nó cũng sẽ phản ứng y hệt với tất cả các giá trị khác trong miền đó.

Các loại miền:

Miền hợp lệ (Valid Domain): Những tập hợp giá trị mà hệ thống chấp nhận và cho đi tiếp.

Miền không hợp lệ (Invalid Domain): Những tập hợp giá trị mà hệ thống phải từ chối hoặc văng ra câu thông báo lỗi (Error message).

Ví dụ trên hệ thống EShop:
Tính năng "Tạo mã giảm giá" quy định: Phần trăm giảm giá phải từ 1 đến 100.

Miền hợp lệ: Các số nguyên từ 1 đến 100 (VD: 15, 50).

Miền không hợp lệ 1: Các số < 1 (VD: 0, -5).

Miền không hợp lệ 2: Các số > 100 (VD: 101, 500).

Thay vì test hàng trăm con số, bạn chỉ cần chọn 1 giá trị đại diện cho mỗi miền.

2. Boundary Value Analysis - BVA (Phân tích giá trị biên)
Khái niệm cốt lõi:
Lập trình viên rất hay sai sót ở các điều kiện so sánh (thường gặp nhất là nhầm lẫn giữa < và <=, hoặc > và >=). Do đó, lỗi thường không nằm ở khoảng giữa của các miền, mà nằm ở rìa (biên). BVA tập trung test ngay tại các ranh giới này.

Cách xác định các điểm cần test:
Với mỗi một ranh giới, bạn thường kiểm tra 3 điểm xung quanh nó (để đạt độ phủ cao nhất):

Tại biên (Boundary): Chính xác giá trị ranh giới.

Dưới biên (Just below): Giá trị gần nhất ngay bên dưới ranh giới.

Trên biên (Just above): Giá trị gần nhất ngay bên trên ranh giới.

Ví dụ áp dụng tiếp tục với "Phần trăm giảm giá (1 - 100)":

Ranh giới dưới (1):

Test giá trị 0 (Invalid - Dưới biên)

Test giá trị 1 (Valid - Tại biên Min)

Test giá trị 2 (Valid - Trên biên Min)

Ranh giới trên (100):

Test giá trị 99 (Valid - Dưới biên Max)

Test giá trị 100 (Valid - Tại biên Max)

Test giá trị 101 (Invalid - Trên biên Max)

3. Mối quan hệ giữa Domain và BVA
Hai kỹ thuật này là "đôi bạn cùng tiến" và thường được thực hiện liên tiếp nhau trong một Workflow (như yêu cầu của HW02):

Domain Testing giúp bạn khoanh vùng bức tranh toàn cảnh (Tạo ra các "thùng" chứa dữ liệu).

BVA giúp bạn nhắm chính xác vào các điểm dễ gãy nhất (Mép của các "thùng" đó).

Sau khi xác định được các giá trị từ BVA, bạn gom các giá trị hợp lệ lại thành các Test Case chuẩn (Happy Path) và các giá trị không hợp lệ thành các Test Case kiểm tra lỗi (Unhappy Path).

Để bắt đầu làm bài, bạn dự định chọn tính năng nào đầu tiên trong nhóm FR-01: Account registration để chúng ta thử xác định các biến đầu vào và áp dụng ngay hai lý thuyết này?
```

## Prompt 2

### AI tool name
* Gemini (Adaptive AI Collaborator)

### Date and time
* 2026-07-03 16:00:00

### Prompt

```text
domain testing là black box testing hả
```

### The AI output

```text
Chính xác! Domain Testing (và cả Boundary Value Analysis - BVA) chính là những kỹ thuật kinh điển nhất của Black Box Testing (Kiểm thử hộp đen).

Để mình giải thích ngắn gọn lý do tại sao:

Bản chất của "Hộp đen": Khi áp dụng Domain Testing, bạn hoàn toàn không cần biết code bên trong được viết bằng ngôn ngữ gì, thuật toán ra sao, hay các câu lệnh if/else được sắp xếp thế nào. Bạn coi tính năng đó (ví dụ nút Đăng ký) như một chiếc hộp kín.

Cách hoạt động: Bạn chỉ quan tâm đến Input (Dữ liệu bạn nhập vào, ví dụ: chuỗi email, mật khẩu) và Output (Kết quả trả về, ví dụ: Đăng ký thành công hoặc văng ra thông báo lỗi màu đỏ).

Mục tiêu: Bạn dùng Domain Testing để khôn khéo chọn ra một lượng nhỏ Input đại diện (thay vì phải test hàng ngàn trường hợp) để "tống" vào cái hộp đen đó, xem nó có phản ứng đúng như mô tả yêu cầu hay không.

Ngược lại với nó là White Box Testing (Kiểm thử hộp trắng), đòi hỏi bạn phải có source code, soi từng dòng lệnh, từng unit test (thường do chính Developer tự làm).

Vì bài tập HW02 yêu cầu bạn kiểm thử hệ thống EShop dựa trên các tính năng nghiệp vụ (FR), việc tiếp cận theo hướng Black Box bằng Domain Testing là hướng đi chuẩn xác mà giảng viên đang mong đợi.

Bạn đã sẵn sàng để chúng ta bắt đầu chạy thử Prompt 1: Phân tích Domain (Xác định Biến) cho tính năng FR-01: Account registration chưa? Mình sẽ hướng dẫn bạn cách "ra lệnh" cho AI mô phỏng lại luồng suy nghĩ này.
```

## Prompt 3

### AI tool name
* Claude (Sonnet 5)

### Date and time
* 2026-07-04 10:30:00

### Prompt

```text
Với yêu cầu bài tập này và các bước trong slide hướng dẫn về domain testing, hãy giúp tôi viết prompt cho Agent skill

#Role definition

#Workflow

Phase 1: ANALYSIS (BUSINESS ANALYST ROLE)

Phân tích danh sách các biến, điều kiện ràng buộc, prompt thêm giúp tôi những thứ khác cần thiết

Phase 2: DOMAIN TESTING

1. Thought process

2. viết yêu cầu sinh test case với các bước trong slide hướng dẫn

Bảng kết quả Test Case IDDescriptionPre-conditionInput DataTest StepsExpected ResultActual ResultStatusDefect IDTested ByDate Tested

Phase 3: BVA

1. thought process

2. viết bảng test case giống như Phase 2

Phase 4: audit log gồm tên, ngày tháng năm,  prompt, kết quả. Thiết kế Skill có "checkpoint" bắt buộc dừng

Quy tắc bắt buộc khi thực thi Skill này- KHÔNG BAO GIỜ thực hiện quá 1 step trong 1 lượt trả lời.- Sau khi hoàn thành Step N, PHẢI dừng lại và hỏi người dùng: "Bạn xác nhận Step N đúng chưa? Có cần sửa gì trước khi sang Step N+1 không?"- Chỉ được sang Step N+1 khi người dùng xác nhận rõ ràng (VD: "ok, tiếp tục").- Nếu người dùng không phản hồi xác nhận, Skill phải dừng, không tự suy diễn tiếp. 
```

### The AI output
```text
Mình đã dựa trên đề bài + phần draft của bạn (4 phase: Analysis → Domain Testing → BVA → Audit Log, cùng quy tắc checkpoint bắt buộc) để viết thành một SKILL.md hoàn chỉnh, có thể nạp thẳng vào Claude làm Agent Skill.
Điểm chính trong file:

Quy tắc checkpoint được đưa lên đầu và nhấn mạnh: mỗi lượt trả lời = đúng 1 Step, luôn dừng hỏi xác nhận, không tự suy diễn tiếp.
8 Step cụ thể thay vì 4 phase mơ hồ, để dễ áp dụng "1 step/1 lượt":

Phân tích nghiệp vụ (BA role) — liệt kê input/output/ràng buộc, kể cả ràng buộc AI tự suy luận thêm (đánh dấu [inferred])
Xác định Input & Output cho domain testing
Xác định Equivalence Classes (áp đúng các guideline: range, enum, "must be", tách EC khi nghi ngờ xử lý khác nhau)
Test Case Report cho Domain Testing (đúng bảng cột theo đề: Test Case ID, Description, Pre-condition, Input Data, Test Steps, Expected Result, Actual Result, Status, Defect ID, Tested By, Date Tested)
BVA thought process (chọn giá trị biên min-1/min/min+1...)
Test Case Report cho BVA
AI Gap Analysis (bắt buộc giải thích nguyên nhân: do prompt, do giới hạn AI, hay do độ phức tạp feature — đúng yêu cầu mục 6.3 của đề)
AI Audit Log entry theo đúng format mục 9 của đề


Mục "Những điều KHÔNG làm" để tránh vi phạm phần "AI-First strategy" và "Human review" của đề (không tự đánh Pass/Fail, không gộp step dù được yêu cầu làm nhanh).
Ghi chú áp dụng lặp lại cho từng feature trong 4 pool, và gợi ý commit message ngắn cho mục Git Commit Log.

Bạn có thể mở file, chỉnh sửa văn phong/tên skill nếu muốn, rồi lưu làm skill trong Claude (nút "Save skill" khi hiển thị file này), hoặc copy nội dung vào thư mục skill của bạn. Nếu muốn mình viết thêm phần test case mẫu (chạy thử skill trên 1 feature cụ thể, ví dụ FR-02 Login and account lockout) để bạn xem thử output thực tế, cứ nói nhé.
---
name: domain-bva-testing
description: Use this skill whenever the user wants to apply Domain Testing (Equivalence Partitioning) and Boundary Value Analysis (BVA) to a software feature or System Under Test — e.g. assignments like "HW02 Domain Testing", ISTQB-style black-box test design, or any request to "phân tích equivalence class", "thiết kế test case theo domain testing/BVA", "viết test case cho feature FR-xx". Guides the user step by step through Business-Analyst-style requirement analysis, Equivalence Class identification, test case selection, Boundary Value Analysis, AI gap analysis, and AI Audit Log generation — in strict, single-step-per-turn mode. Always trigger this skill instead of generating a full test suite in one shot when the user references domain testing, equivalence partitioning, or boundary value analysis.
---

# Domain Testing & Boundary Value Analysis Skill

## 0. Vai trò (Role definition)

Trong skill này, Claude đóng đồng thời 3 vai trò:

1. **Business Analyst** – đọc đặc tả / user story, làm rõ input, output, ràng buộc nghiệp vụ, và **chủ động đề xuất** những ràng buộc người dùng có thể đã bỏ sót.
2. **Test Analyst** – áp dụng đúng quy trình Domain Testing (Equivalence Partitioning) và Boundary Value Analysis theo giáo trình (không phải "generate test case" chung chung).
3. **Disciplined Assistant** – không tự ý làm tắt, không gộp nhiều bước, luôn dừng lại để người dùng xác nhận (xem Quy tắc checkpoint bên dưới). Đây là quy tắc quan trọng nhất của skill này và không được phá vỡ dù người dùng yêu cầu "làm nhanh gọn giúp mình" — trong trường hợp đó, hãy nhắc lại quy tắc và đề nghị rút gọn nội dung mỗi bước thay vì gộp bước.

## 1. QUY TẮC BẮT BUỘC — CHECKPOINT (đọc kỹ trước khi chạy skill)

- **KHÔNG BAO GIỜ** thực hiện quá **1 Step** (xem danh sách Step ở mục 3) trong **1 lượt trả lời**.
- Sau khi hoàn thành xong nội dung của 1 Step, **PHẢI dừng lại**, in ra đúng dòng đánh dấu `[DỪNG LẠI CHỜ XÁC NHẬN]`, rồi hỏi người dùng đúng dạng:
  > "✅ Đã hoàn thành **Step N: <tên step>**. Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang **Step N+1: <tên step tiếp theo>** không?
  >
  > `[DỪNG LẠI CHỜ XÁC NHẬN]`"
- Dòng `[DỪNG LẠI CHỜ XÁC NHẬN]` phải xuất hiện ở **cuối mỗi Step**, không được bỏ qua dù nội dung step ngắn hay dài.
- Chỉ được sang Step N+1 khi người dùng xác nhận rõ ràng (ví dụ: "ok", "tiếp tục", "đúng rồi", "next").
- Nếu người dùng phản hồi yêu cầu sửa, ở lượt kế tiếp chỉ sửa lại Step N (không tự ý sang Step N+1).
- Nếu người dùng không phản hồi xác nhận mà hỏi việc khác, Claude dừng ở Step hiện tại, không tự suy diễn tiếp.
- Ở đầu phiên làm việc (Step 0), Claude phải nói rõ cho người dùng biết tổng số Step sẽ đi qua cho feature này, để họ biết mình đang ở đâu trong quy trình.

## 2. Input cần thu thập trước khi bắt đầu (Step 0)

Trước khi vào Step 1, hỏi/xác nhận với người dùng:
- Tên & mã tính năng (ví dụ: FR-02 Login and account lockout)
- Đặc tả / mô tả nghiệp vụ của tính năng (đoạn text, link repo, hoặc file)
- Có ràng buộc nghiệp vụ nào đã biết trước không (range số lượt đăng nhập sai, độ dài password, v.v.)
- Tên AI tool đang dùng (mặc định: Claude) — để ghi vào AI Audit Log ở Step cuối

Nếu người dùng chưa cung cấp đặc tả, yêu cầu họ cung cấp trước khi chạy Step 1 (đây là ngoại lệ duy nhất được phép hỏi trước khi thực thi, vì thiếu input thì không thể phân tích).

`[DỪNG LẠI CHỜ XÁC NHẬN]`

## 3. Danh sách Step (mỗi Step = đúng 1 lượt trả lời)

### Step 1 — Phân tích nghiệp vụ (Business Analyst)
- Liệt kê toàn bộ **Input** và **Output** của tính năng.
- Liệt kê điều kiện / ràng buộc cho từng input (kiểu dữ liệu, range, format, enum, "must be" case, quan hệ giữa các field).
- Chủ động đề xuất thêm các ràng buộc/điều kiện hợp lý mà đặc tả có thể chưa nêu rõ (ví dụ: giới hạn ký tự username, case-sensitivity, khoảng thời gian lock account…), **đánh dấu rõ** đâu là ràng buộc lấy từ đặc tả gốc, đâu là ràng buộc Claude suy luận thêm (`[inferred]`).
- Trình bày dưới dạng bảng: `Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Nguồn (spec/inferred)`

`[DỪNG LẠI CHỜ XÁC NHẬN]`

### Step 2 — Xác định Input & Output cho Domain Testing
- Thu gọn lại danh sách ở Step 1 thành các input/output sẽ dùng để phân lớp tương đương (loại bỏ input không ảnh hưởng tới logic test).
- Xác nhận input nào độc lập, input nào có quan hệ ràng buộc lẫn nhau (cần lưu ý khi chọn test case ở Step 4).

`[DỪNG LẠI CHỜ XÁC NHẬN]`

### Step 3 — Xác định Equivalence Classes (Valid/Invalid)
Với từng input condition, áp dụng đúng guideline đã học:
- Range (VD "1 ≤ count ≤ 999") → 1 valid EC + 2 invalid EC (nhỏ hơn / lớn hơn).
- Tập giá trị rời rạc, mỗi giá trị được xử lý khác nhau (enum) → 1 valid EC/giá trị + 1 invalid EC chung.
- "Must be" case (VD "ký tự đầu phải là chữ") → 1 valid EC + 1 invalid EC.
- Nếu nghi ngờ các phần tử trong 1 EC không được xử lý giống nhau → tách nhỏ EC đó ra.
- Trình bày bảng: `EC ID | Input/Output | Điều kiện | Loại (Valid/Invalid) | Mô tả`

`[DỪNG LẠI CHỜ XÁC NHẬN]`

### Step 4 — Test Case Report (Domain Testing)
- Chọn tập test case tối thiểu:
  - Valid EC: gộp nhiều EC valid vào 1 test case cho tới khi tất cả valid EC được cover.
  - Invalid EC: mỗi test case chỉ cover đúng 1 invalid EC.
- Xuất bảng Test Case Report đúng format bắt buộc của đề bài:

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|

- Actual Result / Status / Defect ID để trống, ghi chú "cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại".

`[DỪNG LẠI CHỜ XÁC NHẬN]`

### Step 5 — BVA Thought Process
1. Xác định các biên (Boundaries) dựa trên các dải giá trị đã có từ **Step 1** (ràng buộc nghiệp vụ) và **Step 3** (Equivalence Classes).
2. Với mỗi biên, phân tích các giá trị cần test:
   - Giá trị ngay dưới biên dưới (min − 1)
   - Giá trị tại biên dưới (min)
   - Giá trị ngay trên biên dưới (min + 1)
   - Giá trị ngay dưới biên trên (max − 1)
   - Giá trị tại biên trên (max)
   - Giá trị ngay trên biên trên (max + 1)
   (kèm giá trị đặc biệt như 0, rỗng, null nếu áp dụng cho input đó).
3. Giải thích ngắn gọn vì sao các giá trị này được chọn, và xác nhận với người dùng cách tiếp cận: **single-fault BVA** (chỉ đẩy 1 input ra biên, các input khác giữ giá trị valid) hay **worst-case BVA** (nhiều input cùng ở biên) — hỏi người dùng nếu chưa rõ muốn dùng cách nào.
4. Trình bày dưới dạng bảng: `Input | Biên (min/max) | Giá trị test (min-1, min, min+1, max-1, max, max+1) | Ghi chú`

`[DỪNG LẠI CHỜ XÁC NHẬN]`

### Step 6 — Kết xuất Bảng Test Case BVA
1. Xuất bảng Test Case BVA theo **đúng cấu trúc bảng đã dùng ở Step 4** (Domain Testing Test Case Report):

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|

2. Mỗi test case phải bám theo các giá trị biên đã xác định ở Step 5 (min-1, min, min+1, max-1, max, max+1) và đúng cách tiếp cận (single-fault/worst-case) đã chốt ở Step 5.
3. **Đối chiếu với bảng Test Case ở Step 4**: nếu một test case BVA trùng (cùng input data / cùng mục tiêu kiểm thử) với test case Domain Testing đã có, **không loại bỏ ngầm** — vẫn liệt kê lại nhưng ghi chú rõ ở cột Description hoặc 1 cột phụ "Ghi chú trùng lặp": `Trùng với TCxx (Domain Testing) — giữ lại vì đại diện giá trị biên`.
4. Actual Result / Status / Defect ID để trống, ghi chú người dùng cần tự thực thi trên SUT rồi điền lại.

`[DỪNG LẠI CHỜ XÁC NHẬN]`

### Step 7 — AI Gap Analysis
- Đối chiếu lại toàn bộ EC/test case đã tạo ở Step 3–6 với đặc tả gốc.
- Liệt kê những test case/bug mà AI (Claude) đã bỏ sót nếu có, hoặc những điểm mơ hồ trong đặc tả khiến việc phân tích chưa chắc chắn.
- Với mỗi điểm thiếu sót, giải thích nguyên nhân theo 1 trong 3 nhóm: (a) do prompt/input người dùng cung cấp chưa đủ rõ, (b) do giới hạn của AI tool, (c) do độ phức tạp vốn có của tính năng.

`[DỪNG LẠI CHỜ XÁC NHẬN]`

### Step 8 — AI Audit Log Entry
- Tổng hợp lại toàn bộ tương tác trong phiên vừa rồi (Step 0–7) thành audit log đúng format:

```
### AI Interaction Log
- Tool: <tên AI tool>
- Date/time: <do người dùng cung cấp hoặc lấy theo hệ thống>
- Prompt: <tóm tắt prompt/instruction đã dùng ở mỗi step>
- Output: <tóm tắt output đã tạo>
```

- Nhắc người dùng: audit log này cần được gộp vào AI Audit Report chung của cả bài (mục 9 trong đề), và họ vẫn cần tự review + chỉnh sửa mọi output trước khi nộp — đây là yêu cầu bắt buộc của đề bài, Claude không thể tự "review thay".

`[DỪNG LẠI CHỜ XÁC NHẬN]` (xác nhận để kết thúc phiên cho feature này trước khi bắt đầu feature tiếp theo)


## 5. Những điều KHÔNG làm

- Không viết 1 prompt tổng quát kiểu "hãy tạo test case và tìm bug cho feature này" rồi trả kết quả luôn — vi phạm yêu cầu "AI-First strategy" của đề.
- Không tự động đánh dấu Actual Result / Status là "Pass" — đây là phần người dùng phải tự thực thi trên SUT.
- Không bỏ qua bước xác nhận (checkpoint) dù người dùng yêu cầu rút gọn — chỉ được rút gọn **nội dung**, không được gộp **số lượng Step**.

```
## Prompt 4
### AI tool name
* Claude (Sonnet 5)

### Date and time
* 2026-07-04 10:45:00

### Prompt
```text
chỉnh lại step bva ngoài thought process (- Xác định các biên (Boundaries) dựa trên các dải giá trị từ Phase 1 và Phase 2. - Phân tích các giá trị cần test tại mỗi biên: Giá trị ngay dưới biên (min - 1), giá trị tại biên (min), giá trị ngay trên biên (min + 1), giá trị ngay dưới biên trên (max - 1), giá trị tại biên trên (max), và giá trị ngay trên biên trên (max + 1).) phải có phần 2. Kết xuất Bảng Test Case BVA theo đúng cấu trúc bảng của Phase 2. Đảm bảo không trùng lặp các test case đã có ở Phase 2 (nếu có trùng, cần ghi chú rõ)
Với mỗi bước cần prompt thêm . -> [DỪNG LẠI CHỜ XÁC NHẬN]
```

### The AI output
```text
Giờ mình thêm marker [DỪNG LẠI CHỜ XÁC NHẬN] cho các Step còn lại (0–4, 7–8) để đồng bộ toàn bộ file.Edited 7 files, ran a command, read a fileEdited 7 files, ran a command, read a fileĐã cập nhật xong. Những thay đổi chính:

Step 5 (BVA Thought Process) giờ tách rõ 2 phần đúng như bạn yêu cầu:

Xác định biên dựa trên range từ Step 1 & Step 3, phân tích 6 giá trị test tại mỗi biên (min-1, min, min+1, max-1, max, max+1)
Chốt cách tiếp cận single-fault vs worst-case với người dùng


Step 6 (Kết xuất Bảng Test Case BVA) dùng đúng cấu trúc bảng như Step 4, và bắt buộc đối chiếu trùng lặp với test case Domain Testing — nếu trùng thì vẫn giữ lại nhưng ghi chú rõ (Trùng với TCxx (Domain Testing)...) thay vì âm thầm bỏ qua.
Mọi Step (0 → 8) giờ đều kết thúc bằng dòng đánh dấu literal `[DỪNG LẠI CHỜ XÁC NHẬN]` trước câu hỏi xác nhận, để mỗi step tự chứa đủ tín hiệu dừng ngay cả khi được đọc/dùng độc lập làm prompt.

Bạn xem lại file, nếu ổn thì mình có thể tiếp tục demo chạy thử skill này trên 1 feature cụ thể để bạn kiểm tra output thực tế trước khi nộp.
```

## Prompt 5

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
| TC-01 | Reset mật khẩu thành công với dữ liệu hợp lệ | Người dùng đã có email đăng ký hợp lệ; đang ở màn hình quên mật khẩu | Email hợp lệ; OTP đúng 6 chữ số và khớp email; Mật khẩu mới hợp lệ; Xác nhận mật khẩu khớp | 1. Nhập email hợp lệ ở Bước 1<br>2. Nhấn lấy OTP<br>3. Nhập OTP đúng<br>4. Nhập mật khẩu mới hợp lệ<br>5. Nhập xác nhận khớp<br>6. Gửi form | Hệ thống chấp nhận yêu cầu reset mật khẩu và hiển thị thành công |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-02 | Kiểm tra email hợp lệ và tồn tại khi lấy OTP | Người dùng đang ở Bước 1 | Email hợp lệ, tồn tại trong hệ thống | 1. Nhập email<br>2. Nhấn lấy OTP | Hệ thống sinh OTP 6 chữ số và hiển thị/gửi OTP theo môi trường demo |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-03 | Email sai định dạng | Người dùng đang ở Bước 1 | Email không đúng định dạng | 1. Nhập email sai định dạng<br>2. Nhấn lấy OTP | Hệ thống từ chối và hiển thị thông báo lỗi email không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-04 | Email hợp lệ nhưng không tồn tại | Người dùng đang ở Bước 1 | Email đúng định dạng nhưng không có trong hệ thống | 1. Nhập email không tồn tại<br>2. Nhấn lấy OTP | Hệ thống từ chối và hiển thị thông báo email không tồn tại |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-05 | OTP không đúng 6 chữ số | Người dùng đã lấy OTP ở Bước 1 | OTP 5 chữ số hoặc 7 chữ số hoặc chứa ký tự không phải số | 1. Nhập email hợp lệ<br>2. Sang Bước 2<br>3. Nhập OTP sai định dạng | Hệ thống từ chối và báo OTP không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-06 | OTP đúng định dạng nhưng sai giá trị | Người dùng đã lấy OTP ở Bước 1 | OTP đúng 6 chữ số nhưng không khớp mã đã sinh | 1. Nhập email hợp lệ<br>2. Sang Bước 2<br>3. Nhập OTP sai giá trị | Hệ thống từ chối và báo OTP không hợp lệ hoặc không khớp |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-07 | OTP hợp lệ nhưng thuộc email khác | Có ít nhất 2 email đã đăng ký | OTP đúng 6 chữ số nhưng được sinh cho email khác | 1. Lấy OTP cho email A<br>2. Sang Bước 2 của email B<br>3. Nhập OTP của email A | Hệ thống từ chối vì OTP không khớp với email đang yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-08 | Mật khẩu mới ngắn hơn 8 ký tự | Người dùng đã qua OTP hợp lệ | Mật khẩu mới dưới 8 ký tự; xác nhận khớp | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu mới ngắn hơn 8 ký tự<br>3. Nhập xác nhận khớp<br>4. Gửi form | Hệ thống từ chối và báo mật khẩu không đạt policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-09 | Mật khẩu mới thiếu ký tự đặc biệt | Người dùng đã qua OTP hợp lệ | Mật khẩu mới đủ độ dài nhưng không có ký tự đặc biệt hợp lệ | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu mới thiếu ký tự đặc biệt<br>3. Nhập xác nhận khớp<br>4. Gửi form | Hệ thống từ chối và báo mật khẩu không đạt policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-10 | Xác nhận mật khẩu không khớp | Người dùng đã qua OTP hợp lệ | Mật khẩu mới hợp lệ; xác nhận khác giá trị | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu mới hợp lệ<br>3. Nhập xác nhận khác mật khẩu mới<br>4. Gửi form | Hệ thống từ chối và báo hai mật khẩu không khớp |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |

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
| TC-BVA-01 | OTP dưới biên độ dài: 5 chữ số | Người dùng đã lấy OTP cho email hợp lệ | OTP = 12345; các trường khác hợp lệ | 1. Nhập email hợp lệ ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP 5 chữ số<br>4. Nhập mật khẩu mới hợp lệ<br>5. Nhập xác nhận khớp<br>6. Gửi form | Hệ thống từ chối vì OTP không đạt biên 6 chữ số |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-02 | OTP tại biên dưới: 6 chữ số nhỏ nhất hợp lệ | Có OTP được kiểm soát/seed trong môi trường test | OTP = 000000; các trường khác hợp lệ | 1. Nhập email hợp lệ ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP 000000<br>4. Nhập mật khẩu mới hợp lệ<br>5. Nhập xác nhận khớp<br>6. Gửi form | Hệ thống chấp nhận OTP nếu nó đúng với email đã yêu cầu và cho phép reset mật khẩu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-03 | OTP ngay trên biên dưới trong miền 6 chữ số | Có OTP được kiểm soát/seed trong môi trường test | OTP = 000001; các trường khác hợp lệ | 1. Nhập email hợp lệ ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP 000001<br>4. Nhập mật khẩu mới hợp lệ<br>5. Nhập xác nhận khớp<br>6. Gửi form | Hệ thống chấp nhận OTP nếu đúng với email đã yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-04 | OTP ngay dưới biên trên trong miền 6 chữ số | Có OTP được kiểm soát/seed trong môi trường test | OTP = 999998; các trường khác hợp lệ | 1. Nhập email hợp lệ ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP 999998<br>4. Nhập mật khẩu mới hợp lệ<br>5. Nhập xác nhận khớp<br>6. Gửi form | Hệ thống chấp nhận OTP nếu đúng với email đã yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-05 | OTP tại biên trên: 6 chữ số lớn nhất hợp lệ | Có OTP được kiểm soát/seed trong môi trường test | OTP = 999999; các trường khác hợp lệ | 1. Nhập email hợp lệ ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP 999999<br>4. Nhập mật khẩu mới hợp lệ<br>5. Nhập xác nhận khớp<br>6. Gửi form | Hệ thống chấp nhận OTP nếu đúng với email đã yêu cầu |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-06 | OTP trên biên độ dài: 7 chữ số | Người dùng đã lấy OTP cho email hợp lệ | OTP = 1234567; các trường khác hợp lệ | 1. Nhập email hợp lệ ở Bước 1<br>2. Sang Bước 2<br>3. Nhập OTP 7 chữ số<br>4. Nhập mật khẩu mới hợp lệ<br>5. Nhập xác nhận khớp<br>6. Gửi form | Hệ thống từ chối vì OTP vượt biên độ dài cho phép |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-07 | Mật khẩu mới dưới biên: 7 ký tự | Người dùng đã qua OTP hợp lệ | Mật khẩu mới = 7 ký tự; xác nhận khớp; OTP hợp lệ | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu mới 7 ký tự<br>3. Nhập xác nhận khớp<br>4. Gửi form | Hệ thống từ chối vì mật khẩu dưới độ dài tối thiểu 8 ký tự |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-08 | Mật khẩu mới tại biên: 8 ký tự hợp lệ | Người dùng đã qua OTP hợp lệ | Mật khẩu mới = 8 ký tự và đủ policy; xác nhận khớp | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu mới 8 ký tự hợp lệ<br>3. Nhập xác nhận khớp<br>4. Gửi form | Hệ thống chấp nhận mật khẩu mới nếu thỏa toàn bộ policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-09 | Mật khẩu mới trên biên: 9 ký tự hợp lệ | Người dùng đã qua OTP hợp lệ | Mật khẩu mới = 9 ký tự và đủ policy; xác nhận khớp | 1. Nhập OTP hợp lệ<br>2. Nhập mật khẩu mới 9 ký tự hợp lệ<br>3. Nhập xác nhận khớp<br>4. Gửi form | Hệ thống chấp nhận mật khẩu mới nếu thỏa toàn bộ policy |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |

Ghi chú trùng lặp:
- TC-BVA-08 và TC-BVA-09 trùng một phần mục tiêu với TC-01 ở Domain Testing nhưng vẫn giữ lại vì đại diện các điểm biên của mật khẩu.
- TC-BVA-02 đến TC-BVA-05 có thể trùng logic với TC-01 nếu cùng dữ liệu hợp lệ, nhưng vẫn giữ lại vì đại diện biên của OTP.
- TC-BVA-07 có thể trùng với TC-08 ở Domain Testing nhưng vẫn giữ lại vì đại diện giá trị ngay dưới biên độ dài mật khẩu.

## Prompt 6
# AI AUDIT REPORT (DỮ LIỆU KIỂM TOÁN HỆ THỐNG)

### AI tool name
* GPT-5.4 mini

### Date and time
* 2026-07-08 22:52:47

### Prompt
* **System Core Blueprint (Full Verbatim Content):**
  0. Vai trò (Role definition)

Trong skill này, hãy đóng đồng thời 3 vai trò:

Business Analyst – đọc đặc tả / user story, làm rõ input, output, ràng buộc nghiệp vụ, và chủ động đề xuất những ràng buộc người dùng có thể đã bỏ sót.
Test Analyst – áp dụng đúng quy trình Domain Testing (Equivalence Partitioning) và Boundary Value Analysis theo giáo trình (không phải "generate test case" chung chung).
Disciplined Assistant – không tự ý làm tắt, không gộp nhiều bước, luôn dừng lại để người dùng xác nhận (xem Quy tắc checkpoint bên dưới). Đây là quy tắc quan trọng nhất của skill này và không được phá vỡ dù người dùng yêu cầu "làm nhanh gọn giúp mình" — trong trường hợp đó, hãy nhắc lại quy tắc và đề nghị rút gọn nội dung mỗi bước thay vì gộp bước.

1. QUY TẮC BẮT BUỘC — CHECKPOINT (đọc kỹ trước khi chạy skill)

KHÔNG BAO GIỜ thực hiện quá 1 Step (xem danh sách Step ở mục 3) trong 1 lượt trả lời.
Sau khi hoàn thành xong nội dung của 1 Step, PHẢI dừng lại, in ra đúng dòng đánh dấu [DỪNG LẠI CHỜ XÁC NHẬN], rồi hỏi người dùng đúng dạng:

"✅ Đã hoàn thành Step N: <tên step>. Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step N+1: <tên step tiếp theo> không?
[DỪNG LẠI CHỜ XÁC NHẬN]"

Dòng [DỪNG LẠI CHỜ XÁC NHẬN] phải xuất hiện ở cuối mỗi Step, không được bỏ qua dù nội dung step ngắn hay dài.
Chỉ được sang Step N+1 khi người dùng xác nhận rõ ràng (ví dụ: "ok", "tiếp tục", "đúng rồi", "next").
Nếu người dùng phản hồi yêu cầu sửa, ở lượt kế tiếp chỉ sửa lại Step N (không tự ý sang Step N+1).
Nếu người dùng không phản hồi xác nhận mà hỏi việc khác, dừng ở Step hiện tại, không tự suy diễn tiếp.

2. Input cần thu thập trước khi bắt đầu (Step 0)

Trước khi vào Step 1, hỏi/xác nhận với người dùng:

Tên & mã tính năng (ví dụ: FR-02 Login and account lockout)
Đặc tả / mô tả nghiệp vụ của tính năng (đoạn text, link repo, hoặc file)
Có ràng buộc nghiệp vụ nào đã biết trước không (range số lượt đăng nhập sai, độ dài password, v.v.)
Tên AI tool đang dùng — để ghi vào AI Audit Log ở Step cuối

Nếu người dùng chưa cung cấp đặc tả, yêu cầu họ cung cấp trước khi chạy Step 1 (đây là ngoại lệ duy nhất được phép hỏi trước khi thực thi, vì thiếu input thì không thể phân tích).
[DỪNG LẠI CHỜ XÁC NHẬN]

3. Danh sách Step (mỗi Step = đúng 1 lượt trả lời)

## Step 1 — Phân tích nghiệp vụ (Business Analyst)

Liệt kê toàn bộ Input và Output của tính năng.
Liệt kê điều kiện / ràng buộc cho từng input (kiểu dữ liệu, range, format, enum, "must be" case, quan hệ giữa các field).
Đề xuất thêm các ràng buộc/điều kiện hợp lý mà đặc tả có thể chưa nêu rõ (ví dụ: giới hạn ký tự username, case-sensitivity, khoảng thời gian lock account…), đánh dấu rõ đâu là ràng buộc lấy từ đặc tả gốc, đâu là ràng buộc tự suy luận thêm ([inferred]).
Trình bày dưới dạng bảng, bắt buộc có cột giá trị mẫu cụ thể (không để trống, không viết "tuỳ ý"):
Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Giá trị mẫu hợp lệ cụ thể | Nguồn (spec/inferred)
Ví dụ cột "Giá trị mẫu hợp lệ cụ thể": với input username ràng buộc "6–20 ký tự alphanumeric" → ghi cụ thể nguyen_van_a01, không ghi chung chung "một chuỗi hợp lệ".
[DỪNG LẠI CHỜ XÁC NHẬN]

## Step 2 — Xác định Input & Output cho Domain Testing

Thu gọn lại danh sách ở Step 1 thành các input/output sẽ dùng để phân lớp tương đương (loại bỏ input không ảnh hưởng tới logic test).
Xác nhận input nào độc lập, input nào có quan hệ ràng buộc lẫn nhau (cần lưu ý khi chọn test case ở Step 4).
Với các input giữ lại, chốt sẵn 1 bộ "giá trị nền" (baseline data) — một bộ dữ liệu hợp lệ đầy đủ, cụ thể cho toàn bộ field của tính năng (ví dụ: 1 tài khoản mẫu hoàn chỉnh: username, password, email...). Bộ baseline này sẽ được tái sử dụng ở Step 4/Step 6: khi 1 test case chỉ tập trung kiểm tra 1 field, các field còn lại lấy nguyên giá trị baseline (hợp lệ) thay vì để trống hoặc ghi "bất kỳ".

[DỪNG LẠI CHỜ XÁC NHẬN]

## Step 3 — Xác định Equivalence Classes (Valid/Invalid)
Với từng input condition, áp dụng đúng guideline đã học:

Range (VD "1 ≤ count ≤ 999") → 1 valid EC + 2 invalid EC (nhỏ hơn / lớn hơn).
Tập giá trị rời rạc, mỗi giá trị được xử lý khác nhau (enum) → 1 valid EC/giá trị + 1 invalid EC chung.
"Must be" case (VD "ký tự đầu phải là chữ") → 1 valid EC + 1 invalid EC.
Nếu nghi ngờ các phần tử trong 1 EC không được xử lý giống nhau → tách nhỏ EC đó ra.
Trình bày bảng, bắt buộc có cột giá trị đại diện cụ thể cho từng EC — đây là cột quan trọng nhất để tránh test data chung chung:
EC ID | Input/Output | Điều kiện | Loại (Valid/Invalid) | Giá trị đại diện cụ thể | Mô tả
Quy tắc bắt buộc cho cột "Giá trị đại diện cụ thể":

Không được viết mô tả trừu tượng như "một giá trị không hợp lệ", "chuỗi bất kỳ", "số ngoài phạm vi" — phải điền con số/chuỗi thật, có thể copy-paste chạy ngay.
Nếu EC là "không phải số nguyên" → ghi cụ thể ví dụ "abc" hoặc 12.5, không ghi chung "không phải số".
Nếu EC là "vượt quá độ dài cho phép" → ghi rõ độ dài thực tế đã dùng, ví dụ nếu giới hạn là 20 ký tự thì ghi chuỗi 21 ký tự cụ thể (có thể viết "a" x 21 kèm chú thích số lượng ký tự).
Nếu EC là giá trị rỗng/null → ghi rõ literal: "" (chuỗi rỗng), NULL, hoặc (không nhập gì) — không gộp chung với các loại invalid khác.
Nếu EC là ký tự đặc biệt/không hợp lệ về định dạng → liệt kê rõ tập ký tự dùng để test, ví dụ "!@#$%^&*()".


[DỪNG LẠI CHỜ XÁC NHẬN]

## Step 4 — Test Case Report (Domain Testing)

KHÔNG có giới hạn số lượng test case. Số lượng test case PHẢI bằng đúng
+số equivalence class hợp lệ tìm được — không được gộp nhiều class vào
+1 test case, không được bỏ sót class nào, và không được tự ý dừng sớm
+vì lý do "đã đủ" hay "để tránh dài".


+Valid EC: gộp nhiều EC valid vào 1 test case cho tới khi tất cả valid EC được cover.
+Invalid EC: mỗi test case chỉ cover đúng 1 invalid EC.
+
+
+Xuất bảng Test Case Report đúng format bắt buộc của đề bài:
+
+Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested
+
+Quy tắc bắt buộc cho cột "Input Data" (test data) — áp dụng cho cả Step 4 và Step 6, đây là phần bắt buộc phải rõ ràng, không chung chung:
+
+Lấy đúng giá trị cụ thể đã chốt ở bảng EC (Step 3) / bảng biên (Step 5) cho field đang test — không tự bịa giá trị mới khác với bảng đã chốt.
+Input Data phải liệt kê đầy đủ tất cả field liên quan của tính năng cho 1 lần thực thi, không chỉ field đang là trọng tâm test. Các field không phải trọng tâm lấy nguyên giá trị baseline hợp lệ đã chốt ở Step 2 (ví dụ test biên độ dài password, thì username/email trong cùng test case vẫn phải có giá trị hợp lệ cụ thể, không để trống hoặc ghi "bất kỳ").
+Dùng thẻ <br> để tách các field trong cùng 1 ô, theo định dạng Tên field: giá trị, ví dụ:
+username: nguyen_van_a01<br>password: Abc@12345<br>email: test01@gmail.com
+Dữ liệu phải đúng định dạng thực tế của loại field (email dạng abc@domain.com, số điện thoại VN 10 số bắt đầu bằng 0, ngày dạng dd/mm/yyyy, v.v.) — không dùng placeholder kiểu <email>, <string>.
+Cấm dùng các cụm mơ hồ sau trong cột Input Data: "giá trị hợp lệ", "giá trị không hợp lệ", "một chuỗi bất kỳ", "số bất kỳ ngoài phạm vi", "dữ liệu ngẫu nhiên", "N/A" (trừ khi field đó thực sự không áp dụng cho tính năng).
+Nếu 2 test case khác nhau vô tình dùng cùng 1 bộ Input Data → phải rà soát lại, vì đây là dấu hiệu 2 EC/test case đang trùng lặp thật sự.
+
+Actual Result / Status / Defect ID để trống, ghi chú "cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại".
+
+[DỪNG LẠI CHỜ XÁC NHẬN]
+
+## Step 5 — BVA Thought Process
+
+Xác định các biên (Boundaries) dựa trên các dải giá trị đã có từ Step 1 (ràng buộc nghiệp vụ) và Step 3 (Equivalence Classes).
+Với mỗi biên, phân tích các giá trị cần test:
+
+Giá trị ngay dưới biên dưới (min − 1)
+Giá trị tại biên dưới (min)
+Giá trị ngay trên biên dưới (min + 1)
+Giá trị ngay dưới biên trên (max − 1)
+Giá trị tại biên trên (max)
+Giá trị ngay trên biên trên (max + 1)
+(kèm giá trị đặc biệt như 0, rỗng, null nếu áp dụng cho input đó).
+
+
+Giải thích vì sao các giá trị này được chọn, và xác nhận với người dùng cách tiếp cận: single-fault BVA (chỉ đẩy 1 input ra biên, các input khác giữ giá trị valid) hay worst-case BVA (nhiều input cùng ở biên) — hỏi người dùng nếu chưa rõ muốn dùng cách nào.
+Trình bày dưới dạng bảng, cột giá trị test phải là số/chuỗi thật, không phải công thức: Input | Biên (min/max) | Giá trị test (min-1, min, min+1, max-1, max, max+1) | Ghi chú
+
+[DỪNG LẠI CHỜ XÁC NHẬN]
+
+## Step 6 — Kết xuất Bảng Test Case BVA
+
+Xuất bảng Test Case BVA theo đúng cấu trúc bảng đã dùng ở Step 4 (Domain Testing Test Case Report):
+
+Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested
+
+Mỗi test case phải bám theo các giá trị biên đã xác định ở Step 5 (min-1, min, min+1, max-1, max, max+1) và đúng cách tiếp cận (single-fault/worst-case) đã chốt ở Step 5.
+Đối chiếu với bảng Test Case ở Step 4: nếu một test case BVA trùng (cùng input data / cùng mục tiêu kiểm thử) với test case Domain Testing đã có, không loại bỏ ngầm — vẫn liệt kê lại nhưng ghi chú rõ ở cột Description hoặc 1 cột phụ "Ghi chú trùng lặp": Trùng với TCxx (Domain Testing) — giữ lại vì đại diện giá trị biên.
+Actual Result / Status / Defect ID để trống, ghi chú người dùng cần tự thực thi trên SUT rồi điền lại.
+
+[DỪNG LẠI CHỜ XÁC NHẬN]
+
+## Step 7 — AI Audit Log Entry
+
+Tổng hợp lại toàn bộ nội dung đã tạo ra trong phiên (Step 1 → Step 6) và tạo ra thực sự 2 file Markdown (không chỉ hiển thị trong khung chat) cho feature đang xử lý, mã feature = FXX (ví dụ FR02):
+File 1 — FXX_report.md (Báo cáo nội dung kiểm thử), phải theo đúng khung sau:
+# BÁO CÁO KIỂM THỬ - <Tên feature> (FXX)
+
+## I. DOMAIN TESTING
+
+### 1. DETAILED STEP-BY-STEP DOMAIN ANALYSIS
+(Chèn toàn bộ phần giải trình tư duy phân tích các miền tương đương Hợp lệ/Không hợp lệ — lấy nguyên nội dung đã chốt ở Step 1 và Step 3, viết chi tiết, mạch lạc bằng tiếng Việt, không tóm tắt qua loa)
+
+### 2. DOMAIN TEST CASES TABLE
+(Chèn bảng test case đầy đủ từ Step 4 — tối thiểu 8–12 kịch bản. Trong các ô Test Steps/Expected Result có nhiều dòng, dùng thẻ `<br>` để xuống dòng thay vì tạo dòng bảng mới)
+
+## II. BOUNDARY VALUE ANALYSIS (BVA)
+
+### 1. DETAILED STEP-BY-STEP BOUNDARY ANALYSIS
+(Chèn toàn bộ phần giải trình tư duy phân tích toán học các điểm biên Boundary, Boundary−1, Boundary+1 — lấy nguyên nội dung đã chốt ở Step 5, viết chi tiết bằng tiếng Việt)
+
+### 2. BOUNDARY TEST CASES TABLE
+(Chèn bảng test case biên đầy đủ từ Step 6 — tối thiểu 6–9 kịch bản, cùng quy tắc dùng thẻ `<br>` như trên)
+Yêu cầu bắt buộc cho File 1:
+
+Nếu số test case ở Step 4 chưa đủ 8–12 kịch bản, hoặc Step 6 chưa đủ 6–9 kịch bản, phải quay lại bổ sung trước khi xuất file (báo cho người dùng biết đang bổ sung thêm bao nhiêu case và vì sao).
+Giữ nguyên cấu trúc cột bảng đã dùng ở Step 4/Step 6; chỉ được thêm <br> bên trong ô, không đổi số cột.
+Không được rút gọn phần giải trình tư duy (mục "1.") thành gạch đầu dòng sơ sài — đây là phần thể hiện năng lực phân tích, cần viết như một bài giải trình thực sự.
+
+File 2 — FRXX_AI_Audit_Log.md (AI Audit Report), phải theo đúng khung sau:
+# AI AUDIT REPORT (DỮ LIỆU KIỂM TOÁN HỆ THỐNG)
+
+### AI tool name
+* <Tên model/AI tool đang thực thi skill này>
+
+### Date and time
+* <Ngày giờ chạy thực tế của phiên làm việc>
+
+### Prompt
+* **System Core Blueprint (Full Verbatim Content):**
+  <Chèn nguyên văn toàn bộ nội dung file SKILL.md này — từ mục "0. Vai trò (Role definition)" đến hết — làm bằng chứng kiểm toán đầy đủ, không tóm tắt>
+* **Input Feature Specification Used:**
+  <Chèn nguyên văn toàn bộ đặc tả thô ban đầu mà người dùng đã cung cấp ở Step 0>
+
+### The AI output
+<Chèn toàn bộ nội dung đã sinh ra từ Step 1 đến Step 6. Đây phải là bản dán đầy đủ (full copy), KHÔNG được viết dưới dạng link sang file khác (kể cả FXX_report.md), vì audit log cần là bằng chứng độc lập, tự đầy đủ.>
+Yêu cầu bắt buộc cho File 2:
+
+Không rút gọn, không paraphrase lại nội dung Step 1–6 khi dán vào "The AI output" — dán nguyên văn những gì đã được người dùng xác nhận qua các checkpoint.
+Không chèn link nội bộ trỏ sang FXX_report.md hoặc bất kỳ file nào khác trong mục này.

+
+* **Input Feature Specification Used:**
+  ### FR-11: Xem lịch sử đơn hàng (User)
+
+  - Người dùng chỉ xem được đơn hàng của chính mình.
+  - Hiển thị: Mã đơn, Ngày đặt, Tổng tiền, Trạng thái hiện tại.
+  - Trạng thái phải được dịch sang tiếng Việt rõ ràng và phân biệt màu sắc.                  [Admin xác nhận]          [Admin giao hàng]      [Admin hoàn tất]
+  ┌──────────┐ ─────────────────► ┌───────────┐ ──────────────► ┌──────────┐ ──────────► ┌───────────┐
+  │ pending  │                    │ confirmed │                  │ shipping │             │ delivered │
+  └──────────┘                    └───────────┘                  └──────────┘             └───────────┘
+       │                               │
+       │ [User/Admin hủy]              │ [User/Admin hủy]
+       ▼                               ▼
+  ┌──────────┐                    ┌──────────┐
+  │ canceled │                    │ canceled │
+  └──────────┘                    └──────────┘
+
+### The AI output
+Step 1 — Phân tích nghiệp vụ
+
+Phạm vi mình giữ cho Step 1 là đúng theo đặc tả bạn đưa: màn hình lịch sử đơn hàng của user, chỉ hiển thị đơn của chính user đang đăng nhập, với 4 thông tin hiển thị chính là mã đơn, ngày đặt, tổng tiền và trạng thái hiện tại. Mình không đưa thêm input như lọc, sắp xếp hay phân trang vì đặc tả chưa nêu, nên hiện tại chưa coi đó là miền cần kiểm thử.
+
+| Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Giá trị mẫu hợp lệ cụ thể | Nguồn (spec/inferred) |
+|---|---|---|---|---|---|
+| current_user_id | Input | string / UUID | Phải là user đã xác thực; chỉ được xem dữ liệu thuộc đúng user này | usr_1042 | spec + inferred |
+| order_owner_id | Input | string / UUID | Phải trùng với current_user_id thì đơn mới được phép hiển thị | usr_1042 | spec |
+| order_code | Output | string | Không rỗng; là mã đơn duy nhất; hiển thị nguyên trạng, không cho sửa | DH20260708-001 | spec + inferred |
+| order_date | Output | date / datetime hiển thị | Ngày đặt hợp lệ; phải là ngày tồn tại; khi hiển thị nên theo dạng ngày rõ ràng cho user | 08/07/2026 | spec + inferred |
+| total_amount | Output | number / decimal | Giá trị tiền hợp lệ, không âm; hiển thị theo đơn vị tiền của hệ thống | 1250000 | spec + inferred |
+| current_status_code | Input | enum | Chỉ nhận một trong các trạng thái nghiệp vụ: pending, confirmed, shipping, delivered, canceled | shipping | spec |
+| status_display_text_vn | Output | string | Phải được dịch sang tiếng Việt rõ nghĩa, không để nguyên tiếng Anh | Đang giao hàng | spec |
+| status_color | Output | string / color token | Mỗi trạng thái phải có màu phân biệt rõ với các trạng thái khác | orange | spec + inferred |
+| visible_order_list | Output | list / array | Chỉ chứa các đơn có order_owner_id = current_user_id; không hiển thị đơn của user khác | DH20260708-001, DH20260708-002 | spec |
+| empty_state_message | Output | string | Chỉ dùng khi user không có đơn hàng nào; hiển thị trạng thái rỗng rõ ràng | Bạn chưa có đơn hàng nào. | inferred |
+
+Quy ước hiển thị trạng thái mình chốt theo đặc tả và luồng nghiệp vụ bạn đưa là: `pending` → `Chờ xác nhận`, `confirmed` → `Đã xác nhận`, `shipping` → `Đang giao hàng`, `delivered` → `Đã giao hàng`, `canceled` → `Đã hủy`. Màu sắc phải là các màu phân biệt rõ nhau giữa các trạng thái, nên ở bước sau mình sẽ coi đó là một thuộc tính output riêng để phân lớp test.
+
+✅ Đã hoàn thành Step 1: Phân tích nghiệp vụ. Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step 2: Xác định Input & Output cho Domain Testing không?
+[DỪNG LẠI CHỜ XÁC NHẬN]
+
+Step 2 — Xác định Input & Output cho Domain Testing
+
+Ở bước này mình chỉ giữ những biến thật sự ảnh hưởng đến logic kiểm thử của FR-11. Các biến như `order_code`, `order_date`, `total_amount`, `current_status_code`, `status_display_text_vn`, `status_color`, `visible_order_list` là phần output cần quan sát, còn input trọng tâm để phân lớp tương đương chủ yếu là quyền sở hữu đơn hàng và trạng thái đơn hàng.
+
+| Biến | Vai trò (Input/Output) | Ghi nhận cho Domain Testing | Quan hệ / phụ thuộc | Giá trị baseline hợp lệ cụ thể |
+|---|---|---|---|---|
+| current_user_id | Input | Giữ lại | Là user đang đăng nhập; dùng để so khớp với chủ đơn | `usr_1042` |
+| order_owner_id | Input | Giữ lại | Phải trùng với `current_user_id` thì đơn mới được hiển thị | `usr_1042` |
+| order_code | Output | Giữ lại | Chỉ là dữ liệu hiển thị, không phân lớp theo logic nhập | `DH20260708-001` |
+| order_date | Output | Giữ lại | Chỉ là dữ liệu hiển thị, phải đúng định dạng ngày | `08/07/2026` |
+| total_amount | Output | Giữ lại | Chỉ là dữ liệu hiển thị tiền | `1250000` |
+| current_status_code | Input | Giữ lại | Quyết định trạng thái hiển thị tiếng Việt và màu sắc | `shipping` |
+| status_display_text_vn | Output | Giữ lại | Phụ thuộc vào `current_status_code` | `Đang giao hàng` |
+| status_color | Output | Giữ lại | Phụ thuộc vào `current_status_code` | `orange` |
+| visible_order_list | Output | Giữ lại | Kết quả cuối của logic lọc theo user | `DH20260708-001, DH20260708-002` |
+| empty_state_message | Output | Giữ lại | Chỉ xuất hiện khi danh sách rỗng | `Bạn chưa có đơn hàng nào.` |
+
+Bộ baseline data mình sẽ dùng xuyên suốt từ đây:
+- User đang đăng nhập: `usr_1042`
+- Đơn hàng hợp lệ thuộc chính user: `DH20260708-001`
+- Ngày đặt: `08/07/2026`
+- Tổng tiền: `1250000`
+- Trạng thái nghiệp vụ mẫu: `shipping`
+- Trạng thái hiển thị tiếng Việt tương ứng: `Đang giao hàng`
+- Màu trạng thái mẫu: `orange`
+
+Quan hệ cần lưu ý cho các bước sau:
+- `order_owner_id` phải bằng `current_user_id` nếu muốn đơn xuất hiện trong lịch sử.
+- `current_status_code` quyết định đồng thời `status_display_text_vn` và `status_color`.
+- `visible_order_list` là output tổng hợp, nên khi test 1 trường vẫn phải giữ các trường còn lại theo baseline hợp lệ.
+
+✅ Đã hoàn thành Step 2: Xác định Input & Output cho Domain Testing. Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step 3: Xác định Equivalence Classes (Valid/Invalid) không?
+[DỪNG LẠI CHỜ XÁC NHẬN]
+
+Step 3 — Xác định Equivalence Classes (Valid/Invalid)
+
+Mình chỉ tách các miền tương đương cho những điều kiện có ảnh hưởng trực tiếp đến logic xem lịch sử đơn hàng: quyền sở hữu đơn và trạng thái đơn hàng. Các cột output như mã đơn, ngày đặt, tổng tiền, màu sắc và text tiếng Việt sẽ được kiểm tra qua kết quả hiển thị tương ứng của các miền input này.
+
+| EC ID | Input/Output | Điều kiện | Loại (Valid/Invalid) | Giá trị đại diện cụ thể | Mô tả |
+|---|---|---|---|---|---|
+| EC-01 | current_user_id / order_owner_id | order_owner_id = current_user_id | Valid | usr_1042 / usr_1042 | Đơn thuộc đúng user đang đăng nhập nên được hiển thị |
+| EC-02 | current_user_id / order_owner_id | order_owner_id khác current_user_id | Invalid | usr_1042 / usr_2048 | Đơn của người khác, không được xuất hiện trong lịch sử của user hiện tại |
+| EC-03 | current_status_code | Trạng thái pending | Valid | pending | Đơn ở trạng thái chờ xác nhận, phải hiển thị đúng nhãn tiếng Việt và màu tương ứng |
+| EC-04 | current_status_code | Trạng thái confirmed | Valid | confirmed | Đơn đã được xác nhận, phải hiển thị đúng nhãn tiếng Việt và màu tương ứng |
+| EC-05 | current_status_code | Trạng thái shipping | Valid | shipping | Đơn đang giao hàng, phải hiển thị đúng nhãn tiếng Việt và màu tương ứng |
+| EC-06 | current_status_code | Trạng thái delivered | Valid | delivered | Đơn đã giao hàng, phải hiển thị đúng nhãn tiếng Việt và màu tương ứng |
+| EC-07 | current_status_code | Trạng thái canceled | Valid | canceled | Đơn đã hủy, phải hiển thị đúng nhãn tiếng Việt và màu tương ứng |
+| EC-08 | current_status_code | Giá trị khác enum cho phép | Invalid | returned | Trạng thái không hợp lệ, hệ thống không được dịch như trạng thái hợp lệ |
+| EC-09 | current_status_code | Giá trị rỗng | Invalid | "" | Không có trạng thái, phải bị chặn hoặc xử lý lỗi dữ liệu |
+| EC-10 | visible_order_list | User có ít nhất 1 đơn của chính mình | Valid | DH20260708-001 | Danh sách lịch sử có dữ liệu và hiển thị đúng các cột |
+| EC-11 | visible_order_list | User không có đơn nào | Valid | [] | Hiển thị trạng thái rỗng rõ ràng, không hiển thị đơn của người khác |
+
+Kết luận phân lớp:
+- Hai input độc lập cần kiểm thử là quyền sở hữu đơn và trạng thái đơn hàng.
+- `order_owner_id` phụ thuộc trực tiếp vào `current_user_id`.
+- `current_status_code` là miền rời rạc, nên mỗi trạng thái hợp lệ được tách riêng thành một EC valid.
+- Trường rỗng và giá trị ngoài enum được giữ thành EC invalid riêng để tránh gộp chung.
+
+✅ Đã hoàn thành Step 3: Xác định Equivalence Classes (Valid/Invalid). Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step 4: Test Case Report (Domain Testing) không?
+[DỪNG LẠI CHỜ XÁC NHẬN]
+
+Step 4 — Test Case Report (Domain Testing)
+
+Mình đã giữ đúng baseline hợp lệ từ Step 2 và tách test case theo các equivalence class ở Step 3. Phần Actual Result, Status, Defect ID mình để trống đúng theo quy tắc; bạn sẽ điền khi thực thi trên SUT.
+
+| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
+|---|---|---|---|---|---|---|---|---|---|---|
+| TC-01 | Hiển thị 1 đơn của chính user với trạng thái pending | User usr_1042 đang đăng nhập và có ít nhất 1 đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-001<br>order_date: 08/07/2026<br>total_amount: 1250000<br>order_owner_id: usr_1042<br>current_status_code: pending | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát dòng đơn hàng DH20260708-001 | Chỉ hiển thị đơn của usr_1042; đơn có mã DH20260708-001 xuất hiện với ngày đặt 08/07/2026, tổng tiền 1250000, trạng thái tiếng Việt tương ứng với pending và có màu trạng thái đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-02 | Hiển thị 1 đơn của chính user với trạng thái confirmed | User usr_1042 đang đăng nhập và có đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-002<br>order_date: 07/07/2026<br>total_amount: 980000<br>order_owner_id: usr_1042<br>current_status_code: confirmed | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát dòng đơn hàng DH20260708-002 | Chỉ hiển thị đơn của usr_1042; đơn có mã DH20260708-002 xuất hiện với ngày đặt 07/07/2026, tổng tiền 980000, trạng thái tiếng Việt tương ứng với confirmed và có màu trạng thái đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-03 | Hiển thị 1 đơn của chính user với trạng thái shipping | User usr_1042 đang đăng nhập và có đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-003<br>order_date: 06/07/2026<br>total_amount: 450000<br>order_owner_id: usr_1042<br>current_status_code: shipping | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát dòng đơn hàng DH20260708-003 | Chỉ hiển thị đơn của usr_1042; đơn có mã DH20260708-003 xuất hiện với ngày đặt 06/07/2026, tổng tiền 450000, trạng thái tiếng Việt tương ứng với shipping và có màu trạng thái đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-04 | Hiển thị 1 đơn của chính user với trạng thái delivered | User usr_1042 đang đăng nhập và có đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-004<br>order_date: 05/07/2026<br>total_amount: 2100000<br>order_owner_id: usr_1042<br>current_status_code: delivered | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát dòng đơn hàng DH20260708-004 | Chỉ hiển thị đơn của usr_1042; đơn có mã DH20260708-004 xuất hiện với ngày đặt 05/07/2026, tổng tiền 2100000, trạng thái tiếng Việt tương ứng với delivered và có màu trạng thái đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-05 | Hiển thị 1 đơn của chính user với trạng thái canceled | User usr_1042 đang đăng nhập và có đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-005<br>order_date: 04/07/2026<br>total_amount: 300000<br>order_owner_id: usr_1042<br>current_status_code: canceled | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát dòng đơn hàng DH20260708-005 | Chỉ hiển thị đơn của usr_1042; đơn có mã DH20260708-005 xuất hiện với ngày đặt 04/07/2026, tổng tiền 300000, trạng thái tiếng Việt tương ứng với canceled và có màu trạng thái đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-06 | Hiển thị danh sách có nhiều đơn đều thuộc chính user | User usr_1042 đang đăng nhập và có nhiều đơn của chính mình | current_user_id: usr_1042<br>order_code: DH20260708-006A, DH20260708-006B<br>order_date: 03/07/2026, 03/07/2026<br>total_amount: 760000, 125000<br>order_owner_id: usr_1042, usr_1042<br>current_status_code: shipping, pending | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát danh sách đơn hàng | Hệ thống hiển thị nhiều đơn thuộc usr_1042; không bỏ sót đơn nào trong tập dữ liệu của user; mỗi dòng hiển thị đúng mã đơn, ngày đặt, tổng tiền, trạng thái tiếng Việt và màu tương ứng |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-07 | Hiển thị đúng 1 đơn của chính user trong danh sách | User usr_1042 đang đăng nhập và có đúng 1 đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-007<br>order_date: 02/07/2026<br>total_amount: 1500000<br>order_owner_id: usr_1042<br>current_status_code: confirmed | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát số lượng dòng hiển thị | Hệ thống hiển thị đúng 1 đơn của usr_1042; các cột mã đơn, ngày đặt, tổng tiền, trạng thái hiện tại đều có dữ liệu đúng và không sinh thêm đơn giả |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-08 | User không có đơn hàng nào nên hiển thị trạng thái rỗng | User usr_1042 đang đăng nhập và không có đơn nào thuộc chính mình | current_user_id: usr_1042<br>orders_in_system: [] | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát vùng danh sách | Không hiển thị đơn hàng nào; hệ thống hiển thị empty state rõ ràng, không hiển thị đơn của user khác |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-09 | Đơn của user khác không được hiển thị | User usr_1042 đang đăng nhập; hệ thống có đơn thuộc user khác | current_user_id: usr_1042<br>order_code: DH20260708-009<br>order_date: 01/07/2026<br>total_amount: 760000<br>order_owner_id: usr_2048<br>current_status_code: shipping | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Tìm đơn DH20260708-009 | Đơn DH20260708-009 không được hiển thị trong lịch sử của usr_1042 vì owner khác user hiện tại |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-10 | Trạng thái không hợp lệ ngoài enum | User usr_1042 đang đăng nhập và có một đơn trả về trạng thái ngoài danh sách cho phép | current_user_id: usr_1042<br>order_code: DH20260708-010<br>order_date: 30/06/2026<br>total_amount: 520000<br>order_owner_id: usr_1042<br>current_status_code: returned | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát đơn DH20260708-010 | Hệ thống không được hiển thị trạng thái returned như một trạng thái hợp lệ; phải chặn dữ liệu sai hoặc hiển thị xử lý lỗi/fallback rõ ràng theo thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-11 | Trạng thái rỗng không hợp lệ | User usr_1042 đang đăng nhập và có một đơn bị thiếu trạng thái | current_user_id: usr_1042<br>order_code: DH20260708-011<br>order_date: 29/06/2026<br>total_amount: 640000<br>order_owner_id: usr_1042<br>current_status_code: "" | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát đơn DH20260708-011 | Hệ thống không được coi trạng thái rỗng là trạng thái hợp lệ; phải chặn, báo lỗi dữ liệu hoặc hiển thị fallback theo thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+
+## II. BOUNDARY VALUE ANALYSIS (BVA)
+
+### 1. DETAILED STEP-BY-STEP BOUNDARY ANALYSIS
+
+Với FR-11, biên kiểm thử chủ yếu là biên logic chứ không phải biên số học thuần túy. Tính năng này không có input kiểu min/max như số lượng hay độ dài mật khẩu, nhưng vẫn có các điểm gãy rất rõ ở mức nghiệp vụ. Điểm biên quan trọng nhất là chuyển từ không có đơn sang có một đơn, rồi từ một đơn sang nhiều đơn. Đây là nơi giao diện lịch sử đơn hàng dễ lỗi nhất: empty state có thể không hiển thị đúng, danh sách có thể render sai số dòng, hoặc lọc đơn có thể lọt dữ liệu của user khác.
+
+Biên thứ hai là quan hệ sở hữu đơn hàng. Một đơn chỉ được hiển thị nếu `order_owner_id` trùng với `current_user_id`. Đây là biên logic nhị phân, vì chỉ có hai trạng thái: khớp và không khớp. Nếu hệ thống xử lý sai biên này, user có thể thấy dữ liệu của người khác, gây lỗi bảo mật và dữ liệu.
+
+Biên thứ ba là vòng đời trạng thái đơn hàng. Trạng thái `pending`, `confirmed`, `shipping`, `delivered`, `canceled` là các điểm nghiệp vụ mà hệ thống phải map sang tiếng Việt và màu sắc khác nhau. Ở đây, giá trị biên không phải là con số mà là các trạng thái đầu, giữa và cuối của luồng xử lý. Nếu map sai ở bất kỳ điểm nào, user sẽ nhìn thấy nhãn hoặc màu không đúng, làm sai nghĩa nghiệp vụ.
+
+Mình dùng single-fault BVA để cô lập lỗi. Mỗi test case chỉ đẩy một biên chính, các dữ liệu còn lại giữ ở mức hợp lệ. Cách này phù hợp với bài toán FR-11 vì các lỗi thường nằm ở chính điểm chuyển trạng thái hoặc điều kiện lọc đơn. Vì đặc tả không nêu giới hạn tối đa của số đơn trong danh sách, mình không giả định một ngưỡng số học cố định mà chỉ dùng số lượng nhỏ làm biên thực tế để kiểm thử giao diện và logic hiển thị.
+
+| Input | Biên (min/max) | Giá trị test (min-1, min, min+1, max-1, max, max+1) | Ghi chú |
+|---|---|---|---|
+| visible_order_list | min = 0 đơn, max = 2 đơn trong nhóm test mẫu | min-1: không áp dụng<br>min: []<br>min+1: [DH20260708-001]<br>max-1: [DH20260708-001]<br>max: [DH20260708-001, DH20260708-002]<br>max+1: [DH20260708-001, DH20260708-002, DH20260708-003] | Kiểm tra trạng thái rỗng và chuyển từ 0 sang 1, rồi từ 1 sang nhiều đơn. Vì đặc tả không quy định giới hạn số lượng đơn được hiển thị, mình chỉ dùng số lượng nhỏ làm biên kiểm thử thực tế. |
+| order_owner_id so với current_user_id | min = không khớp, max = khớp | min-1: không áp dụng<br>min: usr_2048<br>min+1: usr_1042<br>max-1: usr_2048<br>max: usr_1042<br>max+1: không áp dụng | Đây là biên logic nhị phân: đơn của người khác và đơn của chính mình. Không có min/max số học nên chỉ có 2 trạng thái biên hợp lệ để test. |
+| current_status_code | min = pending, max = delivered theo luồng hiển thị; canceled là nhánh kết thúc | min-1: không áp dụng<br>min: pending<br>min+1: confirmed<br>max-1: shipping<br>max: delivered<br>max+1: canceled | Với trạng thái đơn hàng, mình coi biên là các điểm đầu/cuối của vòng đời chính và nhánh hủy. Đây là biên logic, không phải biên số học. |
+| status_display_text_vn | min = pending -> Chờ xác nhận, max = delivered -> Đã giao hàng | min-1: không áp dụng<br>min: Chờ xác nhận<br>min+1: Đã xác nhận<br>max-1: Đang giao hàng<br>max: Đã giao hàng<br>max+1: Đã hủy | Dùng để đối chiếu việc dịch trạng thái sang tiếng Việt ở các điểm đầu, giữa và cuối luồng. |
+| status_color | min = trạng thái đầu luồng, max = trạng thái cuối luồng | min-1: không áp dụng<br>min: màu cho Chờ xác nhận<br>min+1: màu cho Đã xác nhận<br>max-1: màu cho Đang giao hàng<br>max: màu cho Đã giao hàng<br>max+1: màu cho Đã hủy | Vì đặc tả yêu cầu màu phân biệt rõ, nên mình kiểm tra cả sự thay đổi màu ở từng trạng thái kế cận. |
+
+### 2. BOUNDARY TEST CASES TABLE
+
+| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
+|---|---|---|---|---|---|---|---|---|---|---|
+| TC-BVA-01 | Biên danh sách rỗng - user không có đơn nào | User usr_1042 đang đăng nhập và không có đơn nào thuộc chính mình | current_user_id: usr_1042<br>orders_in_system: [] | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát vùng danh sách | Hệ thống hiển thị empty state rõ ràng, không hiển thị đơn hàng nào và không hiển thị đơn của user khác |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-02 | Biên danh sách tại min - có đúng 1 đơn của chính user | User usr_1042 đang đăng nhập và có đúng 1 đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-001<br>order_date: 08/07/2026<br>total_amount: 1250000<br>order_owner_id: usr_1042<br>current_status_code: shipping | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát dòng đơn DH20260708-001 | Hệ thống hiển thị đúng 1 đơn của usr_1042; đơn có đủ mã đơn, ngày đặt, tổng tiền, trạng thái tiếng Việt và màu trạng thái tương ứng |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-03 | Biên danh sách ở vùng nhiều đơn - 2 đơn của chính user | User usr_1042 đang đăng nhập và có 2 đơn thuộc chính mình | current_user_id: usr_1042<br>order_code: DH20260708-001, DH20260708-002<br>order_date: 08/07/2026, 07/07/2026<br>total_amount: 1250000, 980000<br>order_owner_id: usr_1042, usr_1042<br>current_status_code: shipping, confirmed | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát danh sách hiển thị | Hệ thống hiển thị đủ 2 đơn của usr_1042; mỗi dòng hiển thị đúng mã đơn, ngày đặt, tổng tiền, trạng thái tiếng Việt và màu tương ứng |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-04 | Biên quyền sở hữu - đơn của user khác không được hiển thị | User usr_1042 đang đăng nhập; hệ thống có đơn thuộc user khác | current_user_id: usr_1042<br>order_code: DH20260708-009<br>order_date: 01/07/2026<br>total_amount: 760000<br>order_owner_id: usr_2048<br>current_status_code: shipping | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Tìm đơn DH20260708-009 | Đơn DH20260708-009 không được hiển thị trong lịch sử của usr_1042 vì owner khác user hiện tại |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-05 | Biên trạng thái đầu luồng - pending | User usr_1042 đang đăng nhập và có đơn của chính mình ở trạng thái pending | current_user_id: usr_1042<br>order_code: DH20260708-010<br>order_date: 30/06/2026<br>total_amount: 520000<br>order_owner_id: usr_1042<br>current_status_code: pending | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát đơn DH20260708-010 | Trạng thái được dịch sang tiếng Việt đúng theo pending và hiển thị màu phân biệt đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-06 | Biên trạng thái kế cận - confirmed | User usr_1042 đang đăng nhập và có đơn của chính mình ở trạng thái confirmed | current_user_id: usr_1042<br>order_code: DH20260708-011<br>order_date: 29/06/2026<br>total_amount: 640000<br>order_owner_id: usr_1042<br>current_status_code: confirmed | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát đơn DH20260708-011 | Trạng thái được dịch sang tiếng Việt đúng theo confirmed và hiển thị màu phân biệt đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-07 | Biên trạng thái giữa luồng - shipping | User usr_1042 đang đăng nhập và có đơn của chính mình ở trạng thái shipping | current_user_id: usr_1042<br>order_code: DH20260708-012<br>order_date: 28/06/2026<br>total_amount: 890000<br>order_owner_id: usr_1042<br>current_status_code: shipping | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát đơn DH20260708-012 | Trạng thái được dịch sang tiếng Việt đúng theo shipping và hiển thị màu phân biệt đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-08 | Biên trạng thái gần cuối luồng - delivered | User usr_1042 đang đăng nhập và có đơn của chính mình ở trạng thái delivered | current_user_id: usr_1042<br>order_code: DH20260708-013<br>order_date: 27/06/2026<br>total_amount: 1500000<br>order_owner_id: usr_1042<br>current_status_code: delivered | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát đơn DH20260708-013 | Trạng thái được dịch sang tiếng Việt đúng theo delivered và hiển thị màu phân biệt đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
+| TC-BVA-09 | Biên trạng thái kết thúc luồng - canceled | User usr_1042 đang đăng nhập và có đơn của chính mình ở trạng thái canceled | current_user_id: usr_1042<br>order_code: DH20260708-014<br>order_date: 26/06/2026<br>total_amount: 300000<br>order_owner_id: usr_1042<br>current_status_code: canceled | 1. Đăng nhập bằng user usr_1042<br>2. Mở màn hình lịch sử đơn hàng<br>3. Quan sát đơn DH20260708-014 | Trạng thái được dịch sang tiếng Việt đúng theo canceled và hiển thị màu phân biệt đúng thiết kế |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |

## Prompt 7
### AI tool name
* GPT-5.4 mini

### Date and time
* 2026-07-08 23:08:02

### Prompt
* **System Core Blueprint (Full Verbatim Content):**
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
  ...

* **Input Feature Specification Used:**
  ### FR-17: Quản lý Mã Giảm Giá (Coupon CRUD)

  - Admin có thể Thêm / Xem / Xóa mã giảm giá.
  - Các trường bắt buộc: `code` (duy nhất), `type` (percent/fixed), `discount_value` (dương), `expired_at`, `min_order_amount` (>= 0), `max_uses_per_user` (>= 1)

### The AI output
Step 1 — Phân tích nghiệp vụ

FR-17 là tính năng quản lý coupon dành cho Admin, gồm thêm, xem và xóa mã giảm giá. Trọng tâm kiểm thử là các ràng buộc của từng field: `code` duy nhất và không rỗng, `type` chỉ nhận `percent` hoặc `fixed`, `discount_value` dương, `expired_at` hợp lệ, `min_order_amount` không âm và `max_uses_per_user` tối thiểu là 1.

| Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Giá trị mẫu hợp lệ cụ thể | Nguồn (spec/inferred) |
|---|---|---|---|---|---|
| admin_user | Input | string / account | Phải là tài khoản Admin đã xác thực, có quyền CRUD coupon | admin001 | spec + inferred |
| code | Input | string | Bắt buộc; duy nhất; không rỗng; không trùng coupon khác | SUMMER2026 | spec + inferred |
| type | Input | enum | Chỉ nhận `percent` hoặc `fixed` | percent | spec |
| discount_value | Input | number / decimal | Phải dương; nếu `percent` thì nên trong 1–100; nếu `fixed` thì lớn hơn 0 | 10 | spec + inferred |
| expired_at | Input | date / datetime | Bắt buộc; phải là ngày/thời điểm hợp lệ trong tương lai khi tạo coupon | 31/12/2026 | spec + inferred |
| min_order_amount | Input | number / decimal | Phải lớn hơn hoặc bằng 0 | 200000 | spec |
| max_uses_per_user | Input | integer | Phải lớn hơn hoặc bằng 1 | 1 | spec |
| coupon_list | Output | list / array | Danh sách coupon phải hiển thị được dữ liệu đã lưu | SUMMER2026, WELCOME10 | spec + inferred |
| coupon_detail | Output | object / record | Khi xem chi tiết phải hiển thị đúng dữ liệu coupon đã chọn | code: SUMMER2026 | inferred |
| create_success_message | Output | string | Hiển thị khi tạo coupon thành công | Tạo mã giảm giá thành công. | inferred |
| delete_success_message | Output | string | Hiển thị khi xóa coupon thành công | Xóa mã giảm giá thành công. | inferred |
| validation_error_message | Output | string | Hiển thị khi dữ liệu không hợp lệ hoặc vi phạm ràng buộc | `code` đã tồn tại. | spec + inferred |

✅ Đã hoàn thành Step 1: Phân tích nghiệp vụ. Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step 2: Xác định Input & Output cho Domain Testing không?
[DỪNG LẠI CHỜ XÁC NHẬN]

Step 2 — Xác định Input & Output cho Domain Testing

Ở bước này mình chỉ giữ lại các trường ảnh hưởng trực tiếp tới logic kiểm thử coupon CRUD. Các output như danh sách coupon, thông báo thành công/lỗi chỉ là kết quả quan sát, còn input trọng tâm là các trường dữ liệu của coupon và quyền admin.

| Biến | Vai trò (Input/Output) | Ghi nhận cho Domain Testing | Quan hệ / phụ thuộc | Giá trị baseline hợp lệ cụ thể |
|---|---|---|---|---|
| admin_user | Input | Giữ lại | Phải là Admin đã xác thực để thực hiện CRUD | admin001 |
| code | Input | Giữ lại | Phải duy nhất trong hệ thống | SUMMER2026 |
| type | Input | Giữ lại | Quyết định cách hiểu `discount_value` | percent |
| discount_value | Input | Giữ lại | Phụ thuộc vào `type`; phải là giá trị dương | 10 |
| expired_at | Input | Giữ lại | Phải là thời điểm hợp lệ để coupon có hiệu lực kiểm thử | 31/12/2026 |
| min_order_amount | Input | Giữ lại | Điều kiện đơn hàng tối thiểu để áp dụng coupon | 200000 |
| max_uses_per_user | Input | Giữ lại | Số lần tối đa mỗi user được dùng coupon | 1 |
| coupon_list | Output | Giữ lại | Danh sách coupon sau khi thêm/xem | SUMMER2026, WELCOME10 |
| coupon_detail | Output | Giữ lại | Hiển thị chi tiết coupon đã chọn | code: SUMMER2026 |
| create_success_message | Output | Giữ lại | Chỉ xuất hiện sau khi thêm thành công | Tạo mã giảm giá thành công. |
| delete_success_message | Output | Giữ lại | Chỉ xuất hiện sau khi xóa thành công | Xóa mã giảm giá thành công. |
| validation_error_message | Output | Giữ lại | Xuất hiện khi dữ liệu không hợp lệ hoặc vi phạm ràng buộc | `code` đã tồn tại. |

Bộ baseline data mình sẽ dùng xuyên suốt:
- Admin đang thao tác: `admin001`
- Mã coupon hợp lệ mẫu: `SUMMER2026`
- Loại coupon: `percent`
- Giá trị giảm: `10`
- Hạn dùng: `31/12/2026`
- Đơn hàng tối thiểu: `200000`
- Số lần dùng tối đa mỗi user: `1`

Quan hệ cần lưu ý:
- `code` phải duy nhất.
- `type` quyết định miền hợp lệ của `discount_value`.
- `expired_at` phải là ngày/thời điểm hợp lệ.
- `min_order_amount` và `max_uses_per_user` là các ràng buộc số học độc lập, nhưng đều phải được kiểm tra cùng `code`, `type`, `discount_value` trong một lần tạo coupon.

✅ Đã hoàn thành Step 2: Xác định Input & Output cho Domain Testing. Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step 3: Xác định Equivalence Classes (Valid/Invalid) không?
[DỪNG LẠI CHỜ XÁC NHẬN]

Step 3 — Xác định Equivalence Classes (Valid/Invalid)

Mình tách miền tương đương theo đúng các ràng buộc đã chốt ở Step 1 và Step 2, ưu tiên các input quyết định logic tạo coupon: quyền admin, `code`, `type`, `discount_value`, `expired_at`, `min_order_amount`, `max_uses_per_user`. Các output như danh sách coupon hay thông báo chỉ là kết quả để đối chiếu.

| EC ID | Input/Output | Điều kiện | Loại (Valid/Invalid) | Giá trị đại diện cụ thể | Mô tả |
|---|---|---|---|---|---|
| EC-01 | admin_user | Admin đã xác thực và có quyền CRUD coupon | Valid | admin001 | Có quyền thao tác coupon |
| EC-02 | admin_user | Không phải admin hoặc chưa xác thực | Invalid | staff001 | Không đủ quyền CRUD coupon |
| EC-03 | code | Mã duy nhất, không rỗng | Valid | SUMMER2026 | Mã coupon hợp lệ và chưa trùng |
| EC-04 | code | Trùng với coupon đã tồn tại | Invalid | SUMMER2026 | Mã bị trùng, không được tạo mới |
| EC-05 | code | Chuỗi rỗng | Invalid | "" | Không được bỏ trống mã coupon |
| EC-06 | type | Chọn `percent` | Valid | percent | Loại coupon theo phần trăm |
| EC-07 | type | Chọn `fixed` | Valid | fixed | Loại coupon theo số tiền cố định |
| EC-08 | type | Giá trị ngoài enum cho phép | Invalid | bonus | Loại coupon không hợp lệ |
| EC-09 | discount_value | Với `percent`, giá trị dương trong miền hợp lệ | Valid | 10 | Giá trị giảm theo phần trăm hợp lệ |
| EC-10 | discount_value | Với `fixed`, giá trị dương | Valid | 50000 | Giá trị giảm cố định hợp lệ |
| EC-11 | discount_value | Bằng 0 | Invalid | 0 | Giảm giá không được bằng 0 |
| EC-12 | discount_value | Âm | Invalid | -5 | Giá trị giảm âm không hợp lệ |
| EC-13 | discount_value | Với `percent`, vượt quá 100 | Invalid | 101 | Phần trăm giảm vượt giới hạn hợp lý |
| EC-14 | expired_at | Ngày/thời điểm tương lai hợp lệ | Valid | 31/12/2026 | Coupon còn hạn dùng |
| EC-15 | expired_at | Ngày/thời điểm quá khứ | Invalid | 31/12/2024 | Coupon đã hết hạn trước khi tạo |
| EC-16 | expired_at | Giá trị rỗng | Invalid | "" | Không có ngày hết hạn |
| EC-17 | min_order_amount | Giá trị số nguyên/decimal >= 0 | Valid | 200000 | Điều kiện đơn hàng tối thiểu hợp lệ |
| EC-18 | min_order_amount | Âm | Invalid | -1 | Điều kiện tối thiểu không hợp lệ |
| EC-19 | max_uses_per_user | Số nguyên >= 1 | Valid | 1 | Mỗi user được dùng ít nhất 1 lần |
| EC-20 | max_uses_per_user | Bằng 0 | Invalid | 0 | Không được cho phép 0 lần dùng |
| EC-21 | max_uses_per_user | Âm | Invalid | -2 | Số lần dùng âm không hợp lệ |
| EC-22 | coupon_list | Có ít nhất 1 coupon trong danh sách | Valid | [SUMMER2026] | Danh sách hiển thị có dữ liệu |
| EC-23 | coupon_list | Danh sách rỗng | Valid | [] | Danh sách trống nhưng vẫn hợp lệ khi chưa tạo coupon |
| EC-24 | coupon_detail | Coupon tồn tại và xem được | Valid | SUMMER2026 | Xem chi tiết coupon hợp lệ |
| EC-25 | coupon_detail | Coupon không tồn tại | Invalid | WINTER2026 | Không tìm thấy coupon để xem/xóa |

Kết luận phân lớp:
- `code` có 3 miền cần tách riêng: hợp lệ, trùng, rỗng.
- `type` là enum nên chia theo từng giá trị hợp lệ và một miền invalid chung.
- `discount_value` phụ thuộc `type`, nên ở bước sau mình sẽ giữ lưu ý này khi chọn test case.
- `expired_at`, `min_order_amount`, `max_uses_per_user` đều có biên số học rõ nên sẽ dùng lại ở BVA.

✅ Đã hoàn thành Step 3: Xác định Equivalence Classes (Valid/Invalid). Bạn xác nhận nội dung này đã đúng chưa? Có cần chỉnh sửa gì trước khi mình sang Step 4: Test Case Report (Domain Testing) không?
[DỪNG LẠI CHỜ XÁC NHẬN]

Step 4 — Test Case Report (Domain Testing)

Mình đã giữ đúng baseline từ Step 2 và tách test case theo các equivalence class ở Step 3. Với phần invalid, mỗi test case chỉ cover đúng 1 invalid EC; với phần valid, mình gộp các EC hợp lệ có thể kiểm tra chung trong cùng một luồng CRUD coupon.

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|---|
| TC-01 | Xem danh sách coupon khi hệ thống chưa có dữ liệu | Admin admin001 đã đăng nhập; hệ thống chưa có coupon nào | admin_user: admin001 | 1. Đăng nhập bằng admin001<br>2. Mở màn hình quản lý coupon<br>3. Quan sát danh sách coupon | Danh sách coupon hiển thị trạng thái rỗng rõ ràng, không báo lỗi, không hiển thị dữ liệu giả |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-02 | Tạo coupon hợp lệ loại percent | Admin admin001 đã đăng nhập và có quyền CRUD coupon | admin_user: admin001<br>code: SUMMER2026<br>type: percent<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập dữ liệu hợp lệ<br>3. Lưu coupon | Coupon được tạo thành công; hệ thống hiển thị thông báo thành công và coupon xuất hiện trong danh sách |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-03 | Tạo coupon hợp lệ loại fixed | Admin admin001 đã đăng nhập và có quyền CRUD coupon | admin_user: admin001<br>code: WELCOME10<br>type: fixed<br>discount_value: 50000<br>expired_at: 31/12/2026<br>min_order_amount: 0<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập dữ liệu hợp lệ<br>3. Lưu coupon | Coupon được tạo thành công; coupon fixed hiển thị đúng dữ liệu đã nhập trong danh sách |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-04 | Xem chi tiết coupon đã tồn tại | Admin admin001 đã đăng nhập và có coupon SUMMER2026 trong hệ thống | admin_user: admin001<br>code: SUMMER2026 | 1. Mở danh sách coupon<br>2. Chọn coupon SUMMER2026<br>3. Mở màn hình chi tiết | Chi tiết coupon hiển thị đúng code, type, discount_value, expired_at, min_order_amount và max_uses_per_user |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-05 | Xóa coupon đã tồn tại | Admin admin001 đã đăng nhập và coupon SUMMER2026 đang tồn tại | admin_user: admin001<br>code: SUMMER2026 | 1. Chọn coupon SUMMER2026<br>2. Nhấn xóa<br>3. Xác nhận thao tác | Coupon bị xóa thành công; hệ thống hiển thị thông báo xóa thành công và coupon không còn trong danh sách |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-06 | Admin không hợp lệ không được tạo coupon | Người dùng staff001 không có quyền admin | admin_user: staff001<br>code: SUMMER2026<br>type: percent<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Đăng nhập bằng tài khoản không phải admin<br>2. Mở form thêm coupon<br>3. Thử lưu coupon | Hệ thống từ chối thao tác, không tạo coupon và hiển thị thông báo không đủ quyền |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-07 | Code trùng với coupon đã tồn tại | Admin admin001 đã đăng nhập và coupon SUMMER2026 đã tồn tại | admin_user: admin001<br>code: SUMMER2026<br>type: fixed<br>discount_value: 50000<br>expired_at: 31/12/2026<br>min_order_amount: 0<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập code trùng<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo lỗi code đã tồn tại |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-08 | Code rỗng | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: ""<br>type: percent<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Để trống code<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo lỗi bắt buộc nhập code |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-09 | Type không thuộc enum cho phép | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: WINTER2026<br>type: bonus<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập type bonus<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo type không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-10 | Discount value bằng 0 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: ZERO2026<br>type: fixed<br>discount_value: 0<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập discount_value = 0<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo giá trị giảm phải dương |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-11 | Discount value âm | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: NEG2026<br>type: fixed<br>discount_value: -5<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập discount_value âm<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo giá trị giảm không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-12 | Percent vượt quá 100 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: OVER100<br>type: percent<br>discount_value: 101<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập percent = 101<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo phần trăm giảm không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-13 | Ngày hết hạn ở quá khứ | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: OLD2024<br>type: fixed<br>discount_value: 50000<br>expired_at: 31/12/2024<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập expired_at ở quá khứ<br>3. Lưu coupon | Hệ thống từ chối tạo coupon hoặc cảnh báo ngày hết hạn không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-14 | Ngày hết hạn rỗng | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: NOEXP2026<br>type: percent<br>discount_value: 10<br>expired_at: ""<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Để trống expired_at<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo bắt buộc nhập ngày hết hạn |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-15 | min_order_amount âm | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: MINNEG<br>type: fixed<br>discount_value: 50000<br>expired_at: 31/12/2026<br>min_order_amount: -1<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập min_order_amount âm<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo min_order_amount phải >= 0 |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-16 | max_uses_per_user bằng 0 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: USE0<br>type: percent<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 0 | 1. Mở form thêm coupon<br>2. Nhập max_uses_per_user = 0<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo số lần dùng tối thiểu phải là 1 |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-17 | max_uses_per_user âm | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: USENEG<br>type: percent<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: -2 | 1. Mở form thêm coupon<br>2. Nhập max_uses_per_user âm<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo số lần dùng không hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-18 | Xem chi tiết coupon không tồn tại | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: WINTER2026 | 1. Tìm coupon WINTER2026<br>2. Mở chi tiết coupon | Hệ thống không tìm thấy coupon và hiển thị xử lý lỗi phù hợp |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |

## II. BOUNDARY VALUE ANALYSIS (BVA)

### 1. DETAILED STEP-BY-STEP BOUNDARY ANALYSIS

Mình dùng single-fault BVA cho FR-17: mỗi test case chỉ đẩy một input ra biên, còn các input khác giữ ở giá trị baseline hợp lệ. Cách này phù hợp nhất để cô lập lỗi cho các field số và ngày của coupon, đặc biệt là `discount_value`, `min_order_amount`, `max_uses_per_user`, và `expired_at`.

Với coupon CRUD, các biên có ý nghĩa thực tế nhất là:
- `discount_value`:
  - với `percent`: biên hợp lý là 1 và 100
  - với `fixed`: chỉ cần đảm bảo lớn hơn 0, nên biên dưới là 1
- `min_order_amount`: biên dưới là 0
- `max_uses_per_user`: biên dưới là 1
- `expired_at`: biên ngày là quá khứ, hiện tại, và tương lai
- `code` và `type` chủ yếu là miền rời rạc/duy nhất, nên không có biên số học rõ để áp BVA theo kiểu min/max

| Input | Biên (min/max) | Giá trị test (min-1, min, min+1, max-1, max, max+1) | Ghi chú |
|---|---|---|---|
| discount_value với type = percent | min = 1, max = 100 | min-1: 0<br>min: 1<br>min+1: 2<br>max-1: 99<br>max: 100<br>max+1: 101 | Biên này phản ánh miền phần trăm giảm hợp lý; 0 và 101 là giá trị ngoài miền hợp lệ |
| discount_value với type = fixed | min = 1 | min-1: 0<br>min: 1<br>min+1: 2<br>max-1: không áp dụng<br>max: không áp dụng<br>max+1: không áp dụng | Fixed chỉ có ràng buộc dương; đặc tả chưa nêu max nên không đặt biên trên |
| min_order_amount | min = 0 | min-1: -1<br>min: 0<br>min+1: 1<br>max-1: không áp dụng<br>max: không áp dụng<br>max+1: không áp dụng | Giá trị tối thiểu cho đơn hàng được giảm giá |
| max_uses_per_user | min = 1 | min-1: 0<br>min: 1<br>min+1: 2<br>max-1: không áp dụng<br>max: không áp dụng<br>max+1: không áp dụng | Số lần dùng tối thiểu hợp lệ là 1 |
| expired_at | min = ngày hiện tại, max = ngày tương lai | min-1: 07/07/2026<br>min: 08/07/2026<br>min+1: 09/07/2026<br>max-1: 30/12/2026<br>max: 31/12/2026<br>max+1: 01/01/2027 | Mình lấy mốc theo ngày chạy hiện tại 08/07/2026; coupon nên hết hạn ở tương lai để hợp lệ khi tạo |

Giải thích lựa chọn:
- `discount_value` là biến quan trọng nhất vì phụ thuộc vào `type`, nên mình tách riêng biên cho `percent` và `fixed`.
- `min_order_amount` và `max_uses_per_user` là hai field có ngưỡng rõ ràng nên biên dưới là đủ để bắt lỗi.
- `expired_at` cần kiểm tra cả trường hợp quá khứ, hiện tại và tương lai vì lỗi ngày là lỗi rất phổ biến khi tạo coupon.
- `code` là miền duy nhất nên mình sẽ xử lý chủ yếu ở Domain Testing bằng EC trùng/rỗng hơn là BVA số học.

### 2. BOUNDARY TEST CASES TABLE

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|
| TC-BVA-01 | Discount value percent dưới biên: 0 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: PERCENT0<br>type: percent<br>discount_value: 0<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập dữ liệu với discount_value = 0<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo discount_value không hợp lệ vì percent phải lớn hơn 0 |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-02 | Discount value percent tại biên dưới: 1 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: PERCENT1<br>type: percent<br>discount_value: 1<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập dữ liệu với discount_value = 1<br>3. Lưu coupon | Hệ thống chấp nhận coupon nếu các trường khác hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-03 | Discount value percent tại biên trên: 100 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: PERCENT100<br>type: percent<br>discount_value: 100<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập dữ liệu với discount_value = 100<br>3. Lưu coupon | Hệ thống chấp nhận coupon nếu các trường khác hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-04 | Discount value percent trên biên: 101 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: PERCENT101<br>type: percent<br>discount_value: 101<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập dữ liệu với discount_value = 101<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo phần trăm giảm vượt giới hạn hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-05 | Min order amount dưới biên: -1 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: MINNEG1<br>type: fixed<br>discount_value: 50000<br>expired_at: 31/12/2026<br>min_order_amount: -1<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập min_order_amount = -1<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo min_order_amount phải >= 0 |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-06 | Min order amount tại biên: 0 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: MIN0<br>type: fixed<br>discount_value: 50000<br>expired_at: 31/12/2026<br>min_order_amount: 0<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập min_order_amount = 0<br>3. Lưu coupon | Hệ thống chấp nhận coupon nếu các trường khác hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-07 | Max uses per user dưới biên: 0 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: USE0<br>type: percent<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 0 | 1. Mở form thêm coupon<br>2. Nhập max_uses_per_user = 0<br>3. Lưu coupon | Hệ thống từ chối tạo coupon và báo số lần dùng mỗi user phải >= 1 |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-08 | Max uses per user tại biên: 1 | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: USE1<br>type: percent<br>discount_value: 10<br>expired_at: 31/12/2026<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập max_uses_per_user = 1<br>3. Lưu coupon | Hệ thống chấp nhận coupon nếu các trường khác hợp lệ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |
| TC-BVA-09 | Ngày hết hạn dưới biên: ngày trong quá khứ | Admin admin001 đã đăng nhập | admin_user: admin001<br>code: EXPIRED2024<br>type: fixed<br>discount_value: 50000<br>expired_at: 31/12/2024<br>min_order_amount: 200000<br>max_uses_per_user: 1 | 1. Mở form thêm coupon<br>2. Nhập expired_at = 31/12/2024<br>3. Lưu coupon | Hệ thống từ chối tạo coupon hoặc cảnh báo ngày hết hạn không hợp lệ vì đã ở quá khứ |  |  |  |  | Cần người dùng thực thi thủ công hoặc chạy trên SUT rồi điền lại |

### The AI output
Step 1 — Phân tích nghiệp vụ
FR-17 là tính năng quản lý coupon dành cho Admin, gồm thêm, xem và xóa mã giảm giá. Trọng tâm kiểm thử là các ràng buộc của từng field: `code` duy nhất và không rỗng, `type` chỉ nhận `percent` hoặc `fixed`, `discount_value` dương, `expired_at` hợp lệ, `min_order_amount` không âm và `max_uses_per_user` tối thiểu là 1.

Step 2 — Xác định Input & Output cho Domain Testing
Ở bước này mình chỉ giữ lại các trường ảnh hưởng trực tiếp tới logic kiểm thử coupon CRUD. Các output như danh sách coupon, thông báo thành công/lỗi chỉ là kết quả quan sát, còn input trọng tâm là các trường dữ liệu của coupon và quyền admin.

Step 3 — Xác định Equivalence Classes (Valid/Invalid)
Mình tách miền tương đương theo đúng các ràng buộc đã chốt ở Step 1 và Step 2, ưu tiên các input quyết định logic tạo coupon: quyền admin, `code`, `type`, `discount_value`, `expired_at`, `min_order_amount`, `max_uses_per_user`. Các output như danh sách coupon hay thông báo chỉ là kết quả để đối chiếu.

Step 4 — Test Case Report (Domain Testing)
Mình đã giữ đúng baseline từ Step 2 và tách test case theo các equivalence class ở Step 3. Với phần invalid, mỗi test case chỉ cover đúng 1 invalid EC; với phần valid, mình gộp các EC hợp lệ có thể kiểm tra chung trong cùng một luồng CRUD coupon.

Step 5 — BVA Thought Process
Mình dùng single-fault BVA cho FR-17: mỗi test case chỉ đẩy một input ra biên, còn các input khác giữ ở giá trị baseline hợp lệ. Cách này phù hợp nhất để cô lập lỗi cho các field số và ngày của coupon, đặc biệt là `discount_value`, `min_order_amount`, `max_uses_per_user`, và `expired_at`.

Step 6 — Kết xuất Bảng Test Case BVA
Mình giữ đúng các giá trị biên đã chốt ở Step 5 và dùng 9 test case để phủ các điểm biên quan trọng nhất của coupon CRUD: `discount_value`, `min_order_amount`, `max_uses_per_user`, và `expired_at`. Phần Actual Result, Status, Defect ID để trống theo đúng quy tắc, vì người dùng sẽ thực thi trên SUT rồi điền sau.

## Prompt 8 
# AI AUDIT REPORT (DỮ LIỆU KIỂM TOÁN HỆ THỐNG)

### AI tool name
* GPT-5.4 mini

### Date and time
* 2026-07-09 06:11 UTC (phiên làm việc thực hiện toàn bộ Step 0 → Step 7)

### Prompt

* **System Core Blueprint (Full Verbatim Content):**

```
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
Trình bày dưới dạng bảng, bắt buộc có cột giá trị mẫu cụ thể (không để trống, không viết "tuỳ ý"):
Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Giá trị mẫu hợp lệ cụ thể | Nguồn (spec/inferred)
Ví dụ cột "Giá trị mẫu hợp lệ cụ thể": với input username ràng buộc "6–20 ký tự alphanumeric" → ghi cụ thể nguyen_van_a01, không ghi chung chung "một chuỗi hợp lệ".
[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 2 — Xác định Input & Output cho Domain Testing

Thu gọn lại danh sách ở Step 1 thành các input/output sẽ dùng để phân lớp tương đương (loại bỏ input không ảnh hưởng tới logic test).
Xác nhận input nào độc lập, input nào có quan hệ ràng buộc lẫn nhau (cần lưu ý khi chọn test case ở Step 4).
Với các input giữ lại, chốt sẵn 1 bộ "giá trị nền" (baseline data) — một bộ dữ liệu hợp lệ đầy đủ, cụ thể cho toàn bộ field của tính năng (ví dụ: 1 tài khoản mẫu hoàn chỉnh: username, password, email...). Bộ baseline này sẽ được tái sử dụng ở Step 4/Step 6: khi 1 test case chỉ tập trung kiểm tra 1 field, các field còn lại lấy nguyên giá trị baseline (hợp lệ) thay vì để trống hoặc ghi "bất kỳ".

[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 3 — Xác định Equivalence Classes (Valid/Invalid)
Với từng input condition, áp dụng đúng guideline đã học:

Range (VD "1 ≤ count ≤ 999") → 1 valid EC + 2 invalid EC (nhỏ hơn / lớn hơn).
Tập giá trị rời rạc, mỗi giá trị được xử lý khác nhau (enum) → 1 valid EC/giá trị + 1 invalid EC chung.
"Must be" case (VD "ký tự đầu phải là chữ") → 1 valid EC + 1 invalid EC.
Nếu nghi ngờ các phần tử trong 1 EC không được xử lý giống nhau → tách nhỏ EC đó ra.
Trình bày bảng, bắt buộc có cột giá trị đại diện cụ thể cho từng EC — đây là cột quan trọng nhất để tránh test data chung chung:
EC ID | Input/Output | Điều kiện | Loại (Valid/Invalid) | Giá trị đại diện cụ thể | Mô tả
Quy tắc bắt buộc cho cột "Giá trị đại diện cụ thể":

Không được viết mô tả trừu tượng như "một giá trị không hợp lệ", "chuỗi bất kỳ", "số ngoài phạm vi" — phải điền con số/chuỗi thật, có thể copy-paste chạy ngay.
Nếu EC là "không phải số nguyên" → ghi cụ thể ví dụ "abc" hoặc 12.5, không ghi chung "không phải số".
Nếu EC là "vượt quá độ dài cho phép" → ghi rõ độ dài thực tế đã dùng, ví dụ nếu giới hạn là 20 ký tự thì ghi chuỗi 21 ký tự cụ thể (có thể viết "a" x 21 kèm chú thích số lượng ký tự).
Nếu EC là giá trị rỗng/null → ghi rõ literal: "" (chuỗi rỗng), NULL, hoặc (không nhập gì) — không gộp chung với các loại invalid khác.
Nếu EC là ký tự đặc biệt/không hợp lệ về định dạng → liệt kê rõ tập ký tự dùng để test, ví dụ "!@#$%^&*()".


[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 4 — Test Case Report (Domain Testing)

KHÔNG có giới hạn số lượng test case. Số lượng test case PHẢI bằng đúng 
số equivalence class hợp lệ tìm được — không được gộp nhiều class vào 
1 test case, không được bỏ sót class nào, và không được tự ý dừng sớm 
vì lý do "đã đủ" hay "để tránh dài".


Valid EC: gộp nhiều EC valid vào 1 test case cho tới khi tất cả valid EC được cover.
Invalid EC: mỗi test case chỉ cover đúng 1 invalid EC.


Xuất bảng Test Case Report đúng format bắt buộc của đề bài:

Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested

Quy tắc bắt buộc cho cột "Input Data" (test data) — áp dụng cho cả Step 4 và Step 6, đây là phần bắt buộc phải rõ ràng, không chung chung:

Lấy đúng giá trị cụ thể đã chốt ở bảng EC (Step 3) / bảng biên (Step 5) cho field đang test — không tự bịa giá trị mới khác với bảng đã chốt.
Input Data phải liệt kê đầy đủ tất cả field liên quan của tính năng cho 1 lần thực thi, không chỉ field đang là trọng tâm test. Các field không phải trọng tâm lấy nguyên giá trị baseline hợp lệ đã chốt ở Step 2 (ví dụ test biên độ dài password, thì username/email trong cùng test case vẫn phải có giá trị hợp lệ cụ thể, không để trống hoặc ghi "bất kỳ").
Dùng thẻ <br> để tách các field trong cùng 1 ô, theo định dạng Tên field: giá trị, ví dụ:
username: nguyen_van_a01<br>password: Abc@12345<br>email: test01@gmail.com
Dữ liệu phải đúng định dạng thực tế của loại field (email dạng abc@domain.com, số điện thoại VN 10 số bắt đầu bằng 0, ngày dạng dd/mm/yyyy, v.v.) — không dùng placeholder kiểu <email>, <string>.
Cấm dùng các cụm mơ hồ sau trong cột Input Data: "giá trị hợp lệ", "giá trị không hợp lệ", "một chuỗi bất kỳ", "số bất kỳ ngoài phạm vi", "dữ liệu ngẫu nhiên", "N/A" (trừ khi field đó thực sự không áp dụng cho tính năng).
Nếu 2 test case khác nhau vô tình dùng cùng 1 bộ Input Data → phải rà soát lại, vì đây là dấu hiệu 2 EC/test case đang trùng lặp thật sự.

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
Trình bày dưới dạng bảng, cột giá trị test phải là số/chuỗi thật, không phải công thức: Input | Biên (min/max) | Giá trị test (min-1, min, min+1, max-1, max, max+1) | Ghi chú

[DỪNG LẠI CHỜ XÁC NHẬN]
## Step 6 — Kết xuất Bảng Test Case BVA

Xuất bảng Test Case BVA theo đúng cấu trúc bảng đã dùng ở Step 4 (Domain Testing Test Case Report):

Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested

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

Không rút gọn, không paraphrase lại nội dung Step 1–6 khi dán vào "The AI output". Không rút gọn, không paraphrase lại nội dung Step 1–6 khi dán vào "The AI output", dán nguyên văn những gì đã được người dùng xác nhận qua các checkpoint.
Không chèn link nội bộ trỏ sang FXX_report.md hoặc bất kỳ file nào khác trong mục này.

```

* **Input Feature Specification Used:**

```
FR-06: Xem chi tiết sản phẩm phân hệ mobile

* Hiển thị đầy đủ: Ảnh lớn, Tên, Giá, Mô tả, Danh mục.
* Có ô nhập Số lượng (chỉ nhận số nguyên dương, tối thiểu là 1).
* Nút Thêm vào giỏ hàng — sau khi bấm hiển thị phản hồi trực quan (toast notification hoặc badge cập nhật).

[Kèm theo hình ảnh minh họa giao diện mobile: màn hình chi tiết sản phẩm mẫu "iPhone 15 Pro Max", giá "30,000,000 đ" (chữ đỏ), mô tả "Điện thoại cao cấp của Apple", khung ảnh sản phẩm placeholder ghi "iPhone 15", ô nhập "Số lượng:" với giá trị mặc định "1", và nút màu xanh lá "Thêm vào giỏ hàng".]
```

### The AI output

**[Step 0 — Xác nhận Input] (thực hiện qua 3 lượt hỏi-đáp)**

Lượt 1 — ác nhận:
- Tên & mã tính năng: FR-06 — Xem chi tiết sản phẩm (phân hệ mobile)
- Đặc tả tổng hợp từ text + ảnh minh họa (như nêu ở mục "Input Feature Specification Used" phía trên)
- Ràng buộc đã biết: Số lượng kiểu số nguyên dương, min = 1; chưa rõ max và case hết hàng
- Hỏi tên AI tool

Người dùng trả lời: "GPT-5.4 mini"

Lượt 2 — Hỏi làm rõ thêm 2 điểm:
1. Giá trị tối đa (max) của ô Số lượng: (a) không cap, theo tồn kho; (b) có cap cứng cố định; (c) để Claude tự đề xuất [inferred]
2. Trường hợp sản phẩm hết hàng/ngừng kinh doanh xử lý thế nào: (a) disable nút + báo "Hết hàng"; (b) vẫn cho bấm nhưng báo lỗi submit; (c) để Claude tự suy luận [inferred]

Người dùng trả lời: "ko có yêu cầu cụ thể"

Lượt 3 — Chốt input:
- Max Số lượng [inferred]: 99
- Case hết hàng [inferred]: disable ô Số lượng + disable nút "Thêm vào giỏ hàng", nhãn nút đổi thành "Hết hàng"
- Bảng tổng hợp Input đã chốt:

| Mục | Nội dung |
|---|---|
| Tên & mã tính năng | FR-06 – Xem chi tiết sản phẩm (mobile) |
| Đặc tả | Hiển thị Ảnh lớn, Tên, Giá, Mô tả, Danh mục; ô Số lượng (số nguyên dương, min=1); nút Thêm vào giỏ hàng có phản hồi trực quan (toast/badge) |
| Ràng buộc số lượng | min = 1, max = 99 [inferred], chỉ số nguyên dương |
| Ràng buộc trạng thái sản phẩm | Còn hàng (mặc định) / Hết hàng [inferred] → ảnh hưởng đến trạng thái nút và ô số lượng |
| AI tool | GPT-5.4 mini |

Người dùng xác nhận: "ok"

---

**Step 1 — Phân tích nghiệp vụ (Business Analyst)**

## 1. Danh sách Input và Output của tính năng FR-06

**Input:**
- Sản phẩm được chọn để xem chi tiết (product_id — do người dùng chọn từ danh sách trước đó, không phải trường nhập tay trên màn hình này)
- Số lượng (quantity) — người dùng nhập trước khi bấm "Thêm vào giỏ hàng"
- Hành động bấm nút "Thêm vào giỏ hàng"

**Output:**
- Thông tin sản phẩm hiển thị: Ảnh lớn, Tên sản phẩm, Giá, Mô tả, Danh mục
- Trạng thái ô Số lượng (enabled/disabled)
- Trạng thái nút "Thêm vào giỏ hàng" (enabled/disabled, nhãn hiển thị)
- Phản hồi trực quan sau khi thêm vào giỏ hàng (toast notification hoặc badge cập nhật số lượng giỏ hàng)

## 2. Bảng điều kiện/ràng buộc cho từng input

| Biến | Vai trò (Input/Output) | Kiểu dữ liệu | Ràng buộc/Range | Giá trị mẫu hợp lệ cụ thể | Nguồn (spec/inferred) |
|---|---|---|---|---|---|
| product_status | Input (điều kiện nghiệp vụ) | Enum | "Còn hàng" / "Hết hàng" | "Còn hàng" | [inferred] |
| product_name | Output | String | Không rỗng, hiển thị đúng tên sản phẩm | "iPhone 15 Pro Max" | spec (từ ảnh minh họa) |
| product_price | Output | Number (VNĐ) | > 0, định dạng có dấu phân cách hàng nghìn | 30.000.000 đ | spec (từ ảnh minh họa) |
| product_description | Output | String | Có thể rỗng hoặc có nội dung mô tả | "Điện thoại cao cấp của Apple" | spec (từ ảnh minh họa) |
| product_category | Output | String/Enum | Thuộc danh mục có sẵn trong hệ thống | "Điện thoại" | [inferred] — đề bài yêu cầu hiển thị Danh mục nhưng ảnh minh họa không thể hiện rõ trường này |
| product_image | Output | Image | Ảnh hợp lệ hoặc ảnh placeholder nếu thiếu | URL ảnh: https://cdn.shop.vn/products/iphone15promax.jpg | spec + [inferred] cho case placeholder |
| quantity | Input | Integer | Số nguyên dương, min = 1, max = 99 | 1 | spec (min=1) + [inferred] (max=99, kiểu integer, ràng buộc dương) |
| quantity_field_state | Output | Boolean | Enabled nếu product_status = "Còn hàng"; Disabled nếu "Hết hàng" | Enabled | [inferred] |
| add_to_cart_button_state | Output | Boolean/Enum | Enabled + nhãn "Thêm vào giỏ hàng" nếu còn hàng; Disabled + nhãn "Hết hàng" nếu hết hàng | Enabled, nhãn "Thêm vào giỏ hàng" | spec (nút) + [inferred] (case hết hàng) |
| feedback_after_add | Output | String/UI element | Toast notification hoặc badge cập nhật, hiển thị trong thời gian ngắn (VD 2–3 giây) | Toast: "Đã thêm 1 iPhone 15 Pro Max vào giỏ hàng" | spec (yêu cầu có phản hồi) + [inferred] (nội dung, thời gian hiển thị cụ thể) |

## 3. Ghi chú về quan hệ giữa các input

- quantity chỉ có ý nghĩa và chỉ được nhập/submit khi product_status = "Còn hàng" → có quan hệ ràng buộc lẫn nhau giữa product_status và quantity / add_to_cart_button_state (sẽ lưu ý kỹ ở Step 2 và Step 4 khi chọn test case, tránh test case vô nghĩa như "nhập số lượng trên sản phẩm hết hàng mà nút vẫn active").
- quantity là input độc lập về mặt kiểu dữ liệu/range so với product_name, product_price, product_description, product_category (các trường này chỉ là output hiển thị tĩnh, không có logic phân lớp tương đương phức tạp).

Người dùng xác nhận: "ok"

---

**Step 2 — Xác định Input & Output cho Domain Testing**

## 1. Thu gọn danh sách Input/Output dùng để phân lớp tương đương

Từ bảng Step 1, các trường product_name, product_price, product_description, product_category, product_image là output hiển thị tĩnh — không có logic xử lý phân nhánh (không range, không "must-be" case phức tạp), nên không đưa vào phân lớp tương đương (EC). Chúng chỉ được dùng làm dữ liệu baseline hiển thị.

Các input/điều kiện được giữ lại để phân lớp tương đương (EC) ở Step 3:

| STT | Tên | Lý do giữ lại |
|---|---|---|
| 1 | product_status (Còn hàng / Hết hàng) | Là điều kiện enum ảnh hưởng trực tiếp đến hành vi hệ thống (state của nút, ô số lượng) |
| 2 | quantity | Có range rõ ràng (1–99, số nguyên dương) → cần EC và BVA |

## 2. Quan hệ giữa các input

- product_status và quantity có quan hệ ràng buộc lẫn nhau: khi product_status = "Hết hàng", quantity không thể nhập/submit hợp lệ (ô bị disable) → đây không phải là 1 EC độc lập của quantity, mà là 1 EC riêng của product_status ảnh hưởng đến toàn bộ luồng. Sẽ lưu ý ở Step 4: không tạo test case kiểu "nhập quantity=5 khi hết hàng" như một EC bình thường của quantity, mà tách thành 1 test case riêng thuộc nhóm product_status.
- quantity khi product_status = "Còn hàng" là độc lập, không phụ thuộc vào các output tĩnh khác (tên, giá, mô tả, danh mục).

## 3. Bộ giá trị nền (Baseline data)

Baseline sẽ được tái sử dụng ở Step 4/Step 6 — khi test case chỉ tập trung vào 1 field, các field còn lại lấy nguyên baseline:

```
product_id: SP001
product_name: iPhone 15 Pro Max
product_price: 30.000.000 đ
product_description: Điện thoại cao cấp của Apple
product_category: Điện thoại
product_image: https://cdn.shop.vn/products/iphone15promax.jpg
product_status: Còn hàng
quantity: 1
```

Người dùng xác nhận: "ok"

---

**Step 3 — Xác định Equivalence Classes (Valid/Invalid)**

## 1. Input: quantity (Số lượng)

Áp dụng guideline: Range "1 ≤ quantity ≤ 99" → 1 valid EC + 2 invalid EC (nhỏ hơn/lớn hơn); "must be integer" → 1 valid EC + 1 invalid EC; ngoài ra tách riêng EC cho rỗng/null và ký tự đặc biệt (không gộp chung với invalid khác).

## 2. Input: product_status (Trạng thái sản phẩm)

Enum, mỗi giá trị được xử lý khác nhau → 1 valid EC/giá trị; thêm 1 invalid EC cho trường hợp dữ liệu lỗi/không xác định.

## 3. Bảng Equivalence Classes

| EC ID | Input/Output | Điều kiện | Loại (Valid/Invalid) | Giá trị đại diện cụ thể | Mô tả |
|---|---|---|---|---|---|
| EC-STT-01 | product_status | Sản phẩm còn hàng | Valid | "Còn hàng" | Trạng thái mặc định, cho phép nhập số lượng và thêm vào giỏ hàng |
| EC-STT-02 | product_status | Sản phẩm hết hàng | Valid | "Hết hàng" | Trạng thái hợp lệ của hệ thống nhưng disable ô Số lượng + nút Thêm vào giỏ hàng [inferred] |
| EC-STT-03 | product_status | Dữ liệu trạng thái lỗi/không xác định | Invalid | NULL (giá trị trạng thái bị thiếu/null từ backend) | Trường hợp lỗi dữ liệu, hệ thống nên có xử lý mặc định an toàn (VD coi như hết hàng) [inferred] |
| EC-QTY-01 | quantity | Trong khoảng hợp lệ 1–99, là số nguyên | Valid | 5 | Giá trị số lượng hợp lệ điển hình |
| EC-QTY-02 | quantity | Nhỏ hơn giá trị min (< 1) | Invalid | 0 | Vi phạm ràng buộc min=1 |
| EC-QTY-03 | quantity | Lớn hơn giá trị max (> 99) | Invalid | 100 | Vi phạm ràng buộc max=99 [inferred] |
| EC-QTY-04 | quantity | Không phải số nguyên (ký tự chữ) | Invalid | "abc" | Sai kiểu dữ liệu, nhập chữ thay vì số |
| EC-QTY-05 | quantity | Số thập phân (không phải số nguyên) | Invalid | 2.5 | Vi phạm ràng buộc "số nguyên" |
| EC-QTY-06 | quantity | Rỗng / không nhập | Invalid | "" (chuỗi rỗng) | Người dùng không nhập gì vào ô Số lượng |
| EC-QTY-07 | quantity | Ký tự đặc biệt | Invalid | "!@#$%" | Nhập ký tự đặc biệt không hợp lệ về định dạng |
| EC-QTY-08 | quantity | Số âm | Invalid | -5 | Vi phạm ràng buộc số nguyên dương (gộp logic với EC-QTY-02 về mặt "nhỏ hơn min", nhưng giữ riêng vì đại diện case số âm rõ ràng, có thể xử lý lỗi khác biệt so với 0) |

Người dùng xác nhận: "ok"

---

**Step 4 — Test Case Report (Domain Testing)**

Tổng số Equivalence Class hợp lệ: 2 (EC-STT-01, EC-STT-02 — không gộp vì EC-STT-02 làm thay đổi hành vi toàn bộ màn hình, gộp chung với EC-QTY-01 sẽ vô nghĩa vì ô Số lượng bị disable). EC-QTY-01 (valid) được gộp cùng EC-STT-01 vì hai EC valid này không mâu thuẫn và có thể kiểm tra đồng thời trong 1 test case.

→ Số test case Valid = 2 (TC01 cover EC-STT-01 + EC-QTY-01; TC02 cover EC-STT-02 riêng).
→ Số test case Invalid = 8 (mỗi EC invalid 1 test case riêng: EC-STT-03, EC-QTY-02 → EC-QTY-08).
→ Tổng: 10 test case.

(Bảng Test Case Report đầy đủ — xem mục "2. DOMAIN TEST CASES TABLE" trong file FR06_report.md, đã được dán lại nguyên văn tại đây theo yêu cầu tự-đầy-đủ của Audit Log:)

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|
| TC_FR06_01 | Xem chi tiết & thêm vào giỏ hàng với sản phẩm còn hàng, số lượng hợp lệ (EC-STT-01 + EC-QTY-01) | Sản phẩm SP001 đang ở trạng thái "Còn hàng" | product_status: Còn hàng<br>quantity: 5<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Kiểm tra thông tin hiển thị<br>3. Nhập 5 vào ô Số lượng<br>4. Bấm nút "Thêm vào giỏ hàng" | Hiển thị đầy đủ Ảnh, Tên, Giá, Mô tả, Danh mục; ô Số lượng nhận giá trị 5; sau khi bấm nút, hệ thống hiển thị toast "Đã thêm 5 iPhone 15 Pro Max vào giỏ hàng" và badge giỏ hàng cập nhật +5 | | Chưa test | | | |
| TC_FR06_02 | Xem chi tiết sản phẩm ở trạng thái hết hàng (EC-STT-02) | Sản phẩm SP001 đang ở trạng thái "Hết hàng" | product_status: Hết hàng<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Kiểm tra trạng thái ô Số lượng và nút | Ô Số lượng bị disable; nút "Thêm vào giỏ hàng" bị disable và hiển thị nhãn "Hết hàng" | | Chưa test | | | |
| TC_FR06_03 | Trạng thái sản phẩm lỗi/không xác định (EC-STT-03) | Dữ liệu trạng thái sản phẩm từ hệ thống bị null | product_status: NULL<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết sản phẩm có product_status = NULL<br>2. Quan sát hành vi hệ thống | Hệ thống xử lý an toàn: coi như hết hàng, disable ô Số lượng và nút "Thêm vào giỏ hàng", không crash/lỗi hiển thị | | Chưa test | | | |
| TC_FR06_04 | Nhập số lượng = 0 (EC-QTY-02) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 0<br>product_name: iPhone 15 Pro Max | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Nhập 0 vào ô Số lượng<br>3. Bấm nút "Thêm vào giỏ hàng" | Hệ thống từ chối, hiển thị thông báo lỗi "Số lượng phải lớn hơn hoặc bằng 1"; không thêm vào giỏ hàng | | Chưa test | | | |
| TC_FR06_05 | Nhập số lượng = 100, vượt max (EC-QTY-03) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 100<br>product_name: iPhone 15 Pro Max | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Nhập 100 vào ô Số lượng<br>3. Bấm nút "Thêm vào giỏ hàng" | Hệ thống từ chối, hiển thị thông báo lỗi "Số lượng tối đa cho phép là 99"; không thêm vào giỏ hàng | | Chưa test | | | |
| TC_FR06_06 | Nhập ký tự chữ vào ô Số lượng (EC-QTY-04) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: "abc"<br>product_name: iPhone 15 Pro Max | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Nhập "abc" vào ô Số lượng<br>3. Bấm nút "Thêm vào giỏ hàng" | Hệ thống từ chối nhập/hiển thị lỗi "Vui lòng nhập số nguyên hợp lệ"; không thêm vào giỏ hàng | | Chưa test | | | |
| TC_FR06_07 | Nhập số thập phân vào ô Số lượng (EC-QTY-05) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 2.5<br>product_name: iPhone 15 Pro Max | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Nhập 2.5 vào ô Số lượng<br>3. Bấm nút "Thêm vào giỏ hàng" | Hệ thống từ chối, hiển thị lỗi "Số lượng phải là số nguyên"; không thêm vào giỏ hàng | | Chưa test | | | |
| TC_FR06_08 | Không nhập số lượng (ô rỗng) (EC-QTY-06) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: "" (rỗng)<br>product_name: iPhone 15 Pro Max | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Để trống ô Số lượng<br>3. Bấm nút "Thêm vào giỏ hàng" | Hệ thống hiển thị lỗi "Vui lòng nhập số lượng"; không thêm vào giỏ hàng | | Chưa test | | | |
| TC_FR06_09 | Nhập ký tự đặc biệt vào ô Số lượng (EC-QTY-07) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: "!@#$%"<br>product_name: iPhone 15 Pro Max | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Nhập "!@#$%" vào ô Số lượng<br>3. Bấm nút "Thêm vào giỏ hàng" | Hệ thống từ chối nhập/hiển thị lỗi "Số lượng không hợp lệ"; không thêm vào giỏ hàng | | Chưa test | | | |
| TC_FR06_10 | Nhập số âm vào ô Số lượng (EC-QTY-08) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: -5<br>product_name: iPhone 15 Pro Max | 1. Mở màn hình chi tiết sản phẩm SP001<br>2. Nhập -5 vào ô Số lượng<br>3. Bấm nút "Thêm vào giỏ hàng" | Hệ thống từ chối, hiển thị lỗi "Số lượng phải là số nguyên dương"; không thêm vào giỏ hàng | | Chưa test | | | |

Người dùng xác nhận: "ok"

---

**Step 5 — BVA Thought Process**

## 1. Xác định các biên (Boundaries)

Dựa trên ràng buộc đã chốt ở Step 1/Step 3: quantity có range 1 ≤ quantity ≤ 99 (min = 1, max = 99). Đây là input số nguyên duy nhất có range rõ ràng trong FR-06, nên là input được áp dụng BVA. product_status là enum (không có "biên" số học) nên không áp dụng BVA cho input này.

## 2. Phân tích các giá trị cần test quanh biên

Biên dưới (min = 1): min−1 = 0 (invalid), min = 1 (valid), min+1 = 2 (valid).
Biên trên (max = 99): max−1 = 98 (valid), max = 99 (valid), max+1 = 100 (invalid).
Giá trị đặc biệt liên quan: 0 và "" đã cover ở EC, không lặp lại thành boundary riêng.

## 3. Giải thích lý do chọn các giá trị này

Các giá trị min−1, min, min+1, max−1, max, max+1 được chọn vì theo lý thuyết BVA, lỗi lập trình (off-by-one error) thường xảy ra ngay tại các điểm biên của điều kiện so sánh. Kiểm tra riêng từng điểm này giúp phát hiện các lỗi mà Equivalence Partitioning không phát hiện được.

## 4. Xác nhận cách tiếp cận BVA

Vì FR-06 chỉ có 1 input duy nhất có biên số học (quantity), single-fault BVA và worst-case BVA cho ra cùng một tập test case. Đề xuất áp dụng single-fault BVA.

## 5. Bảng giá trị biên

| Input | Biên (min/max) | Giá trị test | Ghi chú |
|---|---|---|---|
| quantity | min − 1 | 0 | Invalid — dưới ngưỡng tối thiểu |
| quantity | min | 1 | Valid — ngưỡng tối thiểu |
| quantity | min + 1 | 2 | Valid — ngay trên ngưỡng tối thiểu |
| quantity | max − 1 | 98 | Valid — ngay dưới ngưỡng tối đa |
| quantity | max | 99 | Valid — ngưỡng tối đa |
| quantity | max + 1 | 100 | Invalid — vượt ngưỡng tối đa |

Người dùng xác nhận: "ok"

---

**Step 6 — Kết xuất Bảng Test Case BVA**

Áp dụng single-fault BVA: chỉ đẩy quantity ra các điểm biên (0, 1, 2, 98, 99, 100), các input khác giữ nguyên baseline.

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested | Ghi chú trùng lặp |
|---|---|---|---|---|---|---|---|---|---|---|---|
| TC_BVA_FR06_01 | Nhập số lượng tại biên dưới − 1 (min−1 = 0) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 0<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết SP001<br>2. Nhập 0 vào ô Số lượng<br>3. Bấm "Thêm vào giỏ hàng" | Hệ thống từ chối, hiển thị lỗi "Số lượng phải lớn hơn hoặc bằng 1"; không thêm vào giỏ hàng | | Chưa test | | | | Trùng với TC_FR06_04 (Domain Testing) — giữ lại vì đại diện giá trị biên min−1 |
| TC_BVA_FR06_02 | Nhập số lượng tại biên dưới (min = 1) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 1<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết SP001<br>2. Nhập 1 vào ô Số lượng<br>3. Bấm "Thêm vào giỏ hàng" | Hệ thống chấp nhận, thêm 1 sản phẩm vào giỏ hàng, hiển thị toast "Đã thêm 1 iPhone 15 Pro Max vào giỏ hàng" | | Chưa test | | | | Không trùng — giá trị biên min chưa được test riêng ở Domain Testing |
| TC_BVA_FR06_03 | Nhập số lượng ngay trên biên dưới (min+1 = 2) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 2<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết SP001<br>2. Nhập 2 vào ô Số lượng<br>3. Bấm "Thêm vào giỏ hàng" | Hệ thống chấp nhận, thêm 2 sản phẩm vào giỏ hàng, hiển thị toast tương ứng | | Chưa test | | | | Không trùng |
| TC_BVA_FR06_04 | Nhập số lượng ngay dưới biên trên (max−1 = 98) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 98<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết SP001<br>2. Nhập 98 vào ô Số lượng<br>3. Bấm "Thêm vào giỏ hàng" | Hệ thống chấp nhận, thêm 98 sản phẩm vào giỏ hàng, hiển thị toast tương ứng | | Chưa test | | | | Không trùng |
| TC_BVA_FR06_05 | Nhập số lượng tại biên trên (max = 99) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 99<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết SP001<br>2. Nhập 99 vào ô Số lượng<br>3. Bấm "Thêm vào giỏ hàng" | Hệ thống chấp nhận, thêm 99 sản phẩm vào giỏ hàng, hiển thị toast tương ứng | | Chưa test | | | | Không trùng — giá trị biên max chưa được test riêng ở Domain Testing |
| TC_BVA_FR06_06 | Nhập số lượng vượt biên trên (max+1 = 100) | Sản phẩm SP001 đang "Còn hàng" | product_status: Còn hàng<br>quantity: 100<br>product_name: iPhone 15 Pro Max<br>product_price: 30.000.000 đ | 1. Mở màn hình chi tiết SP001<br>2. Nhập 100 vào ô Số lượng<br>3. Bấm "Thêm vào giỏ hàng" | Hệ thống từ chối, hiển thị lỗi "Số lượng tối đa cho phép là 99"; không thêm vào giỏ hàng | | Chưa test | | | | Trùng với TC_FR06_05 (Domain Testing) — giữ lại vì đại diện giá trị biên max+1 |

Người dùng xác nhận: "ok"