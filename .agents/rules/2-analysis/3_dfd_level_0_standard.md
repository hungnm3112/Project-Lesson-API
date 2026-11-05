## BƯỚC 3: THIẾT KẾ SƠ ĐỒ NGỮ CẢNH (DFD LEVEL 0 STANDARD)
> **File đầu ra:** `md/analysis/3-dfd-context-diagram.md`

### 1. Nguyên tắc Hộp đen (Yourdon–DeMarco) & Định danh Mã Hành động
- **Nguyên lý Hộp đen (Black Box — Ký hiệu Yourdon–DeMarco):** Toàn bộ hệ thống được biểu diễn bằng 1 khối tròn trung tâm duy nhất `SYS(("<b>Tên Hệ Thống</b><br/>Process 0"))` (màu tím `#4338ca`). Tuyệt đối không vẽ các bảng Database hay phân hệ con bên trong sơ đồ DFD Level 0.
- **Quy tắc định danh mã hành động trong Bảng 5 cột:** Bắt buộc chuẩn hóa theo cú pháp **`[Tên vai trò - STT]`** (ví dụ: `[Admin - 01]`, `[Quản lý kho - 02]`). Cấm dùng chữ viết tắt tối nghĩa (`ND1`, `QT2`).

### 2. Bảng Đặc tả 5 Cột & Quy tắc Vẽ Sơ đồ DFD Level 0
- Trong tài liệu đặc tả, bắt buộc lập **Bảng Actor - Hành động gồm 5 cột tinh gọn** (làm nguồn dữ liệu trực tiếp để xây dựng FDD ở Bước 4 và Database ở Bước 5):
  `| Actor | Mã | Hành động | Dữ liệu Input (Actor gửi vào) | Dữ liệu Output (Actor nhận lại) |`
- **Quy tắc hiển thị trên Sơ đồ Mermaid DFD Level 0 (Chống rối & đè chữ):**
  - Đặt sơ đồ ở **Mục 2** của tài liệu; mỗi Actor có **đúng 2 mũi tên song song** nối với `Process 0` (không cắt nhau):
    - **Mũi tên màu xanh lá (`#10b981`):** Luồng vào hệ thống (**Input** — Actor ➔ Hệ thống).
    - **Mũi tên màu cam (`#f97316`):** Luồng ra khỏi hệ thống (**Output** — Hệ thống ➔ Actor).
  - **Nhãn trên mũi tên sơ đồ:** Chỉ giữ **tên nhóm dữ liệu ngắn gọn** (tối đa 2 dòng, mỗi dòng dưới 20 ký tự); chi tiết mã `[Vai trò - STT]` đặt ở Bảng đặc tả 5 cột.


