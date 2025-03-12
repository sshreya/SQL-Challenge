Question Link : https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50

Solution 1

```sql
with cte as(
select 
    machine_id,
    process_id,
    coalesce((case when activity_type = 'start' then timestamp end),0) as start_time,
    coalesce((case when activity_type = 'end' then timestamp end),0) as end_time 
from 
    activity
)
,process_time as(
select 
    machine_id,
    process_id,
    sum(end_time - start_time) as time 
from 
    cte 
group by 1,2
)
select 
    machine_id,
    round(avg(time),3) as processing_time 
from 
    process_time 
group by 1;
```

Solution 2 
```sql
select 
    a.machine_id,
    round(avg(b.timestamp - a.timestamp),3) as processing_time
from 
    activity a 
join 
    activity b 
on a.machine_id = b.machine_id
and a.process_id = b.process_id
and a.activity_type = 'start'
and b.activity_type = 'end'
group by 1;
```
