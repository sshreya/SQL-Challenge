Question Link: https://leetcode.com/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=top-sql-50

Solution

```sql
select  
    date_format(trans_date,'%Y-%m') as month,
    country,
    count(*) as trans_count,
    count(case when state = 'approved' then 1 end) as approved_count,
    sum(case when state = 'approved' then amount else 0 end) as approved_total_amount,
    sum(amount) as trans_total_amount   
from 
    Transactions 
group by 1,2;
```
