# Bài 3: Xử lý xung đột phức tạp trong quá trình Rebase

## Quá trình thực hiện
1. **Khởi tạo repository và các nhánh:**
   - Khởi tạo repo và thêm file `config.json`.
   - Tạo nhánh `feature-api`, sửa file qua 2 commit (`feat: change port`, `feat: enable debug`).
   - Quay lại `main`, sửa file qua 2 commit (`update port on main`, `add env config`).

2. **Rebase `feature-api` lên `main` và xử lý conflict:**
   - **Chặng 1 (commit `feat: change port`):**
     - **Nguyên nhân:** Xung đột xảy ra ở `config.json` do ở `main` đã sửa giá trị `"port"` và thêm `"env"`, trong khi commit `feat: change port` của nhánh `feature-api` cũng sửa `"port"`.
     - **Cách giải quyết:** Mở file `config.json` và sửa thủ công để giữ thay đổi port của `feature-api` (`"port": 9000`) và giữ lại cả các thay đổi từ `main` (`"env": "production"`). Sau đó chạy `git add config.json` và `git rebase --continue`.
   - **Chặng 2 (commit `feat: enable debug`):**
     - **Nguyên nhân:** Xung đột xảy ra do cấu trúc file ở bản nháp hiện hành đã có trường `"env"` và `"debug": false`, nhưng patch của nhánh `feature-api` có thay đổi `"debug": true` nhưng thiếu trường `"env"`.
     - **Cách giải quyết:** Mở file `config.json` và đồng nhất các thay đổi: `"port": 9000`, `"debug": true`, `"env": "production"`. Lưu lại, chạy `git add config.json` và `git rebase --continue`.

## Kết quả
Lịch sử Git sau khi rebase hoàn toàn thẳng tắp, không tạo ra merge commit, toàn bộ lịch sử nhánh tính năng được đưa lên trên đầu `main`.

Kết quả lệnh `git log --graph --oneline`:
```text
* 6863222 feat: enable debug
* 1defc66 feat: change port
* fa95ac0 add env config
* 854f66a update port on main
* 0f81481 init config
```
