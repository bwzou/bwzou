---
layout: post
title: "Pandas（二）"
description: 本章涉及Pandas的基本语法。
date: 2018-04-23 00:00:00 +0800
categories: python
author: bwzou
---
前面我们讲了pandas的两种数据结构，pandas很多操作都是基于这两张数据结构的。pandas内容很多，仅仅靠一篇博客是很难把pandas方方面面都讲到了，所以这里仅仅只是一个抛砖引玉。

其实，[官网](http://pandas.pydata.org/pandas-docs/version/0.21/tutorials.html)就有一套很不错的教程，认真跟着学是可以很快入门的。

## 数据加载
#### 读取文本格式的数据
pandas提供了一些用于将表格型数据读取为DataFrame对象的函数。其中read_csv和read_table可能会是你今后用得最多的。

函数          |      说明
--------------|----------------------------------
read_csv      | 从文件、URL、文件型对象中加载带分隔符的数据。默认分隔符为逗号
read_table    | 从文件、URL、文件型对象中加载带分隔符的数据。默认分隔符为制表符（"\t")
read_fwf      | 读取定宽列格式数据（也就是说，没有分隔符）
read_clipboard| 读取剪贴板中的数据，可以看做read_table的剪贴板版。在将网页转换为表格时很有用。
 
首先我们来看一个以逗号分隔的（CSV）文本文件：
```python
import pandas as pd
f = pd.read_csv('../data/day13/example1.csv')
print(f)
```
我们也可以用read_table,只不过需要指定分隔符而已：
```python
import pandas as pd
f = pd.read_table('../data/day13/example1.csv',sep=',')
print(f)
```
如果所读文件没有标题行，那么读取文件时有两个办法。你可以让pandas为其分配默认的列名，也可以自己定义列名：
```python
import pandas as pd
f = pd.read_csv('../data/day13/example1.csv', header=None)
print(f)
f2 = pd.read_csv('../data/day13/example2.csv', names=['a', 'b', 'c', 'd', 'message'])
print(f2)
```
假设你希望将message列做成DataFrame的索引。你可以明确表示要将该列放到索引4的位置上，也可以通过index_col参数指定"message"：
```python
import pandas as pd
f = pd.read_csv('../data/day13/example1.csv', names=['a', 'b', 'c', 'd', 'message'])
f1 = f.set_index('message')
print(f1)
f2 = pd.read_csv('../data/day13/example2.csv', names=['a', 'b', 'c', 'd', 'message'], index_col='message')
print(f2)
```
如果希望将多个列做成一个层次化索引，只需传入由列编号或列名组成的列表即可：
```python
import pandas as pd
parsed = pd.read_csv('../data/day13/example3.csv', index_col=['key1', 'key2'])
print(parsed)
```
缺失值处理是文件解析任务中的一个重要组成部分。缺失数据经常是要么没有（空字符串），要么用某个标记值表示。默认情况下，pandas会用一组经常出现的标记值进行识别，如NA、-1.#IND以及NULL等：
```python
import pandas as pd
result = pd.read_csv('../data/day13/example4.csv')
print(result)
```
na_values可以接受一组用于表示缺失值的字符串：
```python
result = pd.read_csv('../data/day13/example4.csv', na_values=['NULL'])
print(result)
``` 
可以用一个字典为各列指定不同的NA标记值：
```python
sentinels = {'message': ['foo', 'NA'], 'something': ['two']}
result = pd.read_csv('../data/day13/example4.csv', na_values=sentinels)
print(result)
```

#### 写出数据到文本格式
利用DataFrame的to_csv方法，我们可以将数据写到一个以逗号分隔的文件中：
```python
f = pd.read_csv('../data/day13/example1.csv', header=None)
f.to_csv('../data/day13/out1.csv')
```
缺失值在输出结果中会被表示为空字符串。你也可以将其表示为别的标记值：
```python
f = pd.read_csv('../data/day13/example1.csv', header=None)
f.to_csv('../data/day13/out1.csv', na_rep='NULL')
```
如果没有设置其他选项，则会写出行和列的标签。当然，它们也都可以被禁用：
```python
f = pd.read_csv('../data/day13/example1.csv', header=None)
f.to_csv('../data/day13/out1.csv', index=False, header=False)
```
此外，你还可以只写出一部分的列，并以你指定的顺序排列：
```python
f = pd.read_csv('../data/day13/example1.csv', header=None)
f.to_csv('../data/day13/out1.csv', index=False, cols=['a', 'b', 'c'])
```
Series也有一个to_csv方法：
```python
dates = pd.date_range('1/1/2000', periods=7)
ts = Series(np.arange(7), index=dates)
ts.to_csv('../data/day13/out1.csv')
```
####  excel表格读取
pandas也可以读取.xls或者.xlsx,虽然没有类似 read_csv() 的方法，但是有 ExcelFile 类，我们可以：
```python
xls = pd.ExcelFile("../data/day13/example5.xlsx")
print(dir(xls))      # 查看xls对象内容
print(xls.sheet_names)
print(xls.parse("Sheet1"))  # 解析每一个sheet
```
结果中，columns 的名字与前面 csv 结果不一样，数据部分是同样结果。从结果中可以看到，sheet1 也是一个 DataFrame 对象。

## 数据规整化
#### 合并数据集
数据集的合并（merge）或连接（join）运算是通过一个或多个键将行链接起来的。pandas主要提供merge函数对数据进行合并。
```python
import pandas as pd
df1 = DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
df2 = DataFrame({'key': ['a', 'b', 'd'],'data2': range(3)})
print(pd.merge(df1, df2))
```
如果没有指明要用哪个列进行连接，merge就会将重叠列的列明当作键。不过最好显式指定一下：
```python
print(pd.merge(df1, df2, on='key'))
```
如果两个对象的列名不同，也可以分别进行指定：
```python
df3 = DataFrame({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
df4 = DataFrame({'rkey': ['a', 'b', 'd'], 'data2': range(3)})
print(pd.merge(df3, df4, left_on='lkey', right_on='rkey'))
```
不知道你有没有发现，结果里面的c、d以及与之相关的数据消失了。默认情况下，merge做的是"inner"连接；结果中的键是交集。其他方式还有"left"、"right"以及"outer"。外连接求取的是键的并集，组合了左连接和右连接的效果：
```python
print(pd.merge(df1, df2, how='outer'))
```
多对多的合并操作非常简单，无需额外的工作。如下所示：
```python
df1 = DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],'data1': range(6)})
df2 = DataFrame({'key': ['a', 'b', 'a', 'b', 'd'],'data2': range(5)})
print(pd.merge(df1, df2, on='key', how='left'))
```
多对多连接产生的是行的笛卡尔积。由于左边的DataFrame有3个"b"行，右边的有2个，所以最终结果中就有6个"b"行。连接方式只影响出现在结果中的键。

要根据多个键进行合并，传入一个由列名组成的列表即可：
```python
left = DataFrame({'key1': ['foo', 'foo', 'bar'],'key2': ['one', 'two', 'one'],'lval': [1, 2, 3]})
right = DataFrame({'key1': ['foo', 'foo', 'bar', 'bar'],'key2': ['one', 'one', 'one', 'two'],'rval': [4, 5, 6, 7]})
print(pd.merge(left, right, on=['key1', 'key2'], how='outer'))
```
结果中会出现哪些键组合取决于所选的合并方式，你可以这样来理解：多个键形成一系列元组，并将其当做单个连接键（当然，实际上并不是这么回事）。

有时候，DataFrame中的连接键位于其索引中。在这种情况下，你可以传入left_index=True或right_index=True（或两个都传）以说明索引应该被用作连接键：
```python
left1 = DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'],'value': range(6)})
right1 = DataFrame({'group_val': [3.5, 7]}, index=['a', 'b'])
print(pd.merge(left1, right1, left_on='key', right_index=True))
```
下表是merge函数的参数

参数       |  说明
-----------|--------------------------------------
left       | 参与合并的左侧DataFrame
right      | 参与合并的右侧DataFrame
how        | "inner","outer","left","right"其中之一。默认为inner
on         | 用于连接的列名。必须存在于左右两个DataFrame对象中。如果未指定，且其他连接键也未指定，则以left和right列名的交集作为连接键
left_on    | 左侧DataFrame中用作连接键的列
right_on   | 右侧DataFrame中用作连接键的列
left_index | 将左侧的行索引用作其连接键
right_index| 类似于left_index
sort       | 根据连接键对合并后的数据进行排序，默认为True。有时在处理大数据集时，禁用该选项可以获得更好的性能
sufflxes   | 字符串值元组，用于追加到重叠列名的末尾，默认为（‘_x','_y'）。列如，如果左右两个DataFrame对象都有"data",则结果中就会出现"data_x"和 ”、"data_y"
copy       | 设置为False，可以在某些特殊情况下避免将数据复制到结果数据结过中。默认总是复制

#### 重塑和轴向旋转
有许多用于重新排列表格型数据的基础运算。这些函数也称作重塑（reshape）或轴向旋转（pivot）运算。

层次化索引为DataFrame数据的重排任务提供了一种具有良好一致性的方式。主要功能有二：

- ·stack：将数据的列“旋转”为行。
-  ·unstack：将数据的行“旋转”为列。

我们来看一个简单的DataFrame，其中的行列索引均为字符串：
```
data = DataFrame(np.arange(6).reshape((2, 3)),
    index=pd.Index(['Ohio', 'Colorado'], name='state'),
    columns=pd.Index(['one', 'two', 'three'], name='number'))
```
使用该数DataFrame的stack方法即可将列转换为行，得到一个Series：
```python
result = data.stack()
print(result)
```
对于一个层次化索引的Series，你可以用unstack将其重排为一个DataFrame：
```python
result2 = result.stack()
print(result2)
```
#### 数据转换

**移除重复数据**

DataFrame中常常会出现重复行。下面就是一个例子：
```python
data = DataFrame({'k1': ['one'] * 3 + ['two'] * 4,'k2': [1, 1, 2, 3, 3, 4, 4]}
print(data)
```
DataFrame的duplicated方法返回一个布尔型Series，表示各行是否是重复行：
```python
print(data.duplicated())
```
还有一个与此相关的drop_duplicates方法，它用于返回一个移除了重复行的DataFrame
```python
print(data.drop_duplicates())
```
这两个方法默认会判断全部列，你也可以指定部分列进行重复项判断。假设你还有一列值，且只希望根据k1列过滤重复项：
```python
data['v1'] = range(7)
print(data.drop_duplicates(['k1']))
```
duplicated和drop_duplicates默认保留的是第一个出现的值组合。如果我想保留最后一个怎么办呢？传入take_last=True则保留最后一个：
```python
print(data.drop_duplicates(['k1', 'k2'], take_last=True))
```

**利用函数或映射进行数据转换**

在对数据集进行转换时，我们可能需要根据数组、Series或DataFrame列中的值来实现该转换工作。比如：
```python
data = DataFrame({'food': ['bacon', 'pulled pork', 'bacon', 'Pastrami','corned beef', 'Bacon', 'pastrami', 'honey ham','nova lox'],'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})
print(data)
```
我们需要将肉类食物替换成该食物来源的动物类型，什么意思呢？就是'bacon'培根是猪肉做的，那我们就替换成‘pig’。要完成这项工作，我们首先需要编写一个肉类到动物的映射：
```python
meat_to_animal = {'bacon': 'pig','pulled pork': 'pig','pastrami': 'cow','corned beef': 'cow','honey ham': 'pig','nova lox': 'salmon'}
```
Series的map方法(还记得我们之前学的map方法吗，他们不是一个东西，但是思想是一样的)可以接受一个函数或含有映射关系的字典型对象，但是这里有一个小问题，即有些肉类的首字母大写了，而另一些则没有。因此，我们还需要将各个值转换为小写：
```python
data['animal'] = data['food'].map(str.lower).map(meat_to_animal)
print(data)
```
我们也可以传入一个能够完成全部这些工作的函数：
```python
data['food'].map(lambda x: meat_to_animal[x.lower()])
```
**替换值**

虽然前面提到的map可用于修改对象的数据子集，而replace则提供了一种实现该功能的更简单、更灵活的方式。我们来看看下面这个Series：
```python
data = Series([1., -999., 2., -999., -1000., 3.])
print(data)
```
-999这个值可能是一个表示缺失数据的标记值。要将其替换为pandas能够理解的NA值，我们可以利用replace来产生一个新的Series：
```python
result = data.replace(-999, np.nan)
print(result)
```
如果你希望一次性替换多个值，可以传入一个由待替换值组成的列表以及一个替换值：
```python
result2 = data.replace([-999, -1000], np.nan)
print(result2)
```
如果希望对不同的值进行不同的替换，则传入一个由替换关系组成的列表即可：
```python
result3 = data.replace([-999, -1000], [np.nan, 0])
print(result3)
```
传入的参数也可以是字典：
```python
result4 = data.replace({-999: np.nan, -1000: 0})
print(result4)
```
**离散化和面元划分**

为了便于分析，连续数据常常被离散化或拆分为“面元”（bin）。假设有一组人员数据，而你希望将它们划分为不同的年龄组：
```python
ages = [20, 22, 25, 27, 21, 23, 37, 31, 61, 45, 41, 32]
```
接下来将这些数据划分为“18到25”、“26到35”、“35到60”以及“60以上”几个面元。要实现该功能，你需要使用pandas的cut函数：
```python
bins = [18, 25, 35, 60, 100]
cats = pd.cut(ages, bins)
print(cats)
```
pandas返回的是一个特殊的Categorical对象。你可以将其看做一组表示面元名称的字符串。实际上，它含有一个表示不同分类名称的levels数组以及一个为年龄数据进行标号的labels属性：
```python
print(cats.labels)
print(cats.levels)
print(pd.value_counts(cats))
```
跟“区间”的数学符号一样，圆括号表示开端，而方括号则表示闭端（包括）。哪边是闭端可以通过right=False进行修改。你也可以设置自己的面元名称，将labels选项设置为一个列表或数组即可。


## 数据聚合与分组运算
对数据集进行分组并对各组应用一个函数（无论是聚合还是转换），这是数据分析工作中的重要环节。在将数据集准备好之后，通常的任务就是计算分组统计或生成透视表。

#### GroupBy技术
分组运算的第一个阶段，pandas对象（无论是Series、DataFrame还是其他的）中的数据会根据你所提供的一个或多个键被拆分（split）为多组。拆分操作是在对象的特定轴上执行的。然后，将一个函数应用（apply）到各个分组并产生一个新值。最后，所有这些函数的执行结果会被合并（combine）到最终的结果对象中。结果对象的形式一般取决于数据上所执行的操作。

看概念是很抽象的，我们来看具体的例子。首先来看看下面这个非常简单的表格型数据集（以DataFrame的形式）：
```python
df = DataFrame({'key1': ['a', 'a', 'b', 'b', 'a'],
                'key2': ['one', 'two', 'one', 'two', 'one'],
                'data1': np.random.randn(5),
                'data2': np.random.randn(5)})
```
假设你想要按key1进行分组，并计算data1列的平均值。我们这里要用的是：访问data1，并根据key1调用groupby：
```python
grouped = df['data1'].groupby(df['key1'])
print(grouped)
```
变量grouped是一个GroupBy对象。它实际上还没有进行任何计算，只是含有一些有关分组键df['key1']的中间数据而已。换句话说，该对象已经有了接下来对各分组执行运算所需的一切信息。例如，我们可以调用GroupBy的mean方法来计算分组平均值：
```python
means = grouped.mean()
print(means)
```
如果我们一次传入多个数组，就会得到不同的结果：
```python
means = df['data1'].groupby([df['key1'], df['key2']]).mean()
print(means)
```
这里，我们通过两个键对数据进行了分组，得到的Series具有一个层次化索引（由唯一的键对组成）。

分组键也可以是任何长度适当的数组：
```python
states = np.array(['Ohio', 'California', 'California', 'Ohio', 'Ohio'])
years = np.array([2005, 2005, 2006, 2005, 2006])
print(df['data1'].groupby([states, years]).mean())
```
此外，你还可以将列名（可以是字符串、数字或其他Python对象）用作分组键：
```python
print(df.groupby('key1').mean())
print(df.groupby(['key1', 'key2']).mean())
```
注意：在执行df.groupby('key1').mean()时，结果中没有key2列。这是因为df['key2']不是数值数据（俗称“问题列”），所以被从结果中排除了。默认情况下，所有数值列都会被聚合，虽然有时可能会被过滤为一个子集。

无论你准备拿groupby做什么，都有可能会用到GroupBy的size方法，它可以返回一个含有分组大小的Series：
```python
ds = df.groupby(['key1', 'key2']).size()
print(ds)
```
注意：

#### 透视表和交叉表
还记得你们给老师布置的第一个任务吗？没错就是制作一个透视表。我们现在就以这个例子讲解pandas的透视表pivot table。

DataFrame有一个pivot_table方法，此外还有一个顶级的pandas.pivot_table函数。除能为groupby提供便利之外，pivot_table还可以添加分项小计（也叫做margins）。我造了一些数据：
```
1,2,3,4,5,6,7,8,9,10,11,12,fme_basename,R_C
1,2,3,4,5,6,7,8,9,10,11,12,BC,48_0
1,2,3,4,5,6,7,8,9,10,11,12,BC,48_1
1,2,3,4,5,6,7,8,9,10,11,12,ALD2,48_3
1,2,3,4,5,6,7,8,9,10,11,12,ALD2,48_4
1,2,3,4,5,6,7,8,9,10,11,12,ALDX,48_5
1,2,3,4,5,6,7,8,9,10,11,12,ALDX,48_6
1,2,3,4,5,6,7,8,9,10,11,12,CH4,48_7
1,2,3,4,5,6,7,8,9,10,11,12,CH4,49_0
1,2,3,4,5,6,7,8,9,10,11,12,ETHA,49_1
1,2,3,4,5,6,7,8,9,10,11,12,ETHA,49_2
1,2,3,4,5,6,7,8,9,10,11,12,ETH,49_3
11,22,33,44,55,66,77,88,99,1010,1111,1212,ETH,49_4
1,2,3,4,5,6,7,8,9,10,11,12,ETOH,49_5
1,2,3,4,5,6,7,8,9,10,11,12,ETOH,49_6
1,2,3,4,5,6,7,8,9,10,11,12,FORM,49_7
1,2,3,4,5,6,7,8,9,10,11,12,FORM,49_8
1,2,3,4,5,6,7,8,9,10,11,12,IOLE,50_0
1,2,3,4,5,6,7,8,9,10,11,12,IOLE,50_1
1,2,3,4,5,6,7,8,9,10,11,12,ISOP,50_0
1,2,3,4,5,6,7,8,9,10,11,12,ISOP,50_1
1,2,3,4,5,6,7,8,9,10,11,12,MEOH,49_3
1,2,3,4,5,6,7,8,9,10,11,12,MEOH,49_4
1,2,3,4,5,6,7,8,9,10,11,12,NVOL,49_5
1,2,3,4,5,6,7,8,9,10,11,12,NVOL,49_6
```
表格前面12列分别代表12个月，而当时的需求是：将上述表格的每一个月的数据单独制作成新表，新的表格格式如下:<br>
![]({{site.baseurl}}/images/20180421_new_table_format.png){: .shadow}<br>

我们可以发现这完全就是一个以fme_basename进行分组的透视表而已啊

首先读取数据。我们需要根据fme_basename进行分组，同时因为R_C不参与分组但是要作为第一列的数据，我们可以设置其为索引
```python
import pandas as pd           
from pandas import DataFrame

df = pd.read_csv('./data/pivot_data.csv')  # 源文件存放路径（根据自己情况修改）
new_df = DataFrame(columns=[str(1), 'R_C', 'fme_basename'])      # 取第一个月的数据
new_df[str(1)] = df[str(1)]
new_df['R_C'] = df['R_C']
new_df['fme_basename'] = df['fme_basename']

new_df1 = new_df.pivot_table(values=['1', 'fme_basename', 'R_C' ], index=['R_C'], columns=['fme_basename'])      # 生成透视表
print(new_df1)
```
输出的数据是符合要求的，要制作12个月数据的表格，那就使用for循环创建即可。
```
                1                                                        
fme_basename ALD2 ALDX   BC  CH4   ETH ETHA ETOH FORM IOLE ISOP MEOH NVOL
R_C                                                                      
48_0          NaN  NaN  1.0  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
48_1          NaN  NaN  1.0  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
48_3          1.0  NaN  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
48_4          1.0  NaN  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
48_5          NaN  1.0  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
48_6          NaN  1.0  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
48_7          NaN  NaN  NaN  1.0   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
49_0          NaN  NaN  NaN  1.0   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN
49_1          NaN  NaN  NaN  NaN   NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN
49_2          NaN  NaN  NaN  NaN   NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN
49_3          NaN  NaN  NaN  NaN   1.0  NaN  NaN  NaN  NaN  NaN  1.0  NaN
49_4          NaN  NaN  NaN  NaN  11.0  NaN  NaN  NaN  NaN  NaN  1.0  NaN
49_5          NaN  NaN  NaN  NaN   NaN  NaN  1.0  NaN  NaN  NaN  NaN  1.0
49_6          NaN  NaN  NaN  NaN   NaN  NaN  1.0  NaN  NaN  NaN  NaN  1.0
49_7          NaN  NaN  NaN  NaN   NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN
49_8          NaN  NaN  NaN  NaN   NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN
50_0          NaN  NaN  NaN  NaN   NaN  NaN  NaN  NaN  1.0  1.0  NaN  NaN
50_1          NaN  NaN  NaN  NaN   NaN  NaN  NaN  NaN  1.0  1.0  NaN  NaN
```
当然如果我们的要求不止这些，我们还可以对这个表作进一步的处理，传入margins=True添加分项小计。这将会添加标签为All的行和列，其值对应于单个等级中所有数据的分组统计。
```
new_df1 = new_df.pivot_table(index=['R_C'], columns=['fme_basename'], margins=True)      # 生成透视表
print(new_df1)
                1                                                            
fme_basename ALD2 ALDX   BC  CH4   ETH ETHA ETOH FORM IOLE ISOP MEOH NVOL All
R_C                                                                          
48_0          NaN  NaN  1.0  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_1          NaN  NaN  1.0  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_3          1.0  NaN  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_4          1.0  NaN  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_5          NaN  1.0  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_6          NaN  1.0  NaN  NaN   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_7          NaN  NaN  NaN  1.0   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
49_0          NaN  NaN  NaN  1.0   NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
49_1          NaN  NaN  NaN  NaN   NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN   1
49_2          NaN  NaN  NaN  NaN   NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN   1
49_3          NaN  NaN  NaN  NaN   1.0  NaN  NaN  NaN  NaN  NaN  1.0  NaN   1
49_4          NaN  NaN  NaN  NaN  11.0  NaN  NaN  NaN  NaN  NaN  1.0  NaN   6
49_5          NaN  NaN  NaN  NaN   NaN  NaN  1.0  NaN  NaN  NaN  NaN  1.0   1
49_6          NaN  NaN  NaN  NaN   NaN  NaN  1.0  NaN  NaN  NaN  NaN  1.0   1
49_7          NaN  NaN  NaN  NaN   NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN   1
49_8          NaN  NaN  NaN  NaN   NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN   1
50_0          NaN  NaN  NaN  NaN   NaN  NaN  NaN  NaN  1.0  1.0  NaN  NaN   1
50_1          NaN  NaN  NaN  NaN   NaN  NaN  NaN  NaN  1.0  1.0  NaN  NaN   1
All           1.0  1.0  1.0  1.0   6.0  1.0  1.0  1.0  1.0  1.0  1.0  1.0   1
```
默认统计项为平均值，要使用其他的聚合函数，将其传给aggfunc即可。例如，使用count或len可以得到有关分组大小的交叉表：
```
new_df1 = new_df.pivot_table(index=['R_C'], columns=['fme_basename'], aggfunc='count', margins=True)      # 生成透视表
print(new_df1)
                1                                                           
fme_basename ALD2 ALDX   BC  CH4  ETH ETHA ETOH FORM IOLE ISOP MEOH NVOL All
R_C                                                                         
48_0          NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_1          NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_3          1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_4          1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_5          NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_6          NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
48_7          NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
49_0          NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN   1
49_1          NaN  NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN   1
49_2          NaN  NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  NaN   1
49_3          NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  1.0  NaN   2
49_4          NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  NaN  1.0  NaN   2
49_5          NaN  NaN  NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  1.0   2
49_6          NaN  NaN  NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN  1.0   2
49_7          NaN  NaN  NaN  NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN   1
49_8          NaN  NaN  NaN  NaN  NaN  NaN  NaN  1.0  NaN  NaN  NaN  NaN   1
50_0          NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  1.0  1.0  NaN  NaN   2
50_1          NaN  NaN  NaN  NaN  NaN  NaN  NaN  NaN  1.0  1.0  NaN  NaN   2
All           2.0  2.0  2.0  2.0  2.0  2.0  2.0  2.0  2.0  2.0  2.0  2.0  24
```
你应该也看到了，存在很多空的组合（也即是NA），我想把它们替换一下，比如0，那么我需要设置一个‘fill_value = 0’：
```
new_df1 = new_df.pivot_table(index=['R_C'], columns=['fme_basename'], aggfunc='count',fill_value=0, margins=True)      # 生成透视表
print(new_df1)
                1                                                       
fme_basename ALD2 ALDX BC CH4 ETH ETHA ETOH FORM IOLE ISOP MEOH NVOL All
R_C                                                                     
48_0            0    0  1   0   0    0    0    0    0    0    0    0   1
48_1            0    0  1   0   0    0    0    0    0    0    0    0   1
48_3            1    0  0   0   0    0    0    0    0    0    0    0   1
48_4            1    0  0   0   0    0    0    0    0    0    0    0   1
48_5            0    1  0   0   0    0    0    0    0    0    0    0   1
48_6            0    1  0   0   0    0    0    0    0    0    0    0   1
48_7            0    0  0   1   0    0    0    0    0    0    0    0   1
49_0            0    0  0   1   0    0    0    0    0    0    0    0   1
49_1            0    0  0   0   0    1    0    0    0    0    0    0   1
49_2            0    0  0   0   0    1    0    0    0    0    0    0   1
49_3            0    0  0   0   1    0    0    0    0    0    1    0   2
49_4            0    0  0   0   1    0    0    0    0    0    1    0   2
49_5            0    0  0   0   0    0    1    0    0    0    0    1   2
49_6            0    0  0   0   0    0    1    0    0    0    0    1   2
49_7            0    0  0   0   0    0    0    1    0    0    0    0   1
49_8            0    0  0   0   0    0    0    1    0    0    0    0   1
50_0            0    0  0   0   0    0    0    0    1    1    0    0   2
50_1            0    0  0   0   0    0    0    0    1    1    0    0   2
All             2    2  2   2   2    2    2    2    2    2    2    2  24
```

pivot_table的参数说明如下：

参数名     |      说明
-----------|---------------------------
values     | 待聚合的列的名称。默认聚合所有数值列
rows       | 用于分组的列名或其他分组键，出现在结果透视表的行
columns    | 用于分组的列名或其他分组键，出现在结果透视表的列
aggfunc    | 聚合函数或函数列表，默认值为‘mean’。可以是任何对groupby有效的函数
fill_value | 用于替换结果表中的缺失值
margins    | 添加行/列小计和总计，默认值为False

交叉表（cross-tabulation，简称crosstab）是一种用于计算分组频率的特殊透视表。下面这个范例数据很典型，取自交叉表的Wikipedia页：
```
Sample,Gender,Handedness
1,Female,Right-handed
2,Male,Left-handed
3,Female,Right-handed
4,Male,Right-handed
5,Male,Left-handed
6,Male,Right-handed
7,Female,Right-handed
8,Female,Left-handed
9,Male,Right-handed
10,Female,Right-handed
```
假设我们想要根据性别和用手习惯对这段数据进行统计汇总。虽然可以用pivot_table实现该功能，但是pandas.crosstab函数会更方便：
```python
import pandas as pd
from pandas import DataFrame,Series

data = pd.read_csv('./data/cross_data.csv')  # 源文件存放路径（根据自己情况修改）
print(pd.crosstab(data.Gender, data.Handedness, margins=True))

# Handedness  Left-handed  Right-handed  All
# Gender                                    
# Female                1             4    5
# Male                  2             3    5
# All                   3             7   10
```
crosstab的前两个参数可以是数组、Series或数组列表。
