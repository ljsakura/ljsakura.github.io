来源：http://blog.csdn.net/qq_27755195/article/details/50919442

简介   
  data.table继承于data.frame。它提供了一个快速通道，让我们能更加快速的读取文件，对数据进行筛选、分组、排序、联表，而且其语法灵活、简介。由于data.table是一个data.frame所以它几乎兼容所有的函数。  

特点  
  1.data.table(DT)的操作语句类似于SQL，DT[i, j, by]中的i, j, by 对应着SQL语句的 i=where, j=select, by=group by。所以DT中的i, j并不是只是像data.frame只代表着行列，它更加的灵活多变。  
  2.符号 ” := “快速的增加或者删除列，类似SQL的update。  
  3.setkey(DT, colA, colB)，可以使得检索和分组更加快速  
  4.order,快速多重排序， 例如对DT按照x,y进行排序DT[order(DT$x, -DT$y),]或者DT[with(DT, order(x, -y)),]  

compare   
  包括使用DT使用Key后与DF的检索速度对比。   
  快速分组（需要设置KEY）,进行计算，和使用tapply分组计算速度
```r
###生成数据

grpsize <- ceiling(1e7/26^2)  ##10^7 rows, 676 groups

DF <- data.frame(x=rep(LETTERS,each=26*grpsize), 

   y=rep(letters,each=grpsize), v=runif(grpsize*26^2),

   stringsAsFactors=FALSE)

head(DF,3)  

x y     v

1 A a 0.5310106

2 A a 0.1980941

3 A a 0.8835322



DT <- as.data.table(DF) ##creat data.table

setkey(DT,x,y) #s et the key

###################################################

比较检索速度，搜索x=="R",y="h"

system.time(ans1 <- DF[DF$x=="R" & DF$y=="h",])  #vector scan

user  system  elapsed

0.5280.0160.544

system.time(ans2 <- DT[list("R","h")]) # binary search

user  system  elapsed

0.0040.0000.001
```
#######################################################

快速分组,按照x分组，然后计算sum(v)
```r
#tapply

system.time(tt <- tapply(DT$v,DT$x,sum)) 

user  system  elapsed

0.7040.0640.767

#syntax of data.table

system.time(ss <- DT[,sum(v),by=x]) 

user  system  elapsed

0.0800.0000.078

#cheak ss and tt

head(ss)  

x     V1

1: A 192213.2

2: B 192183.3

3: C 192601.7

4: D 192308.0

5: E 192428.5

6: F192071.0

head(tt)   

A        B        C        D        E        F

192213.2192183.3192601.7192308.0192428.5192071.0
```


其他基本操作   
联表和统计计算，更清晰认知，DT[i, j, by]中的i, j, by 对应着SQL语句的 i=where, j=select, by=group by。

```r
###Data preparation

DT = data.table(x=rep(c("a","b","c"),each=3), y=c(1,3,6), v=1:9) ##creat data.table DT

X = data.table(c("b","c"),foo=c(4,2)) ##use to join

setkey(DT,x)    #set the key

########################cheak the data#######

DT   

x y v

1: a 11

2: a 32

3: a 63

4: b 14

5: b 35

6: b 66

7: c 17

8: c 38

9: c 69

X

V1 foo

1:  b   4

2:  c   2
```
#################联表，注key1联的另一个表的第一列##################
```r
#join类型1，联表，X中有b,c

DT[X]   ##join X,by the key

x.   x  y  v  foo

1: b 144

2: b 354

3: b 664

4: c 172

5: c 382

6: c 692

##join类型2，类似查询,.() 表示list,类似于联一个1行2列的表DT[.("a",3)]     

x y v V2

1: a 113

2: a 323

3: a 633

#############################计算建表##############

where=DT, select=sum(v)....., group by

DT$xDT[,list(MySum=sum(v), 

        MyMin=min(v), 

        MyMax=max(v)),   by=.(x)]

       x MySum MyMin MyMax

1: a     613

2: b    1546

3: c    2479
```
