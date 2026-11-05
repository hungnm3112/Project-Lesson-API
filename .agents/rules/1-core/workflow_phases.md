## QUY TẮC LUỒNG CÔNG VIỆC & CHUYỂN GIAI ĐOẠN (WORKFLOW PHASES)

### 1. Chuỗi Truy vết Kiến trúc Bắt buộc (End-to-End Traceability)
Mọi thành phần kỹ thuật trong dự án không được tự ý sinh ra cảm tính mà phải truy vết một-một theo đúng trình tự từng bước:

1. **[Bước 1 — Khảo sát & Hiểu bài toán (5 Câu hỏi Khảo sát)](../2-analysis/1_problem_survey_5_questions.md)**
2. **[Bước 2 — Truy vết & Bóc tách Tác nhân (7 Câu hỏi Truy vết Actor)](../2-analysis/2_actor_identification_7_questions.md)**
3. **[Bước 3 — Thiết kế Sơ đồ Ngữ cảnh (DFD Level 0)](../2-analysis/3_dfd_level_0_standard.md)**
4. **[Bước 4 — Sơ đồ Phân rã Chức năng (FDD 3 Cấp) & Ma trận RBAC](../2-analysis/4_fdd_and_rbac_standard.md)**
5. **[Bước 5 — Ánh xạ FDD sang Database & Lập Bảng Đặc tả 4 Cột](../3-database/1_fdd_to_database_mapping.md)**
6. **[Bước 6 — Thiết kế & Viết Tài liệu RESTful API](../4-api/api_doc_and_postman_standard.md)**
7. **[Bước 7 — Triển khai Code Backend (NestJS) & Frontend (Next.js)](../5-backend/nestjs_architecture.md)**

*(Tài liệu giải thích chi tiết mối liên hệ nhân quả xuyên suốt các bước xem tại: [`md/analysis/SystemsThinking.md`](../../../md/analysis/SystemsThinking.md)).*

### 2. Kiểm soát Chuyển Giai đoạn (Phase Gate)
- **Không nhảy bước:** Bắt buộc hoàn thành và chốt tài liệu thiết kế của bước trước rồi mới chuyển sang bước tiếp theo (tuyệt đối không viết Schema/Code khi chưa chốt bảng đặc tả Database và API).
- **Báo cáo & Đợi Xác nhận:** Sau khi hoàn thành mỗi bước hoặc khối công việc, hãy **báo cáo kết quả và DỪNG LẠI** chờ người dùng xác nhận ("Ok", "Tiếp tục") mới thực hiện bước tiếp theo.
