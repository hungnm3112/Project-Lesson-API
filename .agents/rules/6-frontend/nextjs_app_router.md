## QUY TẮC NEXT.JS APP ROUTER & COMPONENT

### 1. Server Component vs Client Component
- Mặc định mọi component là **Server Component**.
- **CHỈ** thêm `'use client'` ở dòng đầu khi thực sự cần: `useState`, `useEffect`, sự kiện (`onClick`, `onChange`) hoặc Browser API (`window`, `navigator`).

### 2. Cấu trúc Thư mục & Định tuyến (`src/app/`)
- Dùng Route Groups để tách layout mà không ảnh hưởng URL: `(public)/`, `(auth)/`, `(admin)/`. Tên thư mục URL viết thường `kebab-case`.
- Phân nhóm component theo tính năng trong `src/components/<feature>/` (không chia thư mục `mobile/` và `desktop/`).
- Luôn dùng Path Alias `@/components/...`, `@/lib/...`, `@/hooks/...` thay vì đường dẫn tương đối `../../`.

### 3. Quy tắc JSX / TSX
- Dùng nháy kép `"` cho thuộc tính JSX (`className="..."`) và nháy đơn `'` cho string trong logic TypeScript.
- Dùng class TailwindCSS thay vì inline `style={{...}}`.

