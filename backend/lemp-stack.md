# LEMP Stack

## 1. LEMP Stack là gì?

LEMP là một bộ phần mềm thường được sử dụng để triển khai các ứng dụng web, đặc biệt là các ứng dụng PHP như Laravel.

LEMP bao gồm:

- **L (Linux)**: Hệ điều hành chạy server.
- **E (Engine-X / Nginx)**: Web Server xử lý request từ client.
- **M (MySQL hoặc MariaDB)**: Hệ quản trị cơ sở dữ liệu.
- **P (PHP)**: Ngôn ngữ lập trình phía server.

Khi người dùng truy cập website:

1. Request được gửi đến Nginx.
2. Nginx chuyển request PHP cho PHP-FPM xử lý.
3. PHP truy vấn dữ liệu từ MySQL nếu cần.
4. Kết quả được trả lại cho Nginx.
5. Nginx gửi response về trình duyệt.

---

## 2. Các bước cài đặt LEMP Stack trên Ubuntu

### Bước 1: Cập nhật hệ thống

```bash
sudo apt update
sudo apt upgrade -y
```

Mục đích:

- Cập nhật danh sách package.
- Nâng cấp các package hiện có.

### Bước 2: Cài đặt Nginx

```bash
sudo apt install nginx -y
```

Kiểm tra:

```bash
systemctl status nginx
```

Mở trình duyệt truy cập:

```text
http://SERVER_IP
```

Nếu thấy trang Welcome to Nginx nghĩa là cài đặt thành công.

### Bước 3: Cài đặt MySQL

```bash
sudo apt install mysql-server -y
```

Kiểm tra:

```bash
systemctl status mysql
```

Thiết lập bảo mật:

```bash
sudo mysql_secure_installation
```

### Bước 4: Cài đặt PHP và PHP-FPM

```bash
sudo apt install php-fpm php-mysql -y
```

Kiểm tra:

```bash
php -v
```

Kiểm tra PHP-FPM:

```bash
systemctl status php8.x-fpm
```

(Phiên bản có thể thay đổi tùy Ubuntu.)

### Bước 5: Cấu hình Nginx sử dụng PHP

Tạo Virtual Host:

```bash
sudo nano /etc/nginx/sites-available/example.com
```

Cấu hình:

```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/example/public;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.x-fpm.sock;
    }
}
```

Kích hoạt:

```bash
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

Kiểm tra:

```bash
sudo nginx -t
```

Reload:

```bash
sudo systemctl reload nginx
```

---

## 3. Tại sao cần Nginx?

Nginx là web server chịu trách nhiệm nhận request từ người dùng.

Các nhiệm vụ chính:

- Phục vụ file tĩnh (HTML, CSS, JS, ảnh).
- Reverse Proxy.
- Load Balancer.
- SSL/TLS termination.
- Chuyển request PHP sang PHP-FPM.

Nếu không có Nginx thì trình duyệt sẽ không thể giao tiếp trực tiếp với ứng dụng PHP một cách hiệu quả.

Nginx được sử dụng rất phổ biến vì:

- Hiệu năng cao.
- Tiêu tốn ít RAM.
- Xử lý nhiều kết nối đồng thời tốt.

---

## 4. Nginx khác gì Apache?

### Apache

Ưu điểm:

- Ra đời sớm.
- Tài liệu phong phú.
- Hỗ trợ .htaccess.

Nhược điểm:

- Tiêu tốn nhiều RAM hơn.
- Hiệu năng thấp hơn khi có nhiều kết nối đồng thời.

### Nginx

Ưu điểm:

- Hiệu năng cao.
- Sử dụng ít tài nguyên.
- Xử lý concurrent connection tốt.

Nhược điểm:

- Không hỗ trợ .htaccess.
- Cấu hình ban đầu khó làm quen hơn Apache.

Hiện nay đa số hệ thống Laravel, NodeJS và các hệ thống có lượng truy cập lớn thường sử dụng Nginx.

---

## 5. HTTP/1.1 khác gì HTTP/2?

### HTTP/1.1

Đặc điểm:

- Một kết nối chỉ xử lý một request tại một thời điểm.
- Xảy ra hiện tượng Head-of-Line Blocking.
- Website phải tạo nhiều kết nối để tải tài nguyên nhanh hơn.

### HTTP/2

Đặc điểm:

- Multiplexing: nhiều request trên cùng một kết nối.
- Header Compression.
- Giảm độ trễ.
- Tăng tốc độ tải trang.

Ví dụ:

Một website có:

- 1 file HTML
- 20 file CSS
- 30 file JS
- 50 hình ảnh

HTTP/2 cho phép tải đồng thời hiệu quả hơn HTTP/1.1 nên website thường nhanh hơn đáng kể.

---

## 6. VPS là gì?

VPS (Virtual Private Server) là máy chủ ảo được tạo ra từ một máy chủ vật lý.

Mỗi VPS có:

- CPU riêng.
- RAM riêng.
- Disk riêng.
- Hệ điều hành riêng.

Người dùng có quyền root giống như một server độc lập.

Ví dụ:

Một server vật lý:

- 32 CPU
- 128GB RAM

Có thể chia thành:

- VPS A: 4 CPU, 8GB RAM
- VPS B: 8 CPU, 16GB RAM
- VPS C: 4 CPU, 8GB RAM

---

## 7. VPS khác gì Server vật lý?

### VPS

Ưu điểm:

- Chi phí thấp.
- Dễ mở rộng.
- Tạo nhanh trong vài phút.

Nhược điểm:

- Chia sẻ tài nguyên với VPS khác.
- Hiệu năng không ổn định bằng server vật lý.

### Server vật lý (Dedicated Server)

Ưu điểm:

- Toàn bộ tài nguyên thuộc về một khách hàng.
- Hiệu năng cao.
- Phù hợp hệ thống lớn.

Nhược điểm:

- Chi phí cao.
- Nâng cấp phần cứng khó hơn.

---

## Tổng kết

LEMP Stack là bộ công nghệ phổ biến để triển khai ứng dụng web PHP trên Linux. Trong đó Nginx đóng vai trò web server, MySQL lưu trữ dữ liệu và PHP xử lý nghiệp vụ. Khi triển khai thực tế, LEMP thường được cài đặt trên VPS hoặc server vật lý và sử dụng HTTP/2 để tăng hiệu năng cho website.
