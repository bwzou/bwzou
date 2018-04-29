---
layout: post
title: "IO编程"
description: IO在计算机中指Input/Output，也就是输入和输出。
date:   2018-04-21 22:00:00 +0800
categories: python
author: bwzou
---
IO在计算机中指的是Input/Output,也就是输入和输出。在这一节我们首先需要了解一个概念，同步IO和异步IO。由于CPU和内存的速度远远高于外设的速度，所以在IO编程中存在速度严重不匹配的问题。举个例子来说，比如要把100M的数据写入磁盘，CPU输出100M的数据只需要0.01秒，可是磁盘要接收这100M数据可能需要10秒，怎么办呢？有两种办法：

第一种是CPU等着，也就是程序暂停执行后续代码，等100M的数据在10秒后写入磁盘，再接着往下执行，这种模式称为同步IO；

另一种是CPU不等待，后续代码可以继续接着执行，而磁盘呢，照着正常速度写入磁盘，写完了就通知CPU说“我写完了”，这种模式称为异步IO。

同步和异步的区别就在于是否等待IO执行的结果。

## 文件读写
读写文件是最常见的IO操作。python内置了读写文件的函数。读写文件前，我们先必须了解一下，在磁盘上读写文件的功能都是由操作系统提供的，现代操作系统不允许普通的程序直接操作磁盘，所以，读写文件就是请求操作系统打开一个文件对象（通常称为文件描述符），然后，通过操作系统提供的接口从这个文件对象中读取数据（读文件），或者把数据写入这个文件对象（写文件）。

python内置了函数open()，它返回文件对象，通常的用法需要两个参数：open(filename, mode)
```python
f = open('day10.py', 'r')
```
第一个参数是一个含有文件名的字符串。第二个参数也是一个字符串，含有描述如何使用该文件的几个字符。mode 为 'r' 时表示只是读取文件；'w' 表示只是写入文件（已经存在的同名文件将被删掉）；'a' 表示打开文件进行追加，写入到文件中的任何数据将自动添加到末尾。 'r+' 表示打开文件进行读取和写入。mode 参数是可选的，默认为 'r'。

如果文件不存在，open()函数就会抛出IOError的错误。
```
>>> f = open('day10.py', 'r')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
FileNotFoundError: [Errno 2] No such file or directory: 'day10'
```
要读取非UTF-8编码的文本文件，需要给open()函数传入encoding参数，例如，读取GBK编码的文件：
```python
f = open('day10.py', 'r', encoding='gbk')
f.read()
```
遇到有些编码不规范的文件，我们可能会遇到UnicodeDecodeError，因为在文本文件中可能夹杂了一些非法编码的字符。遇到这种情况，open()函数还接收一个errors参数，表示如果遇到编码错误后如何处理。最简单的方式是直接忽略：
```python
f = open('day10.py', 'r', encoding='gbk', errors='ignore')
```

#### 文件对象方法
如果文件打开成功，我们可以使用f.read(size)要读取文件内容，该方法读取若干数量的数据并以字符串形式返回其内容，size是可选的数值，指定字符串长度。如果没有指定size或者指定为负数，就会读取并返回整个文件。如果到了文件末尾，f.read()会返回一个空字符串'':
```python
f = open('day10.py', 'r')
f.read()
f.read()
f.close()
```
调用readline()可以每次读取一行内容，调用readlines()一次读取所有内容并按行返回list。因此要根据需要决定怎么调用。如果文件很小，read()一次性读取最方便；如果不能确定文件大小，反复调用read(size)比较保险；如果是配置文件，调用readlines()最方便：
```python
for line in f.readlines():
    print(line.strip()) # 把末尾的'\n'删掉
```
你可以循环遍历文件对象来读取文件中的每一行。这是一种内存高效、快速，并且代码简介的方式:
```python
for line in f:
    print(line, end='')
```
f.write(string) 方法将 string 的内容写入文件，并返回写入字符的长度:
```python
f.write('This is a test\n')
```
想要写入其他非字符串内容，首先要将它转换为字符串:
```python
value = ('the answer', 42)
s = str(value)
f.write(s)
```
最后一步是调用close()方法关闭文件。文件使用完毕后必须关闭，因为文件对象会占用操作系统的资源，并且操作系统同一时间能打开的文件数量也是有限的。在调用 f.close() 方法后，试图再次使用该文件对象将会自动失败。
```
>>> f.close()
>>> f.read()
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
ValueError: I/O operation on closed file
```
由于文件读写时都有可能产生IOError，一旦出错，就停止运行，那么后面的f.close()就不会调用。所以为了保证无论是否出错都能正确地关闭文件，我们可以使用try ... finally来实现：
```python
try:
    f = open('day10.py', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```
python提供了关键字with处理文件对象，它的先进之处在于文件用完后会自动关闭，就算发生异常也没关系。它是 try-finally 块的简写:
```python
with open('workfile', 'r') as f:
    read_data = f.read()
```
#### 使用json存储结构化数据
从文件中读写字符串很容易。数值就要多费点儿周折，因为 read() 方法只会返回字符串，应将其传入 int() 这样的函数，就可以将 '123' 这样的字符串转换为对应的数值 123。当你想要保存更为复杂的数据类型，例如嵌套的列表和字典，手工解析和序列化它们将变得更复杂。

Python 允许你使用常用的数据交换格式[JSON（JavaScript Object Notation）](http://json.org/)。标准模块[json](https://docs.python.org/3/library/json.html#module-json)可以接受Python数据结构，并将它们转换为字符串表示形式；此过程称为 **序列化**。从字符串表示形式重新构建数据结构称为 **反序列化**。序列化和反序列化的过程中，表示该对象的字符串可以存储在文件或数据中，也可以通过网络连接传送给远程的机器。

JSON表示的对象就是标准的JavaScript语言的对象，JSON和Python内置的数据类型对应如下：

JSON类型     |  Python类型
-------------|----------------------
{}           | dict
[]           | list
"string"     | str
1234.56      | int或者float
true/false   | True/False
null         | None

如果你有一个对象 x，你可以用简单的一行代码查看其 JSON 字符串表示形式:
```python
import json
x = [1, 'simple', 'list']
json.dumps(x)
```
dumps() 函数的另外一个变体 dump()，直接将对象序列化到一个file-like Object。所以如果 f 是为写入而打开的一个[file-like Object](https://docs.python.org/3/glossary.html#term-file-object)，我们可以这样做:
```python
json.dump(x, f)
```
要把JSON反序列化为Python对象，用loads()或者对应的load()方法，前者把JSON的字符串反序列化，后者从file-like Object中读取字符串并反序列化:
```python
json_str = '{"age": 20, "score": 88, "name": "Bob"}'
json.loads(json_str)
```
为了重新解码对象，如果 f 是为读取而打开的file-like Object:
```python
x = json.load(f)
```
## 操作文件和目录

## 读取csv文件



