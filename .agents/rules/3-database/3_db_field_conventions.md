## QUY ƯỚC TRƯỜNG DỮ LIỆU DATABASE (FIELD CONVENTIONS)

1. **Cơ chế Định danh (`_id` và mã nghiệp vụ `ID`):**
   - **`_id` (`ObjectId`, bắt buộc 100% các bảng):** Khóa chính kỹ thuật do hệ thống/MongoDB tự sinh, dùng làm định danh duy nhất của bản ghi và làm khóa ngoại tham chiếu (`ref`) giữa các bảng.
   - **`ID` (Mã định danh nghiệp vụ, `unique` — CHỈ DÙNG KHI CÓ NHU CẦU NGHIỆP VỤ THỰC TẾ):**
     - **Nguyên tắc cốt lõi:** Tuyệt đối KHÔNG tự động gắn trường `ID` cho mọi bảng một cách cảm tính/máy móc.
     - **Khi NÀO DÙNG `ID`:** Chỉ áp dụng cho các Thực thể nghiệp vụ hoặc Chứng từ giao dịch mà con người/kế toán cần một mã định danh ngắn gọn ngoài đời để đối soát, in phiếu, dán tem hoặc tìm kiếm nhanh:
       - *Thực thể con người & Đối tác:* `employees` (Mã nhân viên: `NV001`), `customers` (Mã đối tác: `KH001`, `NCC001`).
       - *Chứng từ & Sổ cái giao dịch:* `import_forms` (`PN001`), `export_forms` (`PX001`), `debts` (`CN001`).
       - *Cá thể hàng hóa QR serial (nếu có):* `products` cá thể (`String, default: Date.now`).
       - Định dạng: `String, unique` (tiền tố chữ + số tự tăng) hoặc `Number, unique` tự sinh.
     - **Khi NÀO KHÔNG DÙNG `ID`:**
       - *Bảng cấu hình hệ thống & Phân quyền:* `roles` (dùng `name` unique: `admin`, `warehouse_manager`...), `permissions`, `refresh_tokens`.
       - *Bảng danh mục phân loại ngắn / chọn theo tên:* `units` (Đơn vị tính: "Cái", "Hộp" ➔ dùng `name` unique), `categories` (Danh mục phân cấp ➔ dùng `name` + `parent_id`), `warehouses` (Kho hàng ➔ dùng `name` unique).
       - *Sản phẩm thương mại đã có chuẩn mã chuyên dụng:* Sản phẩm (`products`) đã có mã `sku` và `barcode` làm định danh thương mại duy nhất ➔ KHÔNG sinh thêm trường `ID` để tránh trùng lặp 3 loại mã.
2. **Tên bảng và tên trường:** Dùng `snake_case` toàn bộ (`parent_id`, `customer_id`, `total_amount`, `unsign_search`). Ngoại lệ: `_id`, `ID`, `createdAt`, `updatedAt`.
3. **Trạng thái (`status`):** Mặc định `status: Number, Index, true, default = 1` (`0` dừng hoạt động/khóa, `1` đang hoạt động). Riêng cá thể sản phẩm cuối + QR dùng `status: Boolean, default: false`.
4. **Tìm kiếm tiếng Việt (`unsign_search`):** `String, Index`. Server tự sinh khi tạo/sửa, lưu **cả nội dung có dấu và không dấu (chữ thường)** của các trường tìm kiếm chính.
5. **Mật khẩu & Token:** Mã hóa **`sha512`**, khai báo `select: false` cho `password` trong Schema.
6. **Giá trị mặc định (`default`):**
   - Tham chiếu/ngày không bắt buộc: `default: null`. Mảng: `default : [ ]`.
   - Tiền, số lượng, thuế, bảo hành, giảm giá: `Number, default = 0`. Chiết khấu `ck`: `Number, default = 0, max = 100`.
   - Ngày tạo chứng từ/sổ cái: `date: Date, Index, true, default: Date.now`.
   - Bật `timestamps: true` cho mọi Schema.

