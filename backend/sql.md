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
- Là quá trình thiết kế cấu trúc cơ sở dữ liệu: xác định các bảng (entity), cột (attribute), kiểu dữ liệu, khóa (key) và mối quan hệ (relationship) giữa các bảng sao cho dữ liệu được tổ chức hợp lý, nhất quán và dễ mở rộng.

Dùng để làm gì?
- Tổ chức thông tin: Giúp tạo ra một "kho" lưu trữ thông tin kỹ thuật số có hệ thống, mạch lạc và chính xác.
- Giảm thiểu dữ liệu dư thừa: Thiết kế chuẩn giúp loại bỏ việc lưu trữ lặp đi lặp lại một thông tin, từ đó tiết kiệm tối đa dung lượng lưu trữ.
- Tối ưu tốc độ truy xuất: Cấu trúc dữ liệu hợp lý giúp các phần mềm, ứng dụng truy cập, tìm kiếm và cập nhật thông tin cực kỳ nhanh chóng.
- Đảm bảo tính toàn vẹn: Hạn chế tối đa các lỗi logic, tránh tình trạng mâu thuẫn hoặc mất mát dữ liệu trong quá trình sử dụng.
- Bảo mật và mở rộng: Dễ dàng kiểm soát quyền truy cập và nâng cấp hệ thống khi quy mô kinh doanh hoặc ứng dụng phát triển lớn hơn.

Khi nào dùng?
- Ngay từ đầu mỗi dự án, trước khi viết code, vì thiết kế sai từ gốc rất tốn kém để sửa về sau.
- Khi thêm nghiệp vụ hoặc tính năng mới có dữ liệu mới cần lưu.

Các mối quan hệ giữa các bảng:
- 1 - 1: một dòng bảng A ứng với đúng một dòng bảng B, ví dụ users với user_profiles. Làm bằng khóa ngoại kèm ràng buộc UNIQUE.
- 1 - N: một dòng bảng A ứng với nhiều dòng bảng B, hay gặp nhất, ví dụ một user có nhiều order. Khóa ngoại đặt ở bảng nhiều là orders.user_id.
- N - N: nhiều với nhiều, ví dụ students với courses. Cần một bảng trung gian như student_courses(student_id, course_id).

```sql
-- 1 - N: một user có nhiều order
CREATE TABLE orders (
  id INT PRIMARY KEY,
  user_id INT,
  total DECIMAL(10,2),
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- N - N: bảng trung gian
CREATE TABLE student_courses (
  student_id INT,
  course_id INT,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

Chuẩn hóa (Normalization):

Là tổ chức lại bảng để giảm dư thừa và tránh sai sót khi thêm, sửa, xóa dữ liệu.
- 1NF: mỗi ô chỉ chứa một giá trị, không lưu kiểu danh sách trong một cột như "toán,lý,hóa", mỗi dòng là duy nhất.
- 2NF: đạt 1NF và mọi cột thường phụ thuộc vào toàn bộ khóa chính, dùng khi khóa chính gồm nhiều cột.
- 3NF: đạt 2NF và không có phụ thuộc bắc cầu, tức cột thường không phụ thuộc vào cột thường khác.

Ví dụ chưa chuẩn rồi chuẩn hóa:
```
-- Chưa chuẩn: lặp thông tin sản phẩm trong mỗi dòng đơn hàng
order_items(id, order_id, product_name, product_price, qty)

-- Chuẩn hóa: tách bảng products, order_items chỉ tham chiếu product_id
products(id, name, price)
order_items(id, order_id, product_id, qty)
```

Denormalization (phi chuẩn hóa):
- Là cố tình lưu dư thừa như gộp bảng hoặc thêm cột tính sẵn để đọc nhanh hơn, đổi lại dữ liệu bị lặp và phức tạp hơn khi ghi.
- Dùng khi hệ thống đọc nhiều hơn ghi rất nhiều và join quá tốn kém, ví dụ lưu sẵn comment_count thay vì COUNT(*) mỗi lần. Xem thêm mục Pre-aggregation trong [sql-advanced.md](sql-advanced.md).

Khác gì với cái tương đương?
- Chuẩn hóa với phi chuẩn hóa: chuẩn hóa ưu tiên toàn vẹn và tiết kiệm chỗ, ghi an toàn nhưng đọc phải join nhiều. Phi chuẩn hóa ưu tiên tốc độ đọc nhưng dữ liệu lặp và phải đồng bộ khi ghi. Thực tế nên thiết kế chuẩn trước, chỉ phi chuẩn hóa có chủ đích ở chỗ hay bị chậm.
- SQL với NoSQL: SQL thiết kế theo bảng quan hệ, chuẩn hóa, mạnh về toàn vẹn và truy vấn phức tạp. NoSQL như MongoDB thiết kế theo document lồng nhau, thường lưu dư thừa sẵn để đọc nhanh, hợp dữ liệu linh hoạt. Chọn SQL khi dữ liệu có quan hệ rõ ràng và cần giao dịch chặt.
- Khóa nghiệp vụ với khóa nhân tạo: khóa nghiệp vụ dùng dữ liệu có sẵn như email làm khóa, có ý nghĩa nhưng dễ đổi. Khóa nhân tạo là id tự tăng hoặc UUID, ổn định hơn nên thường được dùng làm khóa chính.

Câu hỏi phỏng vấn:

Q: Chuẩn hóa là gì, có mấy dạng?
Trả lời: Là tổ chức lại bảng để giảm dư thừa và tránh sai sót khi thêm, sửa, xóa. Ba mức hay dùng là 1NF (mỗi ô một giá trị), 2NF (không phụ thuộc một phần vào khóa chính ghép), 3NF (không phụ thuộc bắc cầu). Đa số hệ thống làm tới 3NF là đủ.

Q: Khi nào nên phi chuẩn hóa?
Trả lời: Khi hệ đọc nhiều hơn ghi rất nhiều và việc join hoặc tổng hợp trở nên quá tốn kém làm chậm query hay dùng. Khi đó cố tình lưu dư thừa để đọc nhanh, chấp nhận phải đồng bộ khi ghi. Nên làm có chủ đích chứ không phi chuẩn hóa bừa từ đầu.

Q: Quan hệ N-N làm thế nào?
Trả lời: Không lưu trực tiếp được, phải tạo một bảng trung gian chứa khóa ngoại tới cả hai bảng, ví dụ student_courses(student_id, course_id). Có thể thêm cột phụ như enrolled_at vào chính bảng trung gian.

Q: Nên dùng khóa chính là id tự tăng hay dữ liệu nghiệp vụ như email?
Trả lời: Nên dùng id tự tăng hoặc UUID làm khóa chính vì nó ổn định và không mang thông tin nhạy cảm. Email nên đặt ràng buộc UNIQUE thay vì làm khóa chính vì nó có thể thay đổi.

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

### Khi nào nên và không nên đánh index?

Nên đánh:
- Cột hay dùng trong WHERE, điều kiện JOIN (khóa ngoại), ORDER BY, GROUP BY.
- Cột có nhiều giá trị khác nhau như email, user_id.
- Cột cần đảm bảo không trùng, dùng UNIQUE INDEX.

Không nên hoặc cân nhắc:
- Bảng nhỏ vài trăm dòng, quét cả bảng còn nhanh hơn tra index.
- Cột ít giá trị như giới tính, trạng thái đúng/sai.
- Cột hiếm khi dùng để lọc.
- Bảng ghi rất nhiều như log, vì mỗi index làm chậm INSERT.

Khác gì với cái tương đương?
- Clustered với non-clustered index: clustered quyết định thứ tự lưu dữ liệu trên disk, mỗi bảng chỉ có một, thường là khóa chính, và nút lá chứa luôn dữ liệu dòng. Non-clustered là cấu trúc riêng tách khỏi dữ liệu, nút lá chứa con trỏ tới dòng thật, một bảng có nhiều loại này.
- B-Tree với Hash: B-Tree hỗ trợ so sánh bằng, so sánh khoảng và sắp xếp nên đa dụng. Hash chỉ hỗ trợ so sánh bằng, tra rất nhanh nhưng không làm được khoảng hay sắp xếp.
- Index với quét cả bảng: index nhanh khi lấy ít dòng. Nhưng nếu query lấy phần lớn bảng thì quét cả bảng lại nhanh hơn, và database sẽ tự chọn quét bảng.

Câu hỏi phỏng vấn:

Q: Vì sao index làm query nhanh hơn?
Trả lời: Không có index thì database phải quét toàn bộ bảng. Index dạng B-Tree lưu dữ liệu đã sắp xếp trong cây cân bằng nên tìm theo kiểu chia để trị, giống tra mục lục sách thay vì lật từng trang. Index cũng nhỏ hơn bảng nên đọc nhanh hơn.

Q: Đánh nhiều index có hại gì không?
Trả lời: Có. Mỗi lần INSERT, UPDATE, DELETE phải cập nhật lại tất cả index liên quan nên ghi chậm hơn, tốn thêm dung lượng, và nhiều index giống nhau có thể làm database chọn nhầm. Chỉ nên đánh index trên cột thực sự hay dùng để lọc, join, sắp xếp.

Q: Có index rồi nhưng query không dùng, vì sao?
Trả lời: Thường do WHERE bọc hàm quanh cột, kiểu dữ liệu không khớp, sai thứ tự cột trong composite index, cột ít giá trị nên database chọn quét bảng, hoặc thống kê của bảng đã cũ cần chạy ANALYZE lại.

Q: Có nên đánh index trên cột giới tính không?
Trả lời: Thường không nên vì cột chỉ có vài giá trị, lọc theo giới tính vẫn khớp khoảng một nửa bảng nên database thấy quét bảng còn nhanh hơn và bỏ qua index. Chỉ hữu ích khi dữ liệu phân bố rất lệch.

Xem thêm về Composite Index chi tiết trong [sql-advanced.md](sql-advanced.md).

## N+1 Problem

Là gì?
- Là vấn đề hiệu năng khi truy vấn dữ liệu: thay vì dùng 1 truy vấn để lấy dữ liệu liên quan, code lại thực hiện thêm 1 truy vấn cho mỗi dòng kết quả
- Tên gọi N+1: 1 truy vấn lấy danh sách (N dòng) + N truy vấn cho từng dòng = N+1 truy vấn
- Ví dụ: lấy 100 user (1 truy vấn), rồi với mỗi user lại truy vấn lấy order của họ (thêm 100 truy vấn) -> tổng cộng 101 truy vấn
- Thường gặp khi dùng ORM (Eloquent, Hibernate, Sequelize...) mà truy cập quan hệ (`$user->orders`) bên trong vòng lặp — ORM tự động bắn thêm một query mỗi lần.

Dùng để làm gì? (nhận biết và xử lý)
- Là lỗi cần phát hiện và tránh, vì gây ra số lượng truy vấn khổng lồ -> chậm nghiêm trọng khi dữ liệu lớn
- Cần để ý khi vòng lặp qua một danh sách và truy cập dữ liệu quan hệ bên trong vòng lặp

Khi nào để ý?
- Khi màn hình list "gọi" ra nhiều dữ liệu liên quan (danh sách bài viết + tác giả, đơn hàng + sản phẩm).
- Khi thấy log database có hàng trăm query gần giống nhau chỉ khác tham số `id`.
- Khi trang list chậm dần theo số dòng hiển thị.

Ví dụ minh họa kiểu ORM:
```php
// N+1: 1 query lấy users và N query lấy orders
$users = User::all();                 // 1 query
foreach ($users as $user) {
    echo $user->orders->count();      // mỗi vòng thêm 1 query
}

// Eager loading: gom còn 2 query
$users = User::with('orders')->get();
// Query 1: SELECT * FROM users
// Query 2: SELECT * FROM orders WHERE user_id IN (1,2,3,...)
```

Cách khắc phục:
- Eager loading: tải sẵn dữ liệu liên quan, gom nhiều query con thành một query dùng IN, đưa 101 query xuống còn 2. Ví dụ with() trong Laravel, JOIN FETCH trong JPA.
- Viết JOIN thủ công: một câu SQL join lấy đủ dữ liệu trong một lượt.
- Tự gom bằng IN: lấy hết id rồi query một lần WHERE id IN (...).

Khác gì với cái tương đương?
- Eager loading với lazy loading: lazy loading chỉ tải quan hệ khi được dùng lần đầu, tiện và tiết kiệm nếu không dùng tới, nhưng chính là nguyên nhân gây N+1 trong vòng lặp. Eager loading tải sẵn từ đầu nên tránh được N+1 nhưng có thể lấy thừa. Nên dùng eager khi biết chắc sẽ dùng dữ liệu quan hệ.
- Gom bằng IN (2 query) với JOIN (1 query): cách IN giữ dữ liệu tách bảng nên không bị lặp dòng cha, hợp khi cần cấu trúc lồng nhau. JOIN gộp trong một query nhưng dữ liệu cha bị lặp theo số con, hợp khi cần dữ liệu dạng bảng phẳng.

Câu hỏi phỏng vấn:

Q: N+1 query problem là gì?
Trả lời: Là lỗi hiệu năng khi lấy một danh sách N dòng bằng 1 query, rồi lặp qua từng dòng lại bắn thêm 1 query để lấy dữ liệu quan hệ, tổng cộng N+1 query. Ví dụ lấy 100 user rồi mỗi user query lấy đơn hàng thành 101 query. Rất chậm khi N lớn, hay gặp khi dùng ORM với lazy loading trong vòng lặp.

Q: Khắc phục N+1 thế nào?
Trả lời: Dùng eager loading để gom lại, ORM sẽ lấy tất cả bản ghi liên quan bằng một query WHERE id IN (...), đưa 101 query xuống còn 2. Trong Laravel là with(), JPA là JOIN FETCH. Hoặc tự viết JOIN lấy đủ dữ liệu trong một câu. Có thể phát hiện N+1 bằng cách bật query log.

Q: Gom bằng IN và JOIN khác gì, khi nào dùng cái nào?
Trả lời: Cách IN chạy 2 query, giữ dữ liệu tách bảng nên không lặp dòng, hợp khi cần cấu trúc lồng nhau như user chứa danh sách order. JOIN chỉ 1 query nhưng dữ liệu cha lặp theo số con nên phải gộp lại, hợp khi cần dữ liệu dạng bảng để hiển thị.
