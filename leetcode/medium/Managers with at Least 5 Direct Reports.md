Question link: https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?source=submission-ac

Solution

```sql
select name from Employee where id in(
select 
    managerId
from 
    Employee 
where managerId is not null
group by 1
having count(id) >= 5
)
;
```
