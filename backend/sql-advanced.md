# SQL nâng cao

## Query Optimization

Là gì?
- Là việc làm cho câu truy vấn SQL chạy nhanh hơn và tốn ít tài nguyên hơn (CPU, RAM, disk, thời gian) nhưng vẫn lấy đúng dữ liệu cần.
- Gồm nhiều cách: viết lại query, đánh index hợp lý, sửa cấu trúc bảng, dùng cache.

Dùng để làm gì?
- Giảm thời gian phản hồi của query.
- Giảm tải cho database, phục vụ được nhiều người dùng hơn.
- Tránh quét toàn bộ bảng (full table scan) không cần thiết.

Khi nào dùng?
- Khi query chạy chậm, thấy trong slow query log hoặc người dùng phàn nàn.
- Khi bảng dữ liệu lớn dần (hàng triệu dòng).
- Khi CPU/RAM database bị đẩy cao.

Một số cách tối ưu hay dùng:

1. Chỉ lấy cột cần thiết, tránh SELECT *
```sql
-- Đọc thừa dữ liệu
SELECT * FROM users WHERE id = 1;
-- Chỉ lấy cột cần
SELECT id, name, email FROM users WHERE id = 1;
```

2. Đánh index cho cột hay dùng trong WHERE, JOIN, ORDER BY
```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
```

3. Tránh bọc hàm quanh cột trong WHERE vì sẽ mất tác dụng index
```sql
-- Không dùng được index
SELECT * FROM orders WHERE YEAR(created_at) = 2026;
-- Dùng được index
SELECT * FROM orders
WHERE created_at >= '2026-01-01' AND created_at < '2027-01-01';
```

4. Tránh LIKE '%abc%' (wildcard ở đầu) vì không dùng được index, nên dùng Full Text Search thay thế.

5. Dùng EXISTS thay cho IN khi subquery trả về nhiều dòng
```sql
SELECT * FROM users u
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id AND o.total > 1000);
```

6. Phân trang bằng keyset thay vì OFFSET lớn (xem mục Pagination).

7. Dùng EXPLAIN để xem database thực sự chạy query thế nào (xem mục Execution Plan).

Khác gì với cái tương đương?
- Với Index: index chỉ là một trong nhiều cách tối ưu. Tối ưu query rộng hơn, còn gồm viết lại query, đổi cấu trúc bảng, cache.
- Với Cache: cache tránh chạm vào database, còn tối ưu query làm cho lần chạm đó nhanh hơn. Cache che vấn đề tạm thời, tối ưu query giải quyết gốc.
- Với nâng cấp phần cứng: nâng cấp server là mua máy mạnh hơn, tối ưu query là làm cùng việc với ít tài nguyên hơn. Nên tối ưu trước rồi mới tính nâng cấp.

Câu hỏi phỏng vấn:

Q: Query chạy chậm thì debug thế nào?
Trả lời: Đầu tiên chạy EXPLAIN để xem query có full table scan không, có dùng index không, ước lượng quét bao nhiêu dòng. Nếu full scan trên cột lọc thì cân nhắc đánh index. Xem lại query có bọc hàm lên cột, có SELECT * thừa, có OFFSET lớn không. Cuối cùng xem slow query log để tìm query nặng nhất.

Q: Đánh index rồi mà query vẫn chậm hoặc không dùng index, vì sao?
Trả lời: Có thể do WHERE bọc hàm quanh cột, kiểu dữ liệu không khớp, sai thứ tự cột trong composite index, cột chọn lọc thấp nên database chọn quét bảng, hoặc thống kê của bảng đã cũ cần chạy ANALYZE lại.

## Execution Plan

Là gì?
- Là kế hoạch mà database vạch ra để chạy một câu query: đọc bảng nào trước, dùng index gì, join kiểu nào, sắp xếp ở đâu.
- Xem bằng EXPLAIN (kế hoạch dự kiến) hoặc EXPLAIN ANALYZE (kế hoạch kèm số liệu thực tế khi chạy).

Dùng để làm gì?
- Hiểu vì sao một query nhanh hay chậm.
- Kiểm tra query có dùng index không hay đang quét cả bảng.
- Xem ước lượng số dòng phải quét.

Khi nào dùng?
- Khi query chạy chậm và cần tìm nguyên nhân.
- Trước khi đưa query phức tạp lên production.
- Khi nghi ngờ index không được dùng.

Ví dụ MySQL:
```sql
EXPLAIN SELECT * FROM orders WHERE user_id = 5;
```
Vài cột cần đọc:
- type: kiểu truy cập, từ tốt tới xấu là const, eq_ref, ref, range, index, và ALL (quét cả bảng, xấu nhất).
- key: index thực sự được dùng, NULL là không dùng index nào.
- rows: số dòng ước lượng phải kiểm tra, càng nhỏ càng tốt.
- Extra: chú ý Using filesort (sắp xếp thủ công, chậm) và Using temporary (tạo bảng tạm).

Ví dụ PostgreSQL:
```sql
EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = 5;
```
- Seq Scan là quét cả bảng, xấu với bảng lớn.
- Index Scan hoặc Index Only Scan là đang dùng index, tốt.

Khác gì với cái tương đương?
- EXPLAIN với EXPLAIN ANALYZE: EXPLAIN chỉ dự đoán, không chạy query nên nhanh và an toàn. EXPLAIN ANALYZE chạy thật và đo thời gian, số dòng thực tế nên chính xác hơn, nhưng với UPDATE/DELETE thì sẽ thay đổi dữ liệu thật.
- Với Slow query log: slow query log cho biết query nào chậm, còn execution plan cho biết vì sao nó chậm. Dùng kết hợp cả hai.

Câu hỏi phỏng vấn:

Q: EXPLAIN cho biết những gì?
Trả lời: Cho biết database chạy query thế nào: có dùng index không, kiểu truy cập là gì (tệ nhất là ALL tức quét cả bảng), ước lượng bao nhiêu dòng phải quét, và các cảnh báo như Using filesort hay Using temporary.

Q: Thấy type = ALL nghĩa là gì?
Trả lời: Nghĩa là quét toàn bộ bảng vì không dùng được index. Với bảng lớn thì rất chậm. Cần xem lại WHERE để đánh index phù hợp hoặc viết lại query.

## Composite Index

Là gì?
- Là index đánh trên nhiều cột cùng lúc, có thứ tự từ trái sang phải, ví dụ (user_id, status, created_at).
- Database sắp xếp index theo cột đầu, rồi trong mỗi giá trị cột đầu lại sắp theo cột thứ hai, giống sắp danh bạ theo họ rồi tới tên.

Dùng để làm gì?
- Tối ưu query lọc hoặc sắp xếp theo nhiều cột cùng lúc.
- Tạo covering index để query lấy đủ dữ liệu ngay từ index, không cần đọc bảng.

Khi nào dùng?
- Khi query hay lọc theo nhiều cột, ví dụ WHERE user_id = ? AND status = ?.
- Khi vừa lọc vừa sắp xếp, ví dụ WHERE user_id = ? ORDER BY created_at.

Quy tắc leftmost prefix (tiền tố trái):
Index (user_id, status, created_at) chỉ dùng được nếu điều kiện bắt đầu từ cột bên trái.
```sql
CREATE INDEX idx_orders ON orders(user_id, status, created_at);

-- Dùng đầy đủ
WHERE user_id = 1 AND status = 'paid' AND created_at > '2026-01-01'
-- Dùng một phần
WHERE user_id = 1 AND status = 'paid'
-- Dùng được cột đầu
WHERE user_id = 1
-- Không dùng được vì thiếu cột trái user_id
WHERE status = 'paid'
```

Cách chọn thứ tự cột:
- Cột lọc bằng (=) đặt trước, cột lọc khoảng (>, <, BETWEEN) đặt sau.
- Cột có nhiều giá trị khác nhau (chọn lọc cao) nên đặt trước cột ít giá trị.
- Nếu có ORDER BY thì cân nhắc đưa cột đó vào cuối để tránh phải sắp xếp lại.

Khác gì với cái tương đương?
- Với nhiều index đơn cột: nếu đánh riêng (user_id) và (status) thì với query WHERE user_id=1 AND status='paid', MySQL thường chỉ dùng được một index. Một composite index (user_id, status) phục vụ cả hai điều kiện nên nhanh hơn.
- Với covering index: covering index là index chứa đủ mọi cột mà query cần nên không phải đọc bảng. Nó thường là một composite index, nhưng không phải composite nào cũng là covering.

Câu hỏi phỏng vấn:

Q: Leftmost prefix là gì?
Trả lời: Là quy tắc composite index chỉ dùng được khi điều kiện WHERE bắt đầu từ cột trái nhất và liên tục. Index (a, b, c) dùng được cho WHERE a, WHERE a AND b, WHERE a AND b AND c, nhưng không dùng được cho WHERE b hoặc WHERE c đứng một mình.

Q: Index (a, b) và (b, a) có khác nhau không?
Trả lời: Khác hẳn nhau. (a, b) phục vụ WHERE a và WHERE a AND b nhưng không phục vụ WHERE b một mình. (b, a) thì ngược lại. Chọn thứ tự dựa trên cách query thực tế hay lọc.

## Transactions

Là gì?
- Là một nhóm nhiều câu lệnh SQL được chạy như một khối: hoặc tất cả cùng thành công (COMMIT), hoặc tất cả cùng hủy và quay lại như cũ (ROLLBACK).
- Đảm bảo bởi 4 tính chất ACID:
  - Atomicity: tất cả hoặc không gì cả, một bước lỗi thì hủy hết.
  - Consistency: dữ liệu luôn thỏa ràng buộc trước và sau giao dịch.
  - Isolation: các giao dịch chạy cùng lúc không làm nhiễu nhau.
  - Durability: đã commit thì dữ liệu được lưu vĩnh viễn, không mất kể cả khi mất điện.

Dùng để làm gì?
- Đảm bảo toàn vẹn khi một nghiệp vụ gồm nhiều bước liên quan.
- Ví dụ chuyển tiền: trừ tài khoản A và cộng tài khoản B phải cùng xảy ra.
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 'A';
UPDATE accounts SET balance = balance + 100 WHERE id = 'B';
COMMIT;
-- Nếu có lỗi giữa chừng thì ROLLBACK để A không bị mất tiền mà B không nhận
```

Khi nào dùng?
- Khi nghiệp vụ gồm nhiều thao tác ghi phải cùng thành công hoặc cùng thất bại: chuyển tiền, đặt hàng (trừ kho, tạo đơn, ghi thanh toán), đăng ký (tạo user, tạo profile).

Khác gì với cái tương đương?
- Với chạy nhiều câu SQL rời rạc: chạy rời rạc không có bảo đảm gì, câu 1 xong mà câu 2 lỗi thì dữ liệu hỏng. Transaction gói lại nên hoặc xong hết hoặc không gì cả.
- Với distributed transaction trong microservices: transaction thường nằm trong một database. Khi nghiệp vụ trải nhiều service hoặc nhiều database thì hay dùng mô hình Saga, tức chia thành chuỗi giao dịch nhỏ ở từng service kèm bước bù trừ để hoàn tác nếu bước sau lỗi.

Câu hỏi phỏng vấn:

Q: ACID là gì?
Trả lời: Là 4 tính chất của transaction: Atomicity (all hoặc nothing), Consistency (luôn thỏa ràng buộc), Isolation (các giao dịch không nhiễu nhau), Durability (đã commit thì không mất). Ví dụ chuyển tiền cần Atomicity để không xảy ra trừ tiền mà không cộng.

Q: Khi nào cần dùng transaction?
Trả lời: Khi một nghiệp vụ gồm nhiều thao tác ghi phải toàn vẹn cùng nhau, ví dụ đặt hàng gồm trừ kho, tạo đơn, ghi thanh toán. Nếu một bước lỗi phải rollback tất cả để tránh trạng thái nửa vời.

## Locking

Là gì?
- Là cơ chế database dùng để kiểm soát khi nhiều giao dịch cùng truy cập một dữ liệu, tránh ghi đè hoặc đọc sai của nhau.
- Khi một giao dịch khóa một dòng, giao dịch khác phải chờ tới khi khóa được nhả.

Các loại lock:
- Shared lock (khóa đọc): nhiều giao dịch cùng đọc được nhưng không ai ghi được.
- Exclusive lock (khóa ghi): chỉ một giao dịch giữ, chặn cả đọc lẫn ghi từ giao dịch khác.
- Row lock: khóa từng dòng, tốt cho nhiều người dùng cùng lúc.
- Table lock: khóa cả bảng, đơn giản nhưng chặn nhiều.

Optimistic và Pessimistic locking:
- Pessimistic (bi quan): giả định sẽ có tranh chấp nên khóa dữ liệu ngay khi đọc.
```sql
START TRANSACTION;
SELECT * FROM products WHERE id = 1 FOR UPDATE;  -- khóa dòng
UPDATE products SET stock = stock - 1 WHERE id = 1;
COMMIT;
```
- Optimistic (lạc quan): giả định ít tranh chấp nên không khóa, mà dùng cột version để kiểm tra lúc ghi.
```sql
-- Lúc đọc version = 5
UPDATE products SET stock = stock - 1, version = version + 1
WHERE id = 1 AND version = 5;
-- Nếu không có dòng nào bị sửa nghĩa là có người sửa trước, báo lỗi và thử lại
```

Dùng để làm gì?
- Ngăn tình trạng nhiều request cùng sửa một dữ liệu gây sai (race condition).
- Ví dụ hai người cùng mua món hàng cuối cùng, lock đảm bảo không bán vượt tồn kho.

Khi nào dùng?
- Pessimistic khi tranh chấp hay xảy ra và hậu quả nghiêm trọng như trừ tiền, trừ kho hàng hot.
- Optimistic khi tranh chấp hiếm, đọc nhiều ghi ít, cần chịu tải cao.

Khác gì với cái tương đương?
- Pessimistic với optimistic: pessimistic khóa trước nên an toàn nhưng giảm số người chạy cùng lúc và dễ deadlock. Optimistic không khóa nên chịu tải cao hơn nhưng phải xử lý thử lại khi đụng độ.
- Với Isolation level: isolation level là mức muốn cô lập tới đâu, còn lock là cơ chế database dùng để đạt được mức đó.

Câu hỏi phỏng vấn:

Q: Phân biệt optimistic và pessimistic locking?
Trả lời: Pessimistic khóa dữ liệu ngay khi đọc bằng SELECT FOR UPDATE, an toàn nhưng giảm hiệu năng và dễ deadlock. Optimistic không khóa, dùng cột version để phát hiện đụng độ lúc ghi, nếu đụng thì thử lại. Chọn theo tần suất tranh chấp.

Q: Xử lý trường hợp nhiều người cùng mua món hàng cuối thế nào?
Trả lời: Có thể dùng SELECT FOR UPDATE để khóa dòng sản phẩm rồi kiểm tra và trừ tồn kho trong cùng transaction. Hoặc dùng update có điều kiện UPDATE products SET stock = stock - 1 WHERE id = ? AND stock > 0 rồi kiểm tra có dòng nào bị sửa không. Cách thứ hai đơn giản và hiệu quả.

## Isolation Level

Là gì?
- Là mức độ một giao dịch bị cô lập khỏi các giao dịch chạy cùng lúc, quyết định nó có nhìn thấy thay đổi chưa commit hoặc vừa commit của giao dịch khác hay không.
- Có 4 mức, đánh đổi giữa an toàn dữ liệu và tốc độ.

Ba hiện tượng cần biết:
- Dirty read: đọc phải dữ liệu mà giao dịch khác chưa commit, nếu nó rollback thì mình đọc phải dữ liệu không có thật.
- Non-repeatable read: đọc lại cùng một dòng ra kết quả khác vì giao dịch khác đã sửa và commit ở giữa.
- Phantom read: chạy lại cùng một query có điều kiện ra số dòng khác vì giao dịch khác đã thêm hoặc xóa dòng.

Bốn mức và hiện tượng chúng chặn được:
- READ UNCOMMITTED: không chặn gì, có cả dirty read.
- READ COMMITTED: chặn dirty read.
- REPEATABLE READ: chặn thêm non-repeatable read.
- SERIALIZABLE: chặn cả phantom read, cô lập tuyệt đối nhưng chậm nhất.

Mặc định:
- MySQL InnoDB mặc định REPEATABLE READ (và nhờ cơ chế khóa khoảng nên chặn được cả phantom read trong hầu hết trường hợp).
- PostgreSQL mặc định READ COMMITTED.

Dùng để làm gì?
- Điều chỉnh cân bằng giữa an toàn dữ liệu và tốc độ tùy nghiệp vụ. Mức thấp thì nhanh nhưng dễ đọc sai, mức cao thì an toàn nhưng chậm và dễ deadlock.

Khi nào dùng mức nào?
- READ COMMITTED hợp cho đa số web app.
- REPEATABLE READ khi trong một giao dịch cần đọc dữ liệu nhất quán nhiều lần, ví dụ báo cáo.
- SERIALIZABLE khi cần đúng tuyệt đối, chấp nhận chậm.
- READ UNCOMMITTED gần như không dùng thực tế.

```sql
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

Khác gì với cái tương đương?
- Với Locking: isolation level là chính sách muốn cô lập tới đâu, còn lock là cách database hiện thực. Mức càng cao thì thường khóa càng nhiều nên càng dễ nghẽn.

Câu hỏi phỏng vấn:

Q: Kể 4 mức isolation và chúng chặn hiện tượng gì?
Trả lời: READ UNCOMMITTED không chặn gì. READ COMMITTED chặn dirty read. REPEATABLE READ chặn thêm non-repeatable read. SERIALIZABLE chặn cả phantom read. Mức càng cao thì càng an toàn nhưng càng chậm và dễ nghẽn.

Q: Dirty read, non-repeatable read, phantom read khác nhau thế nào?
Trả lời: Dirty read là đọc phải dữ liệu chưa commit. Non-repeatable read là đọc lại cùng một dòng ra giá trị khác vì bị sửa ở giữa. Phantom read là chạy lại cùng query ra thêm hoặc bớt dòng vì bị thêm/xóa ở giữa. Non-repeatable nói về dòng đã có, phantom nói về số lượng dòng thỏa điều kiện.

## Deadlock

Là gì?
- Là tình huống hai hay nhiều giao dịch chờ nhau nhả khóa tạo thành vòng chờ vô tận, không ai chạy tiếp được.
- Ví dụ: giao dịch 1 khóa dòng A rồi đợi dòng B, giao dịch 2 khóa dòng B rồi đợi dòng A, thế là cả hai kẹt.

Ví dụ gây deadlock:
```sql
-- Giao dịch 1
UPDATE accounts SET balance = balance - 100 WHERE id = 1;  -- khóa dòng 1
UPDATE accounts SET balance = balance + 100 WHERE id = 2;  -- đợi dòng 2

-- Giao dịch 2 chạy cùng lúc, thứ tự ngược lại
UPDATE accounts SET balance = balance - 50 WHERE id = 2;   -- khóa dòng 2
UPDATE accounts SET balance = balance + 50 WHERE id = 1;   -- đợi dòng 1
```

Dùng để làm gì? (nhận biết và xử lý)
- Deadlock là sự cố cần tránh, không phải công cụ.
- Database thường tự phát hiện và hủy một giao dịch để giải vòng, giao dịch bị hủy nhận lỗi và cần thử lại.

Cách phòng tránh:
- Luôn khóa các tài nguyên theo cùng một thứ tự, ví dụ luôn xử lý id nhỏ trước. Đây là cách quan trọng nhất.
- Giữ giao dịch ngắn gọn, commit sớm để nhả khóa nhanh.
- Chỉ khóa dòng cần thiết, tránh khóa cả bảng.
- Có cơ chế thử lại khi bị deadlock.

Khi nào cần quan tâm?
- Khi hệ thống có nhiều giao dịch ghi cùng lúc trên cùng tập dữ liệu.
- Khi thấy log báo lỗi kiểu Deadlock found, try restarting transaction.

Khác gì với cái tương đương?
- Với lock wait timeout: deadlock là vòng chờ thật sự không thể tự giải, phải hủy một bên. Lock wait timeout chỉ là một giao dịch chờ lấy khóa quá lâu rồi bỏ cuộc, không nhất thiết có vòng chờ.
- Với race condition: race condition là kết quả sai do thứ tự chạy không kiểm soát, deadlock là kẹt do chờ khóa lẫn nhau. Dùng lock để chống race condition nhưng lock sai cách lại gây deadlock.

Câu hỏi phỏng vấn:

Q: Deadlock là gì và xảy ra khi nào?
Trả lời: Là khi hai hay nhiều giao dịch chờ khóa của nhau tạo vòng chờ vô tận, ví dụ giao dịch 1 giữ A đợi B, giao dịch 2 giữ B đợi A. Xảy ra khi có nhiều giao dịch ghi cùng lúc khóa các tài nguyên theo thứ tự khác nhau.

Q: Làm sao phòng tránh deadlock?
Trả lời: Quan trọng nhất là luôn khóa tài nguyên theo cùng một thứ tự để không tạo vòng chờ. Ngoài ra giữ giao dịch ngắn và commit sớm, chỉ khóa dòng cần thiết, và có cơ chế thử lại khi bị deadlock.

## Pagination

Là gì?
- Là chia một tập kết quả lớn thành nhiều trang nhỏ để trả về từng phần thay vì lấy hết một lúc.
- Hai cách chính: dùng OFFSET và dùng keyset (dựa trên mốc của dòng cuối trang trước).

Dùng để làm gì?
- Hiển thị danh sách dài như bài viết, sản phẩm, giao dịch theo từng trang.
- Giảm tải vì không phải load hàng triệu dòng cùng lúc.

Khi nào dùng?
- Bất cứ khi nào trả về danh sách có thể lớn cho client.

Cách 1, dùng OFFSET:
```sql
-- Trang 3, mỗi trang 20 dòng, bỏ qua 40 dòng đầu
SELECT * FROM posts ORDER BY created_at DESC LIMIT 20 OFFSET 40;
```
Ưu điểm: đơn giản, nhảy tới trang bất kỳ dễ, biết được tổng số trang.
Nhược điểm: OFFSET lớn rất chậm vì database vẫn phải quét và bỏ qua toàn bộ số dòng đầu. Ngoài ra dễ bị lệch dòng nếu dữ liệu thay đổi giữa các lần lật trang.

Cách 2, dùng keyset:
```sql
-- Trang đầu
SELECT * FROM posts ORDER BY id DESC LIMIT 20;
-- Trang sau, dùng id nhỏ nhất của trang trước làm mốc, giả sử là 981
SELECT * FROM posts WHERE id < 981 ORDER BY id DESC LIMIT 20;
```
Ưu điểm: nhanh và ổn định dù ở trang sâu, không lệch dòng khi có thêm/xóa.
Nhược điểm: không nhảy tới trang số bất kỳ, chỉ đi tiếp hoặc lùi.

Khác gì với cái tương đương?
- OFFSET với keyset: OFFSET dễ dùng, đi được tới trang bất kỳ nhưng chậm dần ở trang sâu. Keyset nhanh và ổn định ở mọi độ sâu nhưng chỉ đi tuần tự. Dùng OFFSET cho trang quản trị hoặc dữ liệu nhỏ, dùng keyset cho cuộn vô hạn và API dữ liệu lớn.

Câu hỏi phỏng vấn:

Q: Tại sao LIMIT OFFSET với offset lớn lại chậm?
Trả lời: Vì database vẫn phải đọc và bỏ đi toàn bộ số dòng trong OFFSET trước khi lấy được dòng cần. OFFSET 1000000 LIMIT 20 nghĩa là phải duyệt qua hơn một triệu dòng rồi vứt đi, chỉ giữ 20 dòng cuối.

Q: Keyset pagination là gì, khi nào dùng?
Trả lời: Là phân trang dựa trên giá trị mốc của dòng cuối trang trước, ví dụ WHERE id < last_id, thay vì đếm offset. Nhờ dùng index nhảy thẳng tới vị trí nên nhanh và ổn định ở trang sâu. Dùng cho cuộn vô hạn và API dữ liệu lớn.

## Full Text Search

Là gì?
- Là cách tìm từ khóa bên trong nội dung văn bản dài một cách hiệu quả, dựa trên inverted index tức bảng ánh xạ mỗi từ tới danh sách tài liệu chứa từ đó.
- Có tách từ, bỏ các từ vô nghĩa (the, a, và, là), và xếp hạng theo độ liên quan.

Dùng để làm gì?
- Tìm kiếm nội dung bài viết, mô tả sản phẩm, bình luận, tài liệu.
- Trả kết quả xếp theo độ liên quan.

Khi nào dùng?
- Khi cần tìm từ khóa nằm ở giữa văn bản chứ không chỉ khớp đầu chuỗi.
- Khi LIKE '%keyword%' quá chậm vì không dùng được index.

Ví dụ MySQL:
```sql
CREATE FULLTEXT INDEX idx_posts_content ON posts(title, content);
SELECT * FROM posts
WHERE MATCH(title, content) AGAINST('database index');
```

Ví dụ PostgreSQL:
```sql
CREATE INDEX idx_posts_fts ON posts
USING GIN (to_tsvector('english', title || ' ' || content));
SELECT * FROM posts
WHERE to_tsvector('english', title || ' ' || content) @@ to_tsquery('english', 'database & index');
```

Khác gì với cái tương đương?
- Với LIKE '%...%': LIKE có wildcard ở đầu thì không dùng được index nên phải quét cả bảng, rất chậm với bảng lớn, và chỉ khớp chuỗi con thô, không hiểu từ, không xếp hạng. Full text search dùng inverted index nên nhanh, hiểu từ, có xếp hạng. Nhưng LIKE vẫn tiện cho khớp mẫu đơn giản trên bảng nhỏ.
- Với Elasticsearch: full text search trong database đủ cho nhu cầu vừa phải và dữ liệu nằm sẵn trong database. Elasticsearch mạnh hơn nhiều về quy mô, chịu lỗi gõ sai, xếp hạng nâng cao, gợi ý, nhưng phải dựng thêm hệ thống và đồng bộ dữ liệu nên phức tạp hơn. Nên dùng full text search của database trước, khi nào không đủ mới chuyển sang Elasticsearch.

Câu hỏi phỏng vấn:

Q: Tại sao không nên dùng LIKE '%keyword%' để tìm kiếm?
Trả lời: Vì wildcard ở đầu khiến index vô hiệu, database phải quét cả bảng nên rất chậm khi bảng lớn. Ngoài ra nó chỉ khớp chuỗi con máy móc, không tách từ, không xếp hạng độ liên quan. Nên dùng full text search thay thế.

Q: Khi nào chuyển từ full text search của database sang Elasticsearch?
Trả lời: Khi tìm kiếm trở thành tính năng cốt lõi, dữ liệu rất lớn, cần chịu lỗi gõ sai, xếp hạng phức tạp hoặc gợi ý. Đánh đổi là phải dựng thêm Elasticsearch và đồng bộ dữ liệu từ database.

## Pre-aggregation

Là gì?
- Là tính sẵn các kết quả tổng hợp như SUM, COUNT, AVG rồi lưu lại, thay vì tính lại từ đầu mỗi lần query.
- Hình thức hay dùng: bảng tổng hợp cập nhật định kỳ, materialized view, và đếm sẵn (counter cache).

Dùng để làm gì?
- Tăng tốc các query báo cáo hoặc dashboard nặng vốn phải gộp hàng triệu dòng.
- Giảm tải cho database vào giờ cao điểm.

Khi nào dùng?
- Khi báo cáo hoặc dashboard chậm vì phải tổng hợp dữ liệu lớn mỗi lần.
- Khi cùng một con số được đọc rất nhiều lần nhưng dữ liệu gốc đổi chậm.
- Khi chấp nhận số liệu trễ một chút, không cần realtime tuyệt đối.

Ví dụ bảng tổng hợp cập nhật hàng đêm:
```sql
CREATE TABLE daily_sales (
  sale_date DATE PRIMARY KEY,
  total_orders INT,
  total_revenue DECIMAL(15,2)
);

-- Job chạy mỗi đêm, tính một lần dùng nhiều lần
INSERT INTO daily_sales (sale_date, total_orders, total_revenue)
SELECT DATE(created_at), COUNT(*), SUM(total)
FROM orders
WHERE DATE(created_at) = '2026-07-01'
GROUP BY DATE(created_at);

-- Đọc báo cáo rất nhanh vì không phải quét bảng orders khổng lồ
SELECT * FROM daily_sales WHERE sale_date BETWEEN '2026-06-01' AND '2026-06-30';
```

Ví dụ đếm sẵn:
```sql
-- Thay vì COUNT(*) mỗi lần, giữ sẵn cột đếm
UPDATE posts SET comment_count = comment_count + 1 WHERE id = 10;
```

Khác gì với cái tương đương?
- Với view thường: view thường chỉ lưu câu query, mỗi lần gọi vẫn tính lại từ đầu nên không tăng tốc. Materialized view lưu kết quả thật ra disk nên đọc nhanh, nhưng dữ liệu có thể cũ và phải refresh.
- Với cache (Redis): cache lưu kết quả ở tầng ứng dụng theo key và hay hết hạn theo thời gian. Pre-aggregation là dữ liệu tổng hợp có cấu trúc, vẫn query được bằng SQL để lọc tiếp. Cache hợp cho vài con số hay dùng, pre-aggregation hợp cho báo cáo cần lọc theo ngày, theo nhóm.

Điểm cần lưu ý:
- Dữ liệu tổng hợp có thể trễ so với gốc nên không hợp nghiệp vụ cần realtime tuyệt đối.
- Tăng độ phức tạp vì phải cập nhật hoặc refresh, và với counter cache dễ bị lệch số nếu cập nhật sai.

Câu hỏi phỏng vấn:

Q: Dashboard thống kê chậm vì phải SUM và COUNT hàng triệu dòng mỗi lần, xử lý sao?
Trả lời: Dùng pre-aggregation, tức tính sẵn số liệu và lưu lại. Ví dụ tạo bảng tổng hợp theo ngày, job chạy định kỳ điền số liệu, dashboard chỉ đọc bảng tổng hợp nhỏ nên rất nhanh. Với PostgreSQL có thể dùng materialized view rồi refresh theo lịch.

Q: Materialized view khác view thường thế nào?
Trả lời: View thường chỉ lưu câu query, mỗi lần truy vấn vẫn tính lại từ đầu nên không tăng tốc. Materialized view lưu sẵn kết quả ra disk nên đọc nhanh, nhưng dữ liệu có thể cũ và phải refresh để cập nhật.

## Monitoring

Là gì?
- Là theo dõi liên tục tình trạng và hiệu năng của database: query chậm, tải CPU/RAM/disk, số kết nối, tỉ lệ đọc trúng cache, lock và deadlock.
- Gồm thu thập số liệu, ghi log, cảnh báo và hiển thị lên dashboard.

Dùng để làm gì?
- Phát hiện sớm vấn đề trước khi người dùng bị ảnh hưởng.
- Tìm query đang gây nghẽn để tối ưu.
- Điều tra khi hệ thống chậm hoặc sập.

Khi nào dùng?
- Luôn luôn ở production, giám sát là bắt buộc.
- Khi điều tra sự cố hiệu năng.

Các chỉ số quan trọng cần theo dõi:
- Query chậm: query vượt ngưỡng thời gian, cần tối ưu trước tiên.
- Số kết nối đang mở so với tối đa, tránh cạn kết nối.
- Tỉ lệ đọc trúng cache, thấp nghĩa là thiếu RAM hoặc query kém.
- Số lần chờ khóa và deadlock.
- CPU, RAM, disk.

Ví dụ MySQL:
```sql
-- Bật slow query log, ghi query chạy quá 1 giây
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 1;

-- Xem các query đang chạy
SHOW FULL PROCESSLIST;
```

Ví dụ PostgreSQL:
```sql
-- Xem top query theo tổng thời gian, cần bật extension pg_stat_statements
SELECT query, calls, total_exec_time, mean_exec_time
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 10;
```

Khác gì với cái tương đương?
- Với logging: logging ghi lại từng sự kiện rời rạc như một query, một lỗi để điều tra chi tiết. Monitoring theo dõi số liệu theo thời gian để thấy tình trạng chung và cảnh báo. Số liệu cho biết có gì đó sai, log giúp biết sai ở đâu, dùng bổ trợ nhau.

Câu hỏi phỏng vấn:

Q: Database production chạy chậm, bắt đầu điều tra từ đâu?
Trả lời: Nhìn vào monitoring trước, kiểm tra CPU, RAM, disk có bị đẩy cao không, số kết nối có cạn không. Rồi xem slow query log để tìm query nặng nhất. Kiểm tra danh sách query đang chạy xem có cái nào treo hoặc bị lock. Sau khi khoanh vùng được query thì chạy EXPLAIN để tối ưu.

Q: Slow query log là gì?
Trả lời: Là log ghi lại các query chạy lâu hơn ngưỡng đặt trước. Dùng để phát hiện query cần tối ưu. Thường phân tích bằng công cụ để gom nhóm và xếp hạng query nặng theo tổng thời gian chứ không chỉ theo một lần chạy.
