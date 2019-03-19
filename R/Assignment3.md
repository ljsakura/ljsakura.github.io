1、
有关医院数据的问题，给出了一张下属于各州的医院名单，其中包含了所列医院近30天内死于各种疾病的概率  
题目1便是要在给定的州（state）和疾病名称（outcome）情况下，计算在该outcome所列示的范畴内，近30天死于该疾病的概率最低的医院，并且要求当最低概率不止一个时，还要按照医院的名称进行排序，去首字母最靠前的那家医院并列示出来  
题目要求还包括判断州名和疾病名称是否存在，即state是否包含在state列中，而outcome是否包含在"heart attack","heart failure","pneumonia"这三个里

问题点为以下四个：  
首字母大小写转换；  
字符串拼接；  
字符串替代；  
排序  

首先是首字母大小写转换，这个主要是因为所有的列名称在进入read.csv之后都变成了首字母大写的形式，而题干要求录入的是首字母小写的outcome，所以必须做首字母大小写转换；

其次是字符串拼接，由于题干只要求我们录入疾病名称，就是前面提到的那三种outcome，但其实列名称全称如下所示
```r
[11] "Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack"  
[17] "Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure"  
[23] "Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia"      
```
所以还要将赋值后的outcome与Hospital.30.Day.Death..Mortality..Rates.from.进行衔接；

再次是字符串替代，同样是因为列名称进入read.csv之后，如上条所示，所有的空格及符号（包括——，（，）等等）都变成了实心圆点“.”，所以需要对上述字符串做替代；

最后是排序，相对来说，排序涉及到的函数比较简单
```r
best <- function(state, outcome) {
newdir<-paste(getwd(),"ProgAssignment3-data",sep = "/")
  x<-read.csv(paste(newdir,"outcome-of-care-measures.csv",sep = "/"),colClasses = "character")
outcome<-unlist(strsplit(outcome," "))
  #这里使用了unlist是为了让strsplit分割后的文本能够以变量的形式显示出来
outcome<-paste(toupper(substring(outcome,1,1)),substring(outcome,2),sep = "",collapse = " ")
  #将上一句执行出来的变量对首字母进行大写转换，并将转换之后的文本再用空格串联起来
  if(state %in% x$State){
    if(outcome %in% c("Heart Attack","Heart Failure","Pneumonia")){
    a<-subset(x,x$State==state)
    b<-paste("Hospital 30-Day Death (Mortality) Rates from",outcome,sep = " ")
    #将outcome与Hospital 30-Day Death (Mortality) Rates from衔接
    b<-gsub("([-() ])", ".", b)
    #替换空格和符号为实心圆点
    a[[b]]<-as.numeric(a[[b]])
   c<-subset(a,a[[b]]==min(a[[b]],na.rm=TRUE))
   d<-sort(c["Hospital.Name"])
    print(d[1,])
    #排序并输出第一位
    }else print("invalid outcome")
  }else print("invalid state")
}

best("TX", "Heart Attack")

[1] "CYPRESS FAIRBANKS MEDICAL CENTER" Warning message: In best("TX", "Heart Attack") : NAs introduced by coercion 报警是因为什么暂时还没想明白

> best("MD", "Pneumonia")

best("MD", "Pneumonia")

[1] "GREATER BALTIMORE MEDICAL CENTER"
```
2、
改到吐血的一道题啊  
讲讲我和这道题不得不说的血泪史

题目2在题目1要求的基础上又新加了几点，在已知州名和疾病名称的情况下，查找排名在num上的医院名，而且当num为best时，即表示排名第一，相反worst则表示排名最后

if else部分便是相较于题目1增加的

本来想用rank函数，可以快速定位排名，可惜当序列中有重复项时，rank的结果便会出现2.5,2.5或者3.3,3.3,3.3  
可以看出，当该值出现了两次时，则有0.5出现，当该值出现了三次时，则有0.3出现

这是多么变态的一个设定啊（当然，可能在其他地方会很有用处，不过在这道题里，真是一点都用不上）

于是只好用order对数据从新排序，x（order（x））便是对x进行正序排列，程序里也调用了这一项

```r
rankhospital <- function(state, outcome, num = "best") {
newdir<-paste(getwd(),"ProgAssignment3-data",sep = "/")
x<-read.csv(paste(newdir,"outcome-of-care-measures.csv",sep = "/"),colClasses = "character")
outcome<-unlist(strsplit(outcome," "))
outcome<-paste(toupper(substring(outcome,1,1)),substring(outcome,2),sep = "",collapse = " ")
  if(state %in% x$State){
    if(outcome %in% c("Heart Attack","Heart Failure","Pneumonia")){
      a<-subset(x,x$State==state)
      b<-paste("Hospital 30-Day Death (Mortality) Rates from",outcome,sep = " ")
      b<-gsub("([-() ])", ".", b)
      a[[b]]<-as.numeric(a[[b]])
      if(num=="best"){
       c<-subset(a,a[[b]]==min(a[[b]],na.rm=TRUE))
      }else if(num=="worst"){
       c<-subset(a,a[[b]]==max(a[[b]],na.rm=TRUE))
      }else if(num<=nrow(a)){
       a<-a[order(a[[b]],a["Hospital.Name"]),]
        c<-a[num,]
      }else print("NA")
     d<-sort(c["Hospital.Name"])
      print(d[1,])
    }else print("invalid outcome")
  }else print("invalid state")
}
> rankhospital("TX","heart failure",4) 

[1] "DETAR HOSPITAL NAVARRO" 

Warning message: In rankhospital("TX", "heart failure", 4) : NAs introduced by coercion

> rankhospital("MD", "heart attack", "worst") 

[1] "HARFORD MEMORIAL HOSPITAL" 

Warning message: In rankhospital("MD", "heart attack", "worst") : NAs introduced by coercion > rankhospital("MN", "heart attack", 5000) 

[1] "NA"

Error in `[.data.frame`(c, "Hospital.Name") : undefined columns selected

```


In addition: Warning message: In rankhospital("MN", "heart attack", 5000) : NAs introduced by coercion

作业描述里也有提到，这次的作业运行程序会狂报错，不要理会就好，SO，我当他们都是真空了

3、
又是一个血泪史  
第3题要求是罗列出所有state下面排名在num上的医院名字以及所在州名，同时要求行名称为州名  
考虑用split做划分，而且需要混合排序
```r
rankall <- function(outcome, num = "best") {
newdir<-paste(getwd(),"ProgAssignment3-data",sep = "/")
x<-read.csv(paste(newdir,"outcome-of-care-measures.csv",sep = "/"),colClasses = "character")
outcome<-unlist(strsplit(outcome," "))
outcome<-paste(toupper(substring(outcome,1,1)),substring(outcome,2),sep = "",collapse = " ")
  if(outcome %in% c("Heart Attack","Heart Failure","Pneumonia")){
    b<-paste("Hospital 30-Day Death (Mortality) Rates from",outcome,sep = " ")
    b<-gsub("([-() ])", ".", b)
    x[[b]]<-as.numeric(x[[b]])
   a<-x[order(x$State,x[[b]],x["Hospital.Name"]),]
   newdata<-split(a[,c(b,"Hospital.Name","State")],a$State)
    newdata<-na.omit(newdata)
    d<-data.frame()
    for(sta in unique(x$State)){
      if(num=="best"){
       temp<-newdata[[sta]][1,c("Hospital.Name","State")]
      }else if(num=="worst"){
       newdata[[sta]]<-newdata[[sta]][order(-newdata[[sta]][b],newdata[[sta]]["Hospital.Name"]),]
        #新学会的，升序和降序同时存在时，可以用负号来表示降序
       temp<-newdata[[sta]][1,c("Hospital.Name","State")]
      }else if(num<=nrow(newdata[[sta]])){
       temp<-newdata[[sta]][num,c("Hospital.Name","State")]
      }else temp<-c("NA",sta)
      d<-rbind(d,temp)
      colnames(d)<-c("Hospital.Name","State")
      rownames(d)<-d$State
    }
  }else print("invalid outcome")
  d<-d[order(d$State),]
}
>   head(rankall("heart attack", 20), 10)                          

Hospital.Name State

AK                                  NA    AK

AL      D W MCMILLAN MEMORIAL HOSPITAL    AL

AR   ARKANSAS METHODIST MEDICAL CENTER    AR

AZ JOHN C LINCOLN DEER VALLEY HOSPITAL    AZ

CA               SHERMAN OAKS HOSPITAL    CA

CO            SKY RIDGE MEDICAL CENTER    CO

CT             MIDSTATE MEDICAL CENTER    CT

DC                                  NA    DC

DE                                  NA    DE

FL      SOUTH FLORIDA BAPTIST HOSPITAL    FL

Warning message: In rankall("heart attack", 20) : NAs introduced by coercion

>  tail(rankall("pneumonia", "worst"), 3)                                 

Hospital.Name State

WI MAYO CLINIC HEALTH SYSTEM - NORTHLAND, INC    WI

WV                     PLATEAU MEDICAL CENTER    WV

WY           NORTH BIG HORN HOSPITAL DISTRICT    WY

Warning message: In rankall("pneumonia", "worst") : NAs introduced by coercion

>  tail(rankall("heart failure"), 10)                                                        

Hospital.Name State

TN                        WELLMONT HAWKINS COUNTY MEMORIAL HOSPITAL    TN

TX                                       FORT DUNCAN MEDICAL CENTER    TX

UT  VA SALT LAKE CITY HEALTHCARE - GEORGE E. WAHLEN VA MEDICAL CENTER  UT

VA                                         SENTARA POTOMAC HOSPITAL    VA

VI                           GOV JUAN F LUIS HOSPITAL & MEDICAL CTR    VI

VT                                             SPRINGFIELD HOSPITAL    VT

WA                                        HARBORVIEW MEDICAL CENTER    WA

WI                                   AURORA ST LUKES MEDICAL CENTER    WI

WV                                        FAIRMONT GENERAL HOSPITAL    WV

WY                                       CHEYENNE VA MEDICAL CENTER    WY

Warning message: In rankall("heart failure") : NAs introduced by coercion
```
