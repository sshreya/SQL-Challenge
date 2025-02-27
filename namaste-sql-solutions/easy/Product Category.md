Question Link - https://www.namastesql.com/coding-problem/2-product-category?page=1&pageSize=10

Solution

```sql
with cte as(
select 
	product_id,
    price,
    case 
    	when price < 100 then 'Low Price'
        when price between 100 and 500 then 'Medium Price'
        else 'High Price'
    end as category 
 from 
 	Products 
)
select 
	category,
    count(product_id) as no_of_products 
from cte 
group by 1
order by no_of_products desc;
```
