我们谈到“文本处理”时，我们通常是指处理的内容。Python 将文本文件的内容读入可以操作的字符串变量非常容易。文件对象提供了三个“读”方法：   
.read()、.readline() 和 .readlines()。  
每种方法可以接受一个变量以限制每次读取的数据量，但它们通常不使用变量。 .read() 每次读取整个文件，它通常用于将文件内容放到一个字符串变量中。然而 .read() 生成文件内容最直接的字符串表示，但对于连续的面向行的处理，它却是不必要的，并且如果文件大于可用内存，则不可能实现这种处理。

.readline() 和 .readlines() 非常相似。它们都在类似于以下的结构中使用：

Python .readlines() 示例
```python
fh = open( 'c:\\autoexec.bat')         
  for line in fh.readlines():                     
  print  line 
```
.readline() 和 .readlines()之间的差异是后者一次读取整个文件，象 .read()一样。.readlines()自动将文件内容分析成一个行的列表，该列表可以由 Python 的 for... in ... 结构进行处理。另一方面，.readline()每次只读取一行，通常比 .readlines()慢得多。仅当没有足够内存可以一次读取整个文件时，才应该使用.readline()。   

写：

writeline()是输出后换行，下次写会在下一行写。write()是输出后光标在行末不会换行，下次写会接着这行写



[python]view plaincopyprint?

通过readline输出，对于比较大的文件，这种占用内存比较小。  
```python
#coding:utf-8  
f = open('poem.txt','r')  
result = list()  
for line in open('poem.txt'):  
    line = f.readline()  
    print line  
    result.append(line)  
print result  
f.close()                  
open('result-readline.txt', 'w').write('%s' % '\n'.join(result))  
```



[python]view plaincopyprint?
```python
#coding:utf-8  
'''''cdays-4-exercise-6.py 文件基本操作 
    @note: 文件读取写入, 列表排序, 字符串操作 
    @see: 字符串各方法可参考hekp(str)或Python在线文档http://docs.python.org/lib/string-methods.html 
'''  
  
f = open('cdays-4-test.txt', 'r')          #以读方式打开文件  
result = list()  
for line in f.readlines():              #依次读取每行  
    line = line.strip()              #去掉每行头尾空白  
    if not len(line) or line.startswith('#'): #判断是否是空行或注释行  
        continue                #是的话，跳过不处理  
    result.append(line)               #保存  
result.sort()                     #排序结果  
print result  
open('cdays-4-result.txt', 'w').write('%s' % '\n'.join(result))#保存入结果文件  
```
