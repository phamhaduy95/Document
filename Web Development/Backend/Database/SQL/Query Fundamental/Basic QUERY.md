Một câu query thường có cấu trúc như sau:  

``` sql
SELECT <column_list>
FROM  <table_name>
JOIN <other tables> ON 
WHERE <condition>
GROUP BY <column>
HAVING <condition>
ORDER BY 
```


WHERE:
ta có thể so sánh các
```
SELECT *
FROM table_name
WHERE column1 > column2;
```
GROUP BY:
HAVING:
ORDER BY:

Thứ tự xuất hiện của các Clause trong câu query không quyết định đến thứ tự thực thi của chúng. Database engine chạy query theo trình tự sau đây:

FROM->JOIN->WHERE->GROUP BY ->HAVING->ORDER_BY->SELECT

- **FROM / JOIN**: Establish the raw dataset.
- **WHERE**: Filter rows. 
- **GROUP BY**: Aggregate data into groups. 
- **HAVING**: Filter the aggregated groups.
- **Window Functions**: Compute values (like `RANK` or `SUM() OVER`) across the rows that survived the previous filters.  Cũng chính vì lý do này, mà ta không thể filter theo các field dùng window function trong WHERE 
- **SELECT**: Choose the final columns/expressions to display.
- **DISTINCT**: Remove duplicate rows.
- **ORDER BY / LIMIT**: Sort and trim the final output.

Các hàm WHERE, GROUP BY, ORDER BY, HAVING và SELECT ta có thể sử dụng các giá trí 

imaginary column
một số query khó cần sử dụng imaginary column



complex logical 
https://leetcode.com/problems/investments-in-2016