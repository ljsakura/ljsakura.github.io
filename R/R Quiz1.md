测试1:  
读取外部数据  
```r
read.csv("C:/******/hw1_data.csv")  
```
读取前n行
```r
head(x,n)
```
OR
```
x[1:n]
```
读取后n行（共计m行）
```r
tail(x,n)
```
OR
```r
x[m-1,m]
```
求行数
```r
nrow(x)
```
读取第n行
```r
x[n,]
```
读取第n行第m列数据值
```
x[n,m]    /** 如列名称为文本，则需标注双引号
```
求Ozone列非NA均值
```r
mean(x[["Ozone"]][!is.na(x[["Ozone"]])])
```
OR
```r
mean(x$Ozone[!is.na(x$Ozone)])
```
求Ozone列缺失值个数
```r
sum(is.na(x[,"Ozone"]))
```
OR
```r
sum(is.na(x$Ozone))
```
取Ozone大于31并且Temp大于90的数据子集，并计算行数
```r
nrow(subset(x,x$Ozone>31&x$Temp>90)) 
```
对上述子集求列均值
```r
mean(subset(x,x$Ozone>31&x$Temp>90)[["Solar.R"]])
```
求Month为6时Temp的均值
```r
mean(subset(x,x$Month==6)[["Temp"]])／**要用恒等
```
求Month为5时Ozone的最大值
```r
max(subset(x,x$Month==5&!is.na(x$Ozone))[["Ozone"]])
```
在网上看到有人用Remove，觉得比我这种方法更简单，借用下，Y(^_^)Y
```r
> m <- subset(mydata,mydata$Month==5)
> mean(m$Ozone,na.rm=TRUE)
```


替换实例： 
```r
> k<-subset(x,x$Ozone>31&x$Temp>90) 
> k 
    Ozone Solar.R Wind Temp Month Day 
69     97     267  6.3   92     7   8 
70     97     272  5.7   92     7   9 
120    76     203  9.7   97     8  28 
121   118     225  2.3   94     8  29 
122    84     237  6.3   96     8  30 
123    85     188  6.3   94     8  31 
124    96     167  6.9   91     9   1 
125    78     197  5.1   92     9   2 
126    73     183  2.8   93     9   3 
127    91     189  4.6   93     9   4 
```
Ozone为97的替换为6 
```r
> 6->k$Ozone[k$Ozone==97] 
> k 
    Ozone Solar.R Wind Temp Month Day 
69      6     267  6.3   92     7   8 
70      6     272  5.7   92     7   9 
120    76     203  9.7   97     8  28 
121   118     225  2.3   94     8  29 
122    84     237  6.3   96     8  30 
123    85     188  6.3   94     8  31 
124    96     167  6.9   91     9   1 
125    78     197  5.1   92     9   2 
126    73     183  2.8   93     9   3 
127    91     189  4.6   93     9   4
```
