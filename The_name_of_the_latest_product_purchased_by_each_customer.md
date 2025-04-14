- Exercise Link:https://www.nowcoder.com/practice/6ff37adae90f490aafa313033a2dcff7
# Solution:
```mysql
WITH last_order AS(
    SELECT product_id,customer_id
    FROM (
        SELECT 
            *,
            rank() over (partition by customer_id order by order_date DESC) AS rk
        FROM orders
    ) AS t1
    WHERE t1.rk=1
)
SELECT  
    a.*,
    c.product_name AS latest_order
FROM customers AS a
LEFT JOIN last_order AS b
ON a.customer_id=b.customer_id
LEFT JOIN products AS c
ON b.product_id=c.product_id;
```
