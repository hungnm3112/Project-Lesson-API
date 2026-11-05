## QUY TẮC UI/UX, BẢNG DỮ LIỆU, BUTTON & HÌNH ẢNH

### 1. Thông báo & Hộp thoại Xác nhận
- **CẤM** dùng `alert()`, `confirm()`, `prompt()` của trình duyệt.
- Bắt buộc dùng Toast (xanh tự ẩn sau 3s khi thành công, đỏ có nút đóng khi lỗi) và Confirm Modal khi xóa/khóa.

### 2. Bảng Dữ liệu & Chống Thao tác Trùng
- Mọi màn hình danh sách **BẮT BUỘC** có: Ô tìm kiếm (`keyword`), Lọc từ ngày – đến ngày (`fromDate`, `toDate`), Phân trang (`page`, `limit`), trạng thái Loading và Empty state.
- Khi bấm lưu/thêm/sửa/xóa: **BẮT BUỘC** khóa nút (`disabled` / `isLoading`) và hiện spinner để chống click đúp (duplicate request).

### 3. Nút bấm (`<button>` vs `<Link>`) & Hình ảnh (`next/image`)
- Mọi nút thao tác phải có **icon đi kèm text**. Dùng `<button>` cho hành động (submit, toggle, delete) và `<Link>` (`next/link`) cho chuyển trang (cấm dùng thẻ `<a>` trần, trừ `tel:` và `mailto:`).
- Hình ảnh bắt buộc dùng `next/image` (cấm dùng `<img>` trần) và phải khai báo rõ `alt`, `width`/`height` (hoặc `fill`).

