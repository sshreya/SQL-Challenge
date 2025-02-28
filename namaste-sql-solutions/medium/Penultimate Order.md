Question link : https://www.namastesql.com/coding-problem/64-penultimate-order?page=1&pageSize=10&question_type=0

Solution

```sql
select order_id,order_date,customer_name,product_name, sales from(
select
	*,
    count(order_id)over(partition by customer_name) as num_orders,
    dense_rank()over(partition by customer_name order by order_date) as rn 
from 
	orders
)x
where x.rn = (case when x.num_orders > 1 then (num_orders - 1) else 1 end);
```
