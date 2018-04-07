---
layout: post
title: "python基础知识（三）"
description: 本章涉及条件判断、循环
date:   2018-04-07 01:10:00 +0800
categories: code
author: bwzou
---
## 条件判断 
python条件语句是通过一条或多条语句的执行结果（True或者False）来决定执行的代码块。

#### if语句
python中的if语句的一般形式如下所示：
```python
if condition_1:
    statement_block_1
elif condition_2:
    statement_block_2
else:
    statement_block_3
```
python中是用了elif代替了else if，所以if语句的关键字为：if-elif-else。<br>
PS：1、每一个条件后面要使用冒号：，表示接下来是满足条件后要执行的语句块。<br>
2、是用缩进来划分语句块，相同缩进的语句在一起组成一个语句块。<br>
3、在python中没有switch-case语句。<br>
4、可以把if-elif-else结构放在另外一个if-elif-else结构中。
```python
age = 16
if age >= 18:
    print("adult")
elif age >= 6:
    print("teenager")
else:
    print("kid")
```
if语句执行特点是，它从上往下执行，如果在某个添加判断为True，就执行对应的语句块，然后忽略掉剩下的所有elif和else；一个if结构里，可以有很多个elif；if判读条件可以简写，比如
```python
if x:  # x是非零数值、非空字符串、非空list等，就判断为True，否则为False。
    print("True")
```
## 循环
python中的循环语句有for和while。
#### for循环
for循环可以遍历任何序列的项目，如一个列表后者一个字符串。for循环的一般格式如下：
```python
for <variable> in <sequence>:
    <statements>
else:
    <statements>
```
举个栗子：
```python
languages = ["C", "C++", "Perl", "Python"]
for x in languages:
    print9(x) 
```
for x in ...循环就是把每个元素代入变量x，然后执行缩进块的语句。<br>
for 语句也可以嵌套多层。<br>
我们来编写一个求和函数sum，计算1..100的值，(使用range()函数可以生成一个整数序列，再使用list()转换成list)
#### while循环
while循环，只要条件满足就不断循环，条件不满足时退出循环。while语句的一般形式：
```python
while 判断条件:
    语句
```
比如我们要计算100以内所有奇数之和，可以用while循环实现：
```python
sum = 0
n = 99
while n > 0:
    sum = sum + n
    n = n - 2
print(sum)
```
while循环使用else语句，在 while … else 在条件语句为 false 时执行 else 的语句块
```python
count = 0
while count < 5:
   print (count, " 小于 5")
   count = count + 1
else:
   print (count, " 大于或等于 5")
```
while语句也可以嵌套多层。<br>
我们来编写一个猜字游戏，预先在纸上写一个字，然后让对方猜，每次只有三中结果，“恭喜，你猜对了”，“猜的数字小了”，“猜的数字大了”。
#### break
break 语句可以跳出 for 和 while 的循环体。意思就是说，如果程序执行到break语句，就退出循环体，所有循环的语句都不在执行。
```python
n = 1
while n <= 100:
    if n > 10: # 当n = 11时，条件满足，执行break语句
        break # break语句会结束当前循环
    print(n)
    n = n + 1
print('END')
```
#### continue
continue语句被用来告诉python跳过当前循环块中的剩余语句，然后继续进行下一轮循环。
```python
n = 0
while n < 10:
    n = n + 1
    if n % 2 == 0: # 如果n是偶数，执行continue语句
        continue # continue语句会直接继续下一轮循环，后续的print()语句不会执行
    print(n)
```
