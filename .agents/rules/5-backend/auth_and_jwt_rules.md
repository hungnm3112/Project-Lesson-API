## QUY TẮC XÁC THỰC JWT, REFRESH TOKEN & PHÂN QUYỀN

### 1. Cơ chế JWT & Lưu trữ Token
- **Access Token:** Thời hạn ngắn (15–30 phút), gửi qua header `Authorization: Bearer <token>`.
- **Refresh Token:** Thời hạn dài (7 ngày), lưu dạng mã hóa **`sha512`** trong bảng `refresh_tokens`.
- **Refresh Token Rotation & Thu hồi:** Mỗi lần refresh phải hủy token cũ (`status = 0`) và cấp cặp mới. Khi khóa tài khoản (`status = 0`) hoặc đổi/đặt lại mật khẩu, phải thu hồi toàn bộ refresh token đang hiệu lực của người dùng đó.
- Trên Frontend Next.js: Lưu token trong **`httpOnly` Cookie** (`secure` ở production, `sameSite: 'lax'`), **CẤM** lưu trong `localStorage` / `sessionStorage`.

### 2. Bảo vệ Route & Phân quyền (Guards)
- Dùng `JwtAuthGuard` + `RolesGuard` (và `ScopeGuard` kiểm tra phạm vi dữ liệu được gán); đánh dấu `@Public()` cho các API không cần đăng nhập.
- Mã quyền chuẩn trong `roles.permissions`: `<resource>.<action>` với `action` thuộc `read`, `write`, `delete`.
- Luồng OTP (nếu có): Hết hạn sau 5 phút, lưu dạng hash trong DB, xóa ngay sau khi verify thành công.

