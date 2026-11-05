## KIẾN TRÚC MODULE & CẤU HÌNH BACKEND (NESTJS)

### 1. Kiến trúc Module & Luồng Xử lý Bắt buộc
- Mỗi miền nghiệp vụ tách thành 1 NestJS Module riêng (`UsersModule`, `AuthModule`, `CategoriesModule`...).
- Luồng bắt buộc: **`Controller` (nhận request, validate DTO) ➔ `Service` (chứa 100% logic nghiệp vụ, gọi DB, tính toán) ➔ `Schema/Model`**.
- **CẤM** viết truy vấn DB hay Aggregation Pipeline trực tiếp trong Controller.

### 2. Đọc Biến Môi trường (`ConfigService`)
- Bắt buộc dùng `ConfigService` (`@nestjs/config`) để đọc biến môi trường.
- **CẤM** gọi trực tiếp `process.env` trong Service/Controller (trừ khi bootstrap tại `app.module.ts` / `main.ts`).

### 3. Global Pipes & Exception Filter
- Bật `ValidationPipe` toàn cục (`whitelist: true`, `forbidNonWhitelisted: true`, `transform: true`).
- Dùng `HttpExceptionFilter` toàn cục để chuẩn hóa mọi response lỗi về `{ error: { code, message, details } }`.

