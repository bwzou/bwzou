---
layout: post
title: "高级特性"
description: python中有一些非常有用的高级特性，可以极大的提高开发效率。
date:   2018-04-12 00:00:00 +0800
categories: python
author: bwzou
---
在开发程序上，我们不仅要考虑把功能实现了，还要考虑如何高效简洁！python提供了很多高级特性可以提高开发效率。
## 切片
取一个list或tuple的部分元素是非常常见的操作。比如，一个list如下：

    L = ['Java','C++','C','Python','PHP']
    
如果要取前3个元素，你怎么做？

当然你可以使用如下方法：

    print(L[0],L[1],L[2])

但是这个方法很有局限性，如果List有1000项，要去前750项，怎么办怎么办？

当然也是有方法的，那就是我们之前的学的循环：
```python
L = [...]  # 假设它有1000个元素
newList = []
n = 750
for i in range(n):
    newlist.append(L[i])

print(L[i]) 
```
但是，python提供了更加强大的切片操作符，极大的简化代码。
```python
print(L[0:750])
```
L[0,750]表示从索引开始，到索引750结束，但是不包括750，是一种左闭右开区间。
```python
L[1:3] # 区间[1,3)
L[:3] # 区间[0,3) 0可以省略
L[-2:] # L[-1]取倒数第一个元素， L[-2]取倒数俩个元素 
L[:10:2] # 前10个数，每两个取一个
L[::5] # 所有数，每5个取一个
L[::-1] # 将原list逆序
```
tuple也是一种具备list属性的序列，唯一区别是tuple不可变。因此，tuple也可以用切片操作，只是操作的结果仍是tuple。

字符串'xxx'也可以看成是一种list，每个元素就是一个字符。因此，字符串也可以用切片操作，只是操作结果仍是字符串。

## 迭代
通常，我们使用for循环来遍历list或者tuple，这种遍历我们成为迭代。在python中通常通过for...in来迭代。

list、tuple、dict、set和字符串都是可以迭代的。但是list这种数据类型有下标，dict没有下标，默认情况下，dict迭代的是key。如果要迭代value，可以使用for value in d.values()，如果要同时迭代key和value，可以用for k, v in d.items()。

python更强大的是，当我们使用for循环时，只要作用于一个可迭代对象，for循环就可以正常运行，而我们不太关心该对象究竟是list还是其他数据类型。那么问题来了，如何判断一个对象是否可迭代呢？python的collections模块的Iterable类型判断:
```python
from collections import Iterable
print(isinstance('abc', Iterable)) # str是否可迭代
print(isinstance([1,2,3], Iterable)) # list是否可迭代
print(isinstance(123, Iterable)) # 整数是否可迭代
```
#### 遍历技巧
当对字典遍历时, 可用 items() 方法同时取回键和对应的值.
```python
knights = {'gallahad': 'the pure', 'robin': 'the brave'}
for k, v in knights.items():
     print(k, v)
```

对序列遍历时, 可以使用 enumerate() 函式来同时取回位置索引和相应的值.
```python
for i, v in enumerate(['tic', 'tac', 'toe']):
     print(i, v)
```

同时对两个或更多的序列进行遍历时, 可用 zip() 进行组合
```python
questions = ['name', 'quest', 'favorite color']
answers = ['lancelot', 'the holy grail', 'blue']
for q, a in zip(questions, answers):
    print('What is your {0}?  It is {1}.'.format(q, a))
```
反向遍历序列时, 先指定这个序列, 然后调用 reversed() 函式
```python
for i in reversed(range(1, 10, 2)):
    print(i)
```
想有序地遍历一个序列, 用 sorted() 函式返回排序后的序列,原序列将不被触及
```python
basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
for f in sorted(set(basket)):
     print(f)
```   
## 列表生成式
列表生成式是python内置的非常简单却强大的可以用来创建list的生成式。

举个例子，要生成list [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]可以用list(range(1, 11))，但是要生成[1x1, 2x2, 3x3, ..., 10x10]呢？

使用列表生成式：``[x * x for x in range(1, 11)]``

加入条件判断：``[x * x for x in range(1, 11) if x % 2 == 0]``

多层循环：``[m + n for m in 'ABC' for n in 'XYZ']``

列表生成式也可以使用两个变量来生成list：
```python
d = {'x': 'A', 'y': 'B', 'z': 'C' }
[k + '=' + v for k, v in d.items()]
```

## 生成器
> 通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

> 所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

要创建一个generator，最简单的就是把一个列表生成式的[]改成()：
```python
L = [x * x for x in range(10)]
print(L)
G = (x * x for x in range(10))
print(G)
```
创建L和g的区别仅在于最外层的[]和()，L是一个list，而g是一个generator。

我们可以通过next()函数获得generator的下一个返回值
```python
print(next(G))
print(next(G))
print(next(G))
print(next(G))
print(next(G))
```

generator保存的是算法，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。

上面这种不断调用next(g)实在是太变态了，正确的方法是使用for循环，因为generator也是可迭代对象:
```python
g = (x * x for x in range(10))
for n in g:
     print(n)
```

如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator。这就是定义generator的另一种方法。
```python
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```
## 迭代器
我们已经知道，可以直接作用于for循环的数据类型有以下几种：<br>
一类是集合数据类型，如list、tuple、dict、set、str等；<br>
一类是generator，包括生成器和带yield的generator function。<br>
这些可以直接作用于for循环的对象统称为可迭代对象：Iterable。<br>

而生成器不但可以作用于for循环，还可以被next()函数不断调用并返回下一个值，直到最后抛出StopIteration错误表示无法继续返回下一个值了。可以被next()函数调用并不断返回下一个值的对象称为迭代器：Iterator。

可以使用isinstance()判断一个对象是否是Iterator对象：
```python
from collections import Iterator
isinstance((x for x in range(10)), Iterator)
```
PS：生成器都是Iterator对象，但list、dict、str虽然是Iterable，却不是Iterator。把list、dict、str等Iterable变成Iterator可以使用iter()函数：
```python
isinstance(iter('abc'), Iterator)
```
想想为什么list、dict、str等数据类型不是Iterator？

这是因为Python的Iterator对象表示的是一个数据流，Iterator对象可以被next()函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误。可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，只能不断通过next()函数实现按需计算下一个数据，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算。

Iterator甚至可以表示一个无限大的数据流，例如全体自然数。而使用list是永远不可能存储全体自然数的。