---
layout: post
title: "函数（一）"
description: 函数式最基本的一种代码抽象的方式
date:   2018-04-11 12:00:00 +0800
categories: python
author: bwzou
---
在数学上，抽象是最常见的概念。而函数确是代码抽象的一种最基本方式。
## 函数定义
你可以定义一个有自己想要功能的函数，定义的简单规则：1、函数代码以def关键字开头，后接函数标识符名称和圆括号()。2、任何传入参数和自变量必须放在圆括号中间。3、函数第一行语句可以使用注释，写出函数的说明。4、函数内容以冒号起始，并且缩进。5、可以使用return返回
```python
def 函数名（参数列表）:   # 函数定义的一般格式
    函数体
```
举个栗子：
```python
def area(radius):
    # 求取圆的面积
    pi = 3.1415926
    return pi * radius ** 2

print(area(2))
```

## 函数调用
调用python函数，需要根据函数定义，传入正确的参数。如果函数调用出错，肯定会报出错的信息，一定要学会看错误信息！！！

python内置了很多有用的函数，我们也可以直接调用，具体可以看[官网](https://docs.python.org/3/library/functions.html)。我们也可以在python交互模式下通过help函数查看函数的帮助信息。
```python
help(max)
Help on built-in function max in module builtins:

max(...)
    max(iterable, *[, default=obj, key=func]) -> value
    max(arg1, arg2, *args, *[, key=func]) -> value

    With a single iterable argument, return its biggest item. The
    default keyword-only argument specifies an object to return if
    the provided iterable is empty.
    With two or more arguments, return the largest argument.
```

## 函数参数
python函数定义非常简单，但却非常灵活。除了正常定义的必选参数外，还可以使用默认参数、可变参数和关键字参数，使得定义出来的函数不但能处理复杂的参数，还可以简化调用者的代码。

#### 位置参数
什么叫位置参数呢？将传入的值按照位置顺序依次赋予参数统称为位置参数。好吧我们还是来举个栗子吧
```python
def power(x,n):
    return x ** n
    
power(2,5)     # 输出32
```
像这样，2和5都必须按位置顺序传入，不能反过来5和2。因为他们都是在函数里表达不同信息。2是基数，5是指数。

#### 默认参数
默认参数可以简化函数的调用。设置默认参数时，有几点要注意：<br>
一是必选参数在前，默认参数在后（为什么）。
二是如何设置默认参数。当函数有多个参数时，把变化大的参数放前面，变化小的参数放后面。变化小的参数就可以作为默认参数。
```python
def power(x,n):
    return x ** n
    
power(2,2)     # 输出4
```

#### 可变参数
在Python函数中，还可以定义可变参数。顾名思义，可变参数就是传入的参数个数是可变的。

我们以数学题为例子，给定一组数字a，b，c……，请计算a2 + b2 + c2 + ……。该怎么做呢？


定义可变参数和定义一个list或tuple参数相比，仅仅在参数前面加了一个*号。在函数内部，参数numbers接收到的是一个tuple，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括0个参数：

如果已经有一个list或者tuple，要调用一个可变参数怎么办？这种写法当然是可行的，问题是太繁琐，所以Python允许你在list或tuple前面加一个*号，把list或tuple的元素变成可变参数传进去


#### 关键字参数
而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。举个栗子：
```python
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)
```

## 匿名函数
python 使用 lambda 来创建匿名函数。所谓匿名，意即不再使用 def 语句这样标准的形式定义一个函数。

- lambda 只是一个表达式，函数体比 def 简单很多。
- lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
- lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。

```python
# 可写函数说明
sum = lambda arg1, arg2: arg1 + arg2;
 
# 调用sum函数
print ("相加后的值为 : ", sum( 10, 20 ))
```

## 递归函数
在函数内部，可以调用其他函数。如果一个函数在内部调用函数本身，那么这就是一个递归函数。

举个例子，我们来计算阶乘n! = 1 x 2 x 3 x ...x n-1 x n，用函数fact(n)表示，可以看出：<br>
fact(n) = n! = 1 x 2 x 3 x ... x n-1 x n = (n-1)! x n = n x fact(n-1) 这就是递推式 <br>
于是，fact(n)可以表示为n x fact(n-1)，只有n=1时需要特殊处理。fact(n)用递归的方式写出来就是：
```python
def fact(n):
    if n==1:
        return 1
    return n * fact(n-1)
```
PS：特别要注意的是，使用递归一定会有递归终止条件，如上面的 `if n==1` 就是递归终止条件，不然会无限递归直到栈溢出。 

