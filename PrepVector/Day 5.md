## Post Completion Rate Analysis

### Problem Statement
Consider the events table, which contains information about the phases of writing a new social media post.
The action column can have values post_enter, post_submit, or post_canceled for when a user starts to write (post_enter), ends up canceling their post (post_cancel), or posts it (post_submit). Write a query to get the post-success rate for each day in the month of January 2020.
Note: Post Success Rate is defined as the number of posts submitted (post_submit) divided by the number of posts entered (post_enter) for each day.

### Schema Setup

```sql
CREATE TABLE events (
user_id INT,
created_at DATETIME,
action VARCHAR(20)
);

INSERT INTO events VALUES
(1, '2020-01-01 10:00:00', 'post_enter'),
(1, '2020-01-01 10:05:00', 'post_submit'),
(2, '2020-01-01 11:00:00', 'post_enter'),
(2, '2020-01-01 11:10:00', 'post_canceled'),
(3, '2020-01-01 15:00:00', 'post_enter'),
(3, '2020-01-01 15:30:00', 'post_submit'),
(4, '2020-01-02 09:00:00', 'post_enter'),
(4, '2020-01-02 09:15:00', 'post_canceled'),
(5, '2020-01-02 10:00:00', 'post_enter'),
(5, '2020-01-02 10:10:00', 'post_canceled'),
(10, '2020-01-15 14:00:00', 'post_enter'),
(10, '2020-01-15 14:30:00', 'post_submit'),
(6, '2019-12-31 23:55:00', 'post_enter'),
(6, '2020-01-01 00:05:00', 'post_submit'),
(7, '2020-02-01 00:00:00', 'post_enter'),
(7, '2020-02-01 00:10:00', 'post_submit'),
(8, '2019-01-15 10:00:00', 'post_enter'),
(8, '2019-01-15 10:30:00', 'post_submit'),
(9, '2021-01-01 09:00:00', 'post_enter'),
(9, '2021-01-01 09:10:00', 'post_canceled');
```

### Expected Output
date |  total_enters  | total_submits  |  success_rate
-- | -- | -- | -- |
2020-01-01 | 3 | 2 | 0.67
2020-01-02 | 2 | 0 | 0.00
2020-01-15 | 1 | 1 | 1.00

### Solution

My first query does not ensure that post_submit correspond to the post_enter of the same day. Hence we are getting 3 post enters and 3 post submits for 2020-01-01 which is incorrect. User Id 6 enters on 
'2019-12-31 23:55:00' and submits on '2020-01-01 00:05:00'.

```sql
  SELECT 
      DATE(created_at) AS post_date,
      count(CASE WHEN action = 'post_enter' THEN 1 END) AS total_enters,
  count(CASE WHEN action = 'post_submit' THEN 1 END) AS total_submits,
      1.0 * SUM(CASE WHEN action = 'post_submit' THEN 1 ELSE 0 END)/ SUM(CASE WHEN action = 'post_enter' THEN 1 ELSE 0 END) as success_rate
  FROM pre_events
  WHERE 
      DATE(created_at) BETWEEN '2020-01-01' AND '2020-01-31'
      AND action IN ('post_enter', 'post_submit')
  GROUP BY DATE(created_at);
```

This second solution makes sure that this edge case is taken care of and post_submit and post_submit should refer to the same post for a day

```sql
WITH enters as (
    SELECT user_id, DATE(created_at) as enter_date
    FROM   pre_events
    WHERE  action = 'post_enter'
)
,submits as (
    SELECT user_id, DATE(created_at) as submit_date
    FROM   pre_events
    WHERE  action = 'post_submit'
)
SELECT 
enter_date,
count(e.user_id) as total_enters,
count(s.user_id) as total_submits,
round(1.0 * count(s.user_id)/count(e.user_id),2) as success_rate
FROM     enters e LEFT JOIN submits s ON e.user_id = s.user_id and e.enter_date = s.submit_date
where enter_date between '2020-01-01' and '2020-01-31'
group by enter_date;
```



