```
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
```
Exercise: 1 (Serge I: 2002-09-30)  
Find the model number, speed and hard drive capacity for all the PCs with prices below $500.  
Result set: model, speed, hd.  
返回 price 小于500的所有 pc 的 model，speed， hd
```sql
Select model, speed, hd from PC
where price < 500
```
Exercise: 2 (Serge I: 2002-09-21)  
List all printer makers.   
Result set: maker.  
返回所有生产 printer 的 makers
```sql
Select distinct maker from product
where type = 'printer'
```
Exercise: 3 (Serge I: 2002-09-30)  
Find the model number, RAM and screen size of the laptops with prices over $1000.  
返回价格高于1000的 laptop 的 model，ram，screen size
```sql
Select model, ram, screen from laptop
where price > 1000
```
Exercise: 4 (Serge I: 2002-09-21)  
Find all records from the Printer table containing data about color printers.  
从 printer 表中返回所有的彩色打印机
```sql
Select * from printer
where color = 'y'
```
Exercise: 5 (Serge I: 2002-09-30)  
Find the model number, speed and hard drive capacity of PCs cheaper than $600 having a 12x or a 24x CD drive.  
返回价格低于600且 cd drive 是 12x 或 24x 的 所有 pc 的 model，speed，hard drive
```sql
Select model, speed, hd from pc
where price < 600 and cd in ('12x','24x')
```
Exercise: 6 (Serge I: 2002-10-28)  
For each maker producing laptops with a hard drive capacity of 10 Gb or higher, find the speed of such laptops.   
Result set: maker, speed.  
在 maker 生产的 laptop 中，返回 hard drive 高于10GB 的 laptop 的speed
```sql
Select distinct maker, speed from product a
left join laptop b on
a.model = b.model
where hd >= 10 and type = 'laptop'
```
Exercise: 7 (Serge I: 2002-11-02)  
Get the models and prices for all commercially available products (of any type) produced by maker B.  
返回 maker B 生产的所有 type 的 model，price
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
返回生产 pc 但不生产 laptop 的 maker
```sql
Select distinct maker from product
where type = 'pc' and maker not in 
(
  select distinct maker from product where type = 'laptop'
)
```
Exercise: 9 (Serge I: 2002-11-02)  
Find the makers of PCs with a processor speed of 450 MHz or more.   
Result set: maker.  
返回所生产的 pc 的 speed 不低于 450 的 maker
```sql
Select distinct maker from product
left join pc on
product.model = pc.model
where speed >= 450
```
Exercise: 10 (Serge I: 2002-09-23)  
Find the printer models having the highest price.   
Result set: model, price.  
返回 printer 中 价格最高的 model
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
返回所有 pc 的 平均 speed
```sql
Select avg(speed)  speed from pc
```
Exercise: 12 (Serge I: 2002-11-02)  
Find out the average speed of the laptops priced over $1000.  
返回所有价格高于1000的 laptop 的平均 speed
```sql
Select avg(speed) speed from laptop
where price > 1000
```
Exercise: 13 (Serge I: 2002-11-02)  
Find out the average speed of the PCs produced by maker A.  
返回 maker A 生产的 pc 的平均 speed
```sql
Select avg(speed) speed from pc
left join product on
pc.model = product.model
where maker = 'a'
```
Exercise: 14 (Serge I: 2012-04-20)  
Get the makers who produce only one product type and more than one model.   
Output: maker, type.  
返回只生产一种 type 但生产多种 model 的 maker
```sql
Select distinct maker, type from product
where maker in
(
  select maker from product
  group by maker
  having count(distinct type) =1 -- 只生产一种 type
  and count(distinct model) >1 -- 生产多种 model
)
```
Exercise: 15 (Serge I: 2003-02-03)  
Get hard drive capacities that are identical for two or more PCs.   
返回具有相同的 hard drive 两个或多个 pc 的 hd
```sql
Select hd from pc
group by hd
having count(*)>1
```
Exercise: 16 (Serge I: 2003-02-03)  
Get pairs of PC models with identical speeds and the same RAM capacity.   
Each resulting pair should be displayed only once, i.e. (i, j) but not (j, i).   
Result set: model with the bigger number, model with the smaller number, speed, and RAM.  
返回具有相同 speed 及 ram 的 pc，两两一组，每种组合只能出现一次
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
返回 speed 比任意一个 pc 都低的 laptop 的 model
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
返回生产最便宜的彩色打印机的 maker
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
对于所生产的 model 包含在 laptop 表中的 maker，返回其所生产的所有 laptop 的 screen size 的平均值
```sql
Select maker, avg(screen) from laptop a
left join product b on
a.model = b.model
group by maker
```
Exercise: 20 (Serge I: 2003-02-13)  
Find the makers producing at least three distinct models of PCs.  
Result set: maker, number of PC models.  
返回至少生产三种不一样 pc model 的 maker
```sql
Select maker, count(model) from product
where type = 'pc'
group by maker
having count(model)>=3
```
Exercise: 21 (Serge I: 2003-02-13)  
Find out the maximum PC price for each maker having models in the PC table.   
Result set: maker, maximum price.  
对于所生产的 model 包含在 pc 表中的 maker，返回其所生产的 pc 的最高价格
```sql
Select maker, max(price) from product a
inner join pc b on
a.model = b.model
group by maker
```
Exercise: 22 (Serge I: 2003-02-13)  
For each value of PC speed that exceeds 600 MHz, find out the average price of PCs with identical speeds.  
Result set: speed, average price.  
对于 speed 高于600MHz的 pc，返回它们中具有相同 speed 的 pc 的平均价格
```sql
Select speed, avg(price) from pc
where speed >600
group by speed
```
Exercise: 23 (Serge I: 2003-02-14)  
Get the makers producing both PCs having a speed of 750 MHz or higher and laptops with a speed of 750 MHz or higher.   
Result set: maker  
返回既能够生产 speed 不低于750MHz的 pc，也能生产 speed 不低于750MHz的 laptop 的 maker，这一结果可通过前后两部分分别做查询后再做交集即可
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
返回数据库中价格高于所有 type 的产品的 model
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
Find the printer makers also producing PCs with the lowest RAM capacity and the highest processor speed of all PCs having the lowest RAM capacity.   
Result set: maker.  
返回生产printer并也能生产pc的厂家，同时要满足pc的ram是最低值，且speed是ram最低的那些pc中最高的  
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
返回 maker A 所生产的所有 pc 及 laptop 的平均价格
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
返回能够生产 pc 和 printer 的 maker 所生产的 pc 的 HDD 平均值
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
返回 product 表中只生产一种 model 的 maker 的个数
```sql
Select count(*) from product
where maker in
(
  select maker from product
  group by maker
  having count(distinct model) = 1
)
```
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
```
Exercise: 29 (Serge I: 2003-02-14)   
Under the assumption that receipts of money (inc) and payouts (out) are registered not more than once a day for each collection point {i.e. the primary key consists of (point, date)}, write a query displaying cash flow data (point, date, income, expense).   
Use Income_o and Outcome_o tables.  
依据 point, date, income, expense 返回一张现金流量表，考虑每天发生的收入及支出交易都不超过一次，既可以使用 union，也可以使用 case 及 full outer join
```sql
--solution 1  union
Select a.point, a.date, inc, out from income_o a
left join outcome_o b on
a.date = b.date and
a.point = b.point
union 
Select a.point, a.date, inc, out from outcome_o a
left join income_o b on
a.date = b.date and
a.point = b.point

--solution 2  case and full outer join
Select 
  case
  when a.point is not null then a.point
    else b.point
    end point,
  case
    when a.date is not null then a.date
    else b.date
  end date,
inc, out from income_o a
full outer join outcome_o b on
a.date = b.date and
a.point = b.point
```
Exercise: 30 (Serge I: 2003-02-14)  
Under the assumption that receipts of money (inc) and payouts (out) can be registered any number of times a day for each collection point (i.e. the code column is the primary key), display a table with one corresponding row for each operating date of each collection point.  
Result set: point, date, total payout per day (out), total money intake per day (inc).   
Missing values are considered to be NULL.  
与上一题的不同之处在于考虑一天可能发生多笔收入及支出交易，所以先将 income 及 outcome 表分别按照 point 及 date 进行 group，余下都与上一题一样，也可以使用 case 和 full outer join
```sql
solution 1  union
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

--solution 2  case and full outer join
Select 
  case
    when a.point is not null then a.point
    else b.point
    end point,
  case
    when a.date is not null then a.date
    else b.date
  end date,
out, inc from 
(
  select point, date, sum(out) out from outcome
  group by point, date
) a
full outer join
(
  select point, date, sum(inc) inc from income
  group by point, date
) b on
a.point = b.point and a.date = b.date
```
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
```
Exercise: 31 (Serge I: 2002-10-22)  
For ship classes with a gun caliber of 16 in. or more, display the class and the country.  
返回 gun 的口径不低于 16in 的船只所属的 class 及 country
```sql
Select class, country from classes
where bore >= 16
```
Exercise: 32 (Serge I: 2003-02-17)  
One of the characteristics of a ship is one-half the cube of the calibre of its main guns (mw). 
Determine the average ship mw with an accuracy of two decimal places for each country having ships in the database.  
mw（口径二分之一的立方）是一个重要指数，返回数据库中每个国家所拥有的船只的 mw 的平均值，并以两位小数显示该结果  
需返回 ships 中的船只name，且也要返回 outcomes 中的ship，最终将两者去重合并
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
返回在 North Atlantic 战役中沉没的船只
```sql
Select ship from outcomes 
where battle = 'North Atlantic'
and result = 'sunk'
```
Exercise: 34 (Serge I: 2002-11-04)  
In accordance with the Washington Naval Treaty concluded in the beginning of 1922, it was prohibited to build battle ships with a displacement of more than 35 thousand tons.   
Get the ships violating this treaty (only consider ships for which the year of launch is known).   
List the names of the ships.  
依据1922年初制定的 Washington Naval 条例，不允许制造排水量高于35 000吨的 battle ship，返回违反该条例的船只
```sql
Select distinct name from classes, ships
where classes.class = ships.class
and type = 'bb' -- battle ship
and launched >= 1922
and displacement >35000
```
Exercise: 35 (qwrqwr: 2012-11-23)  
Find models in the Product table consisting either of digits only or Latin letters (A-Z, case insensitive) only.  
Result set: model, type.  
返回 product 表中只含有数字或拉丁字母的 model，本题是对余集及补集等集合知识的考察
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
返回数据库中的 lead ship，也包括 outcomes 中的数据
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
返回在数据库中只有一个 ship 的 class，也包括 outcomes 中的数据
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
返回既有 battleship 也有 cruiser 的 country，依旧是将前后两部分分别做查询，然后取交集
```sql
Select country from classes
where type = 'bb'
intersect -- 交集
Select country from classes
where type = 'bc'
```
Exercise: 39 (Serge I: 2003-02-14)  
Find the ships that `survived for future battles`; that is, after being damaged in a battle, they participated in another one, which occurred later.  
返回在一场战斗中 damaged 并又在此之后参加了另一场战斗的 ship
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
从 ships 表中找出 guns 的数量至少为10的 ship，返回其 class，name，country
```sql
Select ships.class, name, country from ships
inner join classes on
ships.class = classes.class
where numguns >= 10
```
Exercise: 41 (Serge I: 2008-08-30)  
For the PC in the PC table with the maximum code value, obtain all its characteristics (except for the code) and display them in two columns:   
- name of the characteristic (title of the corresponding column in the PC table);  
- its respective value.  
pc 表中 code 最高的 pc，获取它的指标（除却 code），并将这些指标以两列展示，一列是指标名称，另一列是指标数据，即平常所说的数据透视表
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
返回在相应 battle 中沉没的船只
```sql
Select distinct ship, battle from outcomes
where result = 'sunk'
```
Exercise: 43 (qwrqwr: 2011-10-28)  
Get the battles that occurred in years when no ships were launched into water.  
返回在没有任何船只下水的年份进行的 battle
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
返回以 R 开头的所有 ship
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
返回名字包含至少三个单词的 ship，本题用不用 trim 都可以，用了 trim 只是为了更严谨
```sql
Select name from ships
where trim(name) like '% % %' -- trim 去掉左右空格
union
Select ship from outcomes
where trim(ship) like '% % %'
```
Exercise: 46 (Serge I: 2003-02-14)  
For each ship that participated in the Battle of Guadalcanal, get its name, displacement, and the number of guns.  
返回所有参加了 Guadalcanal 战役的 ship 的 name, displacement, and the number of guns
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
为 product 表生成序号，要求如下：maker 以所生产的 model 数量降序排列（对于生产的 model 数量相同的 maker 来说，以 maker 的首字母升序排列），以 model 升序排列，这里用到了开窗函数 row_number（）over（order by ……）
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
返回至少有一架船只在战役中沉没的 class
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
返回 gun 口径为16in的 ship，包括 outcomes 中的数据
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
返回 ships 表中 class 为 Kongo 的船只所参与的战役
```sql
Select distinct battle from ships a
inner join outcomes b on
a.name = b.ship
where  a.class = 'kongo' 
```
Exercise: 51 (Serge I: 2003-02-17)  
Find the names of the ships with the largest number of guns among all ships having the same displacement (including ships in the Outcomes table).  
返回具有相同 displacement 中 gun 数量最大的 ship，即按 displacement 分组后求 gun 数量最大值，并返回其对应的 ship
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
返回 ships 表中日本的 battleship，并且这些 ship 具备以下特征：至少有9个 gun，口径低于19in，排水量不高于65 000吨
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
返回 battleship 的 gun 数量的均值，精度为两位小数
```sql
Select cast(avg(numGuns*1.0) as decimal(10,2)) from classes
where type = 'bb' 
```
Exercise: 54 (Serge I: 2003-02-14)  
With a precision of two decimal places, determine the average number of guns for all battleships (including the ones in the Outcomes table).  
返回 battleship 的 gun 数量的均值，精度为两位小数，考虑 outcomes 中的数据
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
返回每个 class 下的第一艘船下水的年份，如果 lead ship 的下水年份未知，则用该 class 下最早的下水年份表示
```sql
Select classes.class, min(launched) from classes
left join ships on
ships.class = classes.class
group by classes.class
```
Exercise: 56 (Serge I: 2003-02-16)  
For each class, find out the number of ships of this class that were sunk in battles.   
Result set: class, number of ships sunk.  
返回每个 class 下沉没船只的数量
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
查询至少有三艘 ships 的 class 且该 class 至少有一条船 sunk 了，本题比上一题多了一个指标，就是每个 class 下的 ship 数量，所以设定两个指标，一个是船只数，另一个是沉没的船只数
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
返回每个 maker 分类下，各个 type 所对应的 model 数量与该 maker 生产的所有 model 数量的比率 
为了能够显示所有 maker 及所有 type，需要使用 cross join 生成一张 maker 和 type 的交叉表
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
 ) x -- 生成交叉表
 left join
 (
  select maker, count(distinct model) as to_model from product
  group by maker -- 按 maker 聚合
 ) y on x.maker = y.maker
left join
(
  select maker, type, count(distinct model) as ty_model from product
  group by maker, type -- 按 maker 和 type 聚合
) z on x.maker = z.maker and x.type = z.type
```
Exercise: 59 (Serge I: 2003-02-15)  
Calculate the cash balance of each buy-back center for the database with money transactions being recorded not more than once a day.  
Result set: point, balance.  
当收入和支出交易每天都不超过一次时，计算每个回收中心的交易情况，本质上与29题及30题类似
```sql
Select  
  case
    when a.point is null then b.point 
    else a.point
  end point,
isnull(inc,0) - isnull(out,0) -- 将 inc 和 out 的 null 置为0
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
当收入和支出交易每天都不超过一次时，计算从4/15/2001起每个回收中心的交易情况，本质上依旧与29题及30题类似
```sql
Select  
  case
    when a.point is null then b.point 
    else a.point
  end point,
isnull(inc,0) - isnull(out,0) -- 将 inc 和 out 的 null 置为0
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
当收入和支出交易每天都会发生多次时，计算所有回收中心的余额
```sql
Select 
  case
    when (select count(*) from income_o) = 0 then (select sum(out) from outcome_o)
    when (select count(*) from outcome_o) = 0 then (select sum(inc) from income_o)
    else (select sum(inc) from income_o) - (select sum(out) from outcome_o)
  end 
-- 网上有人在这里加了 from income_o 结果就是出现多行，其实不用加的，加了反而会以 income_o 相同的行数显示计算结果
-- 这里也涉及到了 case 语句里嵌套 select ，基本上，无论是 when 还是 then，它们后面都可以接查询语句
```
Exercise: 62 (Serge I: 2003-02-15)  
For the database with money transactions being recorded not more than once a day, calculate the total cash balance of all buy-back centers at the beginning of 04/15/2001.  
当收入和支出交易每天都不超过一次时，计算从04/15/2001起所有回收中心的余额
```sql
Select 
  case
    when (select count(*) from income_o) = 0 then (select sum(out) from outcome_o where date < '2001-4-15')
    when (select count(*) from outcome_o) = 0 then (select sum(inc) from income_o where date < '2001-4-15')
    else (select sum(inc) from income_o where date < '2001-4-15') - (select sum(out) from outcome_o where date < '2001-4-15')
  end
```
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
```
Exercise: 63 (Serge I: 2003-04-08)  
Find the names of different passengers that ever travelled more than once occupying seats with the same number.  
返回那些不止一次乘坐过相同座位的旅客
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
使用 Income 和 Outcome 表找出每个回收中心在哪些天里只有收入没有支出，反之亦然
```sql
Select 
  case
    when a.point is null then b.point
    else a.point
  end point,
  case
    when a.date is null then b.date
    else a.date
  end date,
  case
    when a.point is null then 'out'
    else 'inc'
  end operation,
  case
    when inc is null then out 
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
生成 maker，type 两列数据，并对结果进行标序，要求如下：maker 按升序排列，type 按 pc，laptop，printer 排列，如果某个 maker 生产多个 type，则 maker 的名字只能显示在第一行，其他行需用空来代替
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
返回于2003-04-01 至 2003-04-07 期间每日从 Rostov 出发的航班数量
```sql
Select a, 
  case
    when no is not null then no
    else 0
  end no 
from
(
  select '2003-04-01 00:00:00.000' a union all select '2003-04-02 00:00:00.000' union all select '2003-04-03 00:00:00.000' union all select '2003-04-04 00:00:00.000' union all select '2003-04-05 00:00:00.000' union all select '2003-04-06 00:00:00.000' union all select '2003-04-07 00:00:00.000'  -- 用 union all 拼接生成一张新表，注意 select 之后的第一个数据需要命名列名，本例中为 a 
) x
left join 
(
  select count(distinct trip_no) as no, date from pass_in_trip
  where trip_no in(select trip_no from trip where town_from = 'rostov')
  group by date
) y on
x.a = y.date
```
Exercise: 67 (Serge I: 2010-03-27)  
Find out the number of routes with the greatest number of flights (trips).  
Notes.   
1) A - B and B - A are to be considered DIFFERENT routes.  
2) Use the Trip table only.  
返回航班数量最多的航线的个数，A-B 与 B-A 是不同的航线
```sql
select count(no) from 
(
  Select count(trip_no) no  from trip
  group by town_from+town_to
  having count(trip_no) >= all (Select count(trip_no)  from trip
  group by town_from+town_to)
)
a 
--起初因为题目理解错误，导致第二个数据库一直报错，后来才发现原来题目的意思是找出航班最多的路线的个数，其实就是有N个路线，它们的航班数是最多的，求这个N
```
Exercise: 68 (Serge I: 2010-03-27)  
Find out the number of routes with the greatest number of flights (trips).  
Notes.   
1) A - B and B - A are to be considered the SAME route.  
2) Use the Trip table only.  
返回航班数量最多的航线的个数，A-B 与 B-A 是相同的航线，比上一题多了一项，重新组合出发地和目的地
```sql
select count(no) from 
(
  Select count(trip_no) no  from  trip
  group by 
    case
      when town_from < town_to then town_from+ town_to
      else town_to + town_from
    end -- 将出发地和目的地重新组合，确保 A-B 与 B-A 是相同的
  having count(trip_no) >= all (Select count(trip_no)  from trip
  group by 
    case
      when town_from < town_to then town_from+ town_to
      else town_to + town_from
    end
  )
) a 
```
Exercise: 69 (Serge I: 2011-01-06)  
Using the Income and Outcome tables, find out the balance for each buy-back center by the end of each day when funds were received or payments were made.   
Note that the cash isn’t withdrawn, and the unspent balance/debt is carried forward to the next day.  
Result set: buy-back center ID (point), date in dd/mm/yyyy format, unspent balance/debt by the end of this day.  
使用 Income 和 Outcome 表计算每天结束时各个回收中心的资金余额，这是一道数据滚动问题，即上一条数据的计算结果需要作为下一条数据的初始值滚动至下一行并参与计算，同时还要对 datetime 进行转换，考虑使用开窗函数 sum（）over（partition by……）
```sql
Select point, convert(varchar(100),date,103) date, sum(inc - out) over(partition by point order by date) balance  from
-- 提示中建议使用 running totals，不过鉴于更熟悉 sum（）over（）所以还是采用了后者，
 (
  select coalesce(a.point, b.point) point, coalesce(a.date, b.date) date, coalesce(inc, 0) inc, coalesce(out, 0) out from
  -- coalesce 作用等同于 case，只不过比 case 更简洁
  (
    select point, date, sum(inc) inc from income 
    --如果在子查询中对 date 进行 convert 转换，则会将 date 的类型从 datetime 转变为 varchar，这样在最后一步的 order by 里就会出现错误
    group by point, date
  ) a
  full join
  (
    select point, date, sum(out) out from outcome
    group by point, date
  ) b on
  a.point = b.point and a.date = b.date
) x 
```
对Running Totals有兴趣的可以详见 [Running Totals](http://www.sql-tutorial.ru/en/book_running_totals.html)   

Exercise: 70 (Serge I: 2003-02-14)  
Get the battles in which at least three ships from the same country took part.  
返回同一个国家至少有三艘船只参与的战役
```sql
Select distinct battle from 
-- 当存在多个国家参加同一场战斗，且每个国家参战的船只数量都不少于3，则必须用 distinct 来去除重复的 battle
(
  select battle, country, ship from outcomes 
  inner join ships on
  outcomes.ship = ships.name
  inner join classes on
  ships.class = classes.class 
  union
  select battle, country, ship from outcomes 
  inner join classes on
  outcomes.ship = classes.class
  where ship not in (select name from ships)
) a
group by country, battle
having count(distinct ship) >= 3
```
Exercise: 71 (Serge I: 2008-02-23) 
Find the PC makers with all personal computer models produced by them being present in the PC table.  
返回所生产的 pc model 都存在于 pc 表中的 maker
```sql
Select distinct maker from product
where model in(select model from pc)
and type = 'pc'
and maker not in(select maker from product where model not in(select model from pc) and type = 'pc')
-- 找出生产的 pc model 都在 pc 表中的 maker。 同时也要剔除生产的 pc model 不在 pc 表中的 maker
```
Exercise: 72 (Serge I: 2003-04-29)  
Among the customers using a single airline, find distinct passengers who have flown most frequently.   
Result set: passenger name, number of trips.  
在那些只乘坐过一家航空公司航班的旅客中，找出飞行最频繁的旅客
```sql
Select distinct name, num from
(
  select pass_in_trip .id_psg, passenger.name, count(pass_in_trip .trip_no) as num from pass_in_trip
  inner join passenger on
  pass_in_trip.id_psg = passenger.id_psg
  inner join trip on
  pass_in_trip.trip_no = trip .trip_no
  group by pass_in_trip .id_psg, passenger.name
  having count(distinct id_comp) = 1
) x  -- 只乘坐一家航空公司航班的旅客
where num = (select max(num) from 
  (
    select pass_in_trip .id_psg, count(pass_in_trip .trip_no) as num from pass_in_trip
    inner join trip on
    pass_in_trip.trip_no = trip .trip_no
    group by pass_in_trip.id_psg
    having count(distinct id_comp) = 1
  ) x -- 只乘坐一家航空公司航班的旅客的最高飞行次数
)
```
Exercise: 73 (Serge I: 2009-04-17)  
For each country, determine the battles in which the ships of this country did not participate.  
Result set: country, battle.  
返回每个国家没有参与的战役，考虑先做一张国家和战役的交叉表，通过 cross join 实现，而后生成每个国家参与的战役数据，两者做差集就成了
```sql
Select country, battle from
(
  select distinct country from classes
) a
cross join
(
  select distinct name as battle from battles
) b
except --  做一个差集
select distinct country, battle from classes
left join ships on
ships.class = classes.class
left join outcomes on
classes.class = outcomes.ship or ships.name = outcomes.ship
```
Exercise: 74 (dorin_larsen: 2007-03-23)  
Get all ship classes of Russia. If there are no Russian ship classes in the database, display classes of all countries present in the DB.  
Result set: country, class.  
返回 Russia 的所有 class，如果数据库中没有 Russia 的 class 数据，那么就显示数据库中所有国家下的 class  
想了好久也没想出来到底要怎么写，偶然看到一段代码，就是下面这段，写的十分简洁清晰
```sql
Select country, class from classes where country='russia'
union
select country, class
from classes
where 'russia' not in (select country from classes)  
-- 或者也可以写作 where not exists (select country from classes where country='russia')
```
Exercise: 75 (Serge I: 2009-04-17)  
For each ship from the Ships table, determine the name of the earliest battle from the Battles table it could have participated in after being launched.   
If the year the ship has been launched is unknown use the latest battle.   
If no battles occurred after the ship was launched, display NULL instead of the battle name.  
It’s assumed a ship can participate in any battle fought in the year of its launching.  
Result set: name of the ship, year it was launched, name of battle.

Note: assume there are no battles fought at the same day.  
对于 ships 表中的每个 ship，返回自该船只下水后所能够参与的战役中最早的一场  
如果船只下水年份未知，则显示最晚的一场战役  
如果船只下水的年份之后都没有战役，则 battle 字段置空  
假定船只下水当年就可以参与战役
```sql
Select distinct ships.name, ships.launched,
  case
    when ships.launched is not null then battle 
    else (select name from battles where date =(select max(date) from battles)) 
    -- 下水年份未知，则显示最晚一场的战役
  end battle 
from ships
left join
(
  select * from 
  (
    select ships.name, launched, battles.name as battle, 
      case
        when date = min(date) over(partition by ships.name) then date
        else null
      end date -- 将对应每个船只最早一场的战役标示出来
    from ships
    inner join battles on
    launched <= year(date)
  )a
  where date is not null -- 选取已经标示出来的战役，其他为空的则剔除
)b on ships.name = b.name
```
Exercise: 76 (Serge I: 2003-08-28)  
Find the overall flight duration for passengers who never occupied the same seat.   
Result set: passenger name, flight duration in minutes.  
返回那些乘坐同一座位从未超过一次的乘客的总飞行时长，具体到分钟  
本题有两个注意点，一是要找出乘坐同一座位从未超过一次的乘客，二是计算总飞行时长  
对于第一点可能直观的想到 group by 后取 having count（）= 1 的结果，其实就算计数项为1，有可能也混入了那些乘坐同一座位超过一次的乘客，比如乘客 A 乘坐1号座位一次，乘坐2号座位两次，乘坐3号座位3次，所以这里需要做的不是选取，而是剔除，剔除乘坐同一座位超过一次的乘客  
而后来计算飞行时长，题目中给了提示，如果起飞是一天，到达是第二天要怎么算，比如表中显示起飞是2009-10-1 12：00，到达是2009-10-1 2：00，明显到达时间要早于起飞时间，很显然飞行时段存在跨天，所以需要一个 case 语句做判断
```sql
Select name, sum(
  case
    when datediff(mi, time_out, time_in) > 0 then datediff(mi, time_out, time_in)
    else datediff(mi, time_out, time_in) + 1440 -- 如果存在跨天，则在原结果上加上1440，即24*60，一天的分钟数
  end
)  from
(
  select trip_no, id_psg from pass_in_trip 
  where pass_in_trip .id_psg not in
  (
    select id_psg from pass_in_trip
    group by id_psg, place
    having count(*) > 1 -- 剔除乘坐同一座位超过一次的乘客，而非选取乘坐同一座位从未超过一次的乘客
  )
)a
left join trip on
a. trip_no = trip. trip_no
left join passenger on
passenger.id_psg = a. id_psg
group by name, a. id_psg
```
Exercise: 77 (Serge I: 2003-04-09)  
Find the days with the maximum number of flights departed from Rostov.   
Result set: number of trips, date.  
返回从 Rostov 出发的航班数量最多的日期
```sql
solution 1 

Select * from
(
  select count(distinct trip.trip_no) as number, date from trip
  inner join pass_in_trip on
  trip.trip_no = pass_in_trip.trip_no
  where town_from = 'rostov'
  group by date
) a
where number = 
(
  select max(number) from
  (
    select count(distinct trip.trip_no) as number, date from trip
    inner join pass_in_trip on
    trip.trip_no = pass_in_trip.trip_no
    where town_from = 'rostov'
    group by date
  )a
)

solution 2 使用 having 可以让代码更简洁

Select count(distinct trip.trip_no) as number, date from trip
inner join pass_in_trip on
trip.trip_no = pass_in_trip.trip_no
where town_from = 'rostov'
group by date
having count(distinct trip.trip_no) >= all
(
  select count(distinct trip.trip_no)  from trip
  inner join pass_in_trip on
  trip.trip_no = pass_in_trip.trip_no
  where town_from = 'rostov'
  group by date
)
```
Exercise: 78 (Serge I: 2005-01-19)  
For each battle, get the first and the last day of the month when the battle occurred.  
Result set: battle name, first day of the month, last day of the month.  
Note: output dates in yyyy-mm-dd format.
对于每场战役，返回战役发生日期所在月份的第一天及最后一天
日期转化及第一天/最后一天详见[(转) sql server中各类日期格式转化及查询第一天或最后一天.md](https://github.com/ljsakura/ljsakura.github.io/blob/master/SQL/(转)%20sql%20server中各类日期格式转化及查询第一天或最后一天.md)
```sql
Select name, 
convert(varchar(100),dateadd(mm, datediff(mm,0,date), 0),23), 
convert(varchar(100),dateadd(ms,-3,dateadd(mm, datediff(m,0,date)+1, 0)),23) from
battles
```
Exercise: 79 (Serge I: 2003-04-29)  
Get the passengers who, compared to others, spent most time flying.   
Result set: passenger name, total flight duration in minutes.  
返回那些飞行时长最高的旅客，总飞行时长以分钟计算
```sql
Select name, sum(duration) from pass_in_trip
inner join passenger on
passenger.id_psg = pass_in_trip. id_psg
inner join 
(
  select trip_no, 
    case
      when datediff(mi, time_out, time_in) > 0 then datediff(mi, time_out, time_in)
      else datediff(mi, time_out, time_in) + 1440 
    end duration
  from trip
) a on
a. trip_no = pass_in_trip. trip_no
group by name, pass_in_trip.id_psg --考虑到旅客有重名的情况，所以这里聚合的时候一定要加上旅客的 ID
having sum(duration) >= all 
(
  select sum(duration) from pass_in_trip
  inner join passenger on
  passenger.id_psg = pass_in_trip. id_psg
  inner join 
  (
    select trip_no, 
      case
        when datediff(mi, time_out, time_in) > 0 then datediff(mi, time_out, time_in)
        else datediff(mi, time_out, time_in) + 1440 
      end duration
    from trip
  ) a on
  a. trip_no = pass_in_trip. trip_no
  group by name, pass_in_trip.id_psg
)
```
Exercise: 80 (Baser: 2011-11-11)  
Find the computer equipment makers not producing any PC models absent in the PC table.  
返回所生产的 pc model 都存在于 pc 表中的 maker，以及不生产 pc 的maker
```sql
Select maker from product
where type = 'pc' and 
model in
(
  select model from pc
) -- 生产的 pc model 存在于 pc 表中的 maker
union  -- 并集
select maker from product
where type != 'pc' -- 生产非 pc 的 maker
except  -- 差集
select maker from product
where type = 'pc' and 
model not in 
(
  select model from pc
) -- 生产的 pc model 不存在于 pc 表中的 maker
```
Exercise: 81 (Serge I: 2011-11-25)  
For each month-year combination with the maximum sum of payments (out), retrieve all records from the Outcome table.  
这道题真是一不小心就会导致超时
```sql
Select outcome.* from outcome
inner join  
(
  select yy, mm from
  (
    select year(date) yy , month(Date)  mm, sum(out) out from outcome
    group by year(date), month(Date)
    having sum(out) >= all
    (
      select sum(out) out from outcome
      group by year(date), month(Date)
    )
  )a
) b on
year(outcome.date) = yy and month(outcome.date) = mm
```
Exercise: 82 (Serge I: 2011-10-08)  
Assuming the PC table is sorted by code in ascending order, find the average price for each group of six consecutive personal computers.   
Result set: the first code value in a set of six records, average price for the respective set.  
假定 pc 表是按 code 升序排列的，返回连续六个电脑的价格均值，求滑动平均问题  
来说说这道题的几个坑，首先 code 的数据虽然是按升序排列，可是它们不一定连续，所以要重新为 pc 表进行编码，这个没问题，row_number 就可以实现了  
然后第二个数据库就一直报错了，显示全部十条都是错的，错的，错的……抓狂十分钟，终于想明白哪里错了，不能按照新的编码显示结果，要显示原编码~~~
```sql
Select newcode, avg(p2.price) from  -- 显示原 pc 表中 code 的数据
(
  select row_number() over(order by code) code, code as newcode from pc 
) p1
inner join 
(
  select row_number() over(order by code) code, price from pc 
) p2 on
p2.code - p1.code <= 5 and p2.code >= p1.code
group by p1.code, newcode
having count(*) = 6
```
Exercise: 83 (dorin_larsen: 2006-03-14)  
Find out the names of the ships in the Ships table that meet at least four criteria from the following list:  
numGuns = 8,   
bore = 15,  
displacement = 32000,  
type = bb,  
launched = 1915,  
class = Kongo,  
country = USA.  
返回至少满足以下四个条件的船只名称  
想了好久也没想到要怎么做这个问题，后来看到有人说可以考虑把这些条件都置换成0/1，这样再对置换后的数值求和，大于等于4的就是要查找的结果，这个想法简直太赞了，果然群众的智慧就是高
```sql
Select name from ships
inner join classes on
ships.class = classes.class
where (
  case numguns      when 8       then 1 else 0 end +
  case bore         when 15      then 1 else 0 end +
  case displacement when 32000   then 1 else 0 end +
  case type         when 'bb'    then 1 else 0 end +
  case launched     when 1915    then 1 else 0 end +
  case ships.class  when 'kongo' then 1 else 0 end +
  case country      when 'usa'   then 1 else 0 end 
 ) >= 4 -- 论坛里也有人用 iif(col_name = value, 1, 0) 来做判断的
```
Exercise: 84 (Serge I: 2003-06-05)  
For each airline, calculate the number of passengers carried in April 2003 (if there were any) by ten-day periods. Consider only flights that departed that month.  
Result set: company name, number of passengers carried for each ten-day period.  
计算2003年4月份，每间隔十天，各个航空公司运输的乘客数量
```sql
Select name, sum(a) as '1-10', sum(b) as '11-20', sum(c) as '21-30' from
(
  select company.name,
    case
      when day(date) >  0 and day(date) < 11 then  1
      else 0
    end a, -- 这里不能直接将列名定义为 ‘1-10’，否则在最上面一行做聚合时会报错，显示列为 varchar 的格式，无法聚合
    case
      when day(date) > 10 and day(date) < 21 then  1
      else 0
    end b, -- 同上
    case
      when day(date) > 20 and day(date) < 31 then  1
      else 0
    end c -- 同上
  from 
  (
  select * from Pass_in_trip
  where year(date) = 2003 and month(date) = 4
  ) a
  inner join Trip on
  a. trip_no = Trip.trip_no
  inner join Company on
  Company. id_comp = trip. id_comp
)a
group by name
```
Exercise: 85 (Serge I: 2012-03-16)  
Get makers producing either printers only or personal computers only; in case of PC manufacturers they should produce at least 3 models.  
返回只生产 printer 或 pc 的 maker ，如果是生产 pc 的，那么 model 的数量至少是3
```sql
Select maker from product
where type = 'pc' 
and maker not in
(
  select maker from product where type != 'pc'
)
group by maker
having count(distinct model) >= 3 -- 只生产 pc 且 model 数量至少为3的 maker
union  -- 并集
select maker from product
where type = 'printer'
and maker not in 
(
  select maker from product where type != 'printer'
) -- 只生产 printer 的 maker
```
Exercise: 86 (Serge I: 2012-04-20)  
For each maker, list the types of products he produces in alphabetic order, using a slash ("/") as a delimiter.  
Result set: maker, list of product types.  
对于每个 maker，将所有 type 按字母排序，并用反斜杠'/'拼接起来  
本题在 mysql 中可以使用 group_concat 函数，在 mssql 中可以使用 stuff 函数
```sql
solution 1  mysql

Select maker,
group_concat(distinct type order by type Separator '/') as types 
from product 
group by maker 

----------
solution 2  mssql

Select maker, 
data=stuff ((select distinct '/'+ type from product t where maker=t1.maker for xml path('')), 1, 1, '')
from product t1
group by maker
```
Exercise: 87 (Serge I: 2003-08-28)  
Supposing a passenger lives in the town his first flight departs from, find non-Muscovites who have visited Moscow more than once.   
Result set: passenger's name, number of visits to Moscow.  
假定旅客第一次出发的城市就是他所居住的城市，返回那些非莫斯科居民且飞往 Moscow 超过一次的旅客，题目有提示，有些旅客可能在同一天多次飞行，所以不仅要按照 date 排序，还要按照 time_out 排序
```sql
Select distinct Passenger.name, count(town_to) from Pass_in_trip
left join Passenger on
Pass_in_trip.id_psg = Passenger.id_psg
left join Trip on
Pass_in_trip. trip_no = Trip. trip_no
where Pass_in_trip .id_psg in
(
  select id_psg from
  (
    select id_psg, row_number() over(partition by id_psg order by date, time_out ) as num, town_from from Pass_in_trip
    -- 需要按照 date，time_out 多重排序，取出发日期和出发时间最小值，用 row_number 是目前我能想到的方法中最简洁的，否则要嵌套多层子查询
    left join Trip on
    Pass_in_trip. trip_no = Trip. trip_no
  ) a 
  where town_from != 'Moscow' and num = 1
) -- 以上子查询返回了第一次出发城市不是 Moscow 的旅客
and town_to = 'Moscow'
group by Pass_in_trip.id_psg, Passenger.name
having count(town_to) > 1
```
Exercise: 88 (Serge I: 2003-04-29)  
Among those flying with a single airline find the names of different passengers who have flown most often.   
Result set: passenger name, number of trips, and airline name.  
真是一道让人欲哭无泪的问题，和72题基本相似，比72题要多返回一列，就是航空公司的名字  
考虑在72题代码的基础上再次 join 其他关系表，从而获取航空公司的名字，可是第二个测试数据库一直报错，思考了许久，猜想大概是因为有重名乘客且他们乘坐的航空公司和飞行次数都相同，导致了最后结果因此而减少，所以采用 group by，依据乘客的 ID 进行分组再返回最终结果，果不其然，正如我所猜想的
```sql
Select x.name, max(num), company.name as comp from
(
  select pass_in_trip .id_psg, passenger.name, count(pass_in_trip .trip_no) as num from pass_in_trip
  inner join passenger on
  pass_in_trip.id_psg = passenger.id_psg
  inner join trip on
  pass_in_trip.trip_no = trip .trip_no
  group by pass_in_trip .id_psg, passenger.name
  having count(distinct id_comp) = 1
) x -- 只乘坐一家航空公司航班的旅客
inner join pass_in_trip on 
pass_in_trip.id_psg = x.id_psg
inner join trip on
trip.trip_no = pass_in_trip.trip_no
inner join company on
company.id_comp = trip.id_comp
where num = (select max(num) from 
  (
    select pass_in_trip .id_psg, count(pass_in_trip .trip_no) as num from pass_in_trip
    inner join trip on
    pass_in_trip.trip_no = trip .trip_no
    group by pass_in_trip.id_psg
    having count(distinct id_comp) = 1
  ) x -- 只乘坐一家航空公司航班的旅客的最高飞行次数
)
group by x.name, x.id_psg, company.name -- 一定要按乘客的 ID 进行聚合，这样才不会将同名乘客忽略掉
```
Exercise: 89 (Serge I: 2012-05-04)  
Get makers having most models in the Product table, as well as those having least.  
Output: maker, number of models.  
返回 product 表中 model 数量最多和最少的 maker
```sql
Select maker, count(distinct model) from product
group by maker
having count(distinct model) >= all
(
  select count(model) from product group by maker
)
union -- 并集
Select maker, count(distinct model) from product
group by maker
having count(distinct model) <= all
(
  select count(model) from product group by maker
)
```
Exercise: 90 (Serge I: 2012-05-04)  
Display all records from the Product table except for three rows with the smallest model numbers and three ones with the greatest model numbers.  
返回除 model 最大和最小的三个值以外所有行
开窗函数走起来，一个升序排列，一个降序排列，然后就一切迎刃而解
```sql
Select maker, model, type from
(
  select *, row_number() over(order by model asc) index1, row_number() over(order by model desc) index2 from product
) a
where index1 not in (1,2,3) and index2 not in (1,2,3)
```
```
91-92 Database 5
Short database description "Painting"
The database schema consists of 3 tables:
utQ (Q_ID int, Q_NAME varchar(35)), 
utV (V_ID int, V_NAME varchar(35), V_COLOR char(1)), 
utB (B_Q_ID int, B_V_ID int, B_VOL tinyint, B_DATETIME datetime).
The utQ table contains the identifiers and names of squares, the initial color of which is black. (Note: black is not a color and is considered unpainted. Only Red, Green and Blue are colors.) 
The utV table contains the identifiers and names of spray cans and the color of paint they are filled with.
The utB table holds information on squares being spray-painted, and contains the square and spray can identifiers, the quantity of paint being applied, and the time of the painting event.
It should be noted that
- a spray can may contain paint of one of three colors: red (V_COLOR='R'), green (V_COLOR='G'), or blue (V_COLOR='B');
- any spray can initially contains 255 units of paint;
- the square color is defined in accordance with the RGB model, i.e. R=0, G=0, B=0 is black, whereas R=255, G=255, B=255 is white;
- any record in the utB table decreases the paint quantity in the corresponding spray can by B_VOL and accordingly increases the amount of paint applied to the square by the same value;
- B_VOL must be greater than 0 and less or equal to 255;
- the paint quantity of a single color applied to one square can’t exceed 255, and there can’t be a less than zero amount of paint in a spray can;
- the time of the painting event (B_DATETIME) is specified with one second precision, i.e. it does not contain milliseconds;
- for historical reasons, the spray cans are referred to as “balloons” by many of the exercises, and the utV table contains spray can names (V_NAME column) such as “Balloon # 01”, etc. 
```
Exercise: 91 (Serge I: 2015-03-20)  
Determine the average quantity of paint per square with an accuracy of two decimal place  
居然是一道四分的题，原本是抱着死磕的心态冲上去的，结果还没有2分的难，这真是……  
返回每张纸板的平均油漆喷涂量，精确到两位小数，需要做数据类型转换
```sql
Select cast(avg(total * 1.0) as decimal(10,2)) from -- 需要对 total 做 *1.0来保证小数位，否则即便是 cast 了也不会保留小数
(
  select 
  sum(case when B_VOL is null then 0 else B_VOL end) as total from utb
  right join utq on
  utb.B_Q_ID = utq.Q_ID
  group by utq.Q_ID
) a
```
Exercise: 92 (ZrenBy: 2003-09-01)  
Get all white squares that have been painted only with spray cans empty at present.   
Output the square names.  
返回被喷涂成白色且喷涂这些画板的喷瓶已用空的画板名称，白色画板的 R G B都应该为255
```sql
select Q_NAME from
(
  select Q_NAME,
    case
      when  V_COLOR = 'R' and sum(B_VOL) = 255 then 1 
      when  V_COLOR = 'G' and sum(B_VOL) = 255 then 1 
      when  V_COLOR = 'B' and sum(B_VOL) = 255 then 1 else 0 -- 判断 RGB 的值
    end value
  from utB
  left join utQ on
  utB. B_Q_ID = utQ. Q_ID
  left join utV on
  utB. B_V_ID = utV. V_ID
  group by Q_NAME, V_COLOR
) a 
group by Q_NAME
having sum(value) = 3 -- RGB 三个值都为255的画板名
except -- 差集
select Q_NAME from utB
left join utQ on
utB. B_Q_ID = utQ. Q_ID
left join
(
  select B_V_ID, sum(B_VOL) to_v from utB
  group by B_V_ID
) b on
utB. B_V_ID = b. B_V_ID
where to_v < 255 -- 所使用的喷瓶没有用空的画板名
```
Exercise: 93 (Serge I: 2003-06-05)  
For each airline that transported passengers calculate the total flight duration of its planes.   
Result set: company name, duration in minutes.  
返回运输过旅客的航空公司所有飞机总的飞行时长，需要注意有的乘客是在同一天搭乘同一班航班，所以要做去重
```sql
Select name, sum(
  case
    when datediff(mi, time_out, time_in) > 0 then datediff(mi, time_out, time_in)
    else datediff(mi, time_out, time_in) + 1440
  end
)  from 
(
  select distinct trip_no, date from Pass_in_trip -- 对同一天搭乘同一航班的数据去重
) Pass_in_trip
inner join Trip on
Pass_in_trip. trip_no = Trip. trip_no
inner join Company on
Trip. id_comp = Company. id_comp
group by name
```
Exercise: 94 (Serge I: 2003-04-09)  
For seven successive days starting with the earliest date when the number of departures from Rostov was maximal, get the number of flights departed from Rostov.  
Result set: date, number of flights.  
返回从 Rostov 出发航班数量最多且最早的那一天，并从该天开始计算自此后连续七天从 Rostov 出发的航班数量
```sql
select dt, 
  case
    when qty is null then 0
    else qty
  end
qty 
from
(
  select dateadd(dd, ad, date) as dt from -- 新日期表
  (
    select date from
    (
      select date, row_number() over(order by count(distinct Pass_in_trip .trip_no)desc, date) num from Pass_in_trip
      left join Trip on
      Pass_in_trip. trip_no = Trip. trip_no
      where town_from = 'rostov'
      group by date
    )a
    where num = 1 -- 从 Rostov 出发航班数量最多且最早的那一天，按航班数量降序排列，按日期升序排列，而后取第一位
  ) b 
  cross join
  (
    select 0 as ad union all select 1 union all select 2 union all select 3 union all select 4 union all select 5 union all select 6 
  )c -- 为了生成连续七天，先做一个1-7的连续表，而后 cross join ，并用 dateadd 生成新日期表
)d
left join
(
  select date, count(distinct Pass_in_trip.trip_no) as qty from Pass_in_trip
  left join Trip on
  Pass_in_trip. trip_no = Trip. trip_no
  where town_from = 'rostov'
  group by date
)e on -- 每天从 Rostov 出发的航班数量
d.dt = e.date
```
Exercise: 95 (qwrqwr: 2013-02-08)  
Using the Pass_in_Trip table, calculate for each airline:  
1) the number of performed flights;  
2) the number of plane types used;  
3) the number of different passengers that have been transported;  
4) the total number of passengers that have been transported by the company.  
Output: airline name, 1), 2), 3), 4).  
返回每家航空公司已执行飞行的航班数量，飞机种类，已运载的不同旅客人数，以及总运载旅客人数
```sql
Select Company.name, 
count(distinct cast(date as varchar(20))+ cast(Pass_in_Trip.trip_no as varchar(20))) as flight, 
count(distinct plane) as plane, 
count(distinct ID_psg) as diff_psg, 
count(ID_psg) as total_psg from Pass_in_trip
left join Trip on
Pass_in_trip. trip_no = Trip. trip_no
left join Company on
Company. id_comp = Trip. id_comp
group by Company.name
```
Exercise: 96 (ZrenBy: 2003-09-01)  
Considering only red spray cans used more than once, get those that painted squares currently having a non-zero blue component.  
Result set: spray can name.  
返回被使用过不止一次的红色喷瓶，且这些喷瓶所喷涂的画板所使用的蓝色涂料用量不为零
```sql
Select V_NAME from utB
left join utV on
B_V_ID = V_ID
where V_NAME in
(
  select V_NAME from utB
  left join utV on
  B_V_ID = V_ID
  where B_Q_ID in 
  (
    Select B_Q_ID from utB
    left join utV on
    B_V_ID = V_ID
    where V_COLOR = 'B'  -- 查找使用了蓝色涂料的画板
  )
  and V_COLOR = 'R'  -- 已喷涂蓝色涂料画板所使用的红色喷瓶的名称
)
group by V_NAME
having count(V_NAME) > 1 -- 筛选使用超过一次的红色喷瓶
```
Exercise: 97 (qwrqwr: 2013-02-15)  
From the Laptop table, select rows fulfilling the following condition:   
the values of the speed, ram, price, and screen columns can be arranged in such a way that each successive value exceeds two or more times the previous one.  
Note: all known laptop characteristics are greater than zero.  
Output: code, speed, ram, price, screen.  
返回满足以下条件的行：  
speed，ram，price，screen 的数值能被排列成一组有序序列，该有序序列特征为每一个数值都是前一个数值的两倍或两倍以上  
这道题我最开始用排除法做，就是第二段代码，但是一直在第二个数据库上报错，冥思苦想近一周，又找人研究了两天，依旧没有什么有效结论，最终决定摒弃排除法，用选择法，没想到居然通过了，这简直就是…………直到现在也没想明白第二段代码错在哪里，莫名脸---  
本题中学到了一个新的运算符，cross apply，其实是 apply 的延伸，有 cross apply/outer apply，详细使用方式可以在 MSDN 官网上浏览
```sql
with test as
(
  select *, row_number() over(partition by code order by value) as number from
  (
    select code, name, value from Laptop
    cross apply
    (
      values('speed', speed)
      ,('ram', ram)
      ,('price', price)
      ,('screen', screen)
    ) 
    spec(name, value)
  ) a
)

select code, speed, ram, price, screen from laptop 
where code in
(
  select code from
  (
    select test.code, 
    case
    when test.value >= temp.value * 2 then 1
    else 0
    end inx
    from test
    inner join test as temp on 
    temp.code = test.code and temp.number + 1 = test.number 
  ) b
  group by code
  having sum(inx) = 3
)

--- code below failed on the second database, but I don't know why
with test as
(
  select *, row_number() over(partition by code order by value) as number from
  (
    select code, name, value from Laptop
    cross apply
    (
      values('speed', speed)
      ,('ram', ram)
      ,('price', price)
      ,('screen', screen)
    ) 
    spec(name, value)
  ) a
)

select code, speed, ram, price, screen from laptop
where code not in
(
  select distinct test.code from test
  inner join test as temp on 
  temp.code = test.code and temp.number + 1 = test.number and temp.value * 2 > test.value
)
```
Exercise: 98 (qwrqwr: 2010-04-26)  
Display the list of PCs, for each of which the result of the bitwise OR operation performed on the binary representations of its respective processor speed and RAM capacity contains a sequence of at least four consecutive bits set to 1.  
Result set: PC code, processor speed, RAM capacity.  
返回 pc 表中 speed 和 ram 的或位运算的二进制结果中至少存在四个连续1的行  
本题既考察了 sql 中的递归，又考察了位运算
```sql
with ctebins as
(
select code, speed, ram, cast((speed | ram)as int) as level, cast('' as varchar(max)) as binval
-- | 代表了或位运算，这里需要注意，因为 speed，ram 是 smallint 的格式，如果不转化为 int 则不能与后面的 c.level / 2 进行拼接
from pc
union all
select c.code, c.speed, c.ram, c.level / 2, cast(c.level % 2 as varchar(max)) + c.binval
from ctebins c
where c.level > 0
) -- 该 with 语句即递归，为 CTE （公用表表达式）

select code, speed, ram from pc 
where code in(
select code from ctebins
where level = 0 and binval like '%1111%' -- 存在至少四个连续1
)
```
有关递归可以参见例题中给出的链接[Simple recursive CTE](http://blogs.lobsterpot.com.au/2006/10/20/simple-recursive-cte/)  
有关位运算的详细说明明可参见[MSDN](https://docs.microsoft.com/zh-cn/sql/t-sql/language-elements/bitwise-operators-transact-sql?view=sql-server-2017)   

Exercise: 99 (qwrqwr: 2013-03-01)  
Only Income_o and Outcome_o tables are considered. It is known that no money transactions are performed on Sundays.  
For each buy-back center (point) and each funds receipt date, determine the encashment date according to the following rules:  
1. The encashment date is the same as the receipt date if there is no payment entry in the Outcome_o table for this date and point.   
2. Otherwise, the first possible date after the receipt date is used that doesn’t fall on Sunday and doesn’t have a corresponding payment entry in the Outcome_o table for the point in question.  
Output: point, receipt date, encashment date.  
使用 Income_o 和 Outcome_o 表并返回如下信息，已知周日无收付款项：  
1、如果收款当日无付款项，则收款日被作为结算日；  
2、如果收款当日有付款项，则付款日后最早的一天将被作为结算日，且该日不为星期日
```sql
with test as	
(	
  select point as po, date as da, date as temp from income_o	
  union all	
  select po, da,	
    case	
      when date is not null and datename(dw, date) = 'Saturday' then dateadd(dd, 2, temp)		
      when date is not null and datename(dw, date) != 'Saturday' then dateadd(dd, 1, temp)	
      else temp
    end	
  from test	
  join outcome_o on	
  po = point and temp = date	
)	
	
select test.* from test
left join outcome_o on
po = point and temp = date
where point is null and datename(dw, temp) != 'Sunday'

---------- Code below failed on the second database
with test1 as
(
  select a.point, a.date from income_o a
  inner join outcome_o b on
  a.point = b.point and a.date = b.date
)
, test as
(
  select point as po, date as da, date as temp from test1
  union all
  select po, da, dateadd(dd, 1, temp) from test
  where temp in (select date from outcome_o) or datename(dw, temp) = 'Sunday'
)
select * from test
where temp not in (select date from outcome_o) and datename(dw, temp) != 'Sunday'
union 
select a.point, a.date, a.date from income_o a
left join outcome_o b on
a.point = b.point and a.date = b.date
where b.point is null
```
Exercise: 100 ($erges: 2009-06-05)  
Write a query that displays all operations from the Income and Outcome tables as follows:  
date, sequential record number for this date, buy-back center receiving funds, funds received, buy-back center making a payment, payment amount.   
All revenue transactions for all centers made during a single day are ordered by the code field, and so are all expense transactions.   
If the numbers of revenue and expense transactions are different for a day, display NULL in the corresponding columns for missing operations.  
使用 Income 和 Outcome 表返回如下信息：  
日期，连续编号，进账中心编号，进账数目，出账中心编号，出账数目；  
所有收支结算都已当天分组并以 code 进行排序；  
如果同一天内收支次数不匹配，则用 Null 值代替缺失信息
```sql
Select coalesce(a.date, b.date),  coalesce(a.num, b.num),
a.point, a.inc, b.point, b.out from
(
  select point, date, inc, row_number() over(partition by date order by code) num from income
) a
full outer join
(
  select point, date, out, row_number() over(partition by date order by code) num from outcome
) b on
a.date = b.date and a.num = b.num
```
Exercise: 101 (qwrqwr: 2013-03-29)  
The Printer table is sorted by the code field in ascending order.  
The ordered rows form groups: the first group starts with the first row, each row having color=’n’ begins a new group, the groups of rows don’t overlap.   
For each group, determine the maximum value of the model field (max_model), the number of unique printer types (distinct_types_cou), and the average price (avg_price).  
For all table rows, display code, model, color, type, price, max_model, distinct_types_cou, avg_price.  
使用按 code 升序排列的 printer 表返回这样的数据：  
第一行作为第一组的第一行，而后每出现 color = 'n' 时则作为新一组的第一行，组与组之间不存在重叠；  
对每一个分组返回其最大 model，distinct type 总和以及平均 price  
该题最坑的地方在于假如第一行 color 不为 n 时，一定要想方设法将其设置为 n，而后才能进行后续分组
```sql
with temp as
(
  select a.code x,
		case
		when b.code is null then (select max(code)+1 from printer)
			else b.code
		end
  y
  from
	(
		select *, row_number() over(order by code) num from 
		(
			select code, model,
				case
					when code = 1 and color != 'n' then 'n' -- 将第一行 color 设置为 'n'
					else color
				end color,
			type, price from printer
		)
		printer
		where color = 'n'
	) a
	left join 
	(
		select *, row_number() over(order by code) num from 
		(
			select code, model,
				case
					when code = 1 and color != 'n' then 'n'
					else color
				end color,
			type, price from printer
		)
		printer
		where color = 'n'
	) b on
	a.num = b.num-1
), -- 使用 CTE 生成范围集 
test as
(
	select * from printer
	left join temp on
	code >= x and code < y
)
select code, model, color, type, price, m, t, p from test
left join
(
	select x, max(model) m, count(distinct type) t, avg(price) p from test
	group by x
) c on
test.x = c.x
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
