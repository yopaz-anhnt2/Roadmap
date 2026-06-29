# SQL

## SQL Basic

Là gì?
- SQL (Structured Query Language) là ngôn ngữ truy vấn có cấu trúc dùng để thao tác và quản lý dữ liệu trong cơ sở dữ liệu quan hệ (relational database)
- Gồm các nhóm lệnh chính:
  - DDL (Data Definition Language): định nghĩa cấu trúc — `CREATE`, `ALTER`, `DROP`
  - DML (Data Manipulation Language): thao tác dữ liệu — `INSERT`, `UPDATE`, `DELETE`
  - DQL (Data Query Language): truy vấn dữ liệu — `SELECT`
  - DCL (Data Control Language): phân quyền — `GRANT`, `REVOKE`
  - TCL (Transaction Control Language): quản lý giao dịch — `COMMIT`, `ROLLBACK`

Dùng để làm gì?
- Thêm dữ liệu vào cơ sở dữ liệu
- Truy xuất (tìm kiếm) dữ liệu
- Cập nhật dữ liệu
- Xóa dữ liệu
- Tạo và quản lý bảng dữ liệu
- Phân tích và xử lý dữ liệu

## Join

Là gì?
- Là phép kết hợp dữ liệu từ hai hay nhiều bảng dựa trên một cột liên quan giữa chúng
- Các loại Join phổ biến:
  - `INNER JOIN`: chỉ lấy các bản ghi khớp ở cả hai bảng
  - `LEFT JOIN`: lấy tất cả bản ghi bảng trái + bản ghi khớp ở bảng phải (không khớp thì NULL)
  - `RIGHT JOIN`: ngược lại với LEFT JOIN
  - `FULL OUTER JOIN`: lấy tất cả bản ghi của cả hai bảng

Dùng để làm gì?
- Ghép lại dữ liệu liên quan nằm rải rác ở nhiều bảng để hiển thị thông tin đầy đủ
- Ví dụ: bảng `orders` chỉ lưu `user_id`, Join với bảng `users` để biết tên người đặt hàng

## Aggregate

Là gì?
- Là các hàm tổng hợp tính toán trên nhiều dòng dữ liệu và trả về một kết quả tổng hợp duy nhất.
- Các hàm phổ biến: `COUNT()`, `SUM()`, `AVG()`, `MAX()`, `MIN()`
- Thường đi kèm `GROUP BY` (nhóm dữ liệu) và `HAVING` (lọc trên nhóm)

Dùng để làm gì?
- Đếm số lượng dữ liệu
- Tính tổng
- Tính trung bình
- Tìm giá trị lớn nhất
- Tìm giá trị nhỏ nhất
- Thống kê và phân tích dữ liệu

## Database Design

Là gì?
- Là quá trình thiết kế cấu trúc cơ sở dữ liệu: xác định các bảng, cột, kiểu dữ liệu, và mối quan hệ giữa các bảng
- Bao gồm chuẩn hóa (normalization) để giảm trùng lặp và mô hình hóa quan hệ:
  - 1-1: một user có một profile
  - 1-n: một user có nhiều order
  - n-n: một order có nhiều product và ngược lại (cần bảng trung gian)

Dùng để làm gì?
- Tạo nền tảng dữ liệu nhất quán, dễ mở rộng, dễ bảo trì và truy vấn hiệu quả
- Tránh dữ liệu trùng lặp, khó cập nhật, dễ sai sót
- Dùng khi bắt đầu một dự án mới, hoặc khi thêm tính năng lớn làm thay đổi cấu trúc dữ liệu

## Migrations

Là gì?
- Là cách quản lý thay đổi cấu trúc database (schema) bằng các file có phiên bản, lưu cùng source code
- Mỗi migration mô tả một thay đổi: tạo bảng, thêm cột, đổi kiểu dữ liệu, tạo index... và thường có 2 phần `up` (áp dụng) / `down` (hoàn tác)

Dùng để làm gì?
- Đồng bộ cấu trúc database giữa các môi trường (local, staging, production) và giữa các thành viên trong team
- Lưu lịch sử thay đổi schema, có thể rollback khi cần
- Tránh phải sửa database thủ công gây sai lệch giữa các môi trường

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

Dùng để làm gì?
- Đảm bảo dữ liệu luôn hợp lệ ngay tại tầng database, không phụ thuộc hoàn toàn vào code ứng dụng
- Ngăn dữ liệu rác, dữ liệu mâu thuẫn (ví dụ: đơn hàng trỏ tới user không tồn tại sẽ bị từ chối)
- Ràng buộc quy tắc nghiệp vụ ngay ở tầng dữ liệu

## Index

Là gì?
- Là cấu trúc dữ liệu phụ (thường là B-Tree) giúp database tìm kiếm dữ liệu nhanh hơn mà không phải quét toàn bộ bảng
- Giống như mục lục của một cuốn sách: tra mục lục nhanh hơn lật từng trang

Dùng để làm gì?
- Tăng tốc các truy vấn `WHERE`, `JOIN`, `ORDER BY` trên bảng có nhiều dữ liệu
- Thường đánh trên các cột hay dùng để tìm kiếm, lọc, join, sắp xếp (ví dụ: email, user_id, created_at)

### Các loại Index trong MySQL / PostgreSQL

MySQL (InnoDB)
- B-Tree (mặc định): loại phổ biến nhất, dùng cho hầu hết trường hợp — tìm theo `=`, khoảng (`>`, `<`, `BETWEEN`), sắp xếp, prefix
- Primary Key / Clustered Index: dữ liệu được lưu vật lý theo thứ tự khóa chính; mỗi bảng chỉ có 1
- Unique Index: như B-Tree nhưng đảm bảo giá trị không trùng
- Composite Index (index nhiều cột): đánh trên nhiều cột cùng lúc, theo thứ tự trái sang phải
- Fulltext Index: tìm kiếm văn bản trong đoạn text dài (search theo từ khóa)
- Spatial Index: cho dữ liệu không gian, tọa độ (GIS)

PostgreSQL (đa dạng hơn)
- B-Tree (mặc định): dùng cho so sánh `=`, `<`, `>`, `BETWEEN`, `ORDER BY`
- Hash: chỉ phục vụ so sánh bằng `=`, tra rất nhanh nhưng không hỗ trợ khoảng
- GIN: cho dữ liệu nhiều giá trị trong một cột — JSONB, mảng, fulltext
- GiST: cho dữ liệu không gian, tìm gần đúng, khoảng (range)
- BRIN: cho bảng rất lớn mà dữ liệu sắp xếp tự nhiên theo cột (ví dụ thời gian), tốn ít bộ nhớ

### Khi nào dùng index nào?

- So sánh bằng và khoảng, sắp xếp -> B-Tree (mặc định, dùng đa số trường hợp)
- Chỉ so sánh bằng `=`, cần cực nhanh -> Hash (PostgreSQL)
- Truy vấn nhiều cột cùng lúc trong `WHERE` -> Composite Index (đặt cột lọc nhiều/chọn lọc cao lên trước)
- Tìm kiếm từ khóa trong văn bản dài -> Fulltext (MySQL) / GIN (PostgreSQL)
- Tìm trong cột JSONB hoặc mảng -> GIN (PostgreSQL)
- Dữ liệu tọa độ, bản đồ -> Spatial (MySQL) / GiST (PostgreSQL)
- Bảng cực lớn, dữ liệu tăng theo thời gian (log, lịch sử) -> BRIN (PostgreSQL)

### Tại sao index làm query nhanh hơn?

- Không có index: database phải quét toàn bộ bảng (full table scan) — duyệt từng dòng để so điều kiện, độ phức tạp O(n)
- Có index (B-Tree): dữ liệu đã được sắp xếp sẵn trong cây cân bằng, database tìm theo kiểu "chia để trị" — độ phức tạp O(log n)
- Ví dụ trực quan: tìm 1 từ trong từ điển 1 triệu từ. Quét cả bảng = lật từng trang. Index = tra mục lục đã sắp xếp -> chỉ vài bước là tới
- Ngoài ra index thường nhỏ hơn cả bảng nên đọc từ disk/bộ nhớ cũng nhanh hơn

### Đánh index nhiều có ảnh hưởng gì không?

Có, index không miễn phí — đánh quá nhiều sẽ gây hại:
- Ghi chậm hơn: mỗi lần `INSERT`/`UPDATE`/`DELETE`, database phải cập nhật lại tất cả index liên quan
- Tốn dung lượng lưu trữ: mỗi index là một cấu trúc dữ liệu riêng cần lưu trên disk
- Tốn bộ nhớ và chậm khi ghi hàng loạt (bulk insert)
- Có thể làm "rối" trình tối ưu (query planner) nếu có nhiều index gần giống nhau -> chọn nhầm index không tối ưu

Nguyên tắc:
- Chỉ đánh index trên cột thực sự hay dùng để lọc/join/sắp xếp
- Ưu tiên composite index hợp lý thay vì tạo nhiều index đơn lẻ trùng mục đích
- Cột có độ chọn lọc thấp (ví dụ giới tính chỉ có 2 giá trị) thì đánh index thường ít hiệu quả
- Bảng ghi nhiều hơn đọc thì cân nhắc kỹ trước khi thêm index

## N+1 Problem

Là gì?
- Là vấn đề hiệu năng khi truy vấn dữ liệu: thay vì dùng 1 truy vấn để lấy dữ liệu liên quan, code lại thực hiện thêm 1 truy vấn cho mỗi dòng kết quả
- Tên gọi N+1: 1 truy vấn lấy danh sách (N dòng) + N truy vấn cho từng dòng = N+1 truy vấn
- Ví dụ: lấy 100 user (1 truy vấn), rồi với mỗi user lại truy vấn lấy order của họ (thêm 100 truy vấn) -> tổng cộng 101 truy vấn

Dùng để làm gì? (nhận biết và xử lý)
- Là lỗi cần phát hiện và tránh, vì gây ra số lượng truy vấn khổng lồ -> chậm nghiêm trọng khi dữ liệu lớn (phổ biến khi dùng ORM: Eloquent, Hibernate, Sequelize...)
- Cách khắc phục — Eager Loading: tải sẵn dữ liệu liên quan, gom nhiều truy vấn con thành một truy vấn dùng `IN (...)` hoặc `JOIN` (101 truy vấn giảm còn 2)
- Cần để ý khi vòng lặp qua một danh sách và truy cập dữ liệu quan hệ bên trong vòng lặp
