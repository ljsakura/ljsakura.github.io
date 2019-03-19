apply函数（对一个数组按行或者按列进行计算）：  
使用格式为：
```r
apply(X, MARGIN, FUN, ...)
```
其中X为一个数组；MARGIN为一个向量（表示要将函数FUN应用到X的行还是列），若为1表示取行，为2表示取列，为c(1,2)表示行、列都计算。   
示例代码： 
```r
> ma <- matrix(c(1:4, 1, 6:8), nrow = 2) 
> ma 
     [,1] [,2] [,3] [,4] 
[1,]    1    3    1    7 
[2,]    2    4    6    8 
> apply(ma, c(1,2), sum) 
     [,1] [,2] [,3] [,4] 
[1,]    1    3    1    7 
[2,]    2    4    6    8 
> apply(ma, 1, sum) 
[1] 12 20 
> apply(ma, 2, sum) 
[1]  3  7  7 15 
```
函数tapply（进行分组统计）：
使用格式为：
```r
tapply(X, INDEX, FUN = NULL, ..., simplify = TRUE)
```
其中X通常是一向量；INDEX是一个list对象，且该list中的每一个元素都是与X有同样长度的因子；FUN是需要计算的函数；simplify是 逻辑变量，若取值为TRUE（默认值），且函数FUN的计算结果总是为一个标量值，那么函数tapply返回一个数组；若取值为FALSE，则函数 tapply的返回值为一个list对象。需要注意的是，当第二个参数INDEX不是因子时，函数 tapply() 同样有效，因为必要时 R 会用 as.factor()把参数强制转换成因子。   
示例代码： 
```r
> fac <- factor(rep(1:3, length = 17), levels = 1:5) 
> fac 
 [1] 1 2 3 1 2 3 1 2 3 1 2 3 1 2 3 1 2 
Levels: 1 2 3 4 5 
> tapply(1:17, fac, sum) 
1  2  3  4  5 
51 57 45 NA NA 
> tapply(1:17, fac, sum, simplify = FALSE) 
$`1` 
[1] 51 

$`2` 
[1] 57 

$`3` 
[1] 45 

$`4` 
NULL 

$`5` 
NULL 
> tapply(1:17, fac, range) 
$`1` 
[1]  1 16 

$`2` 
[1]  2 17 

$`3` 
[1]  3 15 

$`4` 
NULL 

$`5` 
NULL 
```
#利用tapply实现类似于excel里的数据透视表的功能： 
```r
> da 
   year province sale 
1  2007        A    1 
2  2007        B    2 
3  2007        C    3 
4  2007        D    4 
5  2008        A    5 
6  2008        C    6 
7  2008        D    7 
8  2009        B    8 
9  2009        C    9 
10 2009        D   10 
> attach(da) 
> tapply(sale,list(year,province)) 
 [1]  1  4  7 10  2  8 11  6  9 12 
> tapply(sale,list(year,province),mean) 
      A  B C  D 
2007  1  2 3  4 
2008  5 NA 6  7 
2009 NA  8 9 10 
```
函数table（求因子出现的频数）：  
使用格式为：
```r
table(..., exclude = if (useNA == "no") c(NA, NaN), useNA = c("no",

    "ifany", "always"), dnn = list.names(...), deparse.level = 1)
```
其中参数exclude表示哪些因子不计算。   
示例代码： 
```r
> d <- factor(rep(c("A","B","C"), 10), levels=c("A","B","C","D","E")) 
> d 
 [1] A B C A B C A B C A B C A B C A B C A B C A B C A B C A B C 
Levels: A B C D E 
> table(d) 
d 
A  B  C  D  E 
10 10 10  0  0 
> table(d, exclude="B") 
d 
A  C  D  E 
10 10  0  0 
```
函数lapply与函数sapply：  
lapply的使用格式为：
```r
lapply(X, FUN, ...)
```
lapply的返回值是和一个和X有相同的长度的list对象，这个list对象中的每个元素是将函数FUN应用到X的每一个元素。其中X为List对象（该list的每个元素都是一个向量）  ，其他类型的对象会被R通过函数as.list()自动转换为list类型。   
函数sapply是函数lapply的一个特殊情形，对一些参数的值进行了一些限定，其使用格式为：
```r
sapply(X, FUN,..., simplify = TRUE, USE.NAMES = TRUE)
```
sapply(*, simplify = FALSE, USE.NAMES = FALSE) 和lapply(*)的返回值是相同的。如果参数simplify=TRUE，则函数sapply的返回值不是一个list，而是一个矩阵；若simplify=FALSE，则函数sapply的返回值仍然是一个list。  
示例代码：
```r
> x <- list(a = 1:10, beta = exp(-3:3), logic = c(TRUE,FALSE,FALSE,TRUE))
> lapply(x, quantile)
$a
   0%   25%   50%   75%  100%
1.00  3.25  5.50  7.75 10.00

$beta
         0%         25%         50%         75%        100%
0.04978707  0.25160736  1.00000000  5.05366896 20.08553692

$logic
  0%  25%  50%  75% 100%
0.0  0.0  0.5  1.0  1.0

> sapply(x, quantile,simplify=FALSE,use.names=FALSE)
$a
   0%   25%   50%   75%  100%
1.00  3.25  5.50  7.75 10.00

$beta
         0%         25%         50%         75%        100%
0.04978707  0.25160736  1.00000000  5.05366896 20.08553692

$logic
  0%  25%  50%  75% 100%
0.0  0.0  0.5  1.0  1.0
#参数simplify=TRUE的情况
> sapply(x, quantile)
         a        beta logic
0%    1.00  0.04978707   0.0
25%   3.25  0.25160736   0.0
50%   5.50  1.00000000   0.5
75%   7.75  5.05366896   1.0
100% 10.00 20.08553692   1.0
```
函数mapply：  
函数mapply是函数sapply的变形版，mapply 将函数 FUN 依次应用每一个参数的第一个元素、第二个元素、第三个元素上。函数mapply的使用格式如下：
```r
mapply(FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE,USE.NAMES = TRUE)
```
其中参数MoreArgs表示函数FUN的参数列表。  
示例代码：
```r
> mapply(rep, times=1:4, x=4:1)
[[1]]
[1] 4

[[2]]
[1] 3 3

[[3]]
[1] 2 2 2

[[4]]
[1] 1 1 1 1
```
#直接使用函数rep的结果：
```r
> rep(1:4,1:4)
 [1] 1 2 2 3 3 3 4 4 4 4
 ```
