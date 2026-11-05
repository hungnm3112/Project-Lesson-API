## QUY TRÌNH & ĐỊNH DẠNG BẢNG ĐẶC TẢ DATABASE (4 CỘT CHUẨN)

### 1. Quy trình Thiết kế Database Bắt buộc
1. Viết bảng đặc tả cho từng Collection trong file Markdown (thư mục `md/analysis/db`) trước khi viết code.
2. Chờ người dùng review và xác nhận bảng đặc tả.
3. Sau khi xác nhận mới chuyển thành Mongoose Schema và DTO.

### 2. Định dạng Bảng Đặc tả 4 Cột
Mỗi Collection là 1 bảng gồm đúng 4 cột, dòng đầu tiên luôn là `_id`:

| field | type | required | des |
|---|---|---|---|
| _id | ObjectId | true | _id hệ thống tự sinh |

- `type`: kiểu dữ liệu, kèm `unique` / `Index` nếu có.
- `required`: `true`, kèm giá trị mặc định nếu có (`default = 1`, `default: null`, `default : [ ]`, `default: Date.now`).
- `des`: mô tả ý nghĩa trường; trường trạng thái phải ghi rõ từng giá trị.
- **Quy tắc tách dòng:** Mỗi trường (`field`) bắt buộc tách thành **1 dòng riêng biệt**, tuyệt đối không gộp nhiều trường vào chung 1 dòng.

