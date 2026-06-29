# Hãy mô tả những câu lệnh Git thường dùng và mô tả Git flow trong dự án các em thường dùng

## Các câu lệnh Git em thường dùng là:

- `git clone`: dùng để clone source code từ remote repository về máy local.
- `git branch`: dùng để xem danh sách branch và kiểm tra branch hiện tại.
- `git checkout branch-name`: dùng để chuyển sang branch khác.
- `git checkout -b branch-name`: dùng để tạo branch mới và chuyển sang branch đó.
- `git pull origin branch-name`: dùng để lấy code mới nhất từ remote về local.
- `git status`: dùng để kiểm tra trạng thái các file đang thay đổi.
- `git add file-name` hoặc `git add .`: dùng để đưa file thay đổi vào staging area.
- `git commit -m "message"`: dùng để lưu lại thay đổi vào lịch sử Git.
- `git log --oneline`: dùng để xem lịch sử commit ở dạng ngắn gọn, mỗi commit hiển thị trên một dòng.
- `git push origin branch-name`: dùng để đẩy branch hoặc commit từ local lên remote.
- `git merge branch-name`: dùng để gộp code từ branch khác vào branch hiện tại.
- `git cherry-pick commit-id`: dùng để lấy một commit cụ thể từ branch khác sang branch hiện tại.
- `git stash`: dùng để lưu tạm code đang làm khi chưa muốn commit nhưng cần chuyển branch.

## Git flow trong dự án em thường dùng là:

1. Team sẽ có branch chính như `main` hoặc `master`, thường chứa code ổn định hoặc code đã release.
2. Branch `develop` là branch dùng để tích hợp code trong quá trình phát triển.
3. Branch `staging` là branch dùng để deploy lên môi trường test cho tester hoặc khách hàng kiểm tra trước khi release.
4. Khi nhận task mới, em sẽ checkout sang `develop` và pull code mới nhất.

```bash
git checkout develop
git pull origin develop
```

5. Sau đó em tạo branch mới từ `develop`. Ví dụ nếu làm chức năng login thì tạo branch:

```bash
git checkout -b feature/login
```

Nếu là sửa bug thì có thể đặt tên branch như:

```bash
git checkout -b bugfix/fix-login-error
```

6. Sau khi code xong, em sẽ tự test chức năng, sau đó kiểm tra lại thay đổi bằng:

```bash
git status
git diff
```

7. Nếu thay đổi đúng, em add file và commit:

```bash
git add .
git commit -m "Implement login feature"
```

8. Sau đó em push branch lên remote:

```bash
git push origin feature/login
```

9. Cuối cùng em tạo Merge Request/Pull Request vào branch `develop`. Trong MR/PR, em sẽ mô tả task đã làm gì, cách test, và có ảnh hưởng đến phần nào khác không.

10. Reviewer sẽ review code. Nếu có comment thì em sửa lại, commit và push tiếp lên branch đó. Khi code được approve, không còn conflict và test pass thì branch sẽ được merge vào `develop`.

11. Khi cần đưa chức năng lên môi trường test, team sẽ merge branch `develop` lên branch `staging`.

```bash
git checkout staging
git pull origin staging
git merge develop
git push origin staging
```

12. Sau khi code được deploy từ `staging`, tester hoặc khách hàng sẽ test trên môi trường staging. Nếu phát hiện bug thì team sẽ tạo branch `bugfix/*` từ `develop`, sửa lỗi, merge lại vào `develop`, rồi merge tiếp từ `develop` lên `staging` để test lại.

13. Khi tester hoặc khách hàng xác nhận ổn, code mới được merge hoặc release lên branch `main/master` tùy quy trình của dự án.
