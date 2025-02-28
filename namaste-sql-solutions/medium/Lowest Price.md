Question Link : https://www.namastesql.com/coding-problem/55-lowest-price?question_type=0&page=1&pageSize=10

Solution

```sql
select 
  	pr.category,
  	coalesce(min(case when pu.product_id is not null then pr.price end),0) as price
from 
	products pr 
left join 
	purchases pu 
on pr.id = pu.product_id
and pu.stars >= 4
group by category
order by category;
```
