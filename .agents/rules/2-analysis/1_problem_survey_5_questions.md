## BƯỚC 1: KHẢO SÁT & HIỂU BÀI TOÁN (5 CÂU HỎI KHẢO SÁT NGHIỆP VỤ)
> **File đầu ra:** `md/analysis/1-problem-survey.md`

### 1. Mô hình Chuyển hóa Yêu cầu Hệ thống
Trước khi nghĩ đến Actor, Database hay API, mọi bài toán phải được bóc tách qua 4 lớp:
> **Pain Points (Nỗi đau thực tại)** ➔ **Business Goals (Mục tiêu hệ thống)** ➔ **System Boundary (Ranh giới hệ thống)** ➔ **Acceptance Criteria (Tiêu chí nghiệm thu)**

### 2. Năm Câu hỏi Khảo sát & Hiểu bài toán gốc (5 Survey Questions)
Bắt buộc trả lời đầy đủ 5 câu hỏi khảo sát sau để làm rõ gốc rễ nghiệp vụ:

| Thứ tự | Câu hỏi khảo sát | Mục đích phân tích | Cấu trúc trình bày đầu ra |
|:---:|---|---|---|
| **Câu 1** | **Tại sao cần xây dựng hệ thống này?** | Xác định mục đích kinh doanh (Business Value) và ranh giới hệ thống. | 2–3 ý nêu rõ lý do ra đời của hệ thống và lợi ích cốt lõi mang lại. |
| **Câu 2** | **Hiện tại quy trình đang làm như thế nào?** | Hiểu quy trình thủ công hiện hữu (As-Is): công cụ đang dùng, kênh phát sinh dữ liệu, các bước thao tác tay. | Liệt kê các bước người dùng đang thực hiện trước khi có hệ thống. |
| **Câu 3** | **Vấn đề (Nỗi đau) hiện tại là gì?** | Tìm điểm nghẽn, sai sót, thất thoát trong Câu 2 để chuyển hóa thành **Mục tiêu hệ thống**. | Bảng 3 cột: `Vấn đề hiện tại \| Hệ quả \| Mục tiêu hệ thống tương ứng`. |
| **Câu 4** | **Ai là người tham gia sử dụng?** | Phác thảo danh sách các nhóm đối tượng chạm vào hệ thống và nhu cầu của họ. | Bảng 3 cột: `Nhóm đối tượng (thô) \| Căn cứ trong đề \| Nhu cầu chính`. |
| **Câu 5** | **Kết quả mong muốn là gì?** | Định nghĩa **Tiêu chí nghiệm thu (Acceptance Criteria)** cho từng mục tiêu. | Bảng 2 cột: `Mục tiêu hệ thống \| Tiêu chí đạt cụ thể (kiểm chứng được)`. |

### 3. Quản lý Giả định & Chốt Quyết định Kiến trúc
- Đánh dấu **`[Giả định]`** cho mọi chi tiết đề bài chưa nói rõ và đặt tối đa 3 câu hỏi làm rõ trọng tâm.
- Sau khi người dùng xác nhận, chuyển thành **`[Đã xác nhận]`** và tổng hợp thành bảng **Các quyết định nghiệp vụ & kiến trúc đã chốt** ở cuối tài liệu để làm đầu vào cho Bước 2.

