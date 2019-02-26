Exercise: 1 (Serge I: 2002-09-30)  
Find the model number, speed and hard drive capacity for all the PCs with prices below $500.
Result set: model, speed, hd.
```sql
Select model, speed, hd from PC
where price < 500
```
Exercise: 2 (Serge I: 2002-09-21)  
List all printer makers. Result set: maker.
```sql
Select distinct maker from product
where type = 'printer'
```
Exercise: 3 (Serge I: 2002-09-30)  
Find the model number, RAM and screen size of the laptops with prices over $1000.
```sql
Select model, ram, screen from laptop
where price > 1000
```
Exercise: 4 (Serge I: 2002-09-21)  
Find all records from the Printer table containing data about color printers.
```sql
Select * from printer
where color = 'y'
```
Exercise: 5 (Serge I: 2002-09-30)  
Find the model number, speed and hard drive capacity of PCs cheaper than $600 having a 12x or a 24x CD drive.
```sql
Select model, speed, hd from pc
where price < 600 and cd in ('12x','24x')
```
Exercise: 6 (Serge I: 2002-10-28)  
For each maker producing laptops with a hard drive capacity of 10 Gb or higher, find the speed of such laptops. Result set: maker, speed.
```sql
Select distinct maker, speed from product a
left join laptop b on
a.model = b.model
where hd >= 10 and type = 'laptop'
```
Exercise: 7 (Serge I: 2002-11-02)  
Get the models and prices for all commercially available products (of any type) produced by maker B.
```sql
Select a.model, price  from product a
inner join 
(
select model, price from pc
union
select model, price from laptop
union
select model, price from printer
) b on
a.model = b.model
where maker = 'b'
```
Exercise: 8 (Serge I: 2003-02-03)  
Find the makers producing PCs but not laptops.
```sql
Select distinct maker from product
where type = 'pc' and maker not in 
(
select distinct maker from product where type = 'laptop'
)
```
Exercise: 9 (Serge I: 2002-11-02)  
Find the makers of PCs with a processor speed of 450 MHz or more. Result set: maker.
```sql
Select distinct maker from product
left join pc on
product.model = pc.model
where speed >= 450
```
Exercise: 10 (Serge I: 2002-09-23)  
Find the printer models having the highest price. Result set: model, price.
```sql
Select model,  price from 
printer
where price =
(
select max(price) from printer
)
```
Exercise: 11 (Serge I: 2002-11-02)  
Find out the average speed of PCs.
```sql
Select avg(speed)  speed from pc
```
Exercise: 12 (Serge I: 2002-11-02)  
Find out the average speed of the laptops priced over $1000.
```sql
Select avg(speed) speed from laptop
where price > 1000
```
Exercise: 13 (Serge I: 2002-11-02)  
Find out the average speed of the PCs produced by maker A.
```sql
Select avg(speed) speed from pc
left join product on
pc.model = product.model
where maker = 'a'
```
Exercise: 14 (Serge I: 2012-04-20)  
Get the makers who produce only one product type and more than one model. Output: maker, type.
```sql
Select distinct maker, type from product
where maker in
(
select maker from product
group by maker
having count(distinct type) =1
and count(distinct model) >1
)
```
Exercise: 15 (Serge I: 2003-02-03)  
Get hard drive capacities that are identical for two or more PCs. 
```sql
Select hd from pc
group by hd
having count(*)>1
```
Exercise: 16 (Serge I: 2003-02-03)  
Get pairs of PC models with identical speeds and the same RAM capacity. Each resulting pair should be displayed only once, i.e. (i, j) but not (j, i). 
Result set: model with the bigger number, model with the smaller number, speed, and RAM.
```sql
Select distinct a.model,  b.model , a.speed, a.ram 
from pc a inner join pc b on
a.speed=b.speed and 
a.ram = b.ram and
a.model >b.model
```
Exercise: 17 (Serge I: 2003-02-03)  
Get the laptop models that have a speed smaller than the speed of any PC. 
Result set: type, model, speed.
```sql
select distinct c.type, a.model, a.speed from laptop a
inner join product c on
a.model = c.model
where a.speed <
(
select min(speed) from pc
)
```
Exercise: 18 (Serge I: 2003-02-03)  
Find the makers of the cheapest color printers.
Result set: maker, price.
```sql
Select distinct maker, price from product
inner join printer
on printer.model = product.model
where price = 
(
select min(price) from printer
where color = 'y'
)
and color = 'y'
```
Exercise: 19 (Serge I: 2003-02-13)  
For each maker having models in the Laptop table, find out the average screen size of the laptops he produces. 
Result set: maker, average screen size.
```sql
Select maker, avg(screen) from laptop a
left join product b on
a.model = b.model
group by maker
```
Exercise: 20 (Serge I: 2003-02-13)  
Find the makers producing at least three distinct models of PCs.
Result set: maker, number of PC models.
```sql
Select maker, count(model) from product
where type = 'pc'
group by maker
having count(model)>=3
```
Exercise: 21 (Serge I: 2003-02-13)  
Find out the maximum PC price for each maker having models in the PC table. Result set: maker, maximum price.
```sql
Select maker, max(price) from product a
inner join pc b on
a.model = b.model
group by maker
```
Exercise: 22 (Serge I: 2003-02-13)  
For each value of PC speed that exceeds 600 MHz, find out the average price of PCs with identical speeds.
Result set: speed, average price.
```sql
Select speed, avg(price) from pc
where speed >600
group by speed
```
Exercise: 23 (Serge I: 2003-02-14)  
Get the makers producing both PCs having a speed of 750 MHz or higher and laptops with a speed of 750 MHz or higher. 
Result set: maker
```sql
Select distinct maker from product a
left join pc b on
a.model = b.model
where b.speed >=750 
intersect -- intersect 交集， except 差集
Select distinct maker from product a
left join laptop c on
a.model = c.model
where c.speed >=750
```
Exercise: 24 (Serge I: 2003-02-03)  
List the models of any type having the highest price of all products present in the database.
```sql
Select distinct model from 
(
  select model, price from pc
  union all
  select model, price from laptop
  union all
  select model, price from printer
) a
where price =
(
  select max(price) from 
  (
    select model, price from pc
    union all
    select model, price from laptop
    union all
    select model, price from printer
  ) a
)
```
Exercise: 25 (Serge I: 2003-02-14)  
Find the printer makers also producing PCs with the lowest RAM capacity and the highest processor speed of all PCs having the lowest RAM capacity. --返回生产printer并也能生产pc的厂家，同时要满足pc的ram是最低值，且speed是ram最低的那些pc中最高的
Result set: maker.
```sql
Select maker from product a
where type = 'printer'
intersect
Select maker from product a
inner join pc c on
a.model = c.model
where ram = 
(
  select min(ram) from pc
) 
and speed = 
(
  select max(speed) from pc 
  where ram = 
  (
    select min(ram) from pc
  )
)
```
Exercise: 26 (Serge I: 2003-02-14)  
Find out the average price of PCs and laptops produced by maker A.
Result set: one overall average price for all items.
```sql
Select avg(price) from
(
  select model, price from pc
  union all
  select model, price from laptop
) new
where model in
(
  select model from product where maker = 'a'
)
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
```sql
```
