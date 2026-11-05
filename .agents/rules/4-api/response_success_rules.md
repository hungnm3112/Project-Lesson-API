## QUY CHUẨN DỮ LIỆU TRẢ VỀ KHI THÀNH CÔNG (`2xx` RESPONSE)

### 1. Nguyên tắc Chung
- HTTP Status Code đặt ở **Response Header** (`200`, `201`, `202`). **TUYỆT ĐỐI KHÔNG** lặp trường `statusCode` trong JSON body.
- Tên trường trong `data` dùng `snake_case`.
- **Trường CẤM trả về:** Tuyệt đối ẩn `password`, `unsign_search`, `__v` trong mọi response thành công.
- **Populate khóa ngoại:** Trả kèm object rút gọn khi cần hiển thị tên (VD: `"category": { "_id": "...", "name": "..." }`).

### 2. Bảng Quy chuẩn Nội dung bên trong `{ "data": ... }`

| Thao tác | Method | HTTP Header | Cấu trúc `{ "data": ... }` bắt buộc |
|---|---|:---:|---|
| **1. Danh sách phân trang** | `GET /resources` | `200 OK` | `{ "data": { "items": [...], "pagination": { "page": 1, "limit": 20, "total": 100, "totalPages": 5 } } }` *(Nếu rỗng vẫn trả `200 OK` với `items: []`, không trả `404`)* |
| **2. Cây / Danh sách phẳng** | `GET /resources/tree` | `200 OK` | `{ "data": [ { "_id": "...", "name": "...", "children": [] } ] }` |
| **3. Chi tiết / Tra cứu đơn** | `GET /resources/:id` | `200 OK` | `{ "data": { "_id": "...", ... } }` |
| **4. Tạo mới đồng bộ** | `POST /resources` | `201 Created` | Trả **toàn bộ object bản ghi vừa tạo** (gồm `_id`, các trường `default` và `timestamps`). |
| **5. Tác vụ chạy ngầm (Queue/Job)** | `POST /resources/:id/action` | `202 Accepted` | Đã đưa vào Background Job, chưa xong ngay: `{ "data": { "job_id": "...", "status": "processing", "message": "..." } }`. |
| **6. Cập nhật bản ghi** | `PUT /resources/:id` | `200 OK` | Trả **toàn bộ object bản ghi mới nhất SAU KHI cập nhật** (`{ new: true }`). |
| **7. Cập nhật Sub-resource** | `PUT /resources/:id/status` | `200 OK` | Chỉ trả `_id` kèm trường vừa đổi: `{ "data": { "_id": "...", "status": 0 } }`. |
| **8. Xóa / Hành động xác nhận** | `DELETE /resources/:id` | `200 OK` | `{ "data": { "_id": "...", "message": "Xóa thành công" } }` *(Không dùng `204` rỗng để tránh crash `res.json()`)*. |

