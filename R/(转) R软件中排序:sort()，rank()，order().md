在R中，和排序相关的函数主要有三个：sort()，rank()，order()。
 

sort(x)是对向量x进行排序，返回值排序后的数值向量。rank()是求秩的函数，它的返回值是这个向量中对应元素的“排名”。而order()的返回值是对应“排名”的元素所在向量中的位置。

下面以一小段R代码来举例说明：
```r
> x<-c(97,93,85,74,32,100,99,67)
> sort(x)
[1]  32  67  74  85  93  97  99 100
> order(x)
[1] 5 8 4 3 2 1 7 6
> rank(x)
[1] 6 5 4 3 1 8 7 2
```
 假设x为一组学生完成某项测试所花费的时间（所用时间越短，排名越靠前），rank()的返回值是这组学生所对应的排名，而order()的返回值是各个排名的学生成绩所在向量中的位置。  

前一段同学问我一个问题，如何返回一个数值向量中满足某条件的元素在向量中的位置？举例来说，
```r
x<- c(97,93,85,74,32,100,99,67)
```
希望返回x中满足值大于50且小于90的元素在向量x中的下标。当时想了想，没觉得有什么好的 方法，使用了比较繁琐的语句
```r
sort(x,index.return=TRUE)[[2]]
[sort(x,index.return=TRUE)[[1]]<90& sort(x,index.return=TRUE)[[1]]>50]，
```
后来发现

sort(x,index.return=TRUE)[[2]] 和order(x)的返回值是一样的，而sort(x,index.return=TRUE)[[1]]和sort(x)的返回值是相同的，因此语句可以简化为order(x)[sort(x)>50&sort(x)<90]。下面是相关的R代码：
```r
> x
[1]  97  93  85  74  32 100  99  67
> sort(x,index.return=TRUE)[[2]][sort(x,index.return=TRUE)[[1]]<90&sort(x,index.return=TRUE)[[1]]>50]
[1] 8 4 3
> order(x)[sort(x)>50&sort(x)<90]
[1] 8 4 3
> sort(x,index.return=TRUE)
$x
[1]  32  67  74  85  93  97  99 100

$ix
[1] 5 8 4 3 2 1 7 6
> order(x)
[1] 5 8 4 3 2 1 7 6
```
