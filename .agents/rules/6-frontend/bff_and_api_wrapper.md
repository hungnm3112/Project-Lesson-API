## KIẾN TRÚC BFF, FETCH WRAPPER (`api.ts`) & SERVER ACTIONS

### 1. Kiến trúc BFF (Backend-For-Frontend)
- Trình duyệt Client **KHÔNG** gọi trực tiếp URL Backend (`localhost:8060`) mà gọi qua Next.js API Route (`src/app/api/*`) hoặc Server Actions.

### 2. Fetch Wrapper Tập trung (`src/lib/api.ts`)
- Tập trung toàn bộ hàm gọi API vào `src/lib/api.ts` với 4 hàm: `apiGet()`, `apiPost()`, `apiPut()`, `apiDelete()`, tự động gắn header `Authorization: Bearer <token>`.
- **QUAN TRỌNG:** Biến `NEXT_PUBLIC_API_URL` đã chứa sẵn `/api/v1` (VD: `http://localhost:8060/api/v1`). Khi gọi `apiGet('/endpoint')`, **TUYỆT ĐỐI KHÔNG** thêm `/api/v1` vào tham số để tránh lỗi lặp `/api/v1/api/v1/...`.
- Luôn kiểm tra `res.ok` và chuẩn hóa kết quả trả về của BFF cho Client:
  - Thành công: `{ success: true, data: T }`
  - Thất bại: `{ success: false, message: string, statusCode: number, code?: string, details?: ApiErrorDetail[] }`

### 3. Server Actions (`'use server'`)
- Dùng cho các thao tác mutate (form submit, CRUD) và gọi `revalidateTag()` sau khi hoàn tất để làm mới cache.

