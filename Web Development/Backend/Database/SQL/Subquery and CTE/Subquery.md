subquery có thể chia thành 3 loại chính
scalar subquery -  subquery trả về 1 giá trí duy nhất
table subquery -  subquery trả về 1 table hoan
column subquery - 

**correlated subqueries**

You can use a subquery in several places in another SELECT, UPDATE, INSERT, or DELETE statement.


correlate and non-correlated subquery


where should we use subquery

table subquery

Checking Whether Values Exist
```sql
SELECT first_name, last_name
FROM employees
WHERE EXISTS (
SELECT id
FROM retirees
WHERE id = employees.emp_id);
```

 ``` sql
 
SELECT first_name, last_name
FROM employees
WHERE emp_id IN (
SELECT id
FROM retirees)
ORDER BY emp_id;
```



```sql
CREATE TABLE us_counties_2019_top10 AS
SELECT * FROM us_counties_pop_est_2019;
DELETE FROM us_counties_2019_top10
WHERE pop_est_2019 < (
SELECT percentile_cont(.9) WITHIN GROUP (ORDER BY
pop_est_2019)
FROM us_counties_2019_top10
);
```

correlated subquery


Using Subqueries with LATERAL


