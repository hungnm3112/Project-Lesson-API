## QUY TẮC UPLOAD FILE, LOGGING & UNIT TEST

### 1. Upload / Download File Tập trung (`FilesModule`)
- Mọi thao tác xử lý lưu file phải gọi qua `FilesService` chung, **CẤM** viết lại logic lưu file rải rác trong các module nghiệp vụ.
- **Validate ảnh upload:** Tối đa `5MB`, chỉ cho phép `image/jpeg`, `image/png`, `image/webp` (sai trả `400 Bad Request`).
- **Đặt tên file:** Đổi tên theo chuẩn `{slug}-{timestamp}.{ext}` (bỏ dấu tiếng Việt và ký tự đặc biệt). Thư mục `uploads/` phải nằm trong `.gitignore`.

### 2. Logging (`nestjs-pino`)
- **CẤM** dùng `console.log()`. Bắt buộc dùng `nestjs-pino` gắn Correlation ID cho mỗi request (JSON ở Production, pretty format ở Dev).

### 3. Unit Testing (Jest)
- Viết Unit Test tối thiểu cho: (1) Hàm chuẩn hóa chuỗi `unsign_search` / `generateSlug()`, (2) Ràng buộc toàn vẹn & phân quyền (`409 Conflict`, `403 Forbidden`), (3) Hàm tính toán nghiệp vụ cốt lõi.

