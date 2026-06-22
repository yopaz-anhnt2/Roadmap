# LEMP Stack

## Các bước cài đặt LEMP stack
Bước 1 Cài đặt Nginx web server
+ Cập nhật hệ thống
- Chạy `sudo apt upgrade -y`

Ghi chú:
- Giúp hệ thống mới nhất trước khi cài gói

+ Cài Nginx
- Chạy `sudo apt install nginx -y`

+ Kiểm tra Nginx 
- Chạy `sudo systemctl status nginx`
Output mong muốn:
nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since...

Bước 2: Cài MySQL
+ Tải Mysql:
- Chạy `sudo apt install mysql-server -y`

+ Bảo mật Mysql sau khi cài đặt
- Chạy `sudo mysql_secure_installation` để tăng bảo mật

+ Test mysql
- Chạy `sudo mysql`
- Thoát `exit`

Bước 3: Cài PHP và PHP-FPM
- Chạy `sudo apt install php8.4-fpm php-mysql -y`
- Tùy thuộc vào php phiên bản bao nhiêu
- `php-fpm` giúp Nginx làm việc với PHP

+ Kiểm tra PHP-FPM đang chạy
- `sudo systemctl status php8.4-fpm`
- `php -v`

Bước 4: Cấu hình Nginx cho PHP
- Tạo thư mục web
`sudo mkdir /var/www/your_domain`

- Cấp quyền cho thư mục
`sudo chown -R $USER:$USER /var/www/your_domain`


- Tạo server block cho project
`sudo nano /etc/nginx/sites-available/your_domain`

- Ví dụ lấy cấu hình NGINX trên NGINX Laravel
`server { ....`

Bước 5: Kích hoạt cấu hình website

`sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/`

- Unlink cấu hình default
`sudo unlink /etc/nginx/sites-enabled/default`

Bước 6: Kiểm tra cấu hình Nginx
- Chạy `sudo nginx -t`

Bước 7: Khởi động lại dịch vụ
- Chạy `sudo systemctl reload nginx`
- Chạy `sudo systemctl restart php*-fpm`

Bước 8: Kiểm tra truy cập nếu có code
- Truy cập `http://server_domain_or_IP`

## LEMP stack là gì
Là gì?
- LEMP là gồm Linux, Nginx, MySQL hoặc MariaDB, và PHP

+ Linux (L)
- hệ điều hành server
- chạy toàn bộ hệ thống

+ Nginx (E)
- nhận request
- xử lý hoặc chuyển tiếp
- tối ưu hiệu năng

+ MySQL (M)
- lưu dữ liệu
- database

+ PHP (P)
- xử lý logic backend


## Tại sao cần tới Nginx
Là gì?
- Nginx là máy chủ web mã nguồn mở 

Tại sao cần:
- Xử lý lượng truy cập lớn
- Reverse Proxy:  Đứng giữa người dùng và máy chủ ứng dụng , nhận yêu cầu, xử lý và chuyển tiếp kết nối, giúp bảo vệ cấu trúc bên trong mạng.
- Cân bằng tải:  Phân phối lưu lượng truy cập đồng đều qua nhiều máy chủ khác nhau, đảm bảo hệ thống không bị quá tải và luôn sẵn sàng
- Tăng tốc website và Tiết kiệm tài nguyên: Xử lý cực nhanh các file tĩnh (ảnh, CSS, Javascript) và lưu trữ bộ nhớ đệm (caching), từ đó giảm tải đáng kể cho các máy chủ backend

## Nginx khác gì Apache
Giống:
+ Đều là web server dùng để
- Nhận request từ browser
- Xử lý hoặc chuyển tiếp tới backend
- Trả response về người dùng

Khác:
+ Kiến trúc
Nginx: Hướng sự kiện (Event-driven). Xử lý nhiều yêu cầu trên cùng một tiến trình, tiết kiệm bộ nhớ.

Apache: Theo tiến trình/luồng (Process/Thread-based). Tạo một luồng/tiến trình mới cho mỗi yêu cầu.

+ Tài nguyên
Nginx: Tiêu thụ ít RAM và CPU, tốc độ xử lý file tĩnh cực nhanh.
Apache: Tốn tài nguyên hơn khi lượng truy cập lớn, dễ bị quá tải (chậm) nếu vượt mức.

+ Tệp cấu hình
Nginx: Chỉ đọc file cấu hình toàn cục (nginx.conf), hiệu năng cao nhưng kém linh hoạt cho các thư mục con.
Apache: Hỗ trợ tệp cấu hình phân tán .htaccess cho phép người dùng tùy chỉnh từng thư mục mà không cần khởi động lại.

+ Nội dung: 
Nginx: Vượt trội khi phục vụ nội dung tĩnh (hình ảnh, video, HTML).
Apache: Xử lý nội dung động tốt, tích hợp sẵn nhiều mô-đun để chạy code


## HTTP/1.1 khác gì HTTP/2
HTTP/1 là gì?
- Phiên bản cũ hơn, mỗi request thường bị giới hạn hơn trong cách truyền

HTTP/2 là gì?
- Phiên bản mới hơn, cho phép nhiều request đi chung một kết nối

HTTP1.1
- 1 kết nối TCP = xử lý lần lượt từng request

HTTP2
- 1 kết nối TCP duy nhất có thể gửi nhiều request cùng lúc

## VPS là gì, khác gì server vật lý
VPS là gì?
- VPS là máy chủ ảo chạy trên một máy chủ vật lý lớn hơn

Server vật lý là gì?
- Là máy chủ thật, tài nguyên dùng riêng cho một hệ thống

Khác nhau ở đâu?
- VPS rẻ hơn và triển khai nhanh hơn
- Server vật lý mạnh hơn, ổn định hơn khi cần tài nguyên riêng
- VPS chia sẻ phần cứng với các máy ảo khác

Khi nào dùng VPS?
- Khi làm web nhỏ, web vừa, môi trường test, hoặc startup

Khi nào dùng server vật lý?
- Khi cần hiệu năng cao, bảo mật riêng, hoặc tải rất lớn
