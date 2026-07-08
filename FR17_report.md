# BÁO CÁO KIỂM THỬ - Quản lý Mã Giảm Giá (FR-17)

## I. DOMAIN TESTING

### 1. DETAILED STEP-BY-STEP DOMAIN ANALYSIS

Tính năng FR-17 là một luồng quản lý coupon dành cho Admin, gồm ba thao tác chính: thêm coupon mới, xem danh sách/chi tiết coupon, và xóa coupon. Vì đây là tính năng CRUD, trọng tâm kiểm thử không chỉ nằm ở việc nhập dữ liệu hợp lệ mà còn ở việc đảm bảo các ràng buộc nghiệp vụ được giữ đúng ở từng field. Mỗi coupon phải có `code` duy nhất, `type` hợp lệ, `discount_value` dương, `expired_at` hợp lệ, `min_order_amount` không âm và `max_uses_per_user` tối thiểu là 1.

Khi phân tích miền tương đương, `admin_user` là điều kiện quyền truy cập cốt lõi. Nếu người thao tác không phải admin hoặc chưa xác thực, mọi thao tác CRUD phải bị chặn. `code` là miền quan trọng nhất trong nghiệp vụ vì vừa phải không rỗng vừa phải duy nhất. `type` là một tập rời rạc, hiện chỉ nhận hai giá trị `percent` và `fixed`; do đó miền hợp lệ của nó tách theo từng giá trị. `discount_value` phụ thuộc vào `type`: nếu là `percent`, giá trị phải nằm trong vùng hợp lý của phần trăm; nếu là `fixed`, chỉ cần đảm bảo lớn hơn 0.

`expired_at` là input theo thời gian, nên cần phân biệt rõ ngày ở quá khứ, hiện tại và tương lai. Coupon hết hạn trong quá khứ không nên được tạo mới. `min_order_amount` là điều kiện ngưỡng đơn hàng, nên miền hợp lệ là số không âm. `max_uses_per_user` là số lần sử dụng giới hạn trên mỗi user, do đó miền hợp lệ bắt đầu từ 1. Các output như danh sách coupon, chi tiết coupon, thông báo tạo/xóa thành công và thông báo validation chỉ là kết quả cần quan sát để xác nhận đúng sai của miền đầu vào.

Từ góc độ Business Analyst, có thể suy ra thêm vài ràng buộc thực tế dù đặc tả không nói quá chi tiết: `code` nên được hiểu là duy nhất toàn hệ thống; `discount_value` với `type = percent` không nên vượt 100; `expired_at` khi tạo mới nên là thời điểm tương lai; và `max_uses_per_user = 0` là một giá trị vô nghĩa vì coupon sẽ không thể dùng được. Những ràng buộc này hỗ trợ kiểm thử đúng bản chất nghiệp vụ coupon.

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

### 2. DOMAIN TEST CASES TABLE

| Test Case ID | Description | Pre-condition | Input Data | Test Steps | Expected Result | Actual Result | Status | Defect ID | Tested By | Date Tested |
|---|---|---|---|---|---|---|---|---|---|---|
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
