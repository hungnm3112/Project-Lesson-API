## QUY TẮC DTO & VALIDATION (`class-validator`)

### 1. Tổ chức File DTO
- Mỗi thao tác tạo 1 file DTO riêng trong thư mục `dto/` của module (`create-xxx.dto.ts`, `update-xxx.dto.ts`).
- `CreateXxxDto`: Khai báo đầy đủ các trường bắt buộc và tùy chọn khi tạo mới.
- `UpdateXxxDto`: Kế thừa `PartialType(CreateXxxDto)` từ `@nestjs/mapped-types` để tự động optional hóa các trường cho method `PUT`.

### 2. Quy tắc Validate
- Sử dụng `class-validator`: `@IsString()`, `@IsNotEmpty()`, `@IsOptional()`, `@IsNumber()`, `@IsEnum()`, `@IsMongoId()`, `@Min()`, `@Max()`.
- Với mảng hoặc object lồng nhau (`detail_product`...): bắt buộc kết hợp `@ValidateNested({ each: true })` và `@Type(() => SubDto)` từ `class-transformer`.

