Exercise: -18 (qwrqwr: 2014-07-18) Exercise: -18 (qwrqwr: 2014-07-18)   
The Income and Income_o tables are considered, with records in them corresponding to the same set of buy-back centers.  
Each center is deemed to have been working on all dates occurring in the aforementioned tables, no matter to which centers they are corresponding, that lie within the time period between the first and the last date this center received funds.  
For each center having the highest average cash receipts per working day, determine the date(s) it received the largest sum of money. For these dates, display ALL information present in the tables, namely point, date, inc, code (or NULL if the entry belongs to Income_o).  
使用 Income 和 Income_o 表，
```sql
with total as
(	
  select point, date, inc, code from Income	
  union all	
  select point, date, inc, '' from Income_o	
),
test as
(
  select point, sum(inc) inc from total
  group by point
),
temp as
(
  select point, count(date) cnt from
  (
    select point, min(date) min_d, max(date) max_d from total
    group by point
  ) a
  left join 
  (
    select distinct date from total
  ) b on
  min_d <= date and date <= max_d
  group by point
),
result as
(
  select test.point, inc/cnt avge from test
  left join temp on
  test.point = temp.point
),
plus as
(
  select point, date, inc, max(inc) over(partition by point) max_i from
  (
    select point, date, sum(inc) inc from total
    group by point, date
  ) a
)

Select point, date, inc, case when code = '' then null else code end from total
where date in
(
  select date from plus
  where inc = max_i and point in (select point from result where avge = (select max(avge) from result))
)

```
