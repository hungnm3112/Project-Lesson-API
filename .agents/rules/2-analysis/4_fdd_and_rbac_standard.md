## BƯỚC 4: SƠ ĐỒ PHÂN RÃ CHỨC NĂNG (FDD 3 CẤP) & MA TRẬN RBAC
> **File đầu ra:** `md/analysis/4-fdd-and-rbac.md`

### 1. Nguyên tắc MECE & Cú pháp Đặt tên
- **Chuyển góc nhìn từ "Ai làm (Who)" sang "Hệ thống có gì (What)":** Tuyệt đối không lấy danh sách Actor ở Bước 2 làm các nhánh Cấp 1 của FDD. Phải mở hộp đen hệ thống và chia nhánh theo **Miền nghiệp vụ / Đối tượng dữ liệu cốt lõi (Core Entity)**.
- **Nguyên tắc MECE (Không trùng lặp - Không bỏ sót):** Mỗi chức năng chỉ xuất hiện **duy nhất 1 lần** trên toàn sơ đồ FDD. Nếu nhiều Actor cùng thực hiện 1 hành động ở Bước 3, phải hợp nhất thành **1 chức năng duy nhất** trên FDD và phân định quyền ở bảng RBAC.
- **Cú pháp bắt buộc:** 100% các nút ở mọi cấp (Cấp 1, Cấp 2, Cấp 3) phải bắt đầu bằng **`Động từ + Danh từ`**.

### 2. Công thức Phân rã Cây Chức năng 3 Cấp
- **Cấp 0 (Root):** Tên hệ thống tổng thể.
- **Cấp 1 (Phân hệ / Module):** Gom các mục tiêu hệ thống (từ Bước 1) thành 4–6 khối nghiệp vụ độc lập tương đối.
- **Cấp 2 (Chức năng nghiệp vụ — Đơn nhiệm):** Mỗi ô Cấp 2 chỉ quản lý đúng 1 đối tượng hoặc 1 nghiệp vụ độc lập. Kiểm soát số lượng bằng công thức bảo toàn:
  > **Tổng Chức năng Cấp 2 = Tổng Hành động Actor (Bước 3) − Số Hành động trùng giữa các Actor + Số Tác vụ tự động (Bước 2)**
- **Cấp 3 (Tác vụ chi tiết — Nhịp IPO):** Bóc tách trực tiếp từ cột `Dữ liệu Input` và `Dữ liệu Output` ở Bước 3 theo 3 nhịp:
  1. **Input:** Tiếp nhận / chọn dữ liệu đầu vào hoặc tiêu chí lọc.
  2. **Process:** Kiểm tra điều kiện, ràng buộc nghiệp vụ, xử lý/tính toán lõi.
  3. **Output:** Lưu trạng thái, phát sinh dữ liệu/hàng đợi và trả kết quả.

### 3. Ma trận Truy vết RBAC & Phân tách Trách nhiệm (SoD)
- Cuối tài liệu FDD bắt buộc lập bảng: `| STT | Mã FDD | Tên chức năng Cấp 2 | Nguồn Hành động / Tác vụ (Bước 2 & 3) | Bảng DB dự kiến | Quyền theo từng Role |`.
- Áp dụng nguyên tắc **Phân tách trách nhiệm (Separation of Duties - SoD)**: Tách bạch quyền **Quản trị cấu hình/dữ liệu nguồn** và quyền **Vận hành tác nghiệp**.

### 4. Quy tắc Vẽ Sơ đồ Cây Phân rã Chức năng FDD (Mermaid `flowchart TD`)
Tài liệu Bước 4 bắt buộc có **Sơ đồ cây FDD tổng thể** phân cấp từ Cấp 0 ➔ Cấp 1 ➔ Cấp 2 bằng Mermaid:
1. **Phạm vi hiển thị trên Sơ đồ Cây:**
   - **Cấp 0 (Root):** 1 nút gốc duy nhất đại diện cho toàn bộ hệ thống (`[0] Tên Hệ Thống`).
   - **Cấp 1 (Phân hệ / Module):** Các khối phân hệ nghiệp vụ lớn, tô màu phân biệt (`fill:#4338ca`).
   - **Cấp 2 (Chức năng nghiệp vụ đơn nhiệm):** Gom trong các `subgraph` xếp dọc (`direction TB`) đặt dưới từng Phân hệ Cấp 1; mỗi ô mang mã định danh rõ ràng: `FDD-01`, `FDD-02`... và bắt đầu bằng cú pháp `Động từ + Danh từ`.
   - **Cấp 3 (Nhịp IPO):** Vì Cấp 3 gồm hàng chục tác vụ chi tiết, không vẽ trực tiếp lên sơ đồ cây để chống quá tải/vỡ layout; thay vào đó, **Cấp 3 bắt buộc được bóc tách chi tiết thành danh sách hoặc bảng ngay sau sơ đồ cây**.
2. **Cấu hình Template chuẩn cho Sơ đồ FDD:**
```mermaid
%%{init: {
  'theme': 'dark',
  'themeVariables': {
    'fontSize': '13px'
  },
  'flowchart': {
    'nodeSpacing': 30,
    'rankSpacing': 40,
    'padding': 15
  }
}}%%
flowchart TD
    %% Cấp 0: Root Node
    ROOT["<b>[0] TÊN HỆ THỐNG TỔNG THỂ</b>"]

    %% Cấp 1: Các Phân hệ lớn
    M1["<b>PHÂN HỆ 1: TÊN PHÂN HỆ</b>"]
    M2["<b>PHÂN HỆ 2: TÊN PHÂN HỆ</b>"]

    ROOT --> M1
    ROOT --> M2

    %% Cấp 2: Các Chức năng đơn nhiệm dưới từng Phân hệ
    subgraph M1_SUBS[" "]
        direction TB
        F11["FDD-01: Động từ + Danh từ"]
        F12["FDD-02: Động từ + Danh từ"]
    end
    M1 --> M1_SUBS

    subgraph M2_SUBS[" "]
        direction TB
        F21["FDD-03: Động từ + Danh từ"]
        F22["FDD-04: Động từ + Danh từ"]
    end
    M2 --> M2_SUBS

    %% Styling chuẩn
    classDef rootNode fill:#1e1b4b,stroke:#818cf8,stroke-width:2px,color:#ffffff;
    classDef moduleNode fill:#312e81,stroke:#6366f1,stroke-width:1.5px,color:#ffffff;
    classDef funcNode fill:#27272a,stroke:#52525b,stroke-width:1px,color:#f4f4f5;

    class ROOT rootNode;
    class M1,M2 moduleNode;
    class F11,F12,F21,F22 funcNode;
```

