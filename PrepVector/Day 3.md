## Single vs Repeat Job Posters

### Problem Statement 
Given a table of job postings, write a query to retrieve the number of users that have posted each job only once and the number of users that have posted at least one job multiple times.

### Schema Setup
```sql
CREATE TABLE job_postings (
    id INT PRIMARY KEY,
    user_id INT,
    job_id INT,
    posted_date DATETIME
);
```

```sql
INSERT INTO job_postings (id, user_id, job_id, posted_date) VALUES
    (1, 1, 101, '2024-01-01'),
    (2, 1, 102, '2024-01-02'),
    (3, 2, 201, '2024-01-01'),
    (4, 2, 201, '2024-01-15'),
    (5, 2, 202, '2024-01-03'),
    (6, 3, 301, '2024-01-01'),
    (7, 4, 401, '2024-01-01'),
    (8, 4, 401, '2024-01-15'),
    (9, 4, 402, '2024-01-02'),
    (10, 4, 402, '2024-01-16'),
    (11, 5, 501, '2024-01-05'),
    (12, 5, 502, '2024-01-10');
```

### Expected Output
single_post_users | multiple_post_users | 
-- | -- |
3 |  2

### My solution

```sql
with cte as(
select 
  user_id,
  count(distinct job_id) as distinct_ads,
  count(*) as total_postings
from 
  job_postings
group by 1
)
select 
  count(case when distinct_ads = total_postings then user_id end) as single_post_users,
  count(case when distinct_ads < total_postings then user_id end) as multiple_post_users 
from 
  cte;
```

