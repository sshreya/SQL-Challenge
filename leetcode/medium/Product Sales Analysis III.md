Question Link: https://leetcode.com/problems/product-sales-analysis-iii/description/?source=submission-noac

Solution

```sql
select product_id,year as first_year, quantity, price from(
select 
    product_id,
    year,
    quantity,
    price,
    dense_rank()over(partition by product_id order by year) as rn 
from 
    sales 
)x
where x.rn = 1;
```
