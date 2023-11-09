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

DA23 统计牛客网用户的名字长度:

简单  通过率：2.87%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Name：用户名<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
运营小周同学想要统计这些用户的名字长度，你可以帮助她吗？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/a47e5ec4-4e07-480e-a220-f0aada16dce0)<br/>
输出描述：<br/>
输出每一行用户名字的长度，包括行号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/ee1ca3e7-d0ef-43d9-b288-ecadbe7be45c)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df['Name'].str.len())
```

DA24 去掉信息不全的用户:

简单  通过率：7.80%  时间限制：5秒  空间限制：256M<br/>
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
运营同学正在做用户调研，为了保证调研的可靠性，想要去掉那些信息不全的用户，即去掉有缺失数据的行，请你帮助他去掉后输出全部数据。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/b52c0281-83e2-4923-ba6d-39424186e111)<br/>
输出描述：<br/>
直接输出清洗后的全部数据。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/6e5c1e23-3379-4471-aa33-d88bafcfb6da)

```python
import pandas as pd 
pd.set_option("display.max_rows", None)
pd.set_option("display.max_columns", None)
pd.set_option("display.width", 3000)
df = pd.read_csv("Nowcoder.csv",dtype=object)
print(df.dropna())
```

DA25 修补缺失的用户数据:

中等  通过率：9.39%  时间限制：5秒  空间限制：256M<br/>
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
运营同学拿到了这份用户文件，但是由于系统BUG，出现了部分缺失的值，请你使用当前的最大年份填充缺失的毕业年份（“Graduate_year”），用Python填充缺失的常用语言（“Language”），用成就值的均值（四舍五入保留整数）填充缺失的成就值（“Achievement_value”）。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/d1a7aa62-2410-4f63-aabe-9ff58fe07ebb)<br/>
输出描述：<br/>
输出修改后的全部数据，不用处理输出时年份与成就值的小数点问题。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/8358ee73-667c-47ad-9799-c85533ef4434)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.width',300)
df['Graduate_year'].fillna(df['Graduate_year'].max())
df['Language'].fillna('Python')
df['Achievement_value'].fillna(df['Achievement_value'].mean().round(0))
print(df)
```

DA26 解决牛客网用户重复的数据:

简单  通过率：27.91%  时间限制：5秒  空间限制：256M<br/>
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
牛牛拿到这份文件的时候一脸懵逼，因为系统错误将很多相同用户的数据输出了多条，导致文件中有很多重复的行，请先检查每一行是否重复，然后输出删除重复行后的全部数据。<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/12af0fdf-b396-4c25-91ae-bb92d6973ea0)<br/>
输出描述：<br/>
先输出每一行是否重复，再输出去重后的文件全部数据。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/97b4bb7a-14b5-4fda-8071-e3a18a760350)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
print(df.duplicated())
print(df.drop_duplicates())
```

DA27 统一最后刷题日期的格式:

中等  通过率：9.79%  时间限制：5秒  空间限制：256M<br/>
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
运营同学发现最后一次提交题目日期这一列有各种各样的日期格式，这对于他分析用户十分不友好，你能够帮他输出用户ID、等级以及统一后的日期吗？（日期格式统一为yyyy-mm-dd）<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/8bcb946e-c23d-4f2b-bb4c-8cb08293f07e)<br/>
输出描述：<br/>
输出用户ID、等级与最后提交日期三列，包括行号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/882d9a8d-d99d-43c0-84df-5ff2fd2754c3)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv", dtype = object)
df['Last_submission_time'] = pd.to_datetime(df['Last_submission_time'], format = '%Y-%m-%d')
print(df[['Nowcoder_ID','Level','Last_submission_time']])
```

DA28 将用户的json文件转换为表格形式:

简单  通过率：11.01%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.json文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
如果你读入了这个json文件，能将其转换为pandas的DataFrame格式吗？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.json文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/4cc61c7e-20d2-4a9c-b962-8a9b665c7334)<br/>
输出描述：<br/>
输出转换为DataFrame的全部数据，包括行号。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/ca464476-2093-4c3c-9006-5aa8e8fbccb0)

```python
import pandas as pd 
df = pd.read_json("Nowcoder.json",dtype = dict)
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.width', 300)
print(df)
```

DA29 牛客网的每日练题量:

较难  通过率：10.43%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有牛客网12月每天练习题目情况的数据集nowcoder.csv。包含如下字段（字段之间用逗号分隔）：<br/>
user_id:用户id<br/>
question_id：问题编号<br/>
result：运行结果<br/>
date：练习日期<br/>
请你统计2021年12月每天练习题目的数量。<br/>
输入描述：<br/>
数据集可以直接从当前目录下nowcoder.csv读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/db2cb7a0-b4aa-42a3-b520-9d1dcc361e5f)<br/>
输出描述：<br/>
以上数据集的输出结果如下：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/2efee65b-7d60-4300-9cdf-2bbac4b44a60)

```python
import pandas as pd 
df = pd.read_csv("nowcoder.csv")
df['date'] = pd.to_datetime(df['date'],format = '%Y-%m-%d')
df = df[(df['date'] <= '2021-12-31')&(df['date'] >= '2021-12-01')]
print(df.groupby('date')['user_id'].count())
```

DA30 牛客网用户练习的平均次日留存率:

较难  通过率：5.93%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
问题描述：<br/>
现有牛客网12月每天练习题目情况的数据集nowcoder.csv。包含如下字段（字段之间用逗号分隔）：<br/>
user_id:用户id<br/>
question_id：问题编号<br/>
result：运行结果<br/>
date：练习日期<br/>
现需要查看用户在某天练习后第二天还会再来练习的留存情况，请计算用户练习的平均次日留存率。<br/>
输入描述：<br/>
数据集可以直接从当前目录下nowcoder.csv读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/81a58f53-0e11-4c14-aab1-fe94833db055)<br/>
输出描述：<br/>
以上数据集中某天练习后第二天还会再来的用户数除以user_id总次数（不考虑重复情况）记为平均此日留存率，结果保留两位小数。

```python
from datetime import timedelta
import pandas as pd 
df = pd.read_csv("nowcoder.csv")
df['date'] = pd.to_datetime(df['date']).dt.date
total = df['user_id'].count()
df['an_date'] = df['date'] + timedelta(days=1)
test = pd.merge(df, df, how = 'inner', left_on = ['user_id', 'date'], right_on = ['user_id', 'an_date'])
n = test['user_id'].count()
print(round(n/total,2))
```

DA31 牛客网每日正确与错误的答题次数:

较难  通过率：9.00%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
问题描述：<br/>
现有牛客网12月每天练习题目的数据集nowcoder.csv。包含如下字段（字段之间用逗号分隔）：<br/>
user_id:用户id<br/>
question_id：问题编号<br/>
result：运行结果<br/>
date：练习日期<br/>
请你统计2021年12月答题结果正确和错误的前提下每天的答题次数。<br/>
输入描述：<br/>
数据集可以直接从当前目录下nowcoder.csv读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/07a20f43-fa9b-4aaa-ba82-9d05c1c136f8)<br/>
输出描述：<br/>
以上数据集的输出结果如下：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/a47e3d5f-a02c-44a8-8eee-47e2bf776c3c)

```python
import pandas as pd 
from datetime import timedelta
df = pd.read_csv("nowcoder.csv")
df['year-month-day'] = pd.to_datetime(df['date']).dt.date
df1 = df.groupby(['result','year-month-day'])['user_id'].count()
print(df1)
```

DA32 牛客网答题正误总数:

中等  通过率：23.19%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
题目描述：<br/>
现有牛客网12月每天练习题目的数据集nowcoder.csv。包含如下字段（字段之间用逗号分隔）：<br/>
user_id:用户id<br/>
question_id：问题编号<br/>
result：运行结果<br/>
date：练习日期<br/>
请你统计答对和答错的总数分别是多少。<br/>
输入描述：<br/>
数据集可以直接从当前目录下nowcoder.csv读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/08616e7b-cfe7-436a-a6eb-0ccc3c59ea0e)<br/>
输出描述：<br/>
以上数据集的输出结果如下：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/93df7d50-1b36-440b-b0d7-fa8b02742e44)

```python
import pandas as pd 
df = pd.read_csv('nowcoder.csv')
print(df.groupby('result').size())
```

DA33 牛客网连续练习题目3天及以上的用户:

较难  通过率：4.99%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
题目描述：<br/>
现有牛客网12月每天练习题目的数据集nowcoder.csv。包含如下字段（字段之间用逗号分隔）：<br/>
user_id:用户id<br/>
question_id：问题编号<br/>
result：运行结果<br/>
date：练习日期<br/>
请你统计2021年12月连续练习题目3天及以上的所有用户。<br/>
输入描述：<br/>
数据集可以直接从当前目录下nowcoder.csv读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/0fc426fe-b838-469a-88a9-002c718c5a30)<br/>
输出描述：<br/>
输出连续3天及以上的用户及对应的连续天数，以上数据集的输出结果如下：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/f6415feb-3286-400c-8c37-bd701aaad4ce)

```python
import pandas as pd 
from datetime import timedelta
df = pd.read_csv('nowcoder.csv')
df = df[['user_id','date']]
df['date'] = pd.to_datetime(df['date']).dt.date
df.drop_duplicates(inplace= True)
df['rank'] = df['date'].groupby(df['user_id']).rank()
df['sub'] = df['date'] - pd.to_timedelta(df['rank'], unit='d')
df = df.groupby(['user_id','sub']).agg(max_continues_days=('date','count'))
df = df[df['max_continues_days'] >= 3]
df = df.groupby('user_id')['max_continues_days'].max()
print(df)
```

DA34 牛客网不同毕业年份的大佬:

中等  通过率：15.25%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Name：用户名<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
牛牛想要知道牛客网这些刷题用户，每年毕业生中最高的成就值分别是多少？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/a9f1287f-a883-442c-9db6-3f818bcf294e)<br/>
输出描述：<br/>
按照毕业年份顺序输出每年毕业生中的最高成就值。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/74144cfa-9951-47bd-ae7d-84680e9a660b)

```python
import pandas as pd 
df = pd.read_csv('Nowcoder.csv')
df = df.groupby('Graduate_year')['Achievement_value'].max()
print(df)
```

DA35 不同等级用户语言使用情况:

中等  通过率：17.50%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Name：用户名<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
正在牛客网学习编程的小白同学，想要知道牛客网的用户们都使用了哪些语言，尤其是不同等级的用户中各类语言的使用分别有多少人，你能帮助他输出一下吗？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/d33e41a1-320a-4deb-bfae-d124a290fd54)<br/>
输出描述：<br/>
按照等级顺序输出每个等级下每种常用语言的使用人数，输出包括最后的dtype。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/e0368f5b-1fc8-4a91-bc3d-16f817ff6b67)

```python
import pandas as pd 
df = pd.read_csv("Nowcoder.csv")
df = df.groupby(['Level','Language'])
print(df.size())
```

DA36 总人数超过5的等级:

中等  通过率：16.05%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
现有一个Nowcoder.csv文件，它记录了牛客网的部分用户数据，包含如下字段（字段与字段之间以逗号间隔）：<br/>
Nowcoder_ID：用户ID<br/>
Name：用户名<br/>
Level：等级<br/>
Achievement_value：成就值<br/>
Num_of_exercise：刷题量<br/>
Graduate_year：毕业年份<br/>
Language：常用语言<br/>
Continuous_check_in_days：最近连续签到天数<br/>
Number_of_submissions：提交代码次数<br/>
Last_submission_time：最后一次提交题目日期<br/>
产品经理小X同学想要分析一下用户的等级数据，他想知道在人数大于5的条件下，各个等级都分别有多少人？<br/>
输入描述：<br/>
数据集直接从当前目录下的Nowcoder.csv文件中读取。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/9665a855-d692-45af-98ab-a4f35800f5dd)<br/>
输出描述：<br/>
按照等级顺序输出人数大于5的等级中每个等级的人数，输出包括最后的dtype。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/57319a0a-bc96-436e-b22a-e4d9ded8c504)

```python
import pandas as pd 
df = pd.read_csv('Nowcoder.csv')
print(df.groupby('Level')['Nowcoder_ID'].count()>5)
```

DA37 统计运动会项目报名人数:

中等  通过率：19.03%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
题目描述<br/>
某公司计划举办一场运动会，现有运动会项目数据集items.csv。 包含以下字段：<br/>
item_id：项目编号；<br/>
item_name:项目名称；<br/>
location:比赛场地。<br/>
有员工报名情况数据集signup.csv。包含以下字段：<br/>
employee_id：员工编号；<br/>
name：员工姓名；<br/>
sex：性别；<br/>
department：所属部门；<br/>
item_id：报名项目id<br/>
请你统计各类型项目的报名人数。<br/>
输入描述：<br/>
数据集可以从当前目录下items.csv、signup.csv读取。<br/>
items.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/c49f0b86-3393-44ee-84dd-6b9d1ca5460e)<br/>
signup.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/5497e7af-2c94-49a1-b0ea-e0c73be2bd55)<br/>
输出描述：<br/>
只输出报名人数不为0的项目及其对应的报名人数。以上数据集的输出如下：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/0e7c63e0-1fdb-4232-ae8e-4e920e3e9786)

```python
import pandas as pd 
df1 = pd.read_csv('items.csv')
df2 = pd.read_csv('signup.csv')
df = pd.merge(df1,df2,how='inner',on=['item_id'])
print(df.groupby('item_name')['employee_id'].count()>0)
```

DA38 统计运动会项目报名人数(二):

中等  通过率：31.32%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
题目描述：<br/>
某公司计划举办一场运动会，现有运动会项目数据集items.csv。 包含以下字段：<br/>
item_id：项目编号；<br/>
item_name:项目名称；<br/>
location:比赛场地。<br/>
有员工报名情况数据集signup.csv。包含以下字段：<br/>
employee_id：员工编号；<br/>
name：员工姓名；<br/>
sex：性别；<br/>
department：所属部门；<br/>
item_id：报名项目id<br/>
请你统计各类型项目的报名人数。<br/>
输入描述：<br/>
数据集可以从当前目录下items.csv、signup.csv读取。<br/>
items.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/57d2304d-4a8e-485f-b797-ef4f972adf0d)<br/>
signup.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/c52a4191-9e76-4ed4-b0fc-665696de0aa7)<br/>
输出描述：<br/>
输出items.csv中所有项目的报名人数（没有人报名的项目的报名人数输出为0即可）。以上数据集的输出如下：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/b144176b-5837-458e-ab7a-c6a93e631970)

```python
import pandas as pd 
df1 = pd.read_csv('items.csv')
df2 = pd.read_csv('signup.csv')
df = pd.merge(df1,df2,how='left',on='item_id')
print(df.groupby('item_name').size())
```

DA39 多报名表的运动项目人数统计:

较难  通过率：16.36%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
题目描述<br/>
某公司计划举办一场运动会，现有部分运动会项目数据集items.csv。 包含以下字段：<br/>
item_id：项目编号；<br/>
item_name:项目名称；<br/>
location:比赛场地。<br/>
有员工报名情况数据集signup.csv。包含以下字段：<br/>
employee_id：员工编号；<br/>
name：员工姓名；<br/>
sex：性别；<br/>
department：所属部门；<br/>
item_id：报名项目id。<br/>
另有signup1.csv，是education部门的报名情况，包含字段同signup.csv。<br/>
请你将signup.csv与signup1.csv的数据集合并后，统计各类型项目的报名人数。<br/>
输入描述：<br/>
数据集可以从当前目录下items.csv、signup.csv、signup1.csv读取。<br/>
items.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/0a9dfb78-7c7a-4558-a116-63b05aaccbb8)<br/>
signup.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/fcd82691-de6f-4e0f-b447-47895304d8c7)<br/>
signup1.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/f9837d72-8e97-4c65-a1eb-2bfe924bbba6)<br/>
输出描述：<br/>
只输出报名人数不为0的项目及其对应的报名人数。以上数据集的输出如下：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/406f99d2-287f-4045-9b59-876e67f1398e)

```python
import pandas as pd 
items = pd.read_csv('items.csv')
signup = pd.read_csv('signup.csv')
signdep = pd.read_csv('signup1.csv')
signuptotal = pd.concat([signup,signdep])
df = pd.merge(items,signuptotal,how='left',on='item_id')
print(df.groupby('item_name').size())
```

DA40 统计职能部分运动会某项目的报名信息:

较难  通过率：6.57%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
题目描述<br/>
某公司计划举办一场运动会，现有运动会项目数据集items.csv。 包含以下字段：<br/>
item_id：项目编号；<br/>
item_name:项目名称；<br/>
location:比赛场地。<br/>
有员工报名情况数据集signup.csv。包含以下字段：<br/>
employee_id：员工编号；<br/>
name：员工姓名；<br/>
sex：性别；<br/>
department：所属部门；<br/>
item_id：报名项目id<br/>
请你统计职能部门（functional）中报名标枪(javenlin)的所有员工的员工编号（employee_id）、姓名（name）及性别（sex）。<br/>
输入描述：<br/>
数据集可以从当前目录下items.csv、signup.csv读取。<br/>
items.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/aa264b34-84ce-436e-8c5f-dd2959f0f5e4)<br/>
signup.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/39037b06-9c21-451c-84c4-46aeb3765dfd)<br/>
输出描述：<br/>
输出职能部门（functional）中报名标枪(javenlin)的所有员工的员工编号（employee_id）、姓名（name）及性别（sex）。<br/>
以上数据集的输出如下（注意：结果中行标签从0开始顺序排序）：<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/c2f1ae99-179d-4384-90c7-57a2d54448ce)

```python
import pandas as pd 
items = pd.read_csv('items.csv')
signup = pd.read_csv('signup.csv')
df = pd.merge(items,signup,how='left',on='item_id')
print(df[(df['department'] == 'functional')&(df['item_name'] == 'javelin')][['employee_id','name','sex']].reset_index(drop=True))
```

DA41 运动会各项目报名透视表:

中等  通过率：10.85%  时间限制：5秒  空间限制：256M<br/>
描述<br/>
题目描述<br/>
某公司计划举办一场运动会，现有运动会项目数据集items.csv。 包含以下字段：<br/>
item_id：项目编号；<br/>
item_name:项目名称；<br/>
location:比赛场地。<br/>
有员工报名情况数据集signup.csv。包含以下字段：<br/>
employee_id：员工编号；<br/>
name：员工姓名；<br/>
sex：性别；<br/>
department：所属部门；<br/>
item_id：报名项目id<br/>
请你输出报名的各个项目情况（不包含没人报名的项目）对应的透视表。<br/>
输入描述：<br/>
数据集可以从当前目录下items.csv、signup.csv读取。<br/>
items.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/aec8d1a4-e629-45f7-89c9-ad4e138594b1)<br/>
signup.csv<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/1a6a9e03-946a-46f6-bdd6-328438df6616)<br/>
输出描述：<br/>
请输出下图所示的透视表。其中行索引键为sex、department，列索引键为item_name。数值为报名对应项目的人数。如education部门男性中报名longJump的人数为1人。<br/>
![image](https://github.com/ljsakura/ljsakura.github.io/assets/19812837/34a1dcea-8812-4813-9bbc-154bfab2dd9c)

```python
import pandas as pd 
import numpy as np 
from numpy.core.defchararray import count
item = pd.read_csv('items.csv')
signup = pd.read_csv('signup.csv')
df = pd.merge(item,signup,how='left',on='item_id')
print(pd.pivot_table(data=df,index=['sex','department'],columns='item_name',values=['employee_id'],aggfunc='count'))
```
