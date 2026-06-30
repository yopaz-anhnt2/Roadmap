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

Dùng để làm gì?
- Tổ chức thông tin: Giúp tạo ra một "kho" lưu trữ thông tin kỹ thuật số có hệ thống, mạch lạc và chính xác.
- Giảm thiểu dữ liệu dư thừa: Thiết kế chuẩn giúp loại bỏ việc lưu trữ lặp đi lặp lại một thông tin, từ đó tiết kiệm tối đa dung lượng lưu trữ.
- Tối ưu tốc độ truy xuất: Cấu trúc dữ liệu hợp lý giúp các phần mềm, ứng dụng truy cập, tìm kiếm và cập nhật thông tin cực kỳ nhanh chóng.
- Đảm bảo tính toàn vẹn: Hạn chế tối đa các lỗi logic, tránh tình trạng mâu thuẫn hoặc mất mát dữ liệu trong quá trình sử dụng.
- Bảo mật và mở rộng: Dễ dàng kiểm soát quyền truy cập và nâng cấp hệ thống khi quy mô kinh doanh hoặc ứng dụng phát triển lớn hơn.

## Migrations

Là gì?
- Là cách quản lý thay đổi cấu trúc CSDL bằng các file có phiên bản, lưu cùng source code
- Mỗi migration mô tả một thay đổi: tạo bảng, thêm cột, đổi kiểu dữ liệu, tạo index... và thường có 2 phần `up` (áp dụng) / `down` (hoàn tác)

Dùng để làm gì?
- Quản lý thay đổi database bằng code
- Đồng bộ database giữa các môi trường (dev, test, production)
- Theo dõi lịch sử thay đổi
- Dễ rollback khi có lỗi
- Hỗ trợ làm việc nhóm

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
- Đảm bảo dữ liệu chính xác
- Tránh dữ liệu trùng lặp
- Ngăn dữ liệu không hợp lệ
- Duy trì mối quan hệ giữa các bảng
- Tăng tính toàn vẹn dữ liệu

## Index

Là gì?
- Là cấu trúc dữ liệu đặc biệt trong database dùng để tăng tốc độ tìm kiếm và truy vấn dữ liệu.
- Giống như mục lục của một cuốn sách: tra mục lục nhanh hơn lật từng trang

Dùng để làm gì?
- Tăng tốc truy vấn
- Giảm thời gian tìm kiếm dữ liệu
- Tăng hiệu năng hệ thống
- Tăng tốc WHERE
- Tăng tốc JOIN
- Tăng tốc ORDER BY
- Tăng tốc GROUP BY

Nhược điểm:

- Tốn thêm bộ nhớ
- Làm chậm INSERT, UPDATE, DELETE
- Quá nhiều Index có thể phản tác dụng

Vì khi dữ liệu thay đổi:

Dữ liệu đổi
↓
Index cũng phải cập nhật

### Các loại Index trong MySQL / PostgreSQL

#### MySQL (InnoDB)

**1. B-Tree Index (mặc định)**
- Là gì: cấu trúc cây cân bằng (balanced tree), loại index phổ biến và mặc định nhất. Dữ liệu trong cây được sắp xếp sẵn.
- Tác dụng: tăng tốc tìm theo `=`, khoảng (`>`, `<`, `>=`, `<=`, `BETWEEN`), sắp xếp (`ORDER BY`), và tìm theo phần đầu chuỗi (prefix, ví dụ `LIKE 'abc%'`).
- Ví dụ:
```sql
CREATE INDEX idx_users_email ON users(email);
-- Tăng tốc: SELECT * FROM users WHERE email = 'a@gmail.com';
```

**2. Primary Key / Clustered Index**
- Là gì: trong InnoDB, dữ liệu của bảng được lưu vật lý (sắp xếp trên disk) theo thứ tự của khóa chính. Mỗi bảng chỉ có duy nhất 1 clustered index.
- Tác dụng: tra theo khóa chính cực nhanh vì dữ liệu nằm ngay tại nút lá của cây; đồng thời đảm bảo mỗi dòng là duy nhất.
- Ví dụ:
```sql
CREATE TABLE users (
  id INT PRIMARY KEY,   -- vừa là PK, vừa là clustered index
  name VARCHAR(100)
);
```

**3. Unique Index**
- Là gì: giống B-Tree nhưng có thêm ràng buộc giá trị không được trùng lặp.
- Tác dụng: vừa tăng tốc truy vấn, vừa đảm bảo toàn vẹn dữ liệu (không cho 2 dòng cùng giá trị).
- Ví dụ:
```sql
CREATE UNIQUE INDEX idx_users_username ON users(username);
-- Chèn username đã tồn tại sẽ bị báo lỗi
```

**4. Composite Index (index nhiều cột)**
- Là gì: index đánh trên nhiều cột cùng lúc, có thứ tự từ trái sang phải.
- Tác dụng: tối ưu truy vấn lọc/sắp xếp theo tổ hợp nhiều cột. Tuân theo quy tắc "leftmost prefix" — chỉ tận dụng được index nếu lọc bắt đầu từ cột bên trái.
- Ví dụ:
```sql
CREATE INDEX idx_orders_user_status ON orders(user_id, status);
-- Dùng được: WHERE user_id = 1 AND status = 'paid'
-- Dùng được: WHERE user_id = 1
-- KHÔNG tận dụng tốt: WHERE status = 'paid'  (thiếu cột trái user_id)
```

**5. Fulltext Index**
- Là gì: index chuyên dùng để tìm kiếm từ khóa trong đoạn văn bản dài.
- Tác dụng: tìm theo từ/cụm từ trong text nhanh hơn nhiều so với `LIKE '%...%'`.
- Ví dụ:
```sql
CREATE FULLTEXT INDEX idx_posts_content ON posts(content);
SELECT * FROM posts
WHERE MATCH(content) AGAINST('database index');
```

**6. Spatial Index**
- Là gì: index cho dữ liệu không gian, tọa độ (GIS — bản đồ, vị trí).
- Tác dụng: tăng tốc truy vấn hình học/khoảng cách (tìm điểm trong vùng, gần một tọa độ).
- Ví dụ:
```sql
CREATE SPATIAL INDEX idx_places_location ON places(location);
-- location có kiểu GEOMETRY / POINT
```

#### PostgreSQL (đa dạng hơn)

**1. B-Tree (mặc định)**
- Là gì: cây cân bằng, index mặc định giống MySQL.
- Tác dụng: so sánh `=`, `<`, `>`, `BETWEEN`, `ORDER BY`.
- Ví dụ:
```sql
CREATE INDEX idx_users_age ON users(age);
-- SELECT * FROM users WHERE age BETWEEN 20 AND 30;
```

**2. Hash Index**
- Là gì: index dựa trên bảng băm (hash table).
- Tác dụng: chỉ phục vụ so sánh bằng `=`, tra cực nhanh nhưng KHÔNG hỗ trợ khoảng (`>`, `<`) hay sắp xếp.
- Ví dụ:
```sql
CREATE INDEX idx_sessions_token ON sessions USING HASH (token);
-- SELECT * FROM sessions WHERE token = 'abc123';
```

**3. GIN (Generalized Inverted Index)**
- Là gì: "inverted index" — phù hợp cho cột chứa nhiều giá trị bên trong (JSONB, mảng, fulltext).
- Tác dụng: tìm phần tử trong mảng, key/value trong JSONB, hoặc từ khóa trong văn bản.
- Ví dụ:
```sql
CREATE INDEX idx_products_tags ON products USING GIN (tags);
-- SELECT * FROM products WHERE tags @> '{"sale"}';

CREATE INDEX idx_docs_data ON docs USING GIN (data jsonb_path_ops);
-- SELECT * FROM docs WHERE data @> '{"type":"invoice"}';
```

**4. GiST (Generalized Search Tree)**
- Là gì: khung index linh hoạt cho dữ liệu không gian và tìm gần đúng.
- Tác dụng: dữ liệu hình học/tọa độ, tìm "gần nhất" (nearest neighbor), kiểu range (khoảng), full-text.
- Ví dụ:
```sql
CREATE INDEX idx_shapes_geom ON shapes USING GiST (geom);
-- Tìm các hình giao nhau với một vùng
```

**5. BRIN (Block Range Index)**
- Là gì: index lưu giá trị min/max theo từng khối (block) dữ liệu, rất gọn nhẹ.
- Tác dụng: cho bảng CỰC LỚN mà dữ liệu đã sắp xếp tự nhiên theo cột (ví dụ thời gian, ID tăng dần). Tốn rất ít bộ nhớ so với B-Tree.
- Ví dụ:
```sql
CREATE INDEX idx_logs_created ON logs USING BRIN (created_at);
-- SELECT * FROM logs WHERE created_at >= '2026-01-01';
```

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
- Là lỗi cần phát hiện và tránh, vì gây ra số lượng truy vấn khổng lồ -> chậm nghiêm trọng khi dữ liệu lớn
- Cách khắc phục — Eager Loading: tải sẵn dữ liệu liên quan, gom nhiều truy vấn con thành một truy vấn dùng `IN (...)` hoặc `JOIN` (101 truy vấn giảm còn 2)
- Cần để ý khi vòng lặp qua một danh sách và truy cập dữ liệu quan hệ bên trong vòng lặp
