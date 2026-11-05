## BƯỚC 5.1: QUY TẮC ÁNH XẠ TỪ SƠ ĐỒ FDD SANG THIẾT KẾ DATABASE

Database không được vẽ cảm tính mà phải được **chuyển đổi trực tiếp từ Sơ đồ phân rã chức năng (FDD - Bước 4)** và **Bảng Actor - Hành động (Bước 3)** qua 3 bước:

### 1. Từ Nhánh Cấp 2 của FDD ➔ Xác định Danh sách Bảng (Collections)
Quét toàn bộ các chức năng Cấp 2 trên FDD và phân thành 3 loại ánh xạ:
1. **Chức năng quản lý Thực thể độc lập** *(VD: Quản lý nhân viên, Quản lý danh mục, Quản lý bộ truyện)* ➔ Tạo **1 Collection độc lập** (`employees`, `categories`, `novels`).
2. **Chức năng gán thuộc tính / phạm vi phụ thuộc vào Thực thể cha** *(VD: Gán kho phụ trách cho nhân viên, Quản lý mã SKU của sản phẩm)* ➔ **Không tách bảng mới**, ánh xạ thành **trường (Field/Array)** nằm trong bảng cha (`employees.warehouse_ids`, `subcategories.sku`).
3. **Chức năng Phiên làm việc / Chứng từ giao dịch / Tác vụ sinh lịch sử** *(VD: Xác thực phiên, Lập phiếu nhập, Ghi nhận thanh toán, Đối soát công nợ)* ➔ Tạo **Bảng Phiên / Chứng từ / Sổ cái** (`refresh_tokens`, `import_form`, `payment_form`, `debts`).

### 2. Từ Nhánh Cấp 3 của FDD & Cột Input/Output (Bước 3) ➔ Xác định Các Trường (Fields)
Mỗi bảng gồm 4 nhóm trường bắt buộc rà soát:
- **Nhóm 1 (Định danh & Hệ thống):** `_id` (`ObjectId`), `ID` (mã nghiệp vụ `unique` nếu có), `status`, `createdAt`, `updatedAt`.
- **Nhóm 2 (Dữ liệu đầu vào — từ Nhịp 1 Input Cấp 3 & Cột `Dữ liệu Input` Bước 3):** Mọi trường Actor gửi vào khi Tạo/Sửa đều phải có cột lưu trữ tương ứng.
- **Nhóm 3 (Dữ liệu xử lý & đầu ra — từ Nhịp 2 Process & Nhịp 3 Output Cấp 3):** Các trường hệ thống tự sinh hoặc tự tính (`unsign_search`, `total_amount`, `expired_at`...).
- **Nhóm 4 (Khóa ngoại — Foreign Keys):** Suy ra từ mối liên kết giữa các đối tượng trên FDD (`role_id`, `category_id`, `parent_id`...).

### 3. Từ Khóa ngoại giữa các Bảng ➔ Xác định Điều kiện Chặn xóa (`409 Conflict`)
- Nếu Bảng B có khóa ngoại trỏ về `_id` của Bảng A, thì khi gọi `DELETE` trên Bảng A bắt buộc phải kiểm tra Bảng B: nếu đã có bản ghi tham chiếu thì chặn xóa (`409 CONFLICT`) và yêu cầu khóa mềm (`status = 0`).

