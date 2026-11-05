# BƯỚC 1: KHẢO SÁT & HIỂU BÀI TOÁN (5 CÂU HỎI KHẢO SÁT NGHIỆP VỤ)

> **Nguồn đầu vào:** [Bước 0 — Đề bài & Yêu cầu gốc (`md/requirement.md`)](../requirement.md)  
> **Quy chuẩn áp dụng:** [`1_problem_survey_5_questions.md`](../../.agents/rules/2-analysis/1_problem_survey_5_questions.md)  
> **Chuỗi chuyển hóa:** `Pain Points (Nỗi đau thực tại)` ➔ `Business Goals (Mục tiêu hệ thống)` ➔ `System Boundary (Ranh giới hệ thống)` ➔ `Acceptance Criteria (Tiêu chí nghiệm thu)`

---

## 1. Câu 1: Tại sao cần xây dựng hệ thống này? (Business Value & System Boundary)

### 1.1. Mục đích kinh doanh (Business Value)
1. **Chuẩn hóa và tập trung hóa dữ liệu nền tảng (Single Source of Truth):** Thay thế việc quản lý rời rạc trên các file Excel tại 3 kho hàng của Công ty ABC bằng một cơ sở dữ liệu tập trung duy nhất cho toàn bộ danh mục Kho hàng, Danh mục sản phẩm, Đơn vị tính, Sản phẩm (`SKU`, `Barcode`), Nhà cung cấp và Khách hàng.
2. **Định danh, xác thực và kiểm soát chặt chẽ quyền hạn nhân sự:** Xác định rõ danh tính từng nhân viên đăng nhập vào hệ thống, phân quyền theo đúng vai trò (`Admin`, `Warehouse Manager`, `Warehouse Staff`, `Accountant`) và giới hạn phạm vi kho hàng mà mỗi nhân sự được phép phụ trách.
3. **Tạo nền tảng kiến trúc vững chắc để kiểm soát tồn kho & giao dịch:** Khi dữ liệu người dùng, phân quyền kho và mã định danh sản phẩm (`SKU`, `Barcode`) đã được chuẩn hóa ở 2 Module nền tảng, hệ thống sẵn sàng mở rộng sang các phân hệ Nhập - Xuất - Điều chuyển - Kiểm kê nhằm chấm dứt tình trạng lệch tồn kho, xuất âm kho hoặc mất dấu lịch sử biến động.

### 1.2. Ranh giới hệ thống (System Boundary — Phạm vi hiện tại)
- **Bên trong ranh giới hệ thống (In-Scope):**
  - Hệ thống Backend RESTful API xây dựng bằng **NestJS + MongoDB (Mongoose)** quản lý tập trung cho **Module 01 (Người dùng & Phân quyền)** và **Module 02 (Dữ liệu Nền tảng)**.
  - Cơ chế xác thực phiên làm việc (`JWT Access Token` + `Refresh Token` mã hóa `sha512`), phân quyền RBAC theo vai trò và phân công kho phụ trách (`warehouse_ids`).
  - Toàn bộ nghiệp vụ CRUD, tìm kiếm tiếng Việt không dấu (`unsign_search`), kiểm tra toàn vẹn tham chiếu (chặn xóa cứng `409 CONFLICT`, khóa mềm `status = 0`) cho: Tài khoản nhân viên, Vai trò, Kho hàng, Danh mục sản phẩm, Đơn vị tính, Sản phẩm (kèm `SKU`, `Barcode`), Nhà cung cấp, Khách hàng.
- **Bên ngoài ranh giới hệ thống (Out-of-Scope trong giai đoạn hiện tại):**
  - Các phân hệ giao dịch chứng từ Nhập kho, Xuất kho, Chuyển kho giữa 3 kho, Kiểm kê kho và Báo cáo Nhập - Xuất - Tồn theo kỳ (sẽ được phát triển ở các giai đoạn tiếp theo trên nền tảng dữ liệu của Module 01 & 02).
  - Khách hàng và Nhà cung cấp không trực tiếp đăng nhập vào hệ thống mà do nhân sự nội bộ của Công ty ABC nhập và quản lý thông tin.

---

## 2. Câu 2: Hiện tại quy trình đang làm như thế nào? (As-Is Process)

Trước khi có hệ thống quản lý kho tập trung, quy trình vận hành tại 3 kho hàng của Công ty ABC đang thực hiện thủ công bằng **Excel** theo các bước:

1. **Quản lý nhân sự và phân công kho thủ công:** Ban quản lý phân công miệng hoặc qua văn bản rời rạc xem nhân viên nào trực thuộc kho nào (Kho 1, Kho 2, Kho 3). Các file Excel dùng chung qua mạng nội bộ/email không có cơ chế đăng nhập định danh cá nhân hay khóa quyền chỉnh sửa theo từng vai trò.
2. **Tạo lập danh mục hàng hóa rời rạc ở từng kho:** Khi có thiết bị điện tử, linh kiện máy tính hoặc phụ kiện mới, nhân viên tại mỗi kho tự thêm dòng vào file Excel của kho mình (tự đặt tên sản phẩm, nhóm hàng, đơn vị tính, mã hàng).
3. **Quản lý mã SKU và Barcode không đồng bộ:** Mã `SKU` và `Barcode` được gõ tay vào các cột Excel mà không có công cụ kiểm tra trùng lặp trên toàn hệ thống 3 kho, dẫn đến cùng một linh kiện nhưng mỗi kho đặt một mã khác nhau hoặc hai mặt hàng bị trùng mã.
4. **Lưu trữ thông tin Nhà cung cấp và Khách hàng phân tán:** Thông tin đối tác (tên, số điện thoại, địa chỉ, mã số thuế...) được ghi chú rải rác trong các sheet nhập/xuất hàng, không có danh bạ đối tác chuẩn hóa.
5. **Ghi nhận nhập - xuất - điều chuyển và tổng hợp báo cáo thủ công:** Nhân viên kho tự cộng/trừ số lượng trên file Excel cá nhân; khi điều chuyển hàng giữa các kho hoặc cuối kỳ kế toán cần báo cáo nhập - xuất - tồn, kế toán phải gom từng file Excel từ 3 kho về ghép thủ công.

---

## 3. Câu 3: Vấn đề (Nỗi đau) hiện tại là gì? (Pain Points ➔ System Objectives)

Bảng chuyển hóa trực tiếp từ các nỗi đau thực tế tại Bước 0 và quy trình thủ công ở Câu 2 thành **Mục tiêu hệ thống (System Objectives)** trong phạm vi Module 01 & Module 02:

| STT | Vấn đề (Nỗi đau) hiện tại | Hệ quả thực tế | Mục tiêu hệ thống tương ứng (Module 01 & Module 02) |
|:---:|---|---|---|
| **1** | Không biết nhân viên nào thực hiện thao tác trên file Excel; không có cơ chế giới hạn quyền hạn giữa Quản lý, Nhân viên kho và Kế toán. | Khi xảy ra sai lệch dữ liệu hoặc thất thoát hàng hóa, không quy được trách nhiệm cá nhân; người không có thẩm quyền vẫn sửa/xóa nhầm dữ liệu. | **Mục tiêu 1 (Xác thực & Định danh):** Xây dựng cơ chế Đăng nhập, Đăng xuất, Refresh Token (`JWT` + `sha512`) và quản lý tài khoản nhân viên định danh duy nhất. |
| **2** | Không kiểm soát được ranh giới quyền hạn theo vai trò và nhân viên nào được phép phụ trách kho nào trong 3 kho. | Nhân viên kho A có thể xem/sửa nhầm dữ liệu của kho B; Kế toán hoặc Nhân viên kho có thể tự ý thay đổi cấu hình hệ thống. | **Mục tiêu 2 (RBAC & Phân công Kho):** Thiết lập phân quyền RBAC theo 4 vai trò (`Admin`, `Warehouse Manager`, `Warehouse Staff`, `Accountant`) và chức năng gán nhân viên phụ trách kho cụ thể. |
| **3** | Thông tin 3 kho hàng, danh mục sản phẩm và đơn vị tính bị ghi chép tự do, thiếu chuẩn hóa trên các file Excel rời rạc. | Tên kho, tên nhóm hàng và đơn vị tính không đồng nhất (cùng một loại ghi "Cái", "Chiếc", "Bộ" lộn xộn), không thể tổng hợp số liệu liên kho. | **Mục tiêu 3 (Chuẩn hóa Danh mục Nền tảng):** Cung cấp bộ API CRUD chuẩn hóa cho Kho hàng (`Warehouses`), Danh mục sản phẩm (`Categories`) và Đơn vị tính (`Units`) kèm kiểm soát trạng thái hoạt động. |
| **4** | Danh mục sản phẩm và mã định danh `SKU`, `Barcode` bị trùng lặp, sai lệch hoặc bỏ trống giữa 3 kho. | Nhầm lẫn mặt hàng khi nhập/xuất/kiểm kê, không tra cứu nhanh được theo mã vạch, dẫn đến không kiểm soát được tồn kho thực tế và xuất vượt tồn. | **Mục tiêu 4 (Quản lý Sản phẩm & Định danh SKU/Barcode):** Xây dựng CRUD Sản phẩm liên kết chặt với Danh mục & Đơn vị tính; quản lý mã `SKU` và `Barcode` duy nhất (`unique`), hỗ trợ tra cứu chính xác. |
| **5** | Thông tin Nhà cung cấp và Khách hàng lưu rải rác trên từng sheet Excel, trùng lặp và thiếu kiểm soát. | Khó truy xuất lịch sử giao dịch theo đối tác, dễ nhập trùng một nhà cung cấp/khách hàng nhiều lần với thông tin SĐT/mã khác nhau. | **Mục tiêu 5 (Quản lý Danh mục Đối tác):** Xây dựng CRUD Nhà cung cấp và Khách hàng tập trung, kiểm tra chống trùng lặp định danh (mã/số điện thoại) và hỗ trợ tìm kiếm nhanh. |
| **6** | Dữ liệu danh mục trên Excel dễ bị xóa nhầm dòng đang sử dụng, làm đứt gãy dữ liệu lịch sử và báo cáo. | Mất toàn vẹn dữ liệu tham chiếu (sản phẩm mất danh mục/đơn vị tính, kho bị xóa khi đang có nhân viên phụ trách). | **Mục tiêu 6 (Bảo toàn Toàn vẹn Tham chiếu):** Áp dụng cơ chế khóa mềm (`status = 0`) cho tài khoản/vai trò và chặn xóa cứng (`409 CONFLICT`) đối với mọi danh mục đang được tham chiếu. |

---

## 4. Câu 4: Ai là người tham gia sử dụng? (Stakeholders & Raw Users)

Phác thảo danh sách các nhóm đối tượng có xuất hiện trong bài toán và nhu cầu của họ:

| STT | Nhóm đối tượng (thô) | Căn cứ trong đề bài (`md/requirement.md`) | Nhu cầu chính trên hệ thống (Module 01 & Module 02) |
|:---:|---|---|---|
| **1** | **Quản trị viên hệ thống (`Admin`)** | Module 01 — Vai trò `Admin`: Toàn quyền hệ thống. | Quản lý tài khoản nhân viên, quản lý vai trò & phân quyền RBAC, gán nhân viên phụ trách kho, và toàn quyền cấu hình/CRUD mọi danh mục ở Module 02. |
| **2** | **Quản lý kho (`Warehouse Manager`)** | Module 01 — Vai trò `Warehouse Manager`: Quản lý kho. | Đăng nhập hệ thống, gán/hủy gán nhân viên phụ trách cho chính kho mình đang quản lý, quản lý (thêm/sửa) và tra cứu danh mục sản phẩm, `SKU`/`Barcode`, đơn vị tính, đối tác (nhà cung cấp, khách hàng, khách thợ) phục vụ vận hành kho. |
| **3** | **Nhân viên kho (`Warehouse Staff`)** | Module 01 — Vai trò `Warehouse Staff`: Nhân viên kho. | Đăng nhập hệ thống, tra cứu danh sách sản phẩm, quét/tìm theo mã `SKU`/`Barcode`, xem danh mục nhóm hàng, đơn vị tính, đối tác và thông tin kho mình được gán phụ trách. |
| **4** | **Kế toán (`Accountant`)** | Module 01 — Vai trò `Accountant`: Kế toán, xem báo cáo. | Đăng nhập hệ thống, tra cứu thông tin kho hàng, danh mục sản phẩm, giá sản phẩm, đơn vị tính, danh sách đối tác (nhà cung cấp, khách hàng, khách thợ) để phục vụ đối soát và lên báo cáo. |
| **5** | **Đối tác: Khách hàng, Nhà cung cấp, Khách thợ (`Customers`)** | Module 02 — Chức năng: CRUD nhà cung cấp, CRUD khách hàng. | *Không trực tiếp đăng nhập hệ thống* — chỉ là đối tượng dữ liệu (Data Entity) được nhân sự công ty ABC lưu trữ chung trong 1 danh mục đối tác (phân biệt qua `type`: `1` Khách hàng, `2` Nhà cung cấp, `3` Khách thợ). |

---

## 5. Câu 5: Kết quả mong muốn là gì? (Acceptance Criteria)

Định nghĩa tiêu chí nghiệm thu cụ thể, kiểm chứng được cho từng mục tiêu hệ thống đã xác lập ở Câu 3:

| Mục tiêu hệ thống | Tiêu chí đạt cụ thể (Acceptance Criteria — Kiểm chứng được qua API) |
|---|---|
| **Mục tiêu 1: Xác thực & Định danh người dùng** | • Đăng nhập trả về cặp `access_token` (JWT ngắn hạn) và `refresh_token` (lưu DB dạng hash `sha512`).<br>• Refresh token thực hiện cơ chế **Rotation** (hủy token cũ `status = 0`, cấp cặp mới).<br>• Khi đăng xuất, khóa tài khoản (`status = 0`) hoặc đổi/đặt lại mật khẩu, toàn bộ `refresh_tokens` đang hiệu lực của nhân viên đó bị thu hồi ngay lập tức.<br>• Mật khẩu nhân viên lưu dạng `sha512` và tuyệt đối không bao giờ xuất hiện trong JSON response. |
| **Mục tiêu 2: Quản lý Nhân viên, Phân quyền RBAC & Gán kho phụ trách** | • Hệ thống hỗ trợ đầy đủ 4 vai trò (`Admin`, `Warehouse Manager`, `Warehouse Staff`, `Accountant`) lưu trong bảng `roles` kèm bộ mã quyền `<resource>.<action>` (`read`, `write`, `delete`).<br>• Gán và hủy gán được kho phụ trách cho nhân viên: `Admin` được gán trên toàn bộ các kho; `Warehouse Manager` được gán/hủy gán nhân viên phụ trách đối với chính kho mà mình đang quản lý.<br>• Lưu danh sách kho phụ trách dạng mảng `warehouse_ids: ObjectId[]` trên bảng nhân viên (cho phép 1 nhân viên phụ trách 1 hoặc nhiều kho).<br>• Mọi API đều được bảo vệ bởi `JwtAuthGuard`, `RolesGuard` và `ScopeGuard` (trả `401` khi chưa xác thực, `403` khi không đủ quyền, ngoài phạm vi kho quản lý hoặc tài khoản bị khóa). |
| **Mục tiêu 3: Chuẩn hóa Danh mục Nền tảng (Kho hàng, Danh mục SP, Đơn vị tính)** | • CRUD đầy đủ cho Kho hàng (`warehouses`), Danh mục sản phẩm (`categories`), Đơn vị tính (`units`).<br>• Danh mục sản phẩm hỗ trợ phân cấp cha - con (`parent_id`) và chặn lỗi trỏ vòng lặp (`400 VALIDATION_ERROR`).<br>• Tự động sinh trường `unsign_search` (chữ thường, cả có dấu và không dấu) khi Tạo/Sửa để tìm kiếm tiếng Việt chính xác qua `?keyword=...`. |
| **Mục tiêu 4: Quản lý Sản phẩm & Mã định danh SKU, Barcode** | • CRUD Sản phẩm (`products`) gắn khóa ngoại hợp lệ tới Danh mục sản phẩm (`category_id`) và Đơn vị tính (`unit_id`).<br>• Lưu trữ và cập nhật mã `sku`, `barcode` trực tiếp trên bảng `products` đảm bảo tính duy nhất (`unique` / Partial Unique Index — trùng trả `409 CONFLICT`), hỗ trợ tra cứu nhanh sản phẩm theo `sku` hoặc `barcode`. |
| **Mục tiêu 5: Quản lý Danh mục Đối tác (Khách hàng, Nhà cung cấp, Khách thợ)** | • Hợp nhất lưu trữ trong 1 Collection duy nhất (`customers`), phân biệt qua trường `type` (`1` = Khách hàng, `2` = Nhà cung cấp, `3` = Khách thợ) vì một đối tác vừa có thể là nhà cung cấp vừa là khách hàng.<br>• Kiểm tra định dạng số điện thoại/email, chống trùng lặp mã/số điện thoại (`409 CONFLICT`) và hỗ trợ lọc theo `?type=1|2|3`, `keyword`, `status`, `fromDate`, `toDate`. |
| **Mục tiêu 6: Toàn vẹn Tham chiếu & Chặn xóa an toàn** | • Tài khoản nhân viên và Vai trò không có API xóa cứng (`DELETE`), chỉ cho phép khóa/mở khóa qua `PUT /:id/status`.<br>• Khi gọi `DELETE /:id` trên Kho hàng, Danh mục SP, Đơn vị tính, Sản phẩm, Đối tác (`customers`): nếu đã có bản ghi con hoặc bảng khác tham chiếu tới thì chặn xóa và trả `409 CONFLICT`, yêu cầu dùng khóa mềm (`status = 0`). |

---

## 6. Làm rõ Chi tiết Nghiệp vụ & Kiến trúc (`[Đã xác nhận]`)

Toàn bộ 6 điểm kiến trúc & nghiệp vụ trong Bước 1 đã được xác nhận:

1. **`[Đã xác nhận 1]` — Cấu trúc lưu trữ Mã `SKU`, `Barcode` và Sản phẩm (Module 02):**
   - Sử dụng 1 bảng Sản phẩm (`products`) lưu thông tin mặt hàng gốc (gắn `category_id`, `unit_id`), và tích hợp trực tiếp trường `sku` cùng `barcode` (dùng Partial Unique Index) ngay trên bảng `products`, kèm chức năng cập nhật & tra cứu theo `SKU`/`Barcode`.
2. **`[Đã xác nhận 2]` — Hợp nhất Đối tác trong 1 Collection `customers` (Module 02):**
   - Sử dụng **1 Collection duy nhất là `customers`** vì nhà cung cấp cũng có thể đồng thời là khách hàng. Phân biệt loại đối tác qua trường `type`:
     - `type = 1`: **Khách hàng** (Khách lẻ)
     - `type = 2`: **Nhà cung cấp**
     - `type = 3`: **Khách thợ**
3. **`[Đã xác nhận 3]` — Phân quyền RBAC & Quyền gán kho của `Warehouse Manager` (Module 01 & Module 02):**
   - **`Admin`:** Toàn quyền (`read`, `write`, `delete`) trên cả Module 01 và Module 02.
   - **`Warehouse Manager`:** Có quyền xem (`read`) thông tin kho & nhân viên thuộc kho mình quản lý; **có quyền gán / hủy gán nhân viên phụ trách kho đối với chính Kho (`Warehouse`) mà mình đang quản lý**; có quyền xem và thêm/cập nhật (`read`, `write`) trên Danh mục SP, Đơn vị tính, Sản phẩm (`SKU`/`Barcode`) và Đối tác (`customers`); chỉ `Admin` mới có quyền thêm/sửa/xóa Kho hàng và xóa cứng (`delete`) danh mục gốc.
   - **`Warehouse Staff` & `Accountant`:** Chỉ có quyền xem/tra cứu (`read`) danh mục sản phẩm, `SKU`/`Barcode`, kho hàng, đơn vị tính và đối tác (`customers`).
4. **`[Đã xác nhận 4]` — Cấu trúc phân cấp Danh mục sản phẩm (`categories`):**
   - Danh mục sản phẩm hỗ trợ phân cấp cây **Cha - Con đa cấp** (`parent_id: ObjectId, default: null`, ví dụ: *Linh kiện máy tính ➔ CPU / RAM / Mainboard*).
5. **`[Đã xác nhận 5]` — Quan hệ Gán nhân viên phụ trách kho (`warehouse_ids`):**
   - Một nhân viên có thể được gán phụ trách **1 hoặc nhiều kho** (lưu dạng mảng `warehouse_ids: ObjectId[]` trực tiếp trong bảng tài khoản nhân viên `employees`/`users`, không tách bảng trung gian).
6. **`[Đã xác nhận 6]` — Cơ chế Quản lý Vai trò (`roles`):**
   - Hệ thống có bảng `roles` lưu 4 vai trò chuẩn (`Admin`, `Warehouse Manager`, `Warehouse Staff`, `Accountant`) kèm mảng quyền `permissions: string[]`, và cung cấp API cho `Admin` xem danh sách vai trò cũng như cập nhật phân quyền (`permissions`) cho từng vai trò.

---

## 7. Bảng Tổng hợp Quyết định Nghiệp vụ & Kiến trúc (`[Đã xác nhận]`)

| Mã QĐ | Hạng mục kiến trúc | Nội dung quyết định | Trạng thái |
|:---:|---|---|:---:|
| **QĐ-01** | **Sản phẩm & Mã SKU / Barcode** | Mã `sku` và `barcode` là thuộc tính (`unique` / Partial Unique Index) nằm trực tiếp trong bảng Sản phẩm (`products`), kèm chức năng cập nhật & tra cứu theo `SKU`/`Barcode`. | `[Đã xác nhận]` |
| **QĐ-02** | **Hợp nhất Đối tác (`customers`)** | Lưu chung trong 1 Collection `customers`, phân biệt bằng trường `type`: `1` = Khách hàng, `2` = Nhà cung cấp, `3` = Khách thợ. | `[Đã xác nhận]` |
| **QĐ-03** | **Ma trận quyền hạn (RBAC & SoD)** | `Admin` toàn quyền Module 01 & 02; `Warehouse Manager` được gán/hủy gán nhân viên vào chính kho mình đang quản lý và `read`/`write` danh mục hàng hóa & đối tác; `Warehouse Staff` & `Accountant` chỉ có quyền `read` tra cứu. | `[Đã xác nhận]` |
| **QĐ-04** | **Cấu trúc Danh mục SP (`categories`)** | Hỗ trợ phân cấp cây Cha - Con đa cấp qua trường tự tham chiếu `parent_id: ObjectId, default: null`. | `[Đã xác nhận]` |
| **QĐ-05** | **Lưu trữ Gán kho phụ trách** | Lưu mảng `warehouse_ids: ObjectId[]` trực tiếp trong bảng tài khoản nhân viên (1 nhân viên có thể phụ trách 1 hoặc nhiều kho). | `[Đã xác nhận]` |
| **QĐ-06** | **Quản lý Vai trò (`roles`)** | Lưu 4 vai trò trong bảng `roles` kèm mảng `permissions`, cho phép `Admin` xem và cấu hình/cập nhật quyền theo vai trò. | `[Đã xác nhận]` |

