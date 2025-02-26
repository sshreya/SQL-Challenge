Question Link - https://www.namastesql.com/coding-problem/8-library-borrowing-habits

Solution

``` sql
select
  bw.BorrowerName,
  group_concat(bo.BookName order by bo.BookName) as BorrowedBooks
from 
	Borrowers bw 
join 
	Books bo 
on bw.BookID = bo.BookID
group by 1
order by 1;
```
