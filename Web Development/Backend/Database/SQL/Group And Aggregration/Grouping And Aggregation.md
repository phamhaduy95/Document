
SUM(), AVG(), MAX(), MIN(), COUNT(), and COUNT(*)

Nếu group by không sử dụng, các hàm sẽ thực hiện tính toán lên toàn bộ tệp data của table đó.



Khi sử dụng các hàm aggregation, ngoài column được làm tham chiếu cho `GROUP BY`, ta không thể sử dụng các column khác trong các hàm `SELECT`, `HAVING` và `ORDER BY`

``` sql
SELECT patient_id, COUNT(*) AS total FROM admissions
```

| patient_id | total |
| ---------- | ----- |
| 1          | 200   |


Để đảm bảo performance cho các query sử dụng group by và aggregation function, ta nên thực hiện filter early trong WHERE kết hợp với index để tránh scan toàn bộ table.

hàm COUNT()  thực hiện phép tính lên toàn bộ row trong scope cho phép trong khi sử dụng COUNT(column_name) chỉ count các row có field column_name khác NULL.


CONDITIONAL AGGREGATION 
``` SQL
SELECT 
	CASE 
    	when patient_id %2 = 0 
        THEN 'Yes'
        ELSE 'NO'
    END as has_insurance,
  	SUM (
      	case 
         	when patient_id % 2 = 0  THEN 10 ELSE 50 
         END
    ) as cost_after_insurance
FROM admissions
group by has_insurance
```


Cẩn thận khi thực hiện các toán tử aggregation trong query join nhiều table khác nhau do có thể tồn tại nhiều duplicate data. Để tránh việc ta thêm keyword DISTINCT trong các aggregate function . VÍ dụ COUNT(DISTINCT user_id) 

Tạo các row không tồn tại trong query 


`string_agg` is an aggregate function used to concatenate values from multiple rows into a single string, separated by a specified delimiter. It is commonly used with the `GROUP BY` clause to aggregate data into a readable, comma-separated list or other formats