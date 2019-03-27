Convert函数的应用: 

 
```sql
Select CONVERT(varchar(100), GETDATE(), 0): 05 16 2006 10:57AM

Select CONVERT(varchar(100), GETDATE(), 1): 05/16/06

Select CONVERT(varchar(100), GETDATE(), 2): 06.05.16

Select CONVERT(varchar(100), GETDATE(), 3): 16/05/06

Select CONVERT(varchar(100), GETDATE(), 4): 16.05.06

Select CONVERT(varchar(100), GETDATE(), 5): 16-05-06

Select CONVERT(varchar(100), GETDATE(), 6): 16 05 06

Select CONVERT(varchar(100), GETDATE(), 7): 05 16, 06

Select CONVERT(varchar(100), GETDATE(), 8): 10:57:46

Select CONVERT(varchar(100), GETDATE(), 9): 05 16 2006 10:57:46:827AM

Select CONVERT(varchar(100), GETDATE(), 10): 05-16-06

Select CONVERT(varchar(100), GETDATE(), 11): 06/05/16

Select CONVERT(varchar(100), GETDATE(), 12): 060516

Select CONVERT(varchar(100), GETDATE(), 13): 16 05 2006 10:57:46:937

Select CONVERT(varchar(100), GETDATE(), 14): 10:57:46:967

Select CONVERT(varchar(100), GETDATE(), 20): 2006-05-16 10:57:47

Select CONVERT(varchar(100), GETDATE(), 21): 2006-05-16 10:57:47.157

Select CONVERT(varchar(100), GETDATE(), 22): 05/16/06 10:57:47 AM

Select CONVERT(varchar(100), GETDATE(), 23): 2006-05-16

Select CONVERT(varchar(100), GETDATE(), 24): 10:57:47

Select CONVERT(varchar(100), GETDATE(), 25): 2006-05-16 10:57:47.250

Select CONVERT(varchar(100), GETDATE(), 100): 05 16 2006 10:57AM

Select CONVERT(varchar(100), GETDATE(), 101): 05/16/2006

Select CONVERT(varchar(100), GETDATE(), 102): 2006.05.16

Select CONVERT(varchar(100), GETDATE(), 103): 16/05/2006

Select CONVERT(varchar(100), GETDATE(), 104): 16.05.2006

Select CONVERT(varchar(100), GETDATE(), 105): 16-05-2006

Select CONVERT(varchar(100), GETDATE(), 106): 16 05 2006

Select CONVERT(varchar(100), GETDATE(), 107): 05 16, 2006

Select CONVERT(varchar(100), GETDATE(), 108): 10:57:49

Select CONVERT(varchar(100), GETDATE(), 109): 05 16 2006 10:57:49:437AM

Select CONVERT(varchar(100), GETDATE(), 110): 05-16-2006

Select CONVERT(varchar(100), GETDATE(), 111): 2006/05/16

Select CONVERT(varchar(100), GETDATE(), 112): 20060516

Select CONVERT(varchar(100), GETDATE(), 113): 16 05 2006 10:57:49:513

Select CONVERT(varchar(100), GETDATE(), 114): 10:57:49:547

Select CONVERT(varchar(100), GETDATE(), 120): 2006-05-16 10:57:49

Select CONVERT(varchar(100), GETDATE(), 121): 2006-05-16 10:57:49.700

Select CONVERT(varchar(100), GETDATE(), 126): 2006-05-16T10:57:49.827

Select CONVERT(varchar(100), GETDATE(), 130): 18 ???? ?????? 1427 10:57:49:907AM

Select CONVERT(varchar(100), GETDATE(), 131): 18/04/1427 10:57:49:920AM
```
查询日期（第一天/最后一天）

```sql
1.一个月第一天的
Select DATEADD(mm, DATEDIFF(mm,0,getdate()), 0)
2.本周的星期一
Select DATEADD(wk, DATEDIFF(wk,0,getdate()), 0)
3.一年的第一天
Select DATEADD(yy, DATEDIFF(yy,0,getdate()), 0)
4.季度的第一天
Select DATEADD(qq, DATEDIFF(qq,0,getdate()), 0)
5.当天的半夜
Select DATEADD(dd, DATEDIFF(dd,0,getdate()), 0)
6.上个月的最后一天
Select dateadd(ms,-3,DATEADD(mm, DATEDIFF(mm,0,getdate()), 0))
7.去年的最后一天
Select dateadd(ms,-3,DATEADD(yy, DATEDIFF(yy,0,getdate()), 0))
8.本月的最后一天
Select dateadd(ms,-3,DATEADD(mm, DATEDIFF(m,0,getdate())+1, 0))
9.本年的最后一天
Select dateadd(ms,-3,DATEADD(yy, DATEDIFF(yy,0,getdate())+1, 0))
10.本月的第一个星期一
select DATEADD(wk, DATEDIFF(wk,0,dateadd(dd,6-datepart(day,getdate()),getdate())), 0)

年 yy, yyyy
季度 qq, q
月 mm, m
年中的日 dy, y
日 dd, d
周 wk, ww
星期 dw, w
小时 hh
分钟 mi, n
秒 ss, s
毫秒 ms
微妙 mcs
纳秒 ns
```
