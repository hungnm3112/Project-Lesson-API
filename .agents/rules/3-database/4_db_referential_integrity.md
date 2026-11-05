## QUY TẮC TOÀN VẸN THAM CHIẾU & XÓA / KHÓA BẢN GHI

### 1. Quy tắc Khóa mềm (`status = 0`) vs Xóa cứng (`DELETE`)
- **Tài khoản người dùng (`employees`/`users`) & Vai trò (`roles`):** Tuyệt đối không xóa cứng, chỉ khóa bằng `PUT /:id/status` (`status = 0`).
- **Các bảng Danh mục & Dữ liệu nguồn:**
  - Chỉ cho phép xóa cứng (`DELETE /:id`) khi bản ghi **chưa được bất kỳ bảng nào khác tham chiếu tới**.
  - Nếu bản ghi đã được tham chiếu (có danh mục con, có sản phẩm, hoặc đã phát sinh chứng từ/tiến trình), **bắt buộc chặn xóa và trả lỗi `409 CONFLICT`**, yêu cầu người dùng chuyển sang khóa mềm (`status = 0`).

### 2. Ràng buộc Khóa ngoại khi Tạo / Sửa
- Mọi khóa ngoại (`role_id`, `category_id`, `parent_id`...) truyền vào khi `POST` / `PUT` phải kiểm tra tồn tại và đang hoạt động (`status = 1`); nếu không tồn tại trả `404 NOT_FOUND`.
- Với danh mục đa cấp (`parent_id`), khi cập nhật phải kiểm tra chống trỏ vòng lặp (không trỏ vào chính nó hoặc danh mục con của nó).

