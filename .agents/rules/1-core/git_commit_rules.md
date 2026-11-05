## QUY TẮC GIT WORKFLOW & COMMIT

### 1. Phân nhánh (Branching)
- `main`: Nhánh production ổn định, không code trực tiếp trên nhánh này.
- `[tên-developer]`: Nhánh làm việc cá nhân. Tạo PR và cross-review trước khi merge.

### 2. Commit Message Convention (Conventional Commits)
- Tiền tố: `feat:`, `fix:`, `refactor:`, `style:`, `docs:`, `chore:`.
- **Tiêu đề (Header):** Bắt buộc **viết thường 100% (lower-case)**, **tiếng Việt KHÔNG DẤU**, tối đa 100 ký tự.
  - *Đúng:* `feat(backend): them api quan ly nguoi dung`
  - *Sai:* `feat(backend): Thêm API quản lý người dùng`
- **Mô tả chi tiết (Body):** Tiếng Việt **KHÔNG DẤU**, mỗi dòng không quá 100 ký tự (`body-max-line-length`).

