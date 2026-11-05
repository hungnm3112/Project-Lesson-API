## CÁC MẪU THIẾT KẾ DỮ LIỆU DOANH NGHIỆP (ENTERPRISE DATA PATTERNS)

1. **Cây phân cấp Cha - Con:** Dùng `parent_id: ObjectId, default: null, Index` tự tham chiếu chính bảng đó.
2. **Partial Unique Index cho Mã tùy chọn:** Với trường không bắt buộc nhập ngay (như `sku: String, default: null`), dùng Partial Unique Index (`unique` chỉ áp dụng khi giá trị là `string`) để nhiều bản ghi được phép để `null`.
3. **Mẫu Thiết kế Phân hệ Kho / Chứng từ / Công nợ (Áp dụng khi dự án có nghiệp vụ tương ứng):**
   - **Tách mẫu sản phẩm & cá thể:** `subcategories` (bảng sản phẩm chung) và `products` (cá thể sản phẩm cuối + mã QR tự sinh sau khi nhập kho).
   - **Hợp nhất đối tác (`customers`):** Lưu chung Khách hàng (`type = 1`), Nhà cung cấp (`type = 2`), Khách thợ (`type = 3`), tham chiếu thống nhất qua `customer_id`.
   - **Chứng từ (`import_form`, `export_form`, `transfer_form`):** Lưu `total_amount` trên phiếu; nhúng mảng chi tiết `product[]` và `service[]` theo cấu trúc `detail_product` (`index`, `subcategory_id`, `price`, `quantity`, `vat`, `ck`, `warranty`, `discount`).
   - **Tách Chứng từ Quỹ & Cặp Sổ cái (Ledger Pattern):** Tách `payment_form` (gắn `type_payments`, `content_payments`) với Sổ cái Công nợ `debts` (`total_import`, `total_payment`, `total_export`, `total_receive`) và Sổ cái Nhập-Xuất-Tồn `stock_histories` (`quantity_in`, `quantity_out`, `price`).

