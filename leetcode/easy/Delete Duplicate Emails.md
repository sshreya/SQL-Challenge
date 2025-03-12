Question Link: https://leetcode.com/problems/delete-duplicate-emails/description/?envType=study-plan-v2&envId=top-sql-50

Solution

```sql
with cte as(
select 
    id,
    email,
    row_number()over(partition by email order by id) as rn
from 
    person
)
delete from person where id in (select id from cte where rn > 1);
```
