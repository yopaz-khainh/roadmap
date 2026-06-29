# SQL, Database và Index

## 1. SQL Basic

### SQL là gì?

SQL là viết tắt của Structured Query Language, là ngôn ngữ dùng để làm việc với cơ sở dữ liệu quan hệ như MySQL, PostgreSQL, SQL Server hoặc SQLite.

SQL giúp mình tạo bảng, thêm dữ liệu, đọc dữ liệu, sửa dữ liệu, xóa dữ liệu và quản lý cấu trúc database.

### SQL dùng để làm gì?

- Lấy dữ liệu từ database để hiển thị ra màn hình.
- Thêm dữ liệu mới, ví dụ tạo user, tạo đơn hàng, tạo bài viết.
- Cập nhật dữ liệu, ví dụ sửa thông tin user hoặc đổi trạng thái đơn hàng.
- Xóa dữ liệu không còn cần thiết.
- Tạo bảng, sửa bảng, tạo index và ràng buộc dữ liệu.

### Các nhóm câu lệnh SQL thường gặp

**DDL - Data Definition Language:** dùng để định nghĩa cấu trúc database.

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255)
);

ALTER TABLE users ADD COLUMN phone VARCHAR(20);

DROP TABLE users;
```

**DML - Data Manipulation Language:** dùng để thao tác dữ liệu.

```sql
INSERT INTO users (id, name, email)
VALUES (1, 'Nguyen Van A', 'a@example.com');

UPDATE users
SET name = 'Nguyen Van B'
WHERE id = 1;

DELETE FROM users
WHERE id = 1;
```

**DQL - Data Query Language:** dùng để truy vấn dữ liệu.

```sql
SELECT id, name, email
FROM users
WHERE id = 1;
```

**DCL - Data Control Language:** dùng để phân quyền.

```sql
GRANT SELECT ON users TO app_user;
REVOKE SELECT ON users FROM app_user;
```

**TCL - Transaction Control Language:** dùng để quản lý transaction.

```sql
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```

Nếu có lỗi thì dùng:

```sql
ROLLBACK;
```

### Một câu SELECT cơ bản

```sql
SELECT name, email
FROM users
WHERE status = 'active'
ORDER BY created_at DESC
LIMIT 10;
```

Ý nghĩa:

- `SELECT`: chọn cột cần lấy.
- `FROM`: chọn bảng lấy dữ liệu.
- `WHERE`: lọc dữ liệu theo điều kiện.
- `ORDER BY`: sắp xếp dữ liệu.
- `LIMIT`: giới hạn số dòng trả về.

###  

SQL là ngôn ngữ dùng để làm việc với database quan hệ. SQL dùng để tạo cấu trúc bảng, truy vấn dữ liệu, thêm, sửa, xóa dữ liệu và quản lý quyền hoặc transaction trong database.

---

## 2. Join

### Join là gì?

Join là cách kết hợp dữ liệu từ nhiều bảng dựa trên mối quan hệ giữa các bảng.

Ví dụ hệ thống có bảng `users` và `orders`. Bảng `orders` có cột `user_id` để biết đơn hàng thuộc user nào. Khi muốn lấy danh sách đơn hàng kèm tên người mua, mình cần join hai bảng này.

```sql
SELECT orders.id, users.name, orders.total
FROM orders
JOIN users ON users.id = orders.user_id;
```

### Join dùng để làm gì?

- Lấy dữ liệu liên quan từ nhiều bảng.
- Tránh lưu trùng dữ liệu trong một bảng.
- Biểu diễn quan hệ như user có nhiều orders, order có nhiều order_items, product thuộc category.

### Các loại join thường gặp

**INNER JOIN**

Chỉ lấy những dòng có dữ liệu khớp ở cả hai bảng.

```sql
SELECT users.name, orders.total
FROM users
INNER JOIN orders ON orders.user_id = users.id;
```

Nếu user chưa có order thì user đó không xuất hiện trong kết quả.

**LEFT JOIN**

Lấy toàn bộ dữ liệu từ bảng bên trái, nếu bảng bên phải không có dữ liệu khớp thì trả về `NULL`.

```sql
SELECT users.name, orders.total
FROM users
LEFT JOIN orders ON orders.user_id = users.id;
```

Cách này dùng khi muốn lấy tất cả user, kể cả user chưa có order.

**RIGHT JOIN**

Lấy toàn bộ dữ liệu từ bảng bên phải, nếu bảng bên trái không có dữ liệu khớp thì trả về `NULL`.

```sql
SELECT users.name, orders.total
FROM users
RIGHT JOIN orders ON orders.user_id = users.id;
```

Trong thực tế thường có thể đổi thứ tự bảng và dùng `LEFT JOIN` cho dễ đọc hơn.

**FULL OUTER JOIN**

Lấy dữ liệu từ cả hai bảng, có khớp hay không khớp đều lấy. PostgreSQL hỗ trợ `FULL OUTER JOIN`, MySQL không hỗ trợ trực tiếp cú pháp này.

```sql
SELECT users.name, orders.total
FROM users
FULL OUTER JOIN orders ON orders.user_id = users.id;
```

**CROSS JOIN**

Ghép mỗi dòng của bảng này với mỗi dòng của bảng kia. Nếu bảng A có 10 dòng và bảng B có 5 dòng thì kết quả có 50 dòng.

```sql
SELECT colors.name, sizes.name
FROM colors
CROSS JOIN sizes;
```

### Lưu ý khi dùng join

- Nên join bằng khóa chính và khóa ngoại, ví dụ `users.id = orders.user_id`.
- Cột dùng để join thường nên có index, đặc biệt là khóa ngoại.
- Cần cẩn thận với quan hệ one-to-many vì kết quả có thể bị nhân dòng.
- Khi dùng `LEFT JOIN`, nếu đặt điều kiện của bảng bên phải trong `WHERE` không đúng cách thì có thể biến kết quả thành giống `INNER JOIN`.

Ví dụ dễ sai:

```sql
SELECT users.name, orders.total
FROM users
LEFT JOIN orders ON orders.user_id = users.id
WHERE orders.status = 'paid';
```

Câu trên sẽ loại bỏ user chưa có order vì `orders.status` là `NULL`. Nếu muốn giữ tất cả user thì nên đưa điều kiện vào `ON`:

```sql
SELECT users.name, orders.total
FROM users
LEFT JOIN orders
    ON orders.user_id = users.id
   AND orders.status = 'paid';
```

###  

Join là cách kết hợp dữ liệu từ nhiều bảng dựa trên điều kiện liên kết, thường là khóa chính và khóa ngoại. Join dùng để lấy dữ liệu liên quan mà không cần lưu trùng thông tin trong một bảng.

---

## 3. Aggregate

### Aggregate là gì?

Aggregate là các hàm tổng hợp dữ liệu từ nhiều dòng thành một kết quả.

Ví dụ đếm số user, tính tổng doanh thu, tính giá trị trung bình hoặc tìm ngày tạo đơn hàng mới nhất.

### Aggregate dùng để làm gì?

- Thống kê dữ liệu.
- Làm báo cáo.
- Tính tổng tiền, số lượng, trung bình.
- Gom nhóm dữ liệu theo user, ngày, tháng, category hoặc trạng thái.

### Các hàm aggregate thường dùng

```sql
SELECT COUNT(*) FROM users;
```

`COUNT(*)` dùng để đếm số dòng.

```sql
SELECT SUM(total) FROM orders;
```

`SUM()` dùng để tính tổng.

```sql
SELECT AVG(total) FROM orders;
```

`AVG()` dùng để tính trung bình.

```sql
SELECT MIN(total), MAX(total) FROM orders;
```

`MIN()` và `MAX()` dùng để lấy giá trị nhỏ nhất và lớn nhất.

### GROUP BY

`GROUP BY` dùng để gom dữ liệu theo một hoặc nhiều cột.

Ví dụ tính tổng doanh thu theo từng user:

```sql
SELECT user_id, SUM(total) AS total_amount
FROM orders
GROUP BY user_id;
```

Ví dụ đếm số đơn hàng theo trạng thái:

```sql
SELECT status, COUNT(*) AS total_orders
FROM orders
GROUP BY status;
```

### HAVING

`WHERE` lọc dữ liệu trước khi group. `HAVING` lọc dữ liệu sau khi đã group.

Ví dụ lấy những user có tổng doanh thu lớn hơn 1,000,000:

```sql
SELECT user_id, SUM(total) AS total_amount
FROM orders
GROUP BY user_id
HAVING SUM(total) > 1000000;
```

### Lưu ý khi dùng aggregate

- Cột không nằm trong hàm aggregate thường phải nằm trong `GROUP BY`.
- `WHERE` chạy trước `GROUP BY`, `HAVING` chạy sau `GROUP BY`.
- Aggregate trên bảng lớn có thể chậm nếu thiếu index hoặc phải scan quá nhiều dữ liệu.
- Có thể dùng index cho các cột lọc trong `WHERE`, cột group hoặc cột sort tùy query.

###  

Aggregate là các hàm tổng hợp dữ liệu như `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`. Aggregate dùng để thống kê, làm báo cáo và tính toán dữ liệu từ nhiều dòng trong database.

---

## 4. Database Design

### Database design là gì?

Database design là quá trình thiết kế cấu trúc database, gồm các bảng, cột, kiểu dữ liệu, quan hệ giữa bảng, constraint và index.

Thiết kế database tốt giúp dữ liệu rõ ràng, ít trùng lặp, dễ truy vấn, dễ mở rộng và giảm lỗi dữ liệu.

### Database design dùng để làm gì?

- Lưu dữ liệu đúng nghiệp vụ.
- Giảm trùng lặp dữ liệu.
- Đảm bảo dữ liệu nhất quán.
- Giúp query dễ viết và dễ tối ưu.
- Giúp hệ thống dễ bảo trì khi nghiệp vụ thay đổi.

### Các bước thiết kế database cơ bản

1. Xác định nghiệp vụ cần lưu dữ liệu.
2. Xác định các entity chính, ví dụ user, order, product, category.
3. Xác định thuộc tính của từng entity, ví dụ user có name, email, password.
4. Xác định quan hệ giữa các entity.
5. Chọn kiểu dữ liệu phù hợp cho từng cột.
6. Thêm primary key, foreign key, unique, not null và các constraint cần thiết.
7. Thêm index cho các query thường dùng.
8. Review lại theo các luồng nghiệp vụ thực tế.

### Các loại quan hệ thường gặp

**One-to-one**

Một user có một profile.

```text
users.id = profiles.user_id
```

**One-to-many**

Một user có nhiều orders.

```text
users.id = orders.user_id
```

**Many-to-many**

Một user có nhiều roles, một role cũng có nhiều users. Cần bảng trung gian.

```text
users
roles
role_user
```

Ví dụ:

```sql
CREATE TABLE role_user (
    user_id BIGINT NOT NULL,
    role_id BIGINT NOT NULL,
    PRIMARY KEY (user_id, role_id)
);
```

### Normalization

Normalization là cách tách dữ liệu thành nhiều bảng hợp lý để giảm trùng lặp và tránh dữ liệu mâu thuẫn.

Ví dụ không nên lưu tên category trực tiếp trong từng product nếu category là dữ liệu riêng. Nên có bảng `categories`, còn `products` chỉ lưu `category_id`.

```text
categories: id, name
products: id, category_id, name, price
```

### Denormalization

Denormalization là cố ý lưu thêm dữ liệu trùng để tăng tốc đọc dữ liệu hoặc làm báo cáo dễ hơn.

Ví dụ lưu `order_total` trong bảng `orders` thay vì lúc nào cũng tính lại từ `order_items`.

Denormalization cần cẩn thận vì khi dữ liệu gốc thay đổi thì dữ liệu lưu thêm cũng phải được cập nhật đúng.

### Lưu ý khi thiết kế database

- Không nên dùng một bảng quá nhiều trách nhiệm.
- Không nên lưu nhiều giá trị trong một cột dạng chuỗi như `tag1,tag2,tag3` nếu cần query theo từng tag.
- Nên chọn kiểu dữ liệu vừa đủ, ví dụ tiền nên dùng `DECIMAL`, không nên dùng `FLOAT` nếu cần chính xác.
- Nên đặt tên bảng, cột rõ nghĩa và nhất quán.
- Các cột hay lọc, join, sort có thể cần index.
- Không nên đánh index theo cảm tính, nên dựa vào query thực tế.

###  

Database design là việc thiết kế bảng, cột, quan hệ, constraint và index để lưu dữ liệu đúng nghiệp vụ. Nó dùng để giúp dữ liệu nhất quán, dễ truy vấn, dễ bảo trì và hỗ trợ hệ thống chạy ổn định hơn.

---

## 5. Migrations

### Migration là gì?

Migration là cách quản lý thay đổi cấu trúc database bằng code.

Thay vì vào database sửa tay, mình viết file migration để tạo bảng, thêm cột, sửa cột, tạo index hoặc xóa cột. Framework như Laravel hỗ trợ migration rất nhiều.

### Migration dùng để làm gì?

- Đồng bộ cấu trúc database giữa các thành viên trong team.
- Theo dõi lịch sử thay đổi database.
- Deploy thay đổi database lên dev, staging, production có kiểm soát.
- Rollback khi cần quay lại thay đổi cũ.

### Ví dụ migration trong Laravel

```php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->string('email')->unique();
    $table->timestamps();
});
```

Thêm cột:

```php
Schema::table('users', function (Blueprint $table) {
    $table->string('phone')->nullable();
});
```

Rollback:

```php
Schema::table('users', function (Blueprint $table) {
    $table->dropColumn('phone');
});
```

### Lưu ý khi viết migration

- Migration đã chạy ở môi trường chung thì không nên sửa trực tiếp file cũ, nên tạo migration mới.
- Cẩn thận khi xóa cột hoặc đổi kiểu dữ liệu vì có thể mất dữ liệu.
- Với bảng lớn, thêm index hoặc sửa cột có thể lock bảng và ảnh hưởng hệ thống.
- Nên backup hoặc có kế hoạch rollback trước khi chạy migration nguy hiểm ở production.
- Migration nên đi kèm code tương ứng, tránh code mới chạy với database cũ hoặc ngược lại.

###  

Migration là cách quản lý thay đổi cấu trúc database bằng code. Migration dùng để tạo bảng, sửa bảng, thêm cột, thêm index và giúp team đồng bộ database qua các môi trường.

---

## 6. Constraint

### Constraint là gì?

Constraint là ràng buộc dữ liệu trong database. Nó giúp database tự kiểm tra dữ liệu trước khi cho phép insert hoặc update.

Nếu dữ liệu không hợp lệ, database sẽ báo lỗi.

### Constraint dùng để làm gì?

- Đảm bảo dữ liệu đúng và nhất quán.
- Tránh dữ liệu trùng lặp không mong muốn.
- Đảm bảo quan hệ giữa các bảng.
- Giảm lỗi do code insert dữ liệu sai.

### Các constraint thường gặp

**PRIMARY KEY**

Khóa chính, dùng để định danh duy nhất một dòng trong bảng.

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY,
    name VARCHAR(255)
);
```

**FOREIGN KEY**

Khóa ngoại, dùng để tạo quan hệ giữa hai bảng.

```sql
CREATE TABLE orders (
    id BIGINT PRIMARY KEY,
    user_id BIGINT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

Nếu `user_id` không tồn tại trong bảng `users`, database sẽ không cho tạo order.

**UNIQUE**

Đảm bảo giá trị không bị trùng.

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY,
    email VARCHAR(255) UNIQUE
);
```

**NOT NULL**

Không cho phép giá trị `NULL`.

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
```

**CHECK**

Kiểm tra dữ liệu theo điều kiện.

```sql
CREATE TABLE products (
    id BIGINT PRIMARY KEY,
    price DECIMAL(12, 2) CHECK (price >= 0)
);
```

**DEFAULT**

Gán giá trị mặc định nếu insert không truyền giá trị.

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY,
    status VARCHAR(20) DEFAULT 'active'
);
```

### Lưu ý khi dùng constraint

- Nên để database bảo vệ các rule quan trọng, không chỉ validate ở backend.
- Foreign key giúp dữ liệu nhất quán nhưng cũng cần hiểu hành vi khi xóa dữ liệu, ví dụ `CASCADE`, `RESTRICT`, `SET NULL`.
- Unique constraint thường tự tạo index để kiểm tra trùng nhanh hơn.
- Constraint quá chặt khi nghiệp vụ chưa rõ có thể làm hệ thống khó thay đổi, nên cần thiết kế theo rule thật sự chắc chắn.

###  

Constraint là ràng buộc dữ liệu trong database như primary key, foreign key, unique, not null, check và default. Constraint dùng để đảm bảo dữ liệu đúng, không trùng sai và giữ quan hệ giữa các bảng nhất quán.

---

## 7. Index

### Index là gì?

Index là cấu trúc dữ liệu phụ giúp database tìm dữ liệu nhanh hơn.

Có thể hiểu đơn giản index giống mục lục của sách. Nếu không có mục lục, muốn tìm một nội dung thì phải đọc từ đầu đến cuối. Nếu có mục lục, mình có thể nhảy nhanh đến đúng vị trí cần tìm.

Trong database, nếu bảng có nhiều dòng và query lọc theo một cột, index giúp database không phải quét toàn bộ bảng trong nhiều trường hợp.

### Index dùng để làm gì?

- Tăng tốc query có `WHERE`.
- Tăng tốc join giữa các bảng.
- Hỗ trợ `ORDER BY`, `GROUP BY`.
- Giúp kiểm tra `UNIQUE` nhanh hơn.
- Có thể tạo covering index để query lấy đủ dữ liệu từ index mà không cần đọc thêm row trong bảng.

Ví dụ:

```sql
CREATE INDEX idx_users_email ON users(email);
```

Query sau có thể dùng index:

```sql
SELECT *
FROM users
WHERE email = 'a@example.com';
```

### Tại sao index làm query nhanh hơn?

Nếu không có index, database thường phải full table scan, nghĩa là đọc nhiều hoặc toàn bộ dòng trong bảng để tìm dữ liệu phù hợp.

Nếu có index phù hợp, database có thể tìm trong cấu trúc index trước. Với B-tree index, dữ liệu trong index được tổ chức theo thứ tự, nên database có thể tìm nhanh hơn rất nhiều so với đọc từng dòng. Sau khi tìm được vị trí trong index, database mới đọc row tương ứng trong bảng nếu cần.

Ví dụ bảng `users` có 10 triệu dòng:

```sql
SELECT *
FROM users
WHERE email = 'a@example.com';
```

Nếu không có index trên `email`, database có thể phải kiểm tra rất nhiều dòng. Nếu có index trên `email`, database có thể tìm theo index và lấy ra dòng cần thiết nhanh hơn.

Index cũng có thể giúp `ORDER BY` vì dữ liệu trong index đã có thứ tự:

```sql
CREATE INDEX idx_orders_created_at ON orders(created_at);

SELECT *
FROM orders
ORDER BY created_at DESC
LIMIT 20;
```

### Các loại index trong MySQL

Theo tài liệu MySQL 8.4, phần lớn index như `PRIMARY KEY`, `UNIQUE`, `INDEX` và nhiều trường hợp khác dùng B-tree. Spatial index dùng cấu trúc phù hợp dữ liệu không gian, Memory table có thể dùng hash index, còn Full-text index của InnoDB dùng inverted list.

Các loại index thường gặp trong MySQL:

**Primary key index**

Tạo tự động khi khai báo primary key. Dùng để định danh duy nhất row.

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY,
    name VARCHAR(255)
);
```

Nên dùng cho cột định danh chính như `id`.

**Unique index**

Đảm bảo giá trị không trùng và tăng tốc tìm kiếm theo cột đó.

```sql
CREATE UNIQUE INDEX idx_users_email ON users(email);
```

Nên dùng cho email, username, code, slug nếu nghiệp vụ yêu cầu không trùng.

**Normal index / Secondary index**

Index thông thường, dùng để tăng tốc query.

```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
```

Nên dùng cho cột hay xuất hiện trong `WHERE`, `JOIN`, `ORDER BY`.

**Composite index / Multiple-column index**

Index trên nhiều cột.

```sql
CREATE INDEX idx_orders_user_status_created
ON orders(user_id, status, created_at);
```

Nên dùng khi query thường lọc theo nhiều cột cùng nhau:

```sql
SELECT *
FROM orders
WHERE user_id = 1
  AND status = 'paid'
ORDER BY created_at DESC;
```

Với composite index cần nhớ nguyên tắc leftmost prefix. Index `(user_id, status, created_at)` có thể hỗ trợ tốt cho query theo:

- `user_id`
- `user_id, status`
- `user_id, status, created_at`

Nhưng thường không tối ưu nếu chỉ lọc theo `status` mà không có `user_id`.

**Full-text index**

Dùng cho tìm kiếm văn bản.

```sql
CREATE FULLTEXT INDEX idx_posts_title_body
ON posts(title, body);
```

Nên dùng khi cần search theo nội dung text dài như title, body, description.

**Spatial index**

Dùng cho dữ liệu không gian như tọa độ, vùng địa lý.

```sql
CREATE SPATIAL INDEX idx_places_location
ON places(location);
```

Nên dùng khi query liên quan đến location, geometry, khoảng cách hoặc vùng địa lý.

**Prefix index**

Index một phần đầu của chuỗi.

```sql
CREATE INDEX idx_users_name_prefix ON users(name(20));
```

Nên cân nhắc khi cột chuỗi dài nhưng chỉ cần index một phần để giảm kích thước index. Không nên dùng nếu phần prefix không đủ phân biệt dữ liệu.

**Descending index**

Index theo thứ tự giảm dần.

```sql
CREATE INDEX idx_orders_created_desc ON orders(created_at DESC);
```

Nên dùng khi query thường sort giảm dần và database có thể tận dụng thứ tự index.

**Invisible index**

Index vẫn tồn tại nhưng optimizer không dùng. Dùng để test xem bỏ index có ảnh hưởng query không trước khi drop thật.

```sql
ALTER TABLE orders ALTER INDEX idx_orders_user_id INVISIBLE;
```

**Functional index**

MySQL 8 hỗ trợ index trên expression thông qua functional key parts.

```sql
CREATE INDEX idx_users_lower_email
ON users ((LOWER(email)));
```

Nên dùng khi query thường lọc bằng expression giống hệt:

```sql
SELECT *
FROM users
WHERE LOWER(email) = 'a@example.com';
```

### Các loại index trong PostgreSQL

PostgreSQL hỗ trợ nhiều loại index hơn MySQL cho các bài toán khác nhau.

**B-tree index**

Đây là index mặc định và phổ biến nhất. Dùng tốt cho so sánh bằng, range, sort.

```sql
CREATE INDEX idx_users_email ON users(email);
```

Phù hợp với:

- `=`
- `<`, `<=`, `>`, `>=`
- `BETWEEN`
- `IN`
- `ORDER BY`
- `LIKE 'abc%'` trong một số điều kiện phù hợp

**Hash index**

Dùng cho so sánh bằng.

```sql
CREATE INDEX idx_users_email_hash
ON users USING HASH (email);
```

Chỉ nên cân nhắc khi query chủ yếu là equality `=`. Trong thực tế B-tree vẫn thường được dùng nhiều hơn vì linh hoạt hơn.

**GIN index**

GIN là inverted index, phù hợp với dữ liệu có nhiều phần tử bên trong một giá trị như array, JSONB hoặc full-text search.

```sql
CREATE INDEX idx_posts_tags
ON posts USING GIN (tags);
```

Ví dụ với JSONB:

```sql
CREATE INDEX idx_users_profile
ON users USING GIN (profile);
```

Nên dùng khi query tìm trong array, JSONB hoặc search text.

**GiST index**

GiST là dạng index linh hoạt, thường dùng cho dữ liệu hình học, địa lý, full-text search hoặc nearest-neighbor search tùy operator class.

```sql
CREATE INDEX idx_places_location
ON places USING GiST (location);
```

Nên dùng cho bài toán dữ liệu không gian, range type hoặc tìm điểm gần nhất.

**SP-GiST index**

SP-GiST phù hợp với một số cấu trúc dữ liệu phân hoạch không cân bằng như quad-tree, k-d tree, trie. Thường dùng cho dữ liệu không gian hoặc text prefix tùy operator class.

```sql
CREATE INDEX idx_places_point
ON places USING SPGIST (point_column);
```

Nên dùng khi dữ liệu và operator phù hợp với SP-GiST, thường là case đặc thù hơn B-tree.

**BRIN index**

BRIN lưu tóm tắt dữ liệu theo từng block vật lý. Rất nhỏ và phù hợp với bảng rất lớn khi dữ liệu có tương quan với thứ tự lưu.

```sql
CREATE INDEX idx_logs_created_at
ON logs USING BRIN (created_at);
```

Nên dùng cho bảng log, event, tracking có rất nhiều dòng và dữ liệu tăng dần theo thời gian như `created_at`, `id`.

**Bloom index**

Bloom là extension của PostgreSQL, dùng cho một số trường hợp muốn index nhiều cột và chấp nhận khả năng false positive. Đây là loại ít dùng hơn trong dự án CRUD thông thường.

### Khi nào sử dụng index nào?

**Dùng B-tree khi:**

- Query lọc theo `=`, range, `BETWEEN`, `IN`.
- Query sort theo `ORDER BY`.
- Query join qua khóa chính, khóa ngoại.
- Không chắc nên chọn index nào thì B-tree thường là lựa chọn đầu tiên.

Ví dụ:

```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
```

**Dùng composite index khi:**

- Query thường lọc hoặc sort theo nhiều cột cùng lúc.
- Thứ tự cột trong index cần bám theo query thực tế.

Ví dụ:

```sql
CREATE INDEX idx_orders_user_status
ON orders(user_id, status);
```

**Dùng unique index khi:**

- Nghiệp vụ yêu cầu dữ liệu không được trùng.

Ví dụ:

```sql
CREATE UNIQUE INDEX idx_users_email ON users(email);
```

**Dùng full-text index hoặc GIN full-text khi:**

- Cần tìm kiếm trong text dài.
- Không phù hợp với kiểu `LIKE '%keyword%'` trên bảng lớn.

**Dùng GIN trong PostgreSQL khi:**

- Query JSONB, array hoặc full-text search.

**Dùng BRIN trong PostgreSQL khi:**

- Bảng rất lớn.
- Dữ liệu có thứ tự tự nhiên, ví dụ log theo thời gian.
- Muốn index nhỏ hơn B-tree rất nhiều.

**Dùng spatial / GiST / SP-GiST khi:**

- Query dữ liệu địa lý, hình học, tọa độ hoặc tìm khoảng cách.

**Không nên dùng index khi:**

- Bảng rất nhỏ.
- Cột có quá ít giá trị khác nhau, ví dụ boolean `is_active`, nếu query không đủ chọn lọc.
- Query thường lấy phần lớn dữ liệu trong bảng.
- Cột hiếm khi dùng để lọc, join hoặc sort.

### Đánh index nhiều có ảnh hưởng gì không?

Có. Index giúp đọc nhanh hơn nhưng không miễn phí.

Ảnh hưởng khi có quá nhiều index:

- Insert chậm hơn vì mỗi lần thêm row database phải cập nhật thêm các index.
- Update chậm hơn nếu cập nhật cột có index.
- Delete chậm hơn vì phải xóa dữ liệu liên quan trong index.
- Tốn thêm dung lượng lưu trữ.
- Tốn thêm bộ nhớ/cache.
- Optimizer có nhiều lựa chọn hơn, đôi khi thống kê không tốt có thể chọn plan chưa tối ưu.
- Migration tạo index trên bảng lớn có thể mất thời gian và ảnh hưởng production nếu không làm cẩn thận.

Vì vậy không nên đánh index theo cảm tính. Nên dựa vào query thực tế, dùng `EXPLAIN` hoặc `EXPLAIN ANALYZE` để kiểm tra database có dùng index không.

### Ví dụ dùng EXPLAIN

MySQL:

```sql
EXPLAIN
SELECT *
FROM orders
WHERE user_id = 1;
```

PostgreSQL:

```sql
EXPLAIN ANALYZE
SELECT *
FROM orders
WHERE user_id = 1;
```

###  

Index là cấu trúc dữ liệu giúp database tìm dữ liệu nhanh hơn, giống như mục lục của sách. Index thường dùng cho cột hay lọc, join, sort hoặc cần unique. Index làm query nhanh hơn vì database có thể tìm trong cấu trúc index thay vì quét toàn bộ bảng. Tuy nhiên đánh quá nhiều index sẽ làm insert, update, delete chậm hơn và tốn thêm dung lượng, nên cần tạo index theo query thực tế.

---

## 8. N+1 Problem

### N+1 Problem là gì?

N+1 Problem là lỗi hiệu năng thường gặp khi code lấy danh sách dữ liệu bằng 1 query, sau đó với mỗi dòng lại chạy thêm 1 query để lấy dữ liệu liên quan.

Ví dụ lấy 100 users:

```sql
SELECT * FROM users LIMIT 100;
```

Sau đó với mỗi user lại query orders:

```sql
SELECT * FROM orders WHERE user_id = 1;
SELECT * FROM orders WHERE user_id = 2;
SELECT * FROM orders WHERE user_id = 3;
...
```

Tổng cộng sẽ có 1 query lấy users và N query lấy orders. Nếu N = 100 thì có 101 query.

### N+1 Problem dùng để nói về vấn đề gì?

N+1 không phải là tính năng, mà là một vấn đề hiệu năng. Nó làm hệ thống chậm vì số lượng query tăng theo số lượng record.

Vấn đề này hay xảy ra khi dùng ORM như Laravel Eloquent, nếu truy cập relationship trong vòng lặp mà không eager load.

Ví dụ dễ bị N+1 trong Laravel:

```php
$users = User::all();

foreach ($users as $user) {
    echo $user->orders->count();
}
```

Nếu chưa load `orders`, mỗi lần gọi `$user->orders` có thể phát sinh thêm query.

### Cách xử lý N+1 Problem

**Dùng eager loading**

Laravel:

```php
$users = User::with('orders')->get();
```

Lúc này Laravel sẽ lấy users và orders bằng ít query hơn.

**Dùng join nếu cần dữ liệu dạng phẳng**

```sql
SELECT users.id, users.name, orders.id AS order_id, orders.total
FROM users
LEFT JOIN orders ON orders.user_id = users.id;
```

**Dùng aggregate query nếu chỉ cần số lượng**

Laravel:

```php
$users = User::withCount('orders')->get();
```

SQL:

```sql
SELECT users.id, users.name, COUNT(orders.id) AS orders_count
FROM users
LEFT JOIN orders ON orders.user_id = users.id
GROUP BY users.id, users.name;
```

### Cách phát hiện N+1

- Bật query log khi chạy local.
- Dùng Laravel Debugbar hoặc Telescope.
- Xem log database.
- Kiểm tra những đoạn code query relationship trong vòng lặp.
- Nếu API trả chậm khi số lượng record tăng, cần nghi ngờ N+1.

###  

N+1 Problem là vấn đề khi lấy danh sách bằng 1 query rồi mỗi item lại phát sinh thêm query để lấy dữ liệu liên quan. Nó làm số lượng query tăng rất nhanh và gây chậm hệ thống. Cách xử lý là eager loading, join hoặc dùng aggregate query như `withCount`.

---

## Tóm tắt nhanh

- SQL Basic: ngôn ngữ để làm việc với database quan hệ.
- Join: kết hợp dữ liệu từ nhiều bảng.
- Aggregate: tổng hợp dữ liệu bằng `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`.
- Database Design: thiết kế bảng, cột, quan hệ, constraint và index.
- Migration: quản lý thay đổi cấu trúc database bằng code.
- Constraint: ràng buộc dữ liệu để đảm bảo tính đúng đắn.
- Index: cấu trúc giúp truy vấn nhanh hơn nhưng làm ghi dữ liệu tốn chi phí hơn.
- N+1 Problem: lỗi hiệu năng do phát sinh quá nhiều query khi lấy dữ liệu liên quan trong vòng lặp.

## Nguồn tham khảo

- MySQL 8.4 Reference Manual - How MySQL Uses Indexes: https://dev.mysql.com/doc/refman/8.4/en/mysql-indexes.html
- MySQL 8.4 Reference Manual - CREATE INDEX Statement: https://dev.mysql.com/doc/refman/8.4/en/create-index.html
- PostgreSQL 18 Documentation - Index Types: https://www.postgresql.org/docs/current/indexes-types.html
