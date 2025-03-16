Question link: https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/description/?envType=study-plan-v2&envId=top-sql-50

Solution

```sql
with cte as(
select 
    requester_id as id
from 
    RequestAccepted 
union all
select 
    accepter_id as id
from 
    RequestAccepted
)
select 
    id,
    count(*) as num 
from 
    cte 
group by 1
order by num desc 
limit 1;
```
