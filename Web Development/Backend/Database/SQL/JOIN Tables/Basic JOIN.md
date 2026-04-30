
lấy ví dụ 2 bảng reference với nhau qua 1 common key.

cộng gộp các trường dữ liệu (columns) của 2 bảng lại.

Lấy ví dụ.


các loại JOIN cơ bản

INNER JOIN: chỉ giữ lại các key đều tồn tại trên 2 bảng. 

LEFT OUTER JOIN:

RIGHT OUTER JOIN:


CROSS JOIN 
tao tổi hơp 
``` sql
WITH
  cte AS (
    SELECT
      a.product_id as product1_id,
      b.product_id as product2_id,
      count(DISTINCT a.user_id) as customer_count
    FROM
      ProductPurchases a
      CROSS JOIN ProductPurchases b
    WHERE
      b.product_id > a.product_id
      AND a.user_id = b.user_id
    GROUP BY
      product1_id,
      product2_id
    HAVING
      count(DISTINCT a.user_id) >= 3
  )
SELECT
  cte.product1_id,
  cte.product2_id,
  pi1.category AS product1_category,
  pi2.category AS product2_category,
  cte.customer_count
FROM
  cte
  JOIN ProductInfo pi1 ON pi1.product_id = cte.product1_id
  JOIN ProductInfo pi2 ON pi2.product_id = cte.product2_id
ORDER BY
  customer_count DESC,
  product1_id,
  product2_id
```


| user_id | product_id | quantity |
| ------- | ---------- | -------- |
| 1       | 101        | 2        |
| 1       | 102        | 1        |
| 1       | 103        | 3        |
| 2       | 101        | 1        |
| 2       | 102        | 5        |
| 2       | 104        | 1        |
| 3       | 101        | 2        |
| 3       | 103        | 1        |
| 3       | 105        | 4        |
| 4       | 101        | 1        |
| 4       | 102        | 1        |
| 4       | 103        | 2        |
| 4       | 104        | 3        |
| 5       | 102        | 2        |
| 5       | 104        | 1        |

| product1_id | product2_id | product1_category | product2_category | customer_count | 
| ----------- | ----------- | ----------------- | ----------------- | -------------- | 
|101 | 102 | Electronics | Books | 3 |
| 101 | 103 | Electronics | Clothing | 3 | 
| 102 | 104 | Books | Kitchen | 3 |

**Specific Use Cases:** Suitable for generating combinations (e.g., all colors x all sizes), creating reports with averages/projections, or filling in missing time-series data.


