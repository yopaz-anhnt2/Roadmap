# Database Basic

## SQL Basic

Là gì?
- SQL (Structured Query Language) là ngôn ngữ truy vấn có cấu trúc dùng để thao tác và quản lý dữ liệu trong cơ sở dữ liệu quan hệ (relational database)
- Gồm các nhóm lệnh chính:
  - DDL (Data Definition Language): định nghĩa cấu trúc — `CREATE`, `ALTER`, `DROP`
  - DML (Data Manipulation Language): thao tác dữ liệu — `INSERT`, `UPDATE`, `DELETE`
  - DQL (Data Query Language): truy vấn dữ liệu — `SELECT`
  - DCL (Data Control Language): phân quyền — `GRANT`, `REVOKE`
  - TCL (Transaction Control Language): quản lý giao dịch — `COMMIT`, `ROLLBACK`

Vì sao cần?
- Là cách chuẩn hóa để giao tiếp với hầu hết các hệ quản trị CSDL quan hệ (MySQL, PostgreSQL, SQL Server, Oracle...)
- Cho phép lấy đúng dữ liệu cần thiết thay vì đọc toàn bộ rồi lọc bằng code
- Dễ học, gần với ngôn ngữ tự nhiên, mạnh mẽ khi xử lý dữ liệu lớn

Hoạt động như thế nào?
- Ứng dụng gửi câu lệnh SQL đến database server
- Database parse (phân tích cú pháp) -> tạo execution plan (kế hoạch thực thi tối ưu) -> thực thi trên dữ liệu -> trả về kết quả

Khi nào dùng?
- Mỗi khi cần thêm, sửa, xóa, hoặc đọc dữ liệu từ cơ sở dữ liệu quan hệ
- Khi cần lọc, sắp xếp, nhóm dữ liệu theo điều kiện

## Join

Là gì?
- Là phép kết hợp dữ liệu từ hai hay nhiều bảng dựa trên một cột liên quan giữa chúng (thường là khóa chính - khóa ngoại)
- Các loại Join phổ biến:
  - `INNER JOIN`: chỉ lấy các bản ghi khớp ở cả hai bảng
  - `LEFT JOIN`: lấy tất cả bản ghi bảng trái + bản ghi khớp ở bảng phải (không khớp thì NULL)
  - `RIGHT JOIN`: ngược lại với LEFT JOIN
  - `FULL OUTER JOIN`: lấy tất cả bản ghi của cả hai bảng

Vì sao cần?
- Trong database quan hệ, dữ liệu được tách thành nhiều bảng để tránh trùng lặp (chuẩn hóa)
- Join giúp ghép lại dữ liệu liên quan để hiển thị thông tin đầy đủ
- Ví dụ: bảng `orders` chỉ lưu `user_id`, cần Join với bảng `users` để biết tên người đặt hàng

Hoạt động như thế nào?
- Database so khớp các dòng giữa các bảng theo điều kiện `ON`
- Với mỗi dòng ở bảng `orders`, database tìm dòng tương ứng ở `users` có `id = user_id` rồi ghép lại

Khi nào dùng?
- Khi cần lấy dữ liệu liên quan nằm ở nhiều bảng khác nhau trong một truy vấn
- Khi cần báo cáo, thống kê tổng hợp từ nhiều nguồn dữ liệu

## Aggregate

Là gì?
- Là các hàm tổng hợp (aggregate functions) tính toán trên một nhóm dòng và trả về một giá trị duy nhất
- Các hàm phổ biến: `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`
- Thường đi kèm `GROUP BY` (nhóm dữ liệu) và `HAVING` (lọc trên nhóm)

Vì sao cần?
- Cần thống kê, tóm tắt dữ liệu thay vì xem từng dòng riêng lẻ
- Ví dụ: tổng doanh thu, số đơn hàng trung bình, sản phẩm bán chạy nhất

Hoạt động như thế nào?
- `GROUP BY` chia dữ liệu thành các nhóm theo cột chỉ định -> hàm aggregate tính toán trên từng nhóm
- Ví dụ: nhóm đơn hàng theo từng user -> đếm số đơn và tổng tiền -> chỉ giữ user có tổng tiền lớn hơn một mức nào đó

Khi nào dùng?
- Khi cần làm báo cáo, dashboard, thống kê
- Khi cần các con số tổng hợp: đếm, tính tổng, trung bình, lớn nhất, nhỏ nhất

## Database Design

Là gì?
- Là quá trình thiết kế cấu trúc cơ sở dữ liệu: xác định các bảng, cột, kiểu dữ liệu, và mối quan hệ giữa các bảng
- Bao gồm chuẩn hóa (normalization) để giảm trùng lặp dữ liệu và mô hình hóa quan hệ (1-1, 1-n, n-n)

Vì sao cần?
- Thiết kế tốt giúp dữ liệu nhất quán, dễ mở rộng, dễ bảo trì, truy vấn hiệu quả
- Thiết kế tệ dẫn đến dữ liệu trùng lặp, khó cập nhật, dễ sai sót và chậm

Hoạt động như thế nào?
- B1: Xác định các thực thể (entity) — ví dụ: User, Product, Order
- B2: Xác định thuộc tính (attribute) của mỗi thực thể — cột và kiểu dữ liệu
- B3: Xác định mối quan hệ giữa các thực thể:
  - 1-1: một user có một profile
  - 1-n: một user có nhiều order
  - n-n: một order có nhiều product và một product thuộc nhiều order (cần bảng trung gian)
- B4: Chuẩn hóa (1NF, 2NF, 3NF) để loại bỏ dữ liệu dư thừa
- B5: Vẽ sơ đồ ERD (Entity Relationship Diagram) để mô tả tổng thể

Khi nào dùng?
- Trước khi bắt đầu xây dựng một dự án mới
- Khi cần thêm tính năng lớn làm thay đổi cấu trúc dữ liệu

## Migrations

Là gì?
- Là cách quản lý thay đổi cấu trúc database (schema) bằng các file có phiên bản, được lưu cùng source code
- Mỗi migration mô tả một thay đổi: tạo bảng, thêm cột, đổi kiểu dữ liệu, tạo index...

Vì sao cần?
- Đồng bộ cấu trúc database giữa các môi trường (local, staging, production) và giữa các thành viên trong team
- Có lịch sử thay đổi rõ ràng, có thể rollback (quay lại) khi cần
- Tránh việc sửa database thủ công gây sai lệch giữa các môi trường

Hoạt động như thế nào?
- Mỗi migration thường có 2 phần: `up` (áp dụng thay đổi) và `down` (hoàn tác)
- Framework lưu lại các migration đã chạy trong một bảng đặc biệt (ví dụ `migrations`)
- Khi chạy lệnh migrate, hệ thống chỉ thực thi các migration chưa được áp dụng

Khi nào dùng?
- Mỗi khi cần thay đổi cấu trúc database trong dự án có nhiều môi trường / nhiều người làm
- Khi muốn version control cho schema database

## Constraint

Là gì?
- Là các ràng buộc đặt lên cột hoặc bảng để đảm bảo tính toàn vẹn và đúng đắn của dữ liệu
- Các loại phổ biến:
  - `PRIMARY KEY`: khóa chính, định danh duy nhất mỗi dòng
  - `FOREIGN KEY`: khóa ngoại, liên kết tới khóa chính bảng khác
  - `UNIQUE`: giá trị không được trùng
  - `NOT NULL`: bắt buộc có giá trị
  - `CHECK`: giá trị phải thỏa điều kiện
  - `DEFAULT`: giá trị mặc định khi không nhập

Vì sao cần?
- Đảm bảo dữ liệu luôn hợp lệ ngay tại tầng database, không phụ thuộc hoàn toàn vào code ứng dụng
- Ngăn dữ liệu rác, dữ liệu mâu thuẫn (ví dụ: đơn hàng trỏ tới user không tồn tại)

Hoạt động như thế nào?
- Khi `INSERT` hoặc `UPDATE`, database kiểm tra các ràng buộc trước khi lưu
- Nếu vi phạm, thao tác bị từ chối và báo lỗi
- Ví dụ: nếu thêm order với `user_id` không có trong bảng `users` -> database báo lỗi (vi phạm khóa ngoại)

Khi nào dùng?
- Khi cần đảm bảo tính toàn vẹn dữ liệu (data integrity)
- Khi muốn ràng buộc quy tắc nghiệp vụ ngay ở tầng dữ liệu

## Index

Là gì?
- Là cấu trúc dữ liệu phụ (thường là B-Tree) giúp database tìm kiếm dữ liệu nhanh hơn mà không phải quét toàn bộ bảng
- Giống như mục lục của một cuốn sách: tra mục lục nhanh hơn lật từng trang

Vì sao cần?
- Khi bảng có hàng triệu dòng, tìm kiếm không có index phải quét toàn bộ (full table scan) -> rất chậm
- Index giúp truy vấn `WHERE`, `JOIN`, `ORDER BY` nhanh hơn nhiều lần

Hoạt động như thế nào?
- Database tạo và duy trì một cấu trúc sắp xếp sẵn theo cột được index
- Khi truy vấn theo cột đó, database tra trên index (đã sắp xếp) thay vì quét cả bảng
- Lưu ý đánh đổi (trade-off):
  - Index làm `SELECT` nhanh hơn nhưng `INSERT`/`UPDATE`/`DELETE` chậm hơn (vì phải cập nhật cả index)
  - Index tốn thêm bộ nhớ lưu trữ

Khi nào dùng?
- Trên các cột thường xuyên dùng để tìm kiếm, lọc, join, sắp xếp (ví dụ: email, user_id, created_at)
- Không nên lạm dụng index trên bảng ghi/cập nhật rất nhiều hoặc cột ít khi truy vấn

## N+1 Problem

Là gì?
- Là vấn đề hiệu năng khi truy vấn dữ liệu: thay vì dùng 1 truy vấn để lấy dữ liệu liên quan, code lại thực hiện thêm 1 truy vấn cho mỗi dòng kết quả
- Tên gọi N+1: 1 truy vấn lấy danh sách (N dòng) + N truy vấn cho từng dòng = N+1 truy vấn

Vì sao cần quan tâm?
- Gây ra số lượng truy vấn database khổng lồ -> chậm nghiêm trọng khi dữ liệu lớn
- Là lỗi hiệu năng phổ biến khi dùng ORM (Eloquent, Hibernate, Sequelize...)

Hoạt động như thế nào (vấn đề xảy ra)?
- Ví dụ: lấy 100 user (1 truy vấn), rồi với mỗi user lại truy vấn lấy danh sách order của họ (thêm 100 truy vấn) -> tổng cộng 101 truy vấn
- Cách khắc phục — Eager Loading: tải sẵn dữ liệu liên quan trong ít truy vấn (gom 100 truy vấn con thành 1 truy vấn), giúp giảm xuống chỉ còn 2 truy vấn
- Bản chất: gom các truy vấn con thành một truy vấn dùng `IN (...)` hoặc `JOIN`

Khi nào cần để ý?
- Khi vòng lặp qua một danh sách và truy cập dữ liệu quan hệ bên trong vòng lặp
- Khi thấy ứng dụng chậm bất thường và log database hiện rất nhiều truy vấn lặp lại giống nhau
