1、先说一下第一题，由于是第一个作业的第一个程序，SO，花了我近三天的时间（智商低木有办法）  
因为不想用绝对路径，所以勤勤恳恳的向程序猿老公请教后，终于将这一部分调试出来了  
332个CSV文件，代表332天的观测，每个观测表中有日期和两类污染数的指标以及观测ID指标，此ID等同于CSV的文件名  
作业的要求是，定义一个function，参变量为directory，pollutant，id＝1:332  
```r
> pollutantmean<-function(directory,pollutant,id=1:332) {
+     newdir<-paste("~",directory,sep="/")  ＃相对路径引用
+     filenames<-list.files(newdir,pattern=".csv")
+     newfiles<-data.frame()
+     for (i in id) {
+         newfiles<-rbind(newfiles,read.csv(paste(newdir,filenames[i],sep="/")))        
+     }  ＃循环读取CSV文件&合并
+     x<-mean(newfiles[[pollutant]],na.rm=TRUE)
+     print(x)
+ }
> pollutantmean("specdata","sulfate",26:35)
[1] 1.792069
> pollutantmean("specdata","sulfate",34)
[1] 1.477143
> pollutantmean("specdata","sulfate",35)
[1] 1.343802
> pollutantmean("specdata","sulfate",26)
[1] 1.806355
> pollutantmean("specdata","sulfate",27)
[1] 3.041873
```
该部分貌似完美的进行完毕了，其实运行过程中曾出现过一个极大的问题：我一开始将mean放在了for循环里面，导致结果是这样的
```r
> pollutantmean("specdata","sulfate",26:35)
[1] 1.806355
[1] 2.258308
[1] 1.670668
[1] 2.041123
[1] 1.761492
[1] 1.663365
[1] 1.805421
[1] 1.852743
[1] 1.839783
[1] 1.792069
```
其运行结果代表了  
26 的均值  
26：27 的均值  
26：28 的均值  
……  
26：35 的均值  
由此可见，一步之差有多大的差距啊！



2、学到了两个很了不得的函数（至少在我这个水平看来是十分有用的）
```r
y<-x[complete.cases(x),]
y<-na.omit(x)
```
complete.cases 与 na.omit 均为去除包含NA行的函数

也可以去除部分列包含NA的行
```r
y<-x[complete.cases(x[,m:n]),] 
```
相对来说，第二道题做起来就趁手多了
求选取的观测表中观测变量值完整的行数
```r
complete <- function(directory, id = 1:332) {
  newdir<-paste("~",directory,sep = "/") ＃依旧用到了相对路径引用  
  filesnames<-list.files(newdir,pattern=".csv")
  newfiles<-matrix(NA,nrow = 0,ncol=2) ＃设置列数为2的空矩阵
  colnames(newfiles)<-c("id","nobs") ＃设定列名称
  for (i in id){
    x<-read.csv(paste(newdir,filesnames[i],sep = "/"))
    y<-na.omit(x)
    newfiles<-rbind(newfiles,c(i,nrow(y)))
  }
  print(newfiles)
}
complete("specdata",30:25)
     id nobs
[1,] 30  932
[2,] 29  711
[3,] 28  475
[4,] 27  338
[5,] 26  586
[6,] 25  463

> complete("specdata",c(2,4,8,10,12))
     id nobs
[1,]  2 1041
[2,]  4  474
[3,]  8  192
[4,] 10  148
[5,] 12   96
```

3、设定阈值threshold，当观测表的complete cases的数值大于阈值时，显示sulfate与nitrate的correlation
```r
corr <- function(directory, threshold = 0) {
  newdir<-paste("~",directory,sep = "/")
  filenames<-list.files(newdir,pattern=".csv")
  newdata<-vector()
  for(i in 1:332){
    newfiles<-read.csv(paste(newdir,filenames[i],sep = "/"))
    x<-na.omit(newfiles)
    if(nrow(x)>=threshold){
      newdata<-rbind(newdata,cor(x$sulfate,x$nitrate))
    }else
      newdata
  }
  print(newdata)
}

cr<-corr("specdata")summary(cr)
> summary(cr)
       V1          
Min.   :-1.00000  
'1st Qu.:-0.05282  
Median : 0.10719  
Mean   : 0.13684  
3rd Qu.: 0.27831  
Max.   : 1.00000  
NA's   :9    

> length(cr)
[1] 332


cr <- corr("specdata",150)
head(cr)
summary(cr)

> head(cr)
            [,1]
[1,] -0.01895754
[2,] -0.14051254
[3,] -0.04389737
[4,] -0.06815956
[5,] -0.12350667
[6,] -0.07588814
> summary(cr)
       V1          
Min.   :-0.21057  
1st Qu.:-0.05147  
Median : 0.09333  
Mean   : 0.12401  
3rd Qu.: 0.26836  
Max.   : 0.76313  

cr <- corr("specdata",5000)
head(cr)
summary(cr)

> cr <- corr("specdata",5000)
logical(0)
> head(cr)
logical(0)
> summary(cr)
   Mode    NA's 
logical       0
```
