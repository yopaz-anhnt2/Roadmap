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
```nginx
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    root /srv/example.com/public;
 
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
 
    index index.php;
 
    charset utf-8;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
 
    error_page 404 /index.php;
 
    location ~ ^/index\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
        fastcgi_hide_header X-Powered-By;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

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

Linux (L)
- hệ điều hành server
- chạy toàn bộ hệ thống

Nginx (E)
- nhận request
- xử lý hoặc chuyển tiếp
- tối ưu hiệu năng

MySQL (M)
- lưu dữ liệu
- database

PHP (P)
- xử lý logic backend


## Tại sao cần tới Nginx
Là gì?
- Nginx là máy chủ web mã nguồn mở dùng để nhận request từ người dùng rồi chuyển đến ứng dụng phía sau

Tại sao cần:
- Hiệu suất và tốc độ xử lý cao: Nhờ kiến trúc bất đồng bộ (asynchronous) và không chặn (non-blocking), NGINX có thể xử lý hàng nghìn kết nối đồng thời mà vẫn đảm bảo hiệu suất ổn định, đặc biệt với các tệp tĩnh như HTML, CSS, JS, hình ảnh,…
- Tiêu thụ tài nguyên thấp: NGINX sử dụng ít RAM và CPU, cải thiện hiệu suất ngay cả trên máy chủ cấu hình thấp
- Reverse Proxy:  Đứng giữa người dùng và máy chủ ứng dụng , nhận yêu cầu, xử lý và chuyển tiếp kết nối, giúp bảo vệ cấu trúc bên trong mạng.
- Cân bằng tải:  Phân phối lưu lượng truy cập đồng đều qua nhiều máy chủ khác nhau, đảm bảo hệ thống không bị quá tải và luôn sẵn sàng
- Hỗ trợ SSL/TLS mạnh mẽ: Hỗ trợ HTTP/2 và TLS 1.3, đảm bảo tốc độ và bảo mật truyền tải dữ liệu
- Khả năng mở rộng linh hoạt: Dễ dàng cấu hình để mở rộng hệ thống từ đơn giản đến phức tạp
- Caching hiệu quả: Bộ nhớ đệm (cache) tích hợp giúp tăng tốc tải trang và giảm tải cho backend
- Hỗ trợ giao thức hiện đại: Hỗ trợ WebSocket, HTTP/2, gRPC và nhiều giao thức hiện đại khác
- Tăng tốc website và Tiết kiệm tài nguyên: Xử lý cực nhanh các file tĩnh (ảnh, CSS, Javascript) và lưu trữ bộ nhớ đệm (caching), từ đó giảm tải đáng kể cho các máy chủ backend
- Cộng đồng lớn và tài liệu phong phú: Dễ dàng tìm kiếm hỗ trợ và giải quyết vấn đề

## Nginx khác gì Apache
Giống:
+ Đều là web server dùng để
- Nhận request từ browser
- Xử lý hoặc chuyển tiếp tới backend
- Trả response về người dùng

Khác:
+ Phương thức xử lý kết nối
Nginx: Hướng sự kiện (Event-driven). Xử lý nhiều yêu cầu trên cùng một tiến trình, tiết kiệm bộ nhớ.

Apache: Theo tiến trình/luồng (Process/Thread-based). Tạo một luồng/tiến trình mới cho mỗi yêu cầu.

+ Tài nguyên
Nginx: Tiêu thụ ít RAM và CPU, tốc độ xử lý file tĩnh cực nhanh.
Apache: Tốn tài nguyên hơn khi lượng truy cập lớn, dễ bị quá tải (chậm) nếu vượt mức.

+ Hiệu năng
Nginx: Vượt trội hơn Apache trong việc phục vụ nội dung tĩnh và xử lý số lượng lớn kết nối đồng thời

Apache: Xử lý đồng thời ít kết nối hơn và tốc độ phục vụ nội dung tĩnh không nhanh bằng NGINX

+ Hệ điều hành hỗ trợ:
Nginx: Chạy tốt trên Linux/Unix, có hỗ trợ Windows nhưng hiệu suất không cao

Apache: Chạy tốt trên cả Linux/Unix và Windows

+ Tệp cấu hình
Nginx: Chỉ đọc file cấu hình toàn cục (nginx.conf), hiệu năng cao nhưng kém linh hoạt cho các thư mục con.
Apache: Hỗ trợ tệp cấu hình phân tán .htaccess cho phép người dùng tùy chỉnh từng thư mục mà không cần khởi động lại.

+ Nội dung: 
Nginx: Vượt trội khi phục vụ nội dung tĩnh (hình ảnh, video, HTML).
Apache: Xử lý nội dung động tốt, tích hợp sẵn nhiều mô-đun để chạy code


## HTTP/1.1 khác gì HTTP/2
Điểm khác nhau:
HTTP1.1
- 1 kết nối TCP = xử lý lần lượt từng request

HTTP2
- 1 kết nối TCP duy nhất có thể gửi nhiều request cùng lúc

Định dạng dữ liệu:
HTTP1.1:
- Gửi thông điệp dưới dạng văn bản thuần túy

HTTP2:
- Mã hóa thành dạng nhị phân (binary) giúp tiêu thụ ít băng thông và phân tích cú pháp hiệu quả hơn

Xử lý Đa kênh (Multiplexing):
HTTP1.1:
- Xử lý tuần tự các yêu cầu; nếu một yêu cầu bị nghẽn, các yêu cầu sau sẽ bị chờ

HTTP2:
- Hỗ trợ gửi/nhận nhiều yêu cầu và phản hồi đồng thời trên cùng một kết nối

Quản lý Header:
HTTP1.1:
- Gửi kèm siêu dữ liệu (metadata) dưới dạng văn bản không nén trong mỗi yêu cầu, gây lãng phí băng thông

HTTP2:
- Sử dụng thuật toán HPACK để nén các header, giảm thiểu kích thước dữ liệu truyền qua mạng

Cơ chế tải:
HTTP1.1:
- Cần mở nhiều kết nối TCP song song (thường là 6 kết nối trên mỗi tên miền) để trình duyệt tải nhanh hơn

HTTP2:
- Sử dụng một kết nối TCP duy nhất để truyền toàn bộ tài nguyên

Mức độ ưu tiên:
HTTP1.1:
- Không hỗ trợ; trình duyệt tự quyết định thứ tự tải tài nguyên

HTTP2:
- Hỗ trợ ưu tiên theo trọng số, cho phép máy chủ biết tài nguyên nào quan trọng để phân phối trước

Server Push:
HTTP1.1:
- Không hỗ trợ

HTTP2:
- Cho phép máy chủ chủ động gửi các tài nguyên (như CSS, JS) về máy khách trước khi trình duyệt yêu cầu

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
