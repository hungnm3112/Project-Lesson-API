## QUY TẮC TYPESCRIPT

### 1. Strict Mode & Kiểu dữ liệu
- Bắt buộc `strict: true` — không tắt bất kỳ strict flag nào.
- **CẤM dùng `any`** — ưu tiên `unknown` nếu chưa biết kiểu, sau đó thu hẹp kiểu (type narrowing).
- Khai báo `export interface` / `export type` riêng biệt cho mọi response API, props component, payload request (đặt type dùng chung tại `src/types/`).

### 2. Enum, Utility Types & Coding Style
- Ưu tiên `as const` + union type cho tập giá trị đơn giản; chỉ dùng `enum` cho giá trị cố định tham chiếu nhiều nơi.
- Tận dụng `Record<string, T>`, `Pick<T, K>`, `Omit<T, K>`, `Partial<T>`, `ReturnType<typeof fn>`.
- Bắt buộc dùng `async/await` (không dùng `.then().catch()`).
- Ưu tiên `const`, chỉ dùng `let` khi cần gán lại, cấm dùng `var`.

