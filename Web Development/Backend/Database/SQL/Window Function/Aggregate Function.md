cho phép tạo các column từ hàm aggregation mà không làm thay đổi cấu trúc ban đầu của table.
Đưa 1 ví dụ về việc tính tỉ lý giữa giá trị trung bình 

**Aggregate Window Functions**

tim max min trong 1 department

u cannot use window functions like `LAG()` directly in a `WHERE` clause.  
**Why:** In the SQL logical order of operations, the `WHERE` filter is applied **before** window functions are calculated

RANK  problem to query the Nth largest value in table


``` sql
# wrong syntax, you can not reference other windowed field
SELECT
	id,
	num,
	num - lag(num,1,0) OVER (ORDER BY id) AS delta,
	LEAD(delta) OVER(ORDER BY id) AS `lead`
FROM Logs
```

- **Placement**: Window functions are only allowed in the `SELECT` list and the `ORDER BY` clause. You cannot use them inside a `CASE` statement that is located in a `WHERE` or `GROUP BY` clause.
- **Aliases**: If you calculate a window function in a `SELECT` statement and give it an alias, you cannot reference that alias inside another `CASE` statement in the same `SELECT` block; you must either repeat the full function or use a CTE/subquery.
- **No Nesting**: Most SQL dialects do not allow you to nest window functions directly (e.g., `LAG(RANK() OVER(...)) OVER(...)` is generally prohibited).




Common Use Cases

You can combine them to get both a summarized value and a comparison within that summary: 

1. **Ranking Groups:** Finding the top-performing group (e.g., ranking departments by total sales).
2. **Aggregated Results:** Displaying a specific row value alongside a total sum of the entire group. 

Example: Finding Top Performing Departments

This query groups by department to find the total salary, and then uses a window function to rank those departments by their total expense

```sql
SELECT 
    department_id, 
    SUM(salary) as total_salary,
    RANK() OVER (ORDER BY SUM(salary) DESC) as salary_rank
FROM employees
GROUP BY department_id;

```