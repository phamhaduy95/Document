### Introduction
PostgreSQL hỗ trợ lưu trữ JSON data theo 2 định dạng sau : 
1. `json` (text format) :  
	- pros : 
		1. cùng là dạng text giống với JSON payload từ phía client
		2. Serve nhanh hơn cho phía client do không mất công đoạn text conversion như `jsonb`
	- cons :
		1. Không hỗ trợ query data bên trong JSON object
2. `jsonb` (Binary JSON format) : JSON được mã hóa theo định dạng binary
	- pros :
		1. tối ưu về vùng nhớ cũng như tốc độ truy vấn
		2. hỗ trợ các lệnh query sâu trong JSON object
	- cons :
		1.  thời gian query từ client sẽ lâu hơn do cần thời gian convert từ binary sang text format.

Nên ưu tiên sử dụng `jsonb` để tận dụng hết được các tính năng của PostgreSQL. Định dạng text `json` chỉ nên được cân nhắc khi JSON object được dùng trực tiếp và không cần phải query sâu bên trong Object đó.

Câu hỏi: Khi nào ta nên sử dụng JSON data trong PostgreSQL ?
Ta có thể sử dụng JSON data thay cho relational data thông thường cho các trường hợp sau:
-  kiểu dữ liệu có nhiều bến thể (invariant) và schema linh động.
- các configuration file cho user hoặc key-value store.
### Query JSON data
để query trực tiếp data của 1 field trong JSON object, ta có thể sử dụng `->` hoặc `->>`. Điểm khác nhau của 2 operator này là output format
-  `->` output trả về theo format JSON
-  `->>` output trả về là dạng text.

JSON format trong `postgreSQL` có JSON object được enclosed bởi  single quote `''` và các text field enclosed by double quote `" "`

``` SQL
SELECT
order_id, order_item_id,
pizza->'size' as pizza_size,
pizza->>'crust' as pizza_crust
FROM pizzeria.order_items
WHERE
order_id = 100 AND
pizza->'size' = '"small"' AND pizza->>'crust' = 'gluten_free';
```

để kiểm tra JSON object có chứa 1 cặp key-value hay không, ta có thể sử dụng toán tử `@>`. Lưu ý chi áp dụng cho scope đầu tiên của JSON object. Không kiểm tra được các nested scope.  
```sql
SELECT count(*)
FROM pizzeria.order_items
WHERE pizza @> '{"crust": "gluten_free"}';
```

Ví dụ: Câu SQL query trên  truy vấn các pizza JSON object có tồn tại key-value pair `crust : gluten_free`.

Với JSON object có nhiều nested và array, ta nên sử dụng JSON path expression 

``` sql
SELECT
count(*) as total_cnt,
jsonb_path_query(pizza,'$.type') as pizza_type
FROM pizzeria.order_items
GROUP BY pizza_type ORDER BY total_cnt DESC;
```

 `jsonb_path_query` cho phép query các value thông qua một path pattern. 
 
 Ví dụ `jsonb_path_query(pizza,$.topping.cheese[*])`: 
- `$` tượng trưng cho root object.
- `*`  *(wildcard symbol)* cho phép lấy tất các kết quả match chúng. Ở ví trên câu query trên sẽ lấy toàn bộ element trong cheese array.

Lưu ý: `jsonb_path_query()` cho phép ta thêm các điều kiện filter trong path pattern. Tuy nhiên cách này không được khuyến nghị do phướng pháp này không thể tối ưu do  không áp dụng được GIN index. Ta nên để filter logic trong `jsonb_path_exist` trên WHERE clause.
### Update JSON data

Để update JSON data ta có thể làm theo 2 cách:
1. cách 1 là thay thế hoàn toàn JSON object cũ bằng 1 JSON object mới. Phương pháp này kém hiệu quả với object lớn hoặc có nhiều nested level.
2. cách 2 sử dụng hàm `json_set` cho phép update riêng lẻ một field hay nhiều field thay vì replace hết toàn bộ (recommended).

### GIN index
Mặc dù ta có thể sử dụng B-tree hoặc Hash index cho các JSON column. Tuy nhiên do tính đa hình cao của JSON data mà các dạng index trên không hiệu quả.