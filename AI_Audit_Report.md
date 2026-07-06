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

## 4. Ghi chú áp dụng cho nhiều feature (Pool A–D)

Khi người dùng có nhiều feature cần làm (VD 4 feature ở 4 pool), chạy toàn bộ Step 0–8 **riêng cho từng feature**, không trộn nhiều feature vào cùng 1 Step. Gợi ý người dùng tạo 1 Git commit sau mỗi Step (theo yêu cầu "Git Commit Log" của đề) — Claude có thể gợi ý nội dung commit message ngắn gọn tương ứng (VD: `test(FR-02): add equivalence classes for login lockout`) nhưng không tự động chạy git thay người dùng trừ khi được yêu cầu rõ.

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