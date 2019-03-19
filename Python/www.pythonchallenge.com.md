1、ord与chr转换ascii与字符
```python
def shift(str):
    newstr=''
    for i in str:
        if i>='a' and i<='z':
            i=ord(i)
            i=((i+2)-97)%26+97
            i=chr(i)
            newstr=newstr+i
        else:
            newstr=newstr+i
    return newstr

>>> shift("g fmnc wms bgblr rpylqjyrc gr zw fylb. rfyrq ufyr amknsrcpq ypc dmp. bmgle gr gl zw fylb gq glcddgagclr ylb rfyr'q ufw rfgq rcvr gq qm jmle. sqgle qrpgle.kyicrpylq() gq pcamkkclbcb. lmu ynnjw ml rfc spj.")
"i hope you didnt translate it by hand. thats what computers are for. doing it in by hand is inefficient and that's why this text is so long. using string.maketrans() is recommended. now apply on the url."
```
以下为简略写法：
```python
def move(str):
    newstr=''
    for i in str:
        if i>='a' and i<='z':
            i=ord(i)
            i=((i+2)-97)%26+97
            i=chr(i)
        newstr += i
    return newstr
```


2、open，read（），isalpha（）
```python
def cha(str):
    newstr=''
    for i in str:
        if i.isalpha():
            newstr += i
    return newstr
>>> file=open('C://Users//Administrator//Desktop//a.txt')  ## 必须用//，不能用\
>>> cha(file.read())
'equality'
```


3、正则表达式re.findall(pattern,string,flag=0)，查找前后均被三个大写字母环绕的小写字母
```python
import re
file=open('c://users//administrator//desktop//test.txt')
pattern="[a-z][A-Z][A-Z][A-Z][a-z][A-Z][A-Z][A-Z][a-z]"
newstr=''
for i in re.findall(pattern,file.read()):
    newstr += i[4]
##此处必须回车，否则后面的print会被认为隶属于for—loop，导致error    
print newstr

linkedlist
```


4、urllib模块，while 循环
```python
import urllib
nextnothing='12345' ## 初始值
while(True):
        filename=urllib.urlopen('http://www.pythonchallenge.com/pc/def/linkedlist.php?nothing=%s'%nextnothing)
        firstline=filename.readline()
        locate=firstline.find('is')
        length=len("is ")
        nextnothing=firstline[(locate+length):]
        print firstline
        if locate<=0:
            break
```
当运行结果出现 Yes. Divide by two and keep going. 时，上一条记录为16044，除2读入nextnothing，继续运行
当运行结果出现 peak.html 时，将文本放入 URL链接，跳转，出现and the next nothing is 72758，继续将72758读入 nextnothing运行，结果依旧是peak.html

所以正确结果是www.pythonchallenge.com/pc/def/peak.html
