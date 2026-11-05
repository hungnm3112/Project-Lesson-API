## QUY CHUẨN MÃ LỖI & THỨ TỰ KIỂM SOÁT REQUEST (`4xx` / `5xx`)

### 1. Cấu trúc JSON Response khi Thất bại
HTTP Status đặt ở Response Header (`400`, `401`, `403`, `404`, `409`, `429`, `500`, `502`, `504`), không có `statusCode` trong body:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Dữ liệu không hợp lệ",
    "details": [{ "field": "phone", "message": "Số điện thoại không đúng định dạng" }]
  }
}
```

### 2. Thứ tự 8 Tầng Kiểm soát Request (Quyết định mã lỗi nào trả về trước)
1. **`401 UNAUTHORIZED` (`JwtAuthGuard`):** Thiếu token, token sai/hết hạn, sai mật khẩu, refresh token bị thu hồi.
2. **`403 FORBIDDEN` (`RolesGuard` / `ScopeGuard`):** Không đủ quyền RBAC, tài khoản bị khóa (`status = 0`), tự khóa chính mình, hoặc ngoài phạm vi dữ liệu được gán.
3. **`400 VALIDATION_ERROR` (`ValidationPipe`):** Sai kiểu dữ liệu, chứa thẻ HTML bị cấm, `:id` không phải `ObjectId`, hoặc `parent_id` trỏ vòng lặp.
4. **`404 NOT_FOUND` (`Service`):** Không tìm thấy bản ghi theo `:id` hoặc khóa ngoại tham chiếu không tồn tại.
5. **`409 CONFLICT` (`Service`):** Trùng trường `unique` (`ID`, `phone`, `sku`, `name`...) hoặc xóa bản ghi đang được tham chiếu.
6. **`429 TOO_MANY_REQUESTS` (`ThrottlerGuard`):** Vượt giới hạn tần suất gọi API (Rate Limit).
7. **`502 BAD_GATEWAY` / `504 GATEWAY_TIMEOUT`:** Dịch vụ tích hợp bên thứ 3 (AI, TTS, YouTube API...) báo lỗi hoặc timeout.
8. **`500 INTERNAL_SERVER_ERROR` (`HttpExceptionFilter`):** Lỗi hệ thống bất ngờ (log qua `nestjs-pino`, không lộ stack trace).

