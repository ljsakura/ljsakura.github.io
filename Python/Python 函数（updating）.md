1、去除空格  
函数：strip()  lstrip()  rstrip()

作用：去除字符串中的空格或指定字符

一、默认用法：去除空格
```python
str.strip()  ： #去除字符串两边的空格
str.lstrip() ： #去除字符串左边的空格
str.rstrip() ： #去除字符串右边的空格
```
注：此处的空格包含'\n', '\r',  '\t',  ' '


默认用法实例
```python
>>> dodo = "  hello boy " >>> dodo.strip() 'hello boy' >>> dodo.lstrip() 'hello boy ' >>> dodo.rstrip() '  hello boy'</span> 
```
二、去除指定字符 
```python
str.strip('do')  ：#去除字符串两端指定的字符 
str.lstrip('do') ：#用于去除左边指定的字符 
str.rstrip('do') ：#用于去除右边指定的字符 
```
三个函数都可以传入一个参数(这里以'do'为例)，指定要去除的首尾字符，编译器会去除两端所有相应的字符，直到没有匹配的字符 

注：   
1.去除指定字符时首尾不能出现空格，否则传入参数的时候也需要加入空格   
2.指定的字符表示的一种组合，例如'do'表示'dd','do','od','oo','ddd','ooo'等 

去除字符实例 
```python
>>> dodo = "say hello say boy saaayaaas" >>> dodo.strip('say') ' hello say boy ' >>> dodo.strip('yas') ' hello say boy ' #当传入的参数中加入空格时 >>> dodo.strip('say ') 'hello say bo' >>> dodo.lstrip('say') ' hello say boy saaayaaas' >>> dodo.rstrip('say') 'say hello say boy ' 
```
2、大小写转换   
Python为string对象提供了转换大小写的方法：upper() 和 lower().还不止这些,Python还为我们提供了首字母大写,其余小写的capitalize()方法,以及所有单词首字母大写,其余小写的title()方法. 

3、连接、拆分字符串   
python join 和 split方法的使用,join用来连接字符串，split恰好相反，拆分字符串的。

join用法示例 
```python
>>>li = ['my','name','is','bob'] 

>>>' '.join(li) 

'my name is bob' 

 

>>>'_'.join(li) 

'my_name_is_bob' 

 

>>> s = ['my','name','is','bob'] 

>>> ' '.join(s) 

'my name is bob' 

 

>>> '..'.join(s) 

'my..name..is..bob' 

 ```

split用法示例 


```python
>>> b = 'my..name..is..bob' 

 

>>> b.split() 

['my..name..is..bob'] 

 

>>> b.split("..") 

['my', 'name', 'is', 'bob'] 

 

>>> b.split("..",0) 

['my..name..is..bob'] 

 

>>> b.split("..",1) 

['my', 'name..is..bob'] 

 

>>> b.split("..",2) 

['my', 'name', 'is..bob'] 

 

>>> b.split("..",-1) 

['my', 'name', 'is', 'bob'] 
```
 

可以看出 b.split("..",-1)等价于b.split("..") 


4、替换replace   
Python replace() 方法把字符串中的 old（旧字符串） 替换成 new(新字符串)，如果指定第三个参数max，则替换不超过 max 次。
```python
str.replace(old,new[, max])   
#!/usr/bin/python
str ="this is string example....wow!!! this is really string";
print str.replace("is","was");
print str.replace("is","was",3);
```
以上实例输出结果如下：
```python
thwas was string example....wow!!! thwas was really string
thwas was string example....wow!!! thwas is really string
```
