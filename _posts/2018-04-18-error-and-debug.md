---
layout: post
title: "错误与调试"
description: 进行程序开发时，很重要的一环就是进行程序调试。
date:   2018-04-21 18:00:00 +0800
categories: python
author: bwzou
---
到目前为止，我们并没有更多提及错误信息，但是我们在上课的例子和作业中肯定已经或多或少的遇到过不少了。我们平时遇到的错误信息可以分为两种：syntax error和exception , 按中文来说, 就是语法错误和异常。

## 语法错误
语法错误，也可以认为是解析时错误，这是在你学习Python过程中最有可能碰到的：
```
>>> while True print('Hello world')
  File "<stdin>", line 1
    while True print('Hello world')
                   ^
SyntaxError: invalid syntax
```
语法分析器指出错误行，然后显示一个小箭头, 指出检测到错误时最早的那个点。错误一般是由箭头所指的地方导致 (或者至少是此处被检测到): 在这个例子中, 错误是在print()函数这里被发现的, 因为在它之前少了一个冒号 (':'). 文件的名称与行号会被打印出来, 以便于你能找到一个脚本中导致错误的地方.

当然如果我们使用一些IDE，如pycharm进行开发，那么一般都会有提示。

## 异常
尽管语句或表达式语法上是没有问题的, 但是同样也会在尝试运行时导致一个错误. 在执行时探测到的错误就叫做exception, 也就是异常, 但它并不是致命的问题，这一类的异常我们是可以处理的。
```
>>> 10 * (1/0)
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
ZeroDivisionError: int division or modulo by zero
>>> 4 + spam*3
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
NameError: name 'spam' is not defined
>>> '2' + 2
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
TypeError: Can't convert 'int' object to str implicitly
```
错误信息的最后一行指出发生了什么错误。异常也有不同的类型，异常类型做为错误信息的一部分显示出来：示例中的异常分别为:零除错误（ZeroDivisionError）,命名错误（NameError）和类型错误（TypeError）。打印错误信息时，异常的类型作为异常的内置名显示。

我们可以在这里看到[内置的异常](https://docs.python.org/3/library/exceptions.html#bltin-exceptions),列出了内置异常和它们的含义。

#### 异常处理
Python内置了一套try...except...finally...的错误处理机制。当我们认为某些代码可能会出错时，就可以使用try来运行这段代码，如果执行出错，则后续代码不会继续执行，而是直接跳转至错误处理代码，即except语句块，执行完except后，如果有finally语句块，则执行finally语句块，至此，执行完毕。我们看一个例子：
```pyhton
try:
    print('try...')
    r = '2' + 2
    print('result:', r)
except TypeError as e:
    print('except:', e)
finally:
    print('finally...')
```
上面的代码在计算'2' + 2时会产生一个类型错误：
```python
try...
except: must be str, not int
finally...
```
TypeError，因此被执行。最后finally语句被执行。然后程序继续按照流程往下走。如果将‘2’改为2，那么就会输出如下结果：
```python
try...
result: 4
finally...
```
由于没有错误发生，所以except语句块不会被执行，但是finally如果有，则一定会被执行（可以没有finally语句）。

在[内置的异常](https://docs.python.org/3/library/exceptions.html#bltin-exceptions)我们可以看到很多异常类型，那么我们要知道一个try结果里except可以有很多个，用来捕获不同的类型。如：
```python
try:
    print('try...')
    r = '2' + 2
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except TypeError as e:
    print('TypeError:', e)
finally:
    print('finally...')
```
此外，如果没有错误发生，可以在except语句块后面加一个else，当没有错误发生时，会自动执行else语句：
```python
try:
    print('try...')
    r = '2' + 2
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except TypeError as e:
    print('TypeError:', e)
else:
    print('no error!')
finally:
    print('finally...')
```
Python的错误其实也是class，所有的错误类型都继承自BaseException，所以在使用except时需要注意的是，它不但捕获该类型的错误，还把其子类也“一网打尽”。因为except只命中一个，有点类似于if语句。比如：
```python
try:
    print('try...')
    r = 4 + spam*3
    print('result:', r)
except NameError as e:
    print('NameError:', e)
except UnboundLocalError as e:
    print('UnboundLocalError:', e)
else:
    print('no error!')
finally:
    print('finally...')
```
第二个except永远也捕获不到UnicodeError，因为UnboundLocalError是NameError的子类，如果有，也被第一个except给捕获了。

Python所有的错误都是从BaseException类派生的，常见的错误类型和继承关系看[这里](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)：
#### 调用栈
使用try...except捕获错误还有一个巨大的好处，就是可以跨越多层调用，比如函数main()调用second()，second()调用first()，结果first()出错了，这时，只要main()捕获到了，就可以处理：
```python
def first(s):
    return s + 2

def second(s):
    return first(s) * 2

def main():
    try:
        second('0')
    except Exception as e:
        print('Error:', e)
    finally:
        print('finally...')

main()
```
也就是说，不需要在每个可能出错的地方去捕获错误，只要在合适的层次去捕获错误就可以了。这样一来，就大大减少了写try...except...finally的麻烦。

如果错误没有被捕获，它就会一直往上抛，最后被Python解释器捕获，打印一个错误信息，然后程序退出。
```
Traceback (most recent call last):
  File "day7.py", line 24, in <module>
    main()
  File "day7.py", line 22, in main
    second('0')
  File "day7.py", line 19, in second
    return first(s) * 2
  File "day7.py", line 16, in first
    return s + 2
TypeError: must be str, not int
```
我们从上往下可以看到整个错误的调用函数链，然后最后是出错信息。
