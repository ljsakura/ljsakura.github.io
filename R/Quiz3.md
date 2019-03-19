有关花瓣和花萼观测值数据集的调用  
首先看一下这组数据集
```r
> iris
    Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
1            5.1         3.5          1.4         0.2     setosa
2            4.9         3.0          1.4         0.2     setosa
3            4.7         3.2          1.3         0.2     setosa
4            4.6         3.1          1.5         0.2     setosa
5            5.0         3.6          1.4         0.2     setosa
6            5.4         3.9          1.7         0.4     setosa
7            4.6         3.4          1.4         0.3     setosa
8            5.0         3.4          1.5         0.2     setosa
9            4.4         2.9          1.4         0.2     setosa
10           4.9         3.1          1.5         0.1     setosa
……
```
因为Speciies是文本型的，所以导致了后来的很多调试无法通过  
我曾尝试过使用Split对Species进行分组，这一步很成功，可是后来各列求均值就无法进行下去了，错误提示为参数不是数值也不是逻辑值：回复NA    
后来发现是由于最后一列的文本导致的，所以果断剔除掉    
按Species分组求Sepal.Length列均值  
最简单的是下面这种写法
```r
> tapply(iris$Sepal.Length,iris$Species,mean)
    setosa versicolor  virginica 
     5.006      5.936      6.588 
```
不过因为想调用Split，所以坚持下发现还可以这样写，就是麻烦些
```r
> a<-split(iris[,c("Sepal.Length","Sepal.Width","Petal.Length","Petal.Width")],iris$Species)
> a<-split(iris[,"Sepal.Length"],iris$Species)
> lapply(a,mean)
$setosa
[1] 5.006

$versicolor
[1] 5.936

$virginica
[1] 6.588

> sapply(a,mean)
    setosa versicolor  virginica 
     5.006      5.936      6.588 
> mapply(mean,a)
    setosa versicolor  virginica 
     5.006      5.936      6.588 
```
这一章主要是学各种apply，所以就都用了一遍，不过apply一直报错：dim(X)的值必需是正数  
表示很困惑,似乎是apply不能对list做操作  

车辆信息数据集调用  
同样有一个分组求均值的问题，cylinder（cyl）数量不同的情况下，gallon（mpg）的平均消耗量
```r
> sapply(split(mtcars$mpg,mtcars$cyl),mean)
       4        6        8 
26.66364 19.74286 15.10000 
```
四缸与八缸车辆在hp上的差别
```r
> x<-sapply(split(mtcars$hp,mtcars$cyl),mean)
> x
        4         6         8 
82.63636 122.28571 209.21429 
> x[1]-x[3]
        4 
-126.5779
```
