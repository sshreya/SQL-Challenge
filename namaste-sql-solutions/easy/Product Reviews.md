Question Link : https://www.namastesql.com/coding-problem/38-product-reviews?question_type=0&page=1&pageSize=10

Solution

```sql
select
	*
from 
	product_reviews 
where (lower(review_text) like '%excellent%' or lower(review_text) like '%amazing%')
and lower(review_text) not like '%not excellent%'
and lower(review_text) not like '%not amazing%';
```
