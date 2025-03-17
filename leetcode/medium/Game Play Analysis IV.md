Question link: https://leetcode.com/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=top-sql-50

Solution
```sql
with cte as(
select 
    player_id,
    event_date,
    event_date - min(event_date)over(partition by player_id) as first_login_diff
from 
    activity
)
select 
    round(
    1.0 *count(case when first_login_diff = 1 then 1 end)/
    (select count(distinct player_id) from activity),2) as fraction
from 
    cte;
```
