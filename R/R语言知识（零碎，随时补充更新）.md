1、eval&parse  
    eval计算函数或expression的值，parse只显示函数或expression  
    例：
```r
> eval({ xx <- pi; xx^2}) ; xx [1] 9.869604 [1] 3.141593
> parse(text = "a") expression(a)
```
2、complete.cases&na.omit  
    在之前的Assignment里面有介绍过这两个函数，用来剔除NA值  
```r
> y<-x[complete.cases(x),]
> y<-na.omit(x)
> y<-x[complete.cases(x[,m:n]),] 
```
也可以作用于列

3、移除某一行或某几行，用“-”  
    例：
```r
> x<-matrix(1:9,3)

 > x[,-1]      

[,1] [,2] 

[1,]    4    7

[2,]   5    8

[3,]    6   9
> x<-matrix(1:16,4) 

> x      

[,1] [,2] [,3] [,4]

[1,]    1    5   9   13

[2,]    2   6   10   14

[3,]   3    7   11  15

[4,]    4    8  12   16

> x[,-c(1,2)]      

[,1] [,2] 

[1,]    9  13

[2,]   10   14

[3,]  11   15

[4,]   12  16
```
4、判断两组data.frame是否完全相等，any&identical，查找不同数据的位置用which  
    例：
```r
> x<-c(1,2) > y<-c(2,3) > any(x!=y) [1] TRUE
> z<-c(1,2) > any(x!=z) [1] FALSE
> identical(x,y) [1] FALSE > identical(x,z) [1] TRUE
> which(x!=y,arr.ind = TRUE) [1] 1 2 > which(x!=z,arr.ind = TRUE) integer(0)
```
5、duplicated查找重复值  
    例：
```r
> x[!duplicated(x)]
```
    去除重复值，同unique效果相同

6、cumsum累积求和  
    例：
```r
> cumsum(1:10)  [1]  1 3  6 10 15 21 28 36 45 55
```

7、tansform对列做变换  
    例：
```r
> x<-matrix(1:16,4)
> colnames(x)<-letters[1:4]
> transform(x,c=-c)   

a b  c  d

1 1 5  -9 13

2 2 6 -10 14

3 3 7 -11 15

4 4 8 -12 16
```
8、pmin&pmax针对多组平行向量  
    例：
```r
> pmin(5:1, pi) 

[1] 3.141593 3.141593 3.000000 2.000000 1.000000
```
9、D求偏导数

10、substring取字符串  
    例：
```r
> substring("abcdef",1:2,1:6) 

[1] "a"     "b"     "abc"   "bcd"   "abcde" "bcdef"
（1,1）（2,2）（1,3）（2,4）（1,5）（2,6）
```
11、difftime求时间差

12、%o%外积，%*% & crossprod & t(x)%*%y叉积

13、table求数据集因子频数  
    例：
```r
> a <- rep(c(NA, 1/0:3), 10) 

> table(a) 

a

0.333333333333333               0.5                 1               Inf                 

               10                10                10                10 

```
14、readline读取文本  
    例：
```r
> readline("Are you a satisfied R user? ") Are you a satisfied R user?  [1] ""
```

15、edit&fix打开表格格式对数据集进行编辑

16、data.table（i,j,by）
