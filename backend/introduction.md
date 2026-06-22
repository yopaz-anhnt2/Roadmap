# Backend Introduction

## Internet
Là gì?
- Mạng lưới kết nối nhiều thiết bị trên toàn thế giới với nhau thông qua giao thức truyền dữ liệu chung

Vì sao cần?
- Giúp trao đổi dữ liệu, truy cập thông tin, sử dụng dịch vụ tù xa, kết nối với nhau ở bất kỳ đâu trên thế giới

Hoạt động như thế nào?
- Định danh thiết bị bằng IP, phân giải tên miền qua DNS, chia dữ liệu thành packet, định tuyến qua nhiều router trên toàn cầu bằng IP routing -> ghép lại ở thiết bị nhận nhờ TCP/IP để đảm bảo dữ liệu đầy đủ và chính xác

Khi nào dùng?
- Khi thiết bị cần giao tiếp hoặc trao đổi dữ liệu với một hệ thống nằm ngoài mạng nội bộ

## HTTP
Là gì?
- HyperText Transfer Protocol - giao thức dùng để trình duyệt và máy chủ web giao tiếp với nhau

Vì sao cần?
- Trình duyệt không biết cách hỏi server lấy dữ liệu
- Server không biết phải trả dữ liệu theo dạng gì
-> Giúp mọi trình duyệt và website hiểu nhau

Hoạt động như thế nào?
- Gửi request: khi truy cập trang web, trình duyệt gửi 1 gói tin HTTP Request đến máy chủ
- Xử lý: Máy chủ tiếp nhận, giải mã và tìm kiếm tài nguyên
- Phản hồi: Máy chủ gửi lại gói tin phản hồi HTTP Response chứa dữ liệu trang web cùng với các mã trạng thái

Khi nào dùng?
- Dùng khi mở website
- Khi gọi API

## Domain Name
Là gì?
- Địa chỉ định danh của một trang web trên internet
Ví dụ google.com

Vì sao cần?
- Dễ dàng ghi nhớ thay vì nhớ địa chỉ IP

Khi nào dùng?
- Dùng khi muốn người dùng truy cập hệ thống bằng tên rõ ràng

## Hosting
Là gì?
- Nơi lưu trữ toàn bộ dữ liệu, mã nguồn, hình ảnh trên server

Vì sao cần?
- Website hoạt động 24/7
- Hiển thị trên internet
- Lưu trữ dữ liệu

Hoạt động như thế nào?
- B1: Lưu trữ: Mua một gói hosting và tải các tệp tin web của mình lên không gian đó.
- B2: Liên kết: liên kết tên miền với địa chỉ IP của Hosting
- B3: Truy cập:  Khi người dùng gõ tên miền của bạn vào trình duyệt, hệ thống sẽ gọi dữ liệu từ máy chủ Hosting. Sau đó, hosting sẽ truyền các tập tin này (HTML, CSS, hình ảnh) về máy người dùng để hiển thị thành trang web hoàn chỉnh.

Khi nào dùng?
- Xây dựng trang web

## DNS
Là gì?
- Domain Name System: Hệ thống phân giải domain name thành địa chỉ IP

Vì sao cần?
- Giúp tìm đúng server khi người dùng nhập tên miền

Hoạt động như thế nào?
- B1: Trình duyệt kiểm tra cache
Có lưu IP của google.com chưa -> chưa -> hỏi DNS
- B2: Gửi yêu cầu đến DNS Server
google.com có IP là gì?
- B3: DNS tra cứu, DNS tìm trong hệ thống
- B4: Trả về IP
- B5: Máy tính kết nối đến server

Khi nào dùng?
- Dùng mỗi khi truy cập domain hoặc cấu hình tên miền cho dịch vụ

## Browser
Là gì?
- Phần mềm dùng để truy cập và hiển thị nội dung web
Ví dụ: Google Chrome, Safari,...

Vì sao cần?
- Giúp người dùng xem website và tương tác với ứng dụng web

Hoạt động như thế nào?
- Nhập địa chỉ website -> Nhận dữ liệu: Máy chủ trả về dữ liệu -> Hiển thị ra màn hình

Khi nào dùng?
- Dùng khi mở website
