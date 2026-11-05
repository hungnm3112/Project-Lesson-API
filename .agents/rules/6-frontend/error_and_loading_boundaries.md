## QUY TẮC ERROR BOUNDARY & FILE HỆ THỐNG NEXT.JS

### 1. Ba File Hệ thống Bắt buộc trên App Router
Mỗi Route Group hoặc Route chính trong `src/app/` **BẮT BUỘC** phải có đủ 3 file hệ thống để đảm bảo UX không bị gãy:
1. `loading.tsx`: Hiển thị Skeleton hoặc Spinner khi đang fetch dữ liệu ở Server Component.
2. `error.tsx`: Xử lý lỗi runtime hoặc lỗi API, bắt buộc có nút **"Thử lại" (Try again)**.
3. `not-found.tsx`: Giao diện trang 404 thân thiện kèm nút quay lại.

### 2. Quy tắc Error Boundary
- File `error.tsx` **luôn phải có `'use client'` ở dòng đầu tiên**.
- Tuyệt đối không để màn hình crash mặc định của Next.js lọt ra môi trường production.

