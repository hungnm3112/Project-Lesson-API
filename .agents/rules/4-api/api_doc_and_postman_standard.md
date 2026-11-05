## QUY CHUẨN BỐ CỤC TÀI LIỆU API & COLLECTION KIỂM THỬ

### 1. Bố cục 4 Phần của File Tài liệu API (`md/api-module-xx.md`)
1. **Mục 1 — Quy ước chung & Ma trận Phân quyền (RBAC):** Bảng quyền của từng Role trên từng Resource và danh mục mã quyền `<resource>.<action>` (`read`, `write`, `delete`).
2. **Mục 2 — Bảng Danh sách Tổng hợp API:** Cột `# | Method | Endpoint | Chức năng (Mã FDD) | Quyền`.
3. **Mục 3 — Chi tiết từng API:**
   - `Method` + `Endpoint` + `Quyền truy cập`.
   - `Query Parameters` hoặc `Request Body` JSON mẫu (`snake_case`).
   - Bảng điều kiện Validate (`Trường | Bắt buộc | Kiểm tra`).
   - Logic xử lý ngầm tại Service (sinh `unsign_search`, hash `sha512`, đẩy hàng đợi Job, thu hồi `refresh_tokens`...).
   - `Response 200 / 201 / 202` JSON mẫu và các mã lỗi có thể xảy ra.
4. **Mục 4 — Bảng Tổng hợp Điều kiện Chặn xóa (`409 CONFLICT`):** Ghi rõ từng endpoint `DELETE` bị chặn khi còn tham chiếu tại `Bảng.Trường` nào.

### 2. Chuẩn Collection Postman / Thunder Client
- Chia folder theo đúng cấu trúc nhánh FDD; đặt tên request gắn kèm mã FDD.
- Không hardcode URL, Token hay ID; bắt buộc dùng biến môi trường `{{base_url}}`, `{{access_token}}`, `{{..._id}}`.

