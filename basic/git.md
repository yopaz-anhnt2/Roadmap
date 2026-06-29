# Git

## Các lệnh Git thường dùng

Khởi tạo & cấu hình
- `git init` — khởi tạo repo mới
- `git clone <url>` — sao chép repo về máy
- `git remote -v` — xem danh sách remote

Thao tác hằng ngày
- `git status` — xem trạng thái file thay đổi
- `git add <file>` / `git add .` — đưa file vào staging
- `git commit -m "message"` — lưu lại thay đổi kèm mô tả
- `git log --oneline` — xem lịch sử commit

Đồng bộ với remote
- `git pull` — kéo code mới nhất về
- `git push` — đẩy commit lên remote
- `git fetch` — tải dữ liệu mới về nhưng chưa merge

Nhánh (branch)
- `git branch` — xem danh sách nhánh
- `git branch <ten>` — tạo nhánh mới
- `git checkout <ten>` / `git switch <ten>` — chuyển nhánh
- `git checkout -b <ten>` — tạo và chuyển sang nhánh mới
- `git merge <ten>` — gộp nhánh khác vào nhánh hiện tại

Khác
- `git stash` — cất tạm thay đổi chưa commit
- `git reset` — hủy thay đổi ở staging
- `git revert <commit>` — tạo commit hoàn tác một commit cũ

## Git Flow trong dự án

Nhánh chính (cố định)
- `main` (hoặc `master`) — code chạy ổn định trên production
- `develop` — nhánh tích hợp, gộp các tính năng đang phát triển

Nhánh phụ (tạo ra rồi xóa sau khi merge)
- `feature/<ten>` — phát triển một tính năng mới. Ví dụ: `feature/login`, `feature/cart`
- `bugfix/<ten>` — sửa lỗi thông thường trong lúc phát triển. Ví dụ: `bugfix/wrong-total`
- `hotfix/<ten>` — sửa lỗi gấp ngay trên production. Ví dụ: `hotfix/payment-error`
- `release/<phien-ban>` — chuẩn bị cho một bản phát hành. Ví dụ: `release/v1.2.0`
- `docs/<ten>` — viết/sửa tài liệu. Ví dụ: `docs/api-guide`
- `refactor/<ten>` — chỉnh sửa code cho gọn, không đổi chức năng. Ví dụ: `refactor/user-service`
- `chore/<ten>` — việc lặt vặt: cấu hình, nâng version, dọn dẹp. Ví dụ: `chore/update-deps`
- `test/<ten>` — thêm hoặc sửa test. Ví dụ: `test/login-unit-test`

Cách đặt tên nhánh
- Cú pháp: `<loai>/<mo-ta-ngan>` — loại đứng trước, mô tả viết thường, nối bằng dấu gạch ngang
- Có thể gắn kèm mã ticket/issue nếu có. Ví dụ: `feature/123-login`

Quy trình làm việc
- B1: Tạo nhánh mới từ `develop` cho tính năng đang làm: `git checkout -b feature/login`
- B2: Code, rồi commit từng phần nhỏ rõ ràng: `git add .` -> `git commit -m "..."`
- B3: Đẩy nhánh lên remote: `git push origin feature/login`
- B4: Tạo Pull Request (PR) để review code
- B5: Sau khi review xong, merge PR vào `develop`
- B6: Khi đủ tính năng và ổn định, merge `develop` vào `main` để release

Một số quy ước
- Mỗi commit nên nhỏ, mô tả rõ ràng đang làm gì
- Luôn `git pull` trước khi bắt đầu làm để tránh xung đột (conflict)
- Không commit trực tiếp lên `main`
