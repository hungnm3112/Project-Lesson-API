## QUY TẮC REQUEST BODY & QUERY PARAMETERS

### 1. Request Body (`POST` / `PUT`)
- Toàn bộ tên trường trong JSON Request Body bắt buộc dùng **`snake_case`**, trùng khớp 100% với tên trường trong Database (`role_id`, `parent_id`, `warehouse_ids`).
- Client không bao giờ gửi các trường tự sinh nội bộ của Server (`_id`, `unsign_search`, `createdAt`, `updatedAt`).

### 2. Query String Parameters (`GET`)
- Toàn bộ tham số điều khiển trên URL Query String dùng **`camelCase`**.
- Mọi API lấy danh sách (`GET /api/v1/resources`) **bắt buộc hỗ trợ bộ tham số chuẩn**:
  `?page=1&limit=20&keyword=...&status=1&fromDate=YYYY-MM-DD&toDate=YYYY-MM-DD`
- Các tham số lọc theo khóa ngoại hoặc thuộc tính riêng cũng viết theo `camelCase`: `roleId`, `categoryId`, `parentId`, `warehouseId`, `type`, `sortBy`.

