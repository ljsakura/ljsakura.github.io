在R学习过程中，遇到了ifelse、which、%in%，下面分别举例，说明他们的用法。

1、ifelse
```r
ifelse(test, yes, no)
```
test为真，输出yes值，否则输出no值。

举例如下：
```r
> x <- c(1,1,1,0,0,1,1)

> ifelse(x != 1, 1, 0) #若果x的值不等于1，输出1，否则输出0

[1] 0 0 0 1 1 0 0
```


2、which

用法which(test)。

返回test为真值的位置（指针）。

举例如下：
```r
> which(x!=1) #返回x中不等于1的变量值得位置

[1] 4 5

> which(c(T,F,T)) #返回c(T,F,T)中为TURE值的位置。

[1] 1 3

> which(c(1,0,1)) #只对T, F做判断。

Error in which(c(1, 0, 1)) : argument to 'which' is not logical
```


3、%in%

用法 a %in% table

a值是否包含于table中，为真输出TURE，否者输出FALSE

例如
```r
> x %in% 1

[1]  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE

> x %in% c(1, 0)

[1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE

> x %in% c(1, 2)

[1]  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE
```




联合使用
```r
> ifelse(x %in% 1, 1, 0) #若x的值包含在1里面，输出1，否者输出0

[1] 1 1 1 0 0 1 1

> ifelse(x %in% 1, 'yes', 'no') #若x的值包含在1里面，输出yes，否者输出no

[1] "yes" "yes" "yes" "no"  "no"  "yes" "yes"

> which(x %in% 1) 输出x包含在1中值的位置

[1] 1 2 3 6 7

> y <- c(2, 1, 3, 4)

> z <- c(1, 4)

> ifelse(y %in% z, which(y==z), 0 ) ##若y的值包含在z里面，输出y==z的位置，否者输出0

[1] 0 2 0 4

> ifelse(y %in% z, which(y==z), 0 ) ##若y的值包含在z里面，输出y==z的位置，否者输出0，

                                     #此例中没有找到y==z的值， 输出为NA。

[1] NA  0  0 NA

> ifelse(y %in% z, 1, 0 )

[1] 1 0 0 1
```
