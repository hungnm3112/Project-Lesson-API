## QUY TẮC ĐẶT TÊN TOÀN DỰ ÁN (NAMING CONVENTIONS)

| Thành phần | Quy ước | Ví dụ chuẩn |
|---|---|---|
| **Thư mục (FE & BE)** | `kebab-case` | `components/product-card/`, `user-management/` |
| **File Backend (NestJS)** | `kebab-case` + hậu tố chuẩn | `users.controller.ts`, `create-user.dto.ts`, `user.schema.ts` |
| **Component React (FE)** | `PascalCase.tsx` | `DataTable.tsx`, `ConfirmModal.tsx` |
| **Hook / Util / Type (FE)** | `camelCase.ts` | `useDebounce.ts`, `formatPrice.ts` |
| **Biến & Hàm (TS/JS)** | `camelCase` | `const userData = {};`, `function getUserInfo() {}` |
| **Class & Interface / Type** | `PascalCase` | `class UsersService {}`, `interface PaginatedData<T> {}` |
| **Hằng số cố định** | `UPPER_SNAKE_CASE` | `const MAX_RETRY_COUNT = 3;` |
| **Database (Bảng & Trường)** | `snake_case` | `parent_id`, `customer_id`, `total_amount`, `unsign_search` *(Ngoại lệ: `_id`, `ID`, `createdAt`, `updatedAt`)* |
| **API Endpoint (URI)** | Danh từ số nhiều, `kebab-case` | `/api/v1/user-profiles`, `/api/v1/type-payments` |
| **API Body & Response** | `snake_case` (trùng DB) | `{ "role_id": "...", "warehouse_ids": [] }` |
| **API Query Parameters** | `camelCase` | `?page=1&limit=20&categoryId=...&fromDate=...` |

