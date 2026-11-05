## QUY TẮC BẢO MẬT & SANITIZE DỮ LIỆU (SECURITY)

### 1. Chống XSS & Sanitize Input
- **Trường văn bản thường (tên, SĐT, email, địa chỉ, mã):** CẤM chứa thẻ HTML.
- **Trường cho phép nhập HTML / Rich Text:** Bắt buộc gọi hàm `sanitizeContent` dùng chung tại `src/common/helpers/sanitize-content.helper.ts` **chỉ lúc GHI (Create/Update)** ở Backend, tuyệt đối không gọi lúc ĐỌC.
- **CẤM:** Dùng `eval()`, `new Function()`, `innerHTML`, hoặc `dangerouslySetInnerHTML` với dữ liệu chưa sanitize.

### 2. Bảo vệ Hạ tầng (Helmet, CORS, Rate Limit, Injection)
- **CSP:** Bật `app.use(helmet())` tại `main.ts`.
- **CORS:** Đọc danh sách domain whitelist từ `ConfigService` (`ALLOWED_ORIGINS`), cấm để `origin: '*'` trên production.
- **Rate Limiting:** Dùng `@nestjs/throttler` giới hạn tần suất các API nhạy cảm (ví dụ: tối đa 5 lần gọi `/auth/login` trong 60 giây).
- **Chống NoSQL Injection:** Không nối chuỗi trực tiếp vào query; luôn kiểm tra `Types.ObjectId.isValid(id)`.

