# Hãy mô tả những câu lệnh Git thường dùng và mô tả Git flow trong dự án các em thường dùng

## Câu trả lời mẫu

Trong dự án, em thường dùng Git để quản lý source code, theo dõi lịch sử thay đổi và phối hợp làm việc với các thành viên khác trong team. Thông thường em không làm trực tiếp trên branch chính, mà sẽ tạo branch riêng cho từng task, sau đó push code lên remote và tạo Merge Request/Pull Request để reviewer kiểm tra trước khi merge.

Các câu lệnh Git em thường dùng là:

- `git clone`: dùng để clone source code từ remote repository về máy local.
- `git branch`: dùng để xem danh sách branch và kiểm tra branch hiện tại.
- `git checkout branch-name`: dùng để chuyển sang branch khác.
- `git checkout -b branch-name`: dùng để tạo branch mới và chuyển sang branch đó.
- `git pull origin branch-name`: dùng để lấy code mới nhất từ remote về local.
- `git status`: dùng để kiểm tra trạng thái các file đang thay đổi.
- `git diff`: dùng để xem chi tiết nội dung code đã thay đổi trước khi commit.
- `git add file-name` hoặc `git add .`: dùng để đưa file thay đổi vào staging area.
- `git commit -m "message"`: dùng để lưu lại thay đổi vào lịch sử Git.
- `git push origin branch-name`: dùng để đẩy branch hoặc commit từ local lên remote.
- `git merge branch-name`: dùng để gộp code từ branch khác vào branch hiện tại.
- `git stash`: dùng để lưu tạm code đang làm khi chưa muốn commit nhưng cần chuyển branch.

Git flow trong dự án em thường dùng là:

1. Team sẽ có branch chính như `main` hoặc `master`, thường chứa code ổn định hoặc code đã release.
2. Branch `develop` là branch dùng để tích hợp code trong quá trình phát triển.
3. Khi nhận task mới, em sẽ checkout sang `develop` và pull code mới nhất.

```bash
git checkout develop
git pull origin develop
```

4. Sau đó em tạo branch mới từ `develop`. Ví dụ nếu làm chức năng login thì tạo branch:

```bash
git checkout -b feature/login
```

Nếu là sửa bug thì có thể đặt tên branch như:

```bash
git checkout -b bugfix/fix-login-error
```

5. Sau khi code xong, em sẽ tự test chức năng, sau đó kiểm tra lại thay đổi bằng:

```bash
git status
git diff
```

6. Nếu thay đổi đúng, em add file và commit:

```bash
git add .
git commit -m "Implement login feature"
```

7. Sau đó em push branch lên remote:

```bash
git push origin feature/login
```

8. Cuối cùng em tạo Merge Request/Pull Request vào branch `develop`. Trong MR/PR, em sẽ mô tả task đã làm gì, cách test, và có ảnh hưởng đến phần nào khác không.

9. Reviewer sẽ review code. Nếu có comment thì em sửa lại, commit và push tiếp lên branch đó. Khi code được approve, không còn conflict và test pass thì branch sẽ được merge vào `develop`.

Nếu trong quá trình làm có conflict, em sẽ dùng `git status` để xem file nào bị conflict, mở file đó kiểm tra cả phần code của mình và phần code từ branch khác, xử lý lại cho đúng logic, xóa dấu conflict rồi test lại chức năng liên quan trước khi commit.

## Phiên bản trả lời ngắn gọn

Em thường dùng các lệnh Git như `git clone`, `git branch`, `git checkout`, `git checkout -b`, `git pull`, `git status`, `git diff`, `git add`, `git commit`, `git push`, `git merge` và `git stash`.

Git flow trong dự án thường là: team có branch `main` hoặc `master` để chứa code ổn định, branch `develop` để phát triển chung. Khi nhận task, em pull code mới nhất từ `develop`, tạo branch riêng như `feature/login` hoặc `bugfix/fix-login-error`, sau đó code, tự test, kiểm tra `git status` và `git diff`, commit, push lên remote và tạo Merge Request/Pull Request. Sau khi reviewer approve, không còn conflict và test ổn thì code sẽ được merge vào `develop`.
