## QUY TẮC RESTFUL URI, HTTP METHODS & SUB-RESOURCES

### 1. Quy ước Định danh URI & Thứ tự Route
- **Prefix bắt buộc:** `/api/v1/<resources>`.
- **Resource Naming:** Dùng **danh từ số nhiều**, chữ thường `kebab-case`, trùng tên Collection DB (dấu `_` đổi thành `-`). **CẤM** đưa động từ vào URI.
- **Thứ tự Route trong Controller:** Các route tĩnh/đặc thù (`GET /tree`, `GET /lookup`, `GET /trending`) **bắt buộc khai báo phía trên** route động `GET /:id`.

### 2. Quy ước HTTP Methods & Tính Idempotency
- **`GET` (Safe, Idempotent):** Truy vấn danh sách, chi tiết `/:id`, cây `/tree`, tra cứu `/lookup`.
- **`POST` (Non-idempotent):** Tạo mới bản ghi (`201 Created`), kích hoạt tiến trình chạy ngầm (`202 Accepted`), hoặc xác thực phiên (`/auth/login`, `/auth/refresh-token`, `/auth/logout` — `200 OK`).
- **`PUT` (Idempotent) — CẤM DÙNG `PATCH`:** Bắt buộc dùng `PUT` cho **toàn bộ** thao tác cập nhật (cả toàn phần lẫn một phần qua DTO `PartialType`) để tránh lỗi tương thích trên Reverse Proxy/WAF/Cloudflare.
- **`DELETE` (Idempotent):** Xóa cứng bản ghi chưa có tham chiếu (`200 OK`); chặn `409 CONFLICT` nếu đang được tham chiếu.

### 3. Quy tắc Tách Sub-resource khi Cập nhật (`PUT`)
- `PUT /:id`: Chỉ cập nhật thông tin thuộc tính thông thường của bản ghi.
- Các thao tác có **hiệu ứng phụ (side-effects)** hoặc phân quyền riêng phải tách thành sub-resource: `PUT /:id/status`, `PUT /:id/role`, `PUT /:id/password`, `PUT /auth/password`.

