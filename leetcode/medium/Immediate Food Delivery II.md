Question Link: https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=top-sql-50

Solution

```sql
with cte as(
select 
    *,
    first_value(order_date)over(partition by customer_id order by order_date) as first_order
from 
    delivery
)
select 
    round(100.0 * count(case when (order_date = customer_pref_delivery_date) and (first_order = order_date) then 1 end)/count(distinct customer_id),2) as immediate_percentage
from 
    cte;
```
