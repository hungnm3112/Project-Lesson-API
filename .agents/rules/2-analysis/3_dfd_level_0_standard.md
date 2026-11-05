## BƯỚC 3: THIẾT KẾ SƠ ĐỒ NGỮ CẢNH (DFD LEVEL 0 STANDARD)

### 1. Nguyên tắc Hộp đen & Định danh Mã Hành động
- **Nguyên lý Hộp đen (Black Box):** Toàn bộ hệ thống được biểu diễn bằng 1 khối trung tâm duy nhất `[0] Tên Hệ Thống`. Tuyệt đối không vẽ các bảng Database hay phân hệ con bên trong sơ đồ DFD Level 0.
- **Quy tắc định danh mã hành động rõ nghĩa:** Bắt buộc chuẩn hóa theo cú pháp **`[Tên vai trò - STT]`** (ví dụ: `[Người dùng - 01]`, `[Quản trị - 02]`, `[YouTube API - 01]`). Cấm dùng chữ viết tắt tối nghĩa (`ND1`, `QT2`).

### 2. Luồng Dữ liệu Đối xứng & Bảng Đặc tả 7 Cột
- Trên sơ đồ DFD Level 0, mỗi hành động tương tác luôn gồm cặp 2 mũi tên mang cùng mã số:
  - **Mũi tên Input (Actor ➔ Hệ thống):** `[Vai trò - STT] Động từ + Danh từ`
  - **Mũi tên Output (Hệ thống ➔ Actor):** `[Vai trò - STT] Kết quả + tên hành động`
- Trong tài liệu đặc tả, bắt buộc lập **Bảng Actor - Hành động gồm đủ 7 cột** (làm nguồn dữ liệu trực tiếp để xây dựng FDD ở Bước 4):
  `| Actor | Mã | Hành động | Nhãn Input (DFD) | Dữ liệu Input (Actor gửi vào) | Nhãn Output (DFD) | Dữ liệu Output (Actor nhận lại) |`

