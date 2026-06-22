## 1. Internet

- Internet là hệ thống mạng toàn cầu, kết nối rất nhiều máy tính, điện thoại, server và thiết bị lại với nhau.
- Nhờ Internet, người dùng có thể truy cập website, gửi email, xem video, nhắn tin, làm việc online.
- Internet không phải là một website, mà là hạ tầng kết nối để các dịch vụ online hoạt động.
- Ví dụ: khi mình vào Google, Facebook, YouTube thì đều đang sử dụng Internet.

## 2. HTTP

- HTTP là viết tắt của HyperText Transfer Protocol.
- Đây là giao thức dùng để truyền dữ liệu giữa trình duyệt và server.
- Khi người dùng nhập địa chỉ website, browser sẽ gửi request đến server thông qua HTTP/HTTPS.
- Server xử lý và trả về response, thường là HTML, CSS, JavaScript, hình ảnh, JSON...
- HTTPS là phiên bản bảo mật hơn của HTTP vì dữ liệu được mã hóa.
- Ví dụ: trình duyệt gửi request GET để lấy trang web, hoặc POST để gửi dữ liệu form đăng nhập.

## 3. Domain Name

- Domain Name là tên miền của website.
- Nó giúp người dùng truy cập website dễ nhớ hơn thay vì phải nhớ địa chỉ IP.
- Ví dụ: google.com, facebook.com, github.com.
- Một domain thường gồm tên chính và phần mở rộng.
- Ví dụ: trong `example.com`, `example` là tên, `.com` là phần mở rộng.
- Domain cần được đăng ký qua nhà cung cấp tên miền.

## 4. Hosting

- Hosting là nơi lưu trữ dữ liệu của website.
- Các file như source code, hình ảnh, video, database có thể được đặt trên hosting/server.
- Khi người dùng truy cập website, dữ liệu sẽ được lấy từ hosting và trả về trình duyệt.
- Có nhiều loại hosting như shared hosting, VPS, cloud hosting, dedicated server.
- Với web nhỏ có thể dùng shared hosting, còn web lớn thường dùng VPS hoặc cloud server.
- Ví dụ: deploy một website Laravel lên VPS thì VPS đó đóng vai trò là nơi hosting website.

## 5. DNS

- DNS là viết tắt của Domain Name System.
- DNS có nhiệm vụ chuyển đổi domain name thành địa chỉ IP.
- Máy tính không hiểu trực tiếp tên miền như `google.com`, mà cần biết IP của server.
- Khi nhập domain vào browser, DNS sẽ tìm IP tương ứng rồi browser mới kết nối đến server.
- Có thể hiểu DNS giống như danh bạ điện thoại của Internet.
- Ví dụ: `example.com` có thể được DNS trỏ tới IP `192.0.2.1`.

## 6. Browser

- Browser là trình duyệt web, dùng để truy cập và hiển thị website.
- Một số browser phổ biến: Google Chrome, Microsoft Edge, Firefox, Safari.
- Browser gửi request đến server, nhận dữ liệu trả về rồi render thành giao diện cho người dùng.
- Browser có thể đọc HTML để tạo cấu trúc trang, CSS để hiển thị giao diện, JavaScript để xử lý tương tác.
- Ngoài ra browser còn hỗ trợ cache, cookie, localStorage, devtools để kiểm tra website.
- Ví dụ: khi mở Chrome và vào một trang web, Chrome sẽ tải dữ liệu từ server rồi hiển thị thành trang web hoàn chỉnh.

## 7. Luồng hoạt động cơ bản khi truy cập website

- Người dùng nhập domain vào browser.
- Browser kiểm tra cache/DNS để tìm địa chỉ IP của domain.
- DNS trả về IP của server tương ứng.
- Browser gửi request HTTP/HTTPS đến server.
- Server xử lý request và trả về response.
- Browser nhận HTML, CSS, JavaScript, hình ảnh...
- Browser render dữ liệu đó thành giao diện website cho người dùng xem.

## 8. Mối liên hệ giữa các khái niệm

- Internet là môi trường kết nối các thiết bị.
- Browser là công cụ để người dùng truy cập website.
- Domain Name là địa chỉ dễ nhớ của website.
- DNS chuyển domain thành IP để tìm đúng server.
- Hosting là nơi chứa website.
- HTTP/HTTPS là giao thức truyền dữ liệu giữa browser và server.

Tóm lại, khi truy cập một website thì các thành phần này phối hợp với nhau. Người dùng dùng browser nhập domain, DNS tìm IP, browser gửi request qua HTTP/HTTPS đến hosting/server, sau đó server trả dữ liệu về để browser hiển thị website.
