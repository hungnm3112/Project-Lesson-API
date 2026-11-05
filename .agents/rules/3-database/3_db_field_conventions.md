## QUY ƯỚC TRƯỜNG DỮ LIỆU DATABASE (FIELD CONVENTIONS)

1. **Định danh kép (`_id` và `ID`):**
   - `_id` (`ObjectId`): hệ thống tự sinh, dùng làm khóa chính và khóa ngoại tham chiếu (`ref`).
   - `ID`: mã định danh nghiệp vụ (`unique`) dạng `String` (VD: `NV001`, `KHO01`), `Number` tự sinh, hoặc `String, default: Date.now` (mã QR cá thể).
2. **Tên bảng và tên trường:** Dùng `snake_case` toàn bộ (`parent_id`, `customer_id`, `total_amount`, `unsign_search`). Ngoại lệ: `_id`, `ID`, `createdAt`, `updatedAt`.
3. **Trạng thái (`status`):** Mặc định `status: Number, Index, true, default = 1` (`0` dừng hoạt động/khóa, `1` đang hoạt động). Riêng cá thể sản phẩm cuối + QR dùng `status: Boolean, default: false`.
4. **Tìm kiếm tiếng Việt (`unsign_search`):** `String, Index`. Server tự sinh khi tạo/sửa, lưu **cả nội dung có dấu và không dấu (chữ thường)** của các trường tìm kiếm chính.
5. **Mật khẩu & Token:** Mã hóa **`sha512`**, khai báo `select: false` cho `password` trong Schema.
6. **Giá trị mặc định (`default`):**
   - Tham chiếu/ngày không bắt buộc: `default: null`. Mảng: `default : [ ]`.
   - Tiền, số lượng, thuế, bảo hành, giảm giá: `Number, default = 0`. Chiết khấu `ck`: `Number, default = 0, max = 100`.
   - Ngày tạo chứng từ/sổ cái: `date: Date, Index, true, default: Date.now`.
   - Bật `timestamps: true` cho mọi Schema.

