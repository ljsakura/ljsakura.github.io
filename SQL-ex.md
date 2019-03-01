1-28 Database 1  
Short database description "Computer firm"  
The database scheme consists of four tables:  
Product(maker, model, type)  
PC(code, model, speed, ram, hd, cd, price)  
Laptop(code, model, speed, ram, hd, screen, price)  
Printer(code, model, color, type, price)  
The Product table contains data on the maker, model number, and type of product ('PC', 'Laptop', or 'Printer'). It is assumed that model numbers in the Product table are unique for all makers and product types.   
Each personal computer in the PC table is unambiguously identified by a unique code, and is additionally characterized by its model (foreign key referring to the Product table), processor speed (in MHz) – speed field, RAM capacity (in Mb) - ram, hard disk drive capacity (in Gb) – hd, CD-ROM speed (e.g, '4x') - cd, and its price.   
The Laptop table is similar to the PC table, except that instead of the CD-ROM speed, it contains the screen size (in inches) – screen. 
For each printer model in the Printer table, its output type (‘y’ for color and ‘n’ for monochrome) – color field, printing technology ('Laser', 'Jet', or 'Matrix') – type, and price are specified.   

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
Exercise: 27 (Serge I: 2003-02-03)  
Find out the average hard disk drive capacity of PCs produced by makers who also manufacture printers.
Result set: maker, average HDD capacity.
```sql
Select maker, avg(hd) from pc
inner join product on
pc.model = product.model
and maker in
(
  select maker from product where type = 'printer'
)
group by maker
```
Exercise: 28 (Serge I: 2012-05-04)  
Using Product table, find out the number of makers who produce only one model.
```sql
Select count(*) from product
where maker in
(
  select maker from product
  group by maker
  having count(distinct model) = 1
)
```

29-30 Database 2
Short database description "Recycling firm"  
The firm owns several buy-back centers for collection of recyclable materials. Each of them receives funds to be paid to the recyclables suppliers. Data on funds received is recorded in the table   
Income_o(point, date, inc)  
The primary key is (point, date), where point holds the identifier of the buy-back center, and date corresponds to the calendar date the funds were received. The date column doesn’t include the time part, thus, money (inc) arrives no more than once a day for each center. Information on payments to the recyclables suppliers is held in the table  
Outcome_o(point, date, out)  
In this table, the primary key (point, date) ensures each buy-back center reports about payments (out) no more than once a day, too. 
For the case income and expenditure may occur more than once a day, another database schema with tables having a primary key consisting of the single column code is used:  
Income(code, point, date, inc)  
Outcome(code, point, date, out)  
Here, the date column doesn’t include the time part, either.   

Exercise: 29 (Serge I: 2003-02-14)   
Under the assumption that receipts of money (inc) and payouts (out) are registered not more than once a day for each collection point {i.e. the primary key consists of (point, date)}, write a query displaying cash flow data (point, date, income, expense). 
Use Income_o and Outcome_o tables.
```sql
Select a.point, a.date, inc, out from income_o a
left join outcome_o b on
a.date = b.date and
a.point = b.point
union --full outer join can not return all point and date data
Select a.point, a.date, inc, out from outcome_o a
left join income_o b on
a.date = b.date and
a.point = b.point
```
Exercise: 30 (Serge I: 2003-02-14)  
Under the assumption that receipts of money (inc) and payouts (out) can be registered any number of times a day for each collection point (i.e. the code column is the primary key), display a table with one corresponding row for each operating date of each collection point.
Result set: point, date, total payout per day (out), total money intake per day (inc). 
Missing values are considered to be NULL.
```sql
Select a.point, a.date, out, inc from 
(
  select point, date, sum(out) out from outcome
  group by point, date
) a
left join
(
  select point, date, sum(inc) inc from income
  group by point, date
) b on
a.point = b.point and a.date = b.date
union
Select c.point, c.date, out, inc from 
(
  select point, date, sum(inc) inc from income
  group by point, date
) c
left join
(
  select point, date, sum(out) out from outcome
  group by point, date
) d on
c.point = d.point and c.date = d.date
```
31-34 Database 3  
Short database description "Ships"  
The database of naval ships that took part in World War II is under consideration. The database consists of the following relations:   
Classes(class, type, country, numGuns, bore, displacement)   
Ships(name, class, launched)   
Battles(name, date)   
Outcomes(ship, battle, result)   
Ships in classes all have the same general design. A class is normally assigned either the name of the first ship built according to the corresponding design, or a name that is different from any ship name in the database. The ship whose name is assigned to a class is called a lead ship.  
The Classes relation includes the name of the class, type (can be either bb for a battle ship, or bc for a battle cruiser), country the ship was built in, the number of main guns, gun caliber (bore diameter in inches), and displacement (weight in tons). The Ships relation holds information about the ship name, the name of its corresponding class, and the year the ship was launched. The Battles relation contains names and dates of battles the ships participated in, and the Outcomes relation - the battle result for a given ship (may be sunk, damaged, or OK, the last value meaning the ship survived the battle unharmed).   
Notes: 1) The Outcomes relation may contain ships not present in the Ships relation. 2) A ship sunk can’t participate in later battles. 3) For historical reasons, lead ships are referred to as head ships in many exercises.4) A ship found in the Outcomes table but not in the Ships table is still considered in the database. This is true even if it is sunk.   

Exercise: 31 (Serge I: 2002-10-22)  
For ship classes with a gun caliber of 16 in. or more, display the class and the country.
```sql
Select class, country from classes
where bore >= 16
```
Exercise: 32 (Serge I: 2003-02-17)  
One of the characteristics of a ship is one-half the cube of the calibre of its main guns (mw). 
Determine the average ship mw with an accuracy of two decimal places for each country having ships in the database.
--需返回ships中的船只name，且也要返回outcomes中的ship，最终将两者去重合并
```sql
select country, convert(numeric(10, 2), avg(power(bore, 3)/2)) weight 
from
(
  select country, bore, name from classes
  inner join ships on
  classes.class = ships.class
  union
  select country, bore, ship from classes
  inner join outcomes on
  classes.class = outcomes.ship
) a
group by country
```
Exercise: 33 (Serge I: 2002-11-02)  
Get the ships sunk in the North Atlantic battle. 
Result set: ship.
```sql
Select ship from outcomes 
where battle = 'North Atlantic'
and result = 'sunk'
```
Exercise: 34 (Serge I: 2002-11-04)  
In accordance with the Washington Naval Treaty concluded in the beginning of 1922, it was prohibited to build battle ships with a displacement of more than 35 thousand tons. 
Get the ships violating this treaty (only consider ships for which the year of launch is known). 
List the names of the ships.
```sql
Select distinct name from classes, ships
where classes.class = ships.class
and type = 'bb'
and launched >= 1922
and displacement >35000
```
Exercise: 35 (qwrqwr: 2012-11-23)  
Find models in the Product table consisting either of digits only or Latin letters (A-Z, case insensitive) only.
Result set: model, type.
```sql
Select model, type from product
where model not like '%[^A-Za-z]%' or model not like '%[^0-9]%' --补集的补集，字符串中包含非A-Za-z（或是0-9）的集合，再对其取补集便是字符串中只包含A-Za-z（或是0-9）的集合。like '%[^0-9]%' 与 not like '%[0-9]%' 的结果也是不同的，前者返回全集-纯数字字符串的集合，则结果为其他字符的集合+其他字符混合数字字符的集合；后者返回全集-所有含数字字符串的集合，则结果为仅有其他字符的集合。
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
