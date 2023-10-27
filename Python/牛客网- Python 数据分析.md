DA1 用pandas查看牛客网用户数据:

入门  通过率：15.94%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
你可以使用pandas打开文件，偷偷看一下里面的内容，请输出你看到的前6行数据。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/cf55a3da-a8b4-41d2-9347-319af5cb46e0)<br/>
输出描述：<br/>
输出该数据集的前6行，如下所示：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/8f4b3948-74d9-4722-96bb-6f937c89c92e)<br/>
备注：<br/>
打开文件时需要添加dtype=object，防止年份信息读取为小数。

```python
import pandas as pd
print(pd.read_csv("Nowcoder.csv", dtype = object, nrows = 6))
```

DA2 牛客网用户数据集的大小:

入门  通过率：23.62%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
你不需要输出全部数据，请直接告诉我们这个数据集的大小，即行数与列数。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/8beb63ce-af1d-43d0-bb8d-2b270a6cc3de)<br/>
输出描述：<br/>
输出该数据集的行数与列数，如下所示：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/6d1749fc-7bff-47b6-b9b4-0fdb0c54bf9d)

```python
import pandas as pd
nc = pd.read_csv("Nowcoder.csv")
df = pd.DataFrame(nc)
print(df.shape)
```

DA3 牛客网的第10位用户:

入门  通过率：16.75%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
现在牛牛想知道这个数据集中第10行的用户的全部信息，请你帮他输出一下。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/99d4c674-4f3c-4327-b9e6-aebcf406ed50)<br/>
输出描述：<br/>
输出该数据集第10行的全部信息，每列信息单独成行，如下所示：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/52a64a1c-b773-4d58-95c7-2936e7af5bba)<br/>
备注：<br/>
打开文件时需要添加dtype=object，防止年份信息读取为小数。

```python
import pandas as pd
rd = pd.read_csv("Nowcoder.csv",dtype = object)
df = pd.DataFrame(rd)
print(df.loc[10])
```

DA4 统计牛客网部分用户使用语言:

入门  通过率：16.04%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
现在牛牛想知道这个数据集中第10行到第20行的用户的常用语言分别是什么，请你帮他输出一下。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/c258b4e2-1b0f-41f3-bfaa-9cf96037b4b5)<br/>
输出描述：<br/>
输出该数据集第10行到第20行的常用语言，每行数据单独成行，如下所示：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/7d0b6753-04d3-4383-a38e-8bd6d9bcd7fc)

```python
import pandas as pd 
rd = pd.read_csv("Nowcoder.csv", dtype = object)
print(rd.loc[10:20,"Language"])
```

DA5 牛客网用户没有补全的信息:

入门  通过率：19.48%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
如果你想知道这份数据是不是所有列的信息都是有数据的，有没有哪些列的数据没有补全，请输出每列信息是否有为空值。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/0a218284-bd55-4f8e-b79b-a537f14e782d)<br/>
输出描述：<br/>
输出该数据集每列信息是否有为空值，如下所示：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/fb536322-3bbb-4ddd-a4cf-151c4f45a306)

```python
import pandas as pd
rd = pd.read_csv("Nowcoder.csv", dtype = object)
print(rd.isnull().any())
```

DA6 查看牛客网哪些用户使用Python:

简单  通过率：14.31%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
如果你想知道哪些人经常使用Python这门语言，并且他们的其他信息是怎么样的，该怎么输出？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/c7da5bc4-06f3-4c80-835c-55bd90351b6d)<br/>
输出描述：<br/>
输出该数据集中语言为Python对应的所有列的信息，包括列号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/db361d18-b74a-4f40-b0f5-a2ea10d1f2da)<br/>
备注：<br/>
打开文件时需要添加dtype=object，防止年份信息读取为小数。

```python
import pandas as pd
df = pd.read_csv("Nowcoder.csv", dtype = object)
print(df.loc[df['Language']=='Python'])
```

DA7 牛客网Python用户的成就值:

简单  通过率：18.23%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
假如你正在学习Python，你想知道牛客网的Python用户的成就值都有多高，请问该如何输出？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/326a62bf-bb93-4ef0-bcfc-9b5929672bd6)<br/>
输出描述：<br/>
输出该数据集中语言为Python对应的成就值这一列的信息，包括列号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/452883cf-cfa9-469a-b4e5-36519af015f8)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv", dtype = object)
print(df.loc[df['Language']=='Python','Achievement_value'])
```

DA8 文件最后用户的部分数据:

简单  通过率：6.81%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
假设你想查看该文件最后5行用户的用户ID、等级、成就值、常用语言，请尝试输出。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/3c5ed30e-1b18-4267-8d6f-f8dfbc39d208)<br/>
输出描述：<br/>
该文件最后5行用户的用户ID、等级、成就值、常用语言等数据，包括行号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/2ae4bf80-d2cf-485d-82f8-f4067f9208c2)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df[-5:][['Nowcoder_ID', 'Level', 'Achievement_value', 'Language']])
```

DA9 2020年毕业的人中最喜欢用Java的用户:

简单  通过率：5.92%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
如果你想知道哪些人是2020年毕业的，并且最常使用的语言是Java的，请输出他们的全部信息。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/937162ae-ed5c-4314-98c0-34a6debcfcbd)<br/>
输出描述：<br/>
输出该数据集中语言为Java且毕业年份为2020的对应的所有列的信息，包括列号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/e05a3cd9-5e91-476c-819c-c83a21f197c7)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.width', 300)
print(df[(df['Language'] == 'Java') & (df['Graduate_year'] == 2020)])
```

DA9 牛客网C系用户们的信息:

简单  通过率：11.59%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
现在运营同学想要你帮忙统计一下使用CPP、C、C#的用户的全部信息，请你帮他输出一下。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/7daf2d95-e877-4edd-87a3-b5f9a25c868c)<br/>
输出描述：<br/>
输出该数据集中语言为CPP、C、C#对应的所有列的信息，包括列号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/c6234ed0-9d8c-4c7d-82fe-c6c6ddf386f1)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.width', 300)
print(df[(df['Language'] == 'CPP')|(df['Language'] == 'C')|(df['Language'] == 'C#')]) ### .isin('CPP','C','C#')
```

DA11 按照毕业年份与使用语言筛选牛客网7级用户:

简单  通过率：8.84%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
假设2018年毕业的你突发奇想，想要知道牛客网有哪些使用CPP的7级用户，且他们的毕业年份和你不是同年的，请问该怎么筛选？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/0d50934b-edcd-4be2-9585-16702c2170d3)<br/>
输出描述：<br/>
输出该数据集中满足筛选条件的全部信息，包括列号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/8603f738-f65e-494f-92a2-35bd7c88bcb5)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.width',300)
print(df.loc[(df['Level']== 7)&(df['Language']=='CPP')&(df['Graduate_year'] != 2018)])
## print(df[(df['Level']== 7)&(df['Language']=='CPP')&(df['Graduate_year'] != 2018)])
```

DA12 牛客网不同语言使用人数:

简单  通过率：14.86%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
刚刚发现牛客网想要学习编程的小白，不知道优先学习什么语言，刷什么题单，你能帮助他从这个csv文件中找到牛客网各种语言使用的用户分别有多少吗？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/da827212-c851-4a2a-9da4-c465a8b43c88)<br/>
输出描述：<br/>
输出该数据集中常用语言所在列各种不同语言出现的次数。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/e96968e0-1435-4f88-b576-b53b490eb9c5)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df['Language'].value_counts())
```

DA13 牛客网用户最近的最长与最短连续签到天数:

简单  通过率：22.00%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交题目数量<br/>
Last_submission_time：最后一次提交题目日期<br/>
运营同学想要统计牛客网的用户的最近的连续签到情况，他想知道最长的用户已经连续签到了多久，最短的用户又连续签到了多久，请帮他输出一下。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/b76be87d-dd4e-4609-9cf9-e9c7bcff8bd0)<br/>
输出描述：<br/>
直接输出最长与最短签到天数的两个数字，数字之间换行。

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df['Continuous_check_in_days'].max())
print(df['Continuous_check_in_days'].min())
```

DA14 Python用户的平均提交次数:

简单  通过率：9.16%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
打算学习Python的小白同学打开了牛客网，他想知道Python到底难不难，于是他想从牛客网Python用户都平均提交了多少次代码来认识，请你帮他找一找。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/8658818f-ab93-4859-bb7f-65bc66188715)<br/>
输出描述：<br/>
直接输出计算的平均数，直接输出，保留一位小数。

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(round(df.loc[df['Language'] == 'Python','Number_of_submissions'].mean(),1))
```

DA15 牛客网用户等级的中位数:

简单  通过率：12.38%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
牛客网运营同学有一个活动，需要统计所有用户等级的中位数，但是为了去掉一些非常不活跃的账号，于是他们只统计刷题数量不低于10题的那部分用户。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/c2f54910-5002-41d7-a0b3-74bb8fd1c99f)<br/>
输出描述：<br/>
直接输出计算的中位数，输出类型为整型Int。

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(int(df.loc[df['Num_of_exercise'] >= 10,'Level'].median()))
```

DA16 用户常用语言有多少:

简单  通过率：18.47%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
你想知道这个文件中记录了多少种常用语言，一并输出这些语言的名字。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/19d9071c-679f-4c58-ab66-79b782edd3bb)<br/>
输出描述：<br/>
直接输出计算的种类数，输出类型为整型Int。<br/>
换行再输出有哪些语言，排序按照在csv中的出现顺序排布。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/f1a92b92-2535-4c8f-9d1e-70c60027fa60)

```ptyhon
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(len(df['Language'].unique()))
print(df['Language'].unique())
```

DA17 牛客网最多的用户等级:

简单  通过率：11.30%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
对于牛客网的等级制度，你很感兴趣，你想知道大部分人都在什么等级，你能找到文件中等级的众数吗？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/969048cd-cafe-4204-928a-f63d06b8a36f)<br/>
输出描述：<br/>
输出等级列的众数。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/64ad6bc4-8e60-4583-8c54-265df5e1b2b7)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df[['Level']].mode())
```

DA18 用分位数分析牛客网用户活动:

简单  通过率：17.57%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
现要分析牛客网用户的活跃情况，请依次输出用户成就值与最近连续签到天数的四分之一分位数以及刷题量与代码提交次数的四分之三分位数。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/0add7466-bbad-4141-9b02-9b27b73bf43d)<br/>
输出描述：<br/>
按照上述要求输出分位数，两种之间换行。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/f5f1a7d8-e08f-4d8c-8694-fa1a64514a23)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df[['Achievement_value','Continuous_check_in_days']].quantile(0.25),df[['Num_of_exercise','Number_of_submissions']].quantile(0.75))
```

DA19 牛客网大佬之间的差距:

简单  通过率：15.46%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
牛客网有很多7级红名大佬，这是众所周知的，但是小白想知道这些大佬的成就值之间有没有什么不同，于是他想从这份文件中输出7级用户中最高成就值与最低成就值之差。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/6a7c7a56-fcae-4db0-858d-08a9bf7a1410)<br/>
输出描述：<br/>
直接输出计算结果，为整数。

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df.loc[df['Level']==7,'Achievement_value'].max()-df.loc[df['Level']==7,'Achievement_value'].min())
```

DA20 牛客用户刷题量的方差与提交次数的标准差:

简单  通过率：20.26%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
假如牛牛正在统计用户的刷题情况，需要知道用户刷题量的方差以及提交代码次数的标准差，你能够帮助他吗？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/bc5b9abe-9f72-4ac2-adb0-07db53135ebd)<br/>
输出描述：<br/>
直接输出计算的结果，各自保留两位小数，第一行为方差，第二行为标准差。

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df['Num_of_exercise'].var().round(2))
print(df['Number_of_submissions'].std().round(2))
```

DA21 大佬用户成就值比例:

简单  通过率：13.51%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
牛客网有很多7级红名大佬这是众所周知的，小白希望知道这些大佬的成就值各自占据了所有人成就值总和的百分之多少，你能帮他吗？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/9acf2568-b099-4350-985b-0cc2081371f4)<br/>
输出描述：<br/>
第一列输出行好，第二列输出计算7级用户的成就值占所有人成就值总和的比例，以小数形式输出，小数位保留不用管。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/56fa5f15-d6d9-4e1a-9843-bc84e45e7da4)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df.loc[df['Level'] == 7, 'Achievement_value']/df['Achievement_value'].sum())
```

DA22 牛客网用户最高的正确率:

中等  通过率：9.75%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
牛客网有那么多刷题的用户，有的人身经百战，刷题无数但是反复提交了多次错误的代码debug之后才能通过，牛牛想知道牛客网最高的正确率能有多少，为了公平起见，他决定只统计刷题数量大于10题的用户，请你帮帮他。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/b73916cd-395e-457a-8e88-bd100e8929dc)<br/>
输出描述：<br/>
直接输出计算的正确率（刷题量/代码提交次数），保留3位小数。

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print((df[df['Num_of_exercise'] > 10]['Num_of_exercise']/df[df['Num_of_exercise'] > 10]['Number_of_submissions']).max().round(3))
```
