---
layout: post
title: "函数（二）"
description: 函数是面向过程的程序设计的基本单元。
date:   2018-04-13 12:00:00 +0800
categories: python
author: bwzou
---
函数是Python内建支持的一种封装，我们通过把大段的代码拆成函数，然后通过函数调用，就可以把复杂任务分解成一个个简单的任务，这种分解可以称之为面向过程的程序设计。
举个例子：比如你要熟练掌握Python，这是你的目标，如果当做一个任务，那这个任务会很复杂，因为你首先要懂Python的基础语法，数据类型、函数、对象等；其次你还要知道不少内建包和常用工具包的使用；再然后呢，你可能还需要懂一些网络编程，数据库开发；如果做web开发的话，还要学习web框架，以及运维相关的知识。

而函数式编程（请注意多了一个“式”字）——Functional Programming，虽然也可以归结到面向过程的程序设计，但其思想更接近数学计算。

函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函数，还允许返回一个函数！

## 高阶函数
**变量可以指向函数**。我们以Python内置的求绝对值的函数abs为例：
```python
print(abs(-10))   # abs(-10)是函数调用
 
print(abs)   # abs是函数本身

f = abs
print(f)  # 函数本身也可以赋值给变量

print(f(-10))  # 直接调用abs()函数和调用变量f()完全相同
```
结论：函数本身也可以赋值给变量，即：变量可以指向函数。

**函数名也是变量**。那么函数名是什么呢？函数名其实就是指向函数的变量！对于abs()这个函数，完全可以把函数名abs看成变量，它指向一个可以计算绝对值的函数！
```python
abs = 10
print(abs(-10))
```
把abs指向10后，就无法通过abs(-10)调用该函数了！因为abs这个变量已经不指向求绝对值函数而是指向一个整数10！当然实际代码绝对不能这么写，这里是为了说明函数名也是变量。要恢复abs函数，请重启Python交互环境。注：由于abs函数实际上是定义在import builtins模块中的，所以要让修改abs变量的指向在其它模块也生效，要用import builtins; builtins.abs = 10

既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。函数式编程就是指这种高度抽象的编程范式。
```python
def add(x, y, f):
    return f(x) + f(y)
```

[MapReduce](https://zh.wikipedia.org/zh-hans/MapReduce)是Google提出的一个软件架构，用于处理大规模海量数据。并在之后广泛的应用于Google的各项应用中，2006年Apache的Hadoop项目正式将MapReduce纳入到项目中。

不过接下来我们要学习的是Python函数式编程中常用的内建函数map()和reduce()函数，而不是Google的MapReduce。

#### map
map()函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回。

当Iterable只有一个时，将函数作用于这个Iterable的每个元素上，得到一个新的Iterator。我们看下面一张图就可以直观的说明map是如何工作的：<br>
![]({{site.baseurl}}/images/20180413_map_single_seq.png){: .shadow} <br>
举个例子,比如我们有一个函数f(x)=x2，要把这个函数作用在一个list [1, 2, 3, 4, 5, 6, 7, 8, 9]上，就可以用map()实现如下:
```python
def f(x):
    return x * x

r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9], 2)
print(list(r))
```
map()传入的第一个参数是f,即函数对象本身。由于结果r是一个Iterator，因此通过list()函数让它把整个序列都计算出来并返回一个list。

当Iterable有多个时，map()会并行地对每个Iterable执行如下过程：<br>
![]({{site.baseurl}}/images/20180413_map_multi_seq.png){: .shadow} <br>
也就是说，每一个Iterable同一位置的元素在执行过一个多元函数之后，得到一个返回值，这些返回值放在一个结果列表中。<br>
还是刚才那个例子，只不过我们拆分成三个序列，然后使用map进行计算：
```python
def f(x,y,z):
    return x * y * z

r = map(f, [1, 2, 3],[4, 5, 6],[ 7, 8, 9])
print(list(r))      #  [28, 80, 162]
```
上面是返回值是一个值的情况，实际上也可以是一个元组。我们改一改刚才的例子：
```python
def f(x,y,z):
    return x * y * z, x+y+z

r = map(f, [1, 2, 3],[4, 5, 6],[ 7, 8, 9])
print(list(r))      #  [(28, 12), (80, 15), (162, 18)]
```
好了，看例子很明白了吧。但是会不会有这样的问题，如果传入的Iterable长度不一样怎么办呢？使用多个迭代器时，当最短迭代器耗尽时，迭代器停止。
```python
def f(x,y,z):
    return x * y * z, x+y+z

r = map(f, [1, 2, 3, 4],[4, 5, 6,7],[ 7, 8, 9])
print(list(r))    #  [(28, 12), (80, 15), (162, 18)]
```
嗯，结果跟上一个一样。

#### reduce
reduce函数即为化简，它是这样一个过程：每次迭代，将上一次的迭代结果（第一次时为init的元素，如没有init则为seq的第一个元素）与下一个元素一同执行一个二元的函数。请注意，在reduce函数中，init是可选的，如果使用，则作为第一次迭代的第一个元素使用。

举个例子，比方说对一个序列求和，就可以用reduce实现：
```python
from functools import reduce
def add(x, y):
    return x + y

reduce(add, [1, 3, 5, 7, 9])
```
请注意啊，Python3里面使用reduce需要添加``from functools import reduce``,原因是reduce在_functools模块里面，而前面讲到map则在builtins，在builtins里面是不需要显示引入的。但是他们都属于built-ins的函数。

结合map函数，我们可以写一个字符串转数字的函数：
```python
from functools import reduce

DIGITS = {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}

def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return DIGITS[s]
    return reduce(fn, map(char2num, s))
```
#### filter
Python内建的filter()函数用于过滤序列。

和map()类似，filter()也接收一个函数和一个序列。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。

例如：在一个list中，删掉偶数，只保留奇数，可以这么写：
```python
def is_odd(n):
    return n % 2 == 1
 
newlist = list(filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]))
print(newlist)
```
比如：把一个序列中的空字符串删掉，可以这么写：
```python
def not_empty(s):
    return s and s.strip()

newlist = list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
print(newlist)
```
过滤出1~100中平方根是整数的数:
```python
import math
def is_sqr(x):
    return math.sqrt(x) % 1 == 0
 
newlist = list(filter(is_sqr, range(1, 101)))
print(newlist)
```

#### sort
Python内置的sorted()函数能够进行排序。
```python
s = sorted([2, 11, -23, -9, 51])
print(s)
```
sorted()是一个高阶函数，接受一个key函数来实现自定义的排序，例如按绝对值大小排序：
```python
s = sorted([2, 11, -23, -9, 51], key=abs)
print(s)
```
对字符串进行排序：
```python
s = sorted(['Yisa','tom','Joe'], keys = str.lower)
print(s)
```
sorted()的可以传入参数：`reverse=True` 取反向排序
```python
s = sorted(['Yisa','tom','Joe'], keys = str.lower, reverse=True)
print(s)
```

<style>.shadow{
    box-shadow: 2px 2px 5px #aaa;
    border-radius: 0;
    margin-top: 1em;
    margin-bottom: 1em;
}</style>