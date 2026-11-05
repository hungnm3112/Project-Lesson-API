## QUY TẮC TAILWINDCSS & RESPONSIVE DESIGN

### 1. Sử dụng Class TailwindCSS Hiện đại (v3+)
**TUYỆT ĐỐI KHÔNG** dùng các class cũ đã deprecated:
- Thay `flex-shrink-0` ➔ Dùng **`shrink-0`**
- Thay `flex-shrink` ➔ Dùng **`shrink`**
- Thay `flex-grow-0` ➔ Dùng **`grow-0`**
- Thay `flex-grow` ➔ Dùng **`grow`**
- Thay `overflow-ellipsis` ➔ Dùng **`text-ellipsis`**

### 2. Tổ chức Class, Màu sắc & Responsive
- Dùng `clsx` hoặc `tailwind-merge` khi xử lý class có điều kiện; cấm nối chuỗi động kiểu ``bg-${color}-500`` làm mất style khi build.
- Ưu tiên màu cấu hình trong theme (`bg-primary`, `text-secondary`); hạn chế dùng arbitrary values (`bg-[#...]`).
- Thiết kế **Mobile-first** (mở rộng qua `sm:`, `md:`, `lg:`, `xl:`), không tách file CSS riêng cho mobile/desktop.

