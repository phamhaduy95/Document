
CTE common table expression 

CTE giúp khai báo 1 table tạm

Common Table Expressions (CTEs), or WITH syntax, are useful bits of syntactic sugar
that allow you to tidy up your query. If you have complex queries, CTEs can make
them more readable by breaking the SQL into smaller, more digestible pieces. Com-
pared to subqueries, CTEs can save you some repetition because you can reference
them multiple times within the same query.


Starting with **PostgreSQL 12** (released in 2019), the behavior became more flexible to improve performance. The current rules are: 

- **Referenced multiple times:** If you refer to a CTE more than once in your query, PostgreSQL **still evaluates it only once** by default. It materializes the result to avoid redundant work.
- **Referenced only once:** If the CTE is used only once, the optimizer may **"inline"** it (treating it like a subquery) to allow better optimization, such as using indexes from the outer query. In this case, it is evaluated as part of the larger query plan rather than as a separate step.


nên ưu tiên sử dụng `CTE` thay thế cho subquery.
lý do:
code dễ đọc và maintain hơn.
performance được cải thiện do CTE chỉ evaluate đúng 1 lần duy nhất.
trong trường hợp `CTE` chỉ sử dụng 1 lần, optimizer sẽ tự động chuyển `CTE` thành subquery và inline vào main query.


cẩn thận khi sử dụng BETWEEN predicate do BETWEEN sẽ include cả 2 cận bên của câu điều kiện. Nêu điều kiện không dùng cần bên ta nên dùng 2 điều kiện so sánh và AND

recursive CTEs 