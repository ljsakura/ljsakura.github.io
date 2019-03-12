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
For the income and expenditure may occur more than once a day, another database schema with tables having a primary key consisting of the single column code is used:  
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
where model not like '%[^A-Za-z]%' or model not like '%[^0-9]%' 
--补集的补集，字符串中包含非A-Za-z（或是0-9）的集合，再对其取补集便是字符串中只包含A-Za-z（或是0-9）的集合。
--like '%[^0-9]%' 与 not like '%[0-9]%' 的结果也是不同的，
--前者返回全集-纯数字字符串的集合，则结果为其他字符的集合+其他字符混合数字字符的集合；
--后者返回全集-所有含数字字符串的集合，则结果为仅有其他字符的集合。
```
Exercise: 36 (Serge I: 2003-02-17)  
List the names of lead ships in the database (including the Outcomes table).
```sql
Select name from ships
inner join classes on
ships.name = classes.class
union
select ship from outcomes
inner join classes on
outcomes.ship = classes.class
```
Exercise: 37 (Serge I: 2003-02-17)  
Find classes for which only one ship exists in the database (including the Outcomes table).
```sql
Select class from
(
select name, classes.class from ships
inner join classes on
ships.class = classes.class
union
select ship, class from outcomes
inner join classes on
outcomes.ship = classes.class
) a
group by class
having count(*) = 1
```
Exercise: 38 (Serge I: 2003-02-19)  
Find countries that ever had classes of both battleships (‘bb’) and cruisers (‘bc’).
```sql
Select country from classes
where type = 'bb'
intersect 
Select country from classes
where type = 'bc'
```
Exercise: 39 (Serge I: 2003-02-14)  
Find the ships that `survived for future battles`; that is, after being damaged in a battle, they participated in another one, which occurred later.
```sql
Select distinct a.ship from
(
select ship, battle, date from outcomes
inner join battles on
outcomes.battle = battles.name
where result = 'damaged'
) a
inner join
(
select ship, battle, date from outcomes
inner join battles on
outcomes.battle = battles.name
) b on
a.ship = b.ship and 
a.date< b.date
```
Exercise: 40 (Serge I: 2002-11-05)
For the ships in the Ships table that have at least 10 guns, get the class, name, and country.
```sql
Select ships.class, name, country from ships
inner join classes on
ships.class = classes.class
where numguns >= 10
```
Exercise: 41 (Serge I: 2008-08-30)  
For the PC in the PC table with the maximum code value, obtain all its characteristics (except for the code) and display them in two columns:
- name of the characteristic (title of the corresponding column in the PC table);
```sql
Select characteristic, value from 
(
  select 
  isnull (cast(model as varchar(255)),'') model, --如果不使用 isnull 则第二个测试数据库就会报错，因为存在空值
  isnull (cast(speed as varchar(255)),'') speed, 
  isnull (cast(ram as varchar(255)),'') ram, 
  isnull (cast(hd as varchar(255)),'') hd, 
  isnull (cast(cd as varchar(255)),'') cd, 
  isnull (cast(price as varchar(255)),'') price  
  from pc
  where code = (select max(code) from pc)
) a
unpivot 
(
  value for characteristic in
  (
    model, speed, ram, hd, cd, price
  )
) p
```
pivot 与 unpivot 的使用详见 [MSDN](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms177410(v=sql.105))

Exercise: 42 (Serge I: 2002-11-05)  
Find the names of ships sunk at battles, along with the names of the corresponding battles.
```sql
Select distinct ship, battle from outcomes
where result = 'sunk'
```
Exercise: 43 (qwrqwr: 2011-10-28)  
Get the battles that occurred in years when no ships were launched into water.
```sql
Select distinct battles.name from battles
where datepart(yyyy, date) not in 
(
  select distinct launched from ships
  where launched is not null
)
```
Exercise: 44 (Serge I: 2002-12-04)  
Find all ship names beginning with the letter R.
```sql
Select name from ships
where name like 'r%'
union
select ship from outcomes
where ship like 'r%'
```
Exercise: 45 (Serge I: 2002-12-04)  
Find all ship names consisting of three or more words (e.g., King George V).
Consider the words in ship names to be separated by single spaces, and the ship names to have no leading or trailing spaces.
```sql
Select name from ships
where trim(name) like '% % %' -- trim 去掉左右空格
union
Select ship from outcomes
where trim(ship) like '% % %'
```
Exercise: 46 (Serge I: 2003-02-14)  
For each ship that participated in the Battle of Guadalcanal, get its name, displacement, and the number of guns.
```sql
Select ship, displacement, numGuns from outcomes a
left join ships b on
a.ship = b.name
left join classes c on
b.class = c.class or
a.ship = c.class -- A ship engaged in this battle should be listed even if its class is not known.
where battle = 'guadalcanal'
```
Exercise: 47 (Serge I: 2011-02-11)  
Number the rows of the Product table as follows: makers in descending order of number of models produced by them (for manufacturers producing an equal number of models, their names are sorted in ascending alphabetical order); model numbers in ascending order.
Result set: row number as described above, manufacturer's name (maker), model.
```sql
Select row_number () over (order by num desc, a.maker, model) no, a.maker, a.model
from product a
left join 
(
  select count(distinct model) as num, maker from product
  group by maker
) b on
a.maker = b.maker
```
Exercise: 48 (Serge I: 2003-02-16)  
Find the ship classes having at least one ship sunk in battles.
```sql
Select distinct class from outcomes a
inner join ships b on
a.ship = b.name
where result = 'sunk'
union
Select distinct class from outcomes a
inner join classes b on
a.ship = b.class
where result = 'sunk'
```
Exercise: 49 (Serge I: 2003-02-17)  
Find the names of the ships having a gun caliber of 16 inches (including ships in the Outcomes table).
```sql
Select name from ships a
inner join classes b on
a.class = b.class
where bore = 16
union
Select ship from outcomes a
inner join classes b on
a.ship = b.class
where bore = 16
```
Exercise: 50 (Serge I: 2002-11-05)  
Find the battles in which Kongo-class ships from the Ships table were engaged.
```sql
Select distinct battle from ships a
inner join outcomes b on
a.name = b.ship
where  a.class = 'kongo' 
```
Exercise: 51 (Serge I: 2003-02-17)  
Find the names of the ships with the largest number of guns among all ships having the same displacement (including ships in the Outcomes table).
```sql
Select name from classes a
inner join
(
  select name, class from ships
  union
  select ship as name, ship as class from outcomes
) b on
a.class = b.class
inner join
(
  select max(numGuns) mn, displacement from classes a
  inner join
  (
    select name, class from ships
    union
    select ship as name, ship as class from outcomes
  ) b on
  a.class = b.class
  group by displacement
) c on
a.numguns = c.mn and a.displacement = c.displacement
```
Exercise: 52 (qwrqwr: 2010-04-23)  
Determine the names of all ships in the Ships table that can be a Japanese battleship having at least nine main guns with a caliber of less than 19 inches and a displacement of not more than 65 000 tons.
```sql
Select name from ships a
left join classes b on
a.class = b.class
where (country = 'japan' or country is null)
and (numGuns >= 9 or numGuns is null)
and (bore < 19 or bore is null)
and (displacement <= 65000 or displacement is null)
and (type = 'bb' or type is null)
```
Exercise: 53 (Serge I: 2002-11-05)  
With a precision of two decimal places, determine the average number of guns for the battleship classes.
```sql
Select cast(avg(numGuns*1.0) as decimal(10,2)) from classes
where type = 'bb' 
```
Exercise: 54 (Serge I: 2003-02-14)  
With a precision of two decimal places, determine the average number of guns for all battleships (including the ones in the Outcomes table).
```sql
Select cast(avg(numGuns*1.0) as decimal(10,2)) from 
(
select b.name, b.class, a.numguns from classes a
inner join ships b on
a.class = b.class
where type = 'bb'
union
select b.ship, b.ship, a.numguns from classes a
inner join outcomes b on
a.class = b.ship
where type = 'bb' 
) x
```
Exercise: 55 (Serge I: 2003-02-16)  
For each class, determine the year the first ship of this class was launched. 
If the lead ship’s year of launch is not known, get the minimum year of launch for the ships of this class.
Result set: class, year.
```sql
Select classes.class, min(launched) from classes
left join ships on
ships.class = classes.class
group by classes.class
```
Exercise: 56 (Serge I: 2003-02-16)  
For each class, find out the number of ships of this class that were sunk in battles. 
Result set: class, number of ships sunk.
```sql
Select class, count(name) from
(
  select name, a.class from classes a
  left join ships b on
  a.class = b.class
  where name in
    (
      select ship from outcomes where result = 'sunk'
    )
  union
  select ship, a.class from classes a
  left join 
  (
    select * from outcomes where result = 'sunk'
  ) b on
  a.class = b.ship
) x
group by class
---------- code below gets wrong result on second database
Select class, count(result) from
(
  select result, a.class from classes a
  left join
  (
    select name, class from ships
    union
    select ship, ship from outcomes
    where ship not in(select name from ships)
  )b on
  a.class = b.class
  left join 
  (
    select * from outcomes where result = 'sunk'
  ) c on
  a.class = c.ship or b.name = c.ship
) x
group by class
```
Exercise: 57 (Serge I: 2003-02-14)  
For classes having irreparable combat losses and at least three ships in the database, display the name of the class and the number of ships sunk.
```sql
Select class, count(result) from
(
  select c.name, a.class, b.name as result from classes a
  left join ships c on
  a.class = c.class 
  left join
  (
    select * from ships where name in(select ship from outcomes where result = 'sunk')
  ) b on
  c.name = b.name and 
  c.class = b.class
  union
  select c.ship, a.class, b.ship as result from classes a
  left join 
  (
    select * from outcomes where ship not in(select name from ships)
  ) c on
  c.ship = a.class 
  left join 
  (
    select * from outcomes where result = 'sunk' and ship not in(select name from ships)
  ) b on
  b.ship = c.ship
where c.ship not in (select name from ships)
) x
group by class
having count(name) > 2 and count(result) > 0
----------code below gets wrong result on second database
select class, count(result) from
(
  select result, name, a.class from classes a
  left join
  (
    select name, class from ships
    union
    select ship, ship from outcomes
    where ship not in(select name from ships)
  )b on
  a.class = b.class
  left join 
  (
    select * from outcomes where result = 'sunk'
  ) c on
  a.class = c.ship or b.name = c.ship
) x
group by class
having count(name) > 2 and count(result) > 0
```
Exercise: 58 (Serge I: 2009-11-13)  
For each product type and maker in the Product table, find out, with a precision of two decimal places, the percentage ratio of the number of models of the actual type produced by the actual maker to the total number of models by this maker.
Result set: maker, product type, the percentage ratio mentioned above.
```sql
Select x.*, cast( (isnull(ty_model, 0)*100.00/to_model) as decimal(10,2)) from 
-- 需要 isnull 将ty_model为空部分转化为零； *100.00 或者 *100.0 转化小数位
(
  select a.maker, b.type from 
  (
    select distinct maker from product
  ) a
  cross join 
  (
    select distinct type from product
  ) b
 ) x
 left join
 (
  select maker, count(distinct model) as to_model from product
  group by maker
 ) y on x.maker = y.maker
left join
(
  select maker, type, count(distinct model) as ty_model from product
  group by maker, type
) z on x.maker = z.maker and x.type = z.type
```
Exercise: 59 (Serge I: 2003-02-15)  
Calculate the cash balance of each buy-back center for the database with money transactions being recorded not more than once a day.
Result set: point, balance.
```sql
Select  
  case
    when a.point is null 
    then b.point 
    else a.point
  end point,
isnull(inc,0) - isnull(out,0)
from
(
  select point, sum(inc) as inc from income_o
  group by point
) a
full join 
(
  select point, sum(out) as out from outcome_o
  group by point
) b
on a.point = b.point 
```
Exercise: 60 (Serge I: 2003-02-15)  
For the database with money transactions being recorded not more than once a day, calculate the cash balance of each buy-back center at the beginning of 4/15/2001. 
Note: exclude centers not having any records before the specified date.
Result set: point, balance
```sql
Select  
  case
    when a.point is null 
    then b.point 
    else a.point
  end point,
isnull(inc,0) - isnull(out,0)
from
(
  select point, sum(inc) as inc from income_o
  where date < '2001-4-15' --截止 2001-4-15的收入
  group by point
) a
full join 
(
  select point, sum(out) as out from outcome_o
  where date < '2001-4-15' --截止 2001-4-15的支出
  group by point
) b
on a.point = b.point 
```
Exercise: 61 (Serge I: 2003-02-14)  
For the database with money transactions being recorded not more than once a day, calculate the total cash balance of all buy-back centers.
```sql
Select 
  case
    when (select count(*) from income_o) = 0
    then (select sum(out) from outcome_o)
    when (select count(*) from outcome_o) = 0
    then (select sum(inc) from income_o)
    else
    (select sum(inc) from income_o) - (select sum(out) from outcome_o)
  end -- 网上有人在这里加了 from income_o 结果就是出现多行，其实不用加的，加了反而会以 income_o 相同的行数显示计算结果
```
Exercise: 62 (Serge I: 2003-02-15)  
For the database with money transactions being recorded not more than once a day, calculate the total cash balance of all buy-back centers at the beginning of 04/15/2001.
```sql
Select 
  case
    when (select count(*) from income_o) = 0
    then (select sum(out) from outcome_o where date < '2001-4-15')
    when (select count(*) from outcome_o) = 0
    then (select sum(inc) from income_o where date < '2001-4-15')
    else
    (select sum(inc) from income_o where date < '2001-4-15') - (select sum(out) from outcome_o where date < '2001-4-15')
  end
```
63 Database 4
Short database description "Airport"  
The database schema consists of 4 tables:  
Company(ID_comp, name)  
Trip(trip_no, id_comp, plane, town_from, town_to, time_out, time_in)  
Passenger(ID_psg, name)  
Pass_in_trip(trip_no, date, ID_psg, place)  
The Company table contains IDs and names of the airlines transporting passengers. The Trip table contains information on the schedule of flights: trip (flight) number, company (airline) ID, plane type, departure city, destination city, departure time, and arrival time. The Passenger table holds IDs and names of the passengers. The Pass_in_trip table contains data on flight bookings: trip number, departure date (day), passenger ID and her seat (place) designation during the flight. It should be noted that  
- scheduled flights are operated daily; the duration of any flight is less than 24 hours; town_from <> town_to;  
- all time and date values are assumed to belong to the same time zone;  
- departure and arrival times are specified with one minute precision;  
- there can be several passengers bearing the same first name and surname (for example, Bruce Willis);  
- the seat (place) designation consists of a number followed by a letter; the number stands for the row, while the letter (a – d) defines the seat position in the grid (from left to right, in alphabetical order;  
- connections and constraints are shown in the database schema below.  

Exercise: 63 (Serge I: 2003-04-08)  
Find the names of different passengers that ever travelled more than once occupying seats with the same number.
```sql
Select name from passenger
where id_psg in
(
  select id_psg from pass_in_trip
  group by id_psg, place
  having count(*) >=2
)
```
Exercise: 64 (Serge I: 2010-06-04)  
Using the Income and Outcome tables, determine for each buy-back center the days when it received funds but made no payments, and vice versa.  
Result set: point, date, type of operation (inc/out), sum of money per day.
```sql
Select 
  case
    when a.point is null
    then b.point
    else a.point
  end point,
  case
    when a.date is null
    then b.date
    else a.date
  end date,
  case
    when a.point is null
    then 'out'
    else 'inc'
  end operation,
  case
    when inc is null
    then out 
    else inc
  end monty
from 
(
  select point, date, sum(inc) inc from income
  group by point, date
) a
full join 
(
  select point, date, sum(out) out from outcome
  group by point, date
) b on
a.point = b.point and a.date = b.date
where a.point is null or b.point is null
```
Exercise: 65 (Serge I: 2009-08-24)  
Number the unique pairs {maker, type} in the Product table, ordering them as follows:  
- maker name in ascending order;  
- type of product (type) in the order PC, Laptop, Printer.  
If a manufacturer produces more than one type of product, its name should be displayed in the first row only;  
other rows for THIS manufacturer should contain an empty string (').  
```sql
Select row_number() over(order by maker, 
  case
    when type = 'pc' then 1
    when type ='laptop' then 2
    else 3
  end
), -- 按 PC, Laptop, Printer 进行排序，使用一次case
  case 
    when  row_number() over(partition by maker order by maker, 
    case
      when type = 'pc' then 1
      when type ='laptop' then 2
      else 3
    end
    ) = 1 then maker
    else ''
  end maker, -- 相同数值只在第一行显示，这次需要用 case 中嵌套 partition by 
type from product
group by maker, type
```
Exercise: 66 (Serge I: 2003-04-09)  
For all days between 2003-04-01 and 2003-04-07 find the number of trips from Rostov. 
Result set: date, number of trips.
```sql
Select a, 
  case
    when no is not null then no
    else 0
  end no 
from
(
  select '2003-04-01 00:00:00.000' a union all select '2003-04-02 00:00:00.000' union all select '2003-04-03 00:00:00.000' union all select '2003-04-04 00:00:00.000' union all select '2003-04-05 00:00:00.000' union all select '2003-04-06 00:00:00.000' union all select '2003-04-07 00:00:00.000' 
) x
left join 
(
  select count(distinct trip_no) as no, date from pass_in_trip
  where trip_no in(select trip_no from trip where town_from = 'rostov')
  group by date
) y on
x.a = y.date
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

