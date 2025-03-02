Question Link : https://leetcode.com/problems/user-activity-for-the-past-30-days-i/description/?envType=study-plan-v2&envId=top-sql-50

Solution

```sql
select 
    activity_date as day,
    count(distinct user_id) as active_users 
from 
    Activity
where activity_date between '2019-06-28' and '2019-07-27'
group by 1;
```
