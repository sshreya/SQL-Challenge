Question Link : https://www.namastesql.com/coding-problem/71-department-average-salary?question_type=0&page=1&pageSize=10

Solution

```sql
select 
	d.department_name,
    round(avg(salary),2) as average_salary
from 
	Employees e 
join 
	Departments d 
on e.department_id = d.department_id
group by department_name
having count(employee_id) > 2
order by average_salary desc;
```
