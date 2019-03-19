ID3算法中指定数据集划分的熵表达式为：

 

第j 种属性——即属性A的划分在整体数据集中的期望为：



故属性A的信息增益为：



 

ID3算法即为计算每处节点下信息增益最大的属性。

 

1、
```python
import math ## as to use the log function, math module must import first

def cal(dataset): ## calculate the Entropy of given dataSet

       num=len(dateset)

       columncount={}

       for element in dataset:

              columnname=element[-1]

              if columnname not in columncount.keys():

                      columncount[columnname]=0

              columncount[columnname]+=1

       result=0.0

       for key in columncount:

              prob=float(columncount[key])/num ## must change columncount[key] to float, otherwise it will return error

              result -=prob*math.log(prob,2) ## here should use math.log but not log as we imoport math at the very first line, if we use 'imoport log from math' at beginning, we could use log here

       return result
```
 

2、
```python
def creatDataSet():

       dataSet=[[*,*……,*]……[*,*,……,*]]## you can change * with charactor or int or float

       columnname=[*,*,……,*]

       return dataSet,columnname
```
 

3、
```python
def split(dataSet,column,value): ## split subset by each/given column

       subset=[]

       for element in dataSet:

              if element[column]==value:

                      partset=element[:column]

                      partset.extend(element[column+1:]) ##  .extend add the element, 

                      subset.append(partset) ## .append add a list, on the other hand, it add a object

       return subset
```
 

4、
```python
def bestsubset(dataSet): ## calculate and find the best split subset for each/given node

       baseEntropy=cal(dataSet)

       columnnum=len(dataSet[0]-1)

       bestinfoGain=0.0

       bestfac=-1

       for i in range(columnnum):

              factorlist=[factor[i] for factor in dataSet]

              uniquefac=set(factorlist) ## set() get the unique value of a list, as the same function of factor in R studio

              newEntropy=0.0

              for value in uniquefac:

                      subset=split(dataSet,i,value)

                      prob=float(len(subset))/float(len(dataSet))

                      newEntropy += prob*cal(subset)

              infoGain=baseEntropy-newEntropy

              if infoGain>bestinfoGain:

                      bestinfoGain=infoGain

                      bestfac=i

       return bestfac
```
 

5、
```python
import operator ## import operator module before use .itemgetter()

def mostcount(classlist):## get the most frequently result

       classcount={}

       for value in classlist:

              if value not in classcount.keys():

                      classcount[value]=0

              classcount[value] += 1

       sortclasslist=sorted(classcount.iteritems(),key=operator.itemgetter(1),reverse=True)

       return sortclasslist[0][0]
```
 

6、
```python
def creatTree(dataSet,columnname):

       classlist=[example[-1] for example in dataSet]

       if classlist.count(classlist[0]) == len(classlist): ## when class is full of one result, there is unnesessary to find the next node

              return classlist[0]

       if len(dataSet[0]) == 1: # if length of each element in dataSet is equal with 1, it means there isn't any other attribute that we could use to split the subset

              reutrn mostcount(classlist)

       bestfac=bestsubset(dataSet)

       bestfaclable=columnname(bestfac)

       myTree={bestfaclable:{}}

       del(columnname(bestfac)) #remove the bestfac_columnname

       facvalue=[example[bestfac] for example in dataSet]

       uniquevale=set(facvalue)

       for value in uniquevale:

              subcolumnname=columnname[:]

              myTree[bestfaclable][value]=creatTree(split(dataSet,bestfac,value),subcolumnname)

       return myTree
```
对于属性少的数据集可以采用ID3算法，但当属性增多，ID3算法的精准性迅速下降。

在此基础上改进的C4.5算法弥补了该项缺点。

故而当属性少时，采用ID3；属性多时，采用C4.5。

 

相对ID3而言，C4.5新增了信息增益率，即

属性A的分列信息为：



属性A的信息增益率表示为：



C4.5算法只需将第4部分进行更改，code如下：
```python
def bestsubset(dataSet): ## calculate and find the best split subset for each/given node

       baseEntropy=cal(dataSet)

       columnnum=len(dataSet[0]-1)

       bestinfoGain=0.0

       bestinfoGainRatio=0.0

       bestfac=-1

       for i in range(columnnum):

              factorlist=[factor[i] for factor in dataSet]

              uniquefac=set(factorlist) ## set() get the unique value of a list, as the same function of factor in R studio

              newEntropy=0.0

              splitAtt=0.0

              for value in uniquefac:

                      subset=split(dataSet,i,value)

                      prob=float(len(subset))/float(len(dataSet))

                      splitAtt -= prob*math.log(prob,2) ## get split information

                      newEntropy += prob*cal(subset)

              infoGain=baseEntropy-newEntropy

              infoGainRatio=float(infoGain)/float(splitAtt) ##calculate Gain_Ratio

              if infoGainRatio>bestinfoGainRatio:

                      bestinfoGainRatio=infoGainRatio

                      bestfac=i

       return bestfac
```
