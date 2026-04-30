### Introduction
#### Những lợi ích và trade off của index 

Sử dụng Index hiệu quả sẽ đêm đến sự gia tăng hiệu năng đáng kể cho tốc độ truy vấn. Bằng việc tạo một vùng nhớ riêng sử dụng 1 data structure chuyên biệt map giữa một giá trị index với địa chỉ vật lý của một hay nhiều row khác nhau trong table, thay vì scan toàn bộ một bảng, database sẽ nhanh chóng access được tới row cần tìm không qua mapping trên. 

Đặc biệt với B-tree index, các phép sorting và range search được tối ưu do các index được lưu trong B-tree đã được sắp xếp từ trước.

Tuy nhiên ta cũng cần lưu lý các trade off:
- Bất lợi đầu tiên là việc database sẽ cần dành dung lượng bộ nhớ nhất định để lưu trữ index dễ dẫn đến phình to bộ nhớ nếu quản lý index không kỹ. 
- Bất lợi thứ 2 là nguy cơ làm giảm thiểu hiệu quả của các câu lệnh `UPDATE` và `INSERT`, do database phải cập nhật lại index storage mỗi khi thực hiện các câu lệnh này
###  Type of index

####  B-tree index

B-tree index sử dụng B-tree cộng làm cơ sở dữ liệu chính để lưu index. B-tree là kiểu dữ liệu dạng 
tree có mỗi node đại diện cho 1 khoảng range value (ví dụ như 0-1000).  

![[Pasted image 20260107155921.png]]

A B-tree index là kiểu index mặc định do hổ trợ nhiều case query khác nhau như:
- query có `WHERE` clause dùng toàn tử `=
- query có `WHERE` clause dùng toàn tử range operator như `<`, `<=`,`>=`,`>`
- query có `WHERE LIKE` expression  tìm kiếm row column có text bắt đầu bởi 1 prefix ví dụ `LIKE 'Dorito%'.
- query có `ORDER BY` clause 
#### Hash index
Hash index dùng key-value table để lưu index. Điểm manh của hash index là ít tốn vùng nhớ và tốc độ truy vấn nhanh hơn so với B-tree. Tuy nhiên Hash index chỉ hỗ trợ các operator = trong `WHERE CLAUSE`.
#### Partial Index

Partial index cho phép thêm điều kiện khi thêm một index mới giúp giảm thiểu số lượng index được lưu trữ và tăng performance cho INSERT và UPDATE operation.

Lưu ý: **Do Not Use Partial Indexes as a Substitute for Partitioning**
#### Unique index

tạo 1 constraint lên index column đảm bảo các index phải unique. Nếu INSERT hoặc UPDATE duplicate index, database sẽ báo lỗi. Với unique index ta sẽ luôn đảm bảo 1 index sẽ chỉ mapping với 1 row duy nhất.

Lưu ý: unique index vẫn cho phép duplicate giá trị NULL.

```sql
CREATE UNIQUE INDEX EMPLOYEES_PK
ON EMPLOYEES (SUBSIDIARY_ID, EMPLOYEE_ID);
```
#### Composite index

```sql
CREATE UNIQUE INDEX EMPLOYEES_PK
ON EMPLOYEES (SUBSIDIARY_ID, EMPLOYEE_ID);
```

The most important consideration when defining a concatenated index is how to choose the column order so it can be used as often as possible.

Nếu thứ tự key trong composite index không trùng với thứ tự column trong ORDER BY clause




Khi đánh index ta cần quan tâm đên selectivity ratio
ta chỉ đánh index cho các column  có high selectivity để look up được tối ưu. Nên ta đánh index cho các 
- **High selectivity** means the index helps quickly narrow down the results (e.g., searching for a unique customer ID in a large table).
- **Low selectivity** means the value appears in a large percentage of the rows (e.g., searching for the "Active" status in a table where 95% of users are active).