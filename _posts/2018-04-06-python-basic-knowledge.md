---
layout: post
title: "python基础知识（一）"
description: 本章涉及基础语法、数据类型与变量
date:   2018-04-06 15:00:00 +0800
categories: code
author: bwzou
---
## 基础语法
在Linux/Unix系统中，你可以在脚本顶部添加以下命令让Python脚本可以像SHELL脚本一样可直接执行：
```python
#!/usr/bin/python3
```

#### 编码
默认情况下，python3源码文件以UTF-8编码，所有字符串都是Unicode字符串。当然你也可以为源码文件指定不同的编码：
```python
# -*- coding: UTF-8 -*- 或者 #coding=utf-8
```

#### 标识符
在编程语言中，对于变量，常量，函数，语句块也有名字，我们统称为标识符。简单来说，标识符就是用来给类、对象、变量等命名的。python标识符有以下三点要注意：
 
 - 第一个字符必须是字母表中字母或下划线_。
 - 标识符的其他部分由字母、数字和下划线组成。
 - 标识符对大小写敏感(student和Student是不同的标识符)。 

#### 保留字
python保留字即关键字，我们不可以把它们用作任何标识符。如何查看python当前版本的所有关键字呢？可以从python的标准库里的keyword模块，可以输出所有关键字：
```python
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 
'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 
'return', 'try', 'while', 'with', 'yield']
```

#### 注释
python里单行注释以#开头；多行注释可以用多个#号，或者使用```和 `"""`
    
    #!/usr/bin/python
    
    # 第一个注释
    # 第二个注释
    
    ```
    第三行注释
    第四行注释
    ```
    
    """
    第五行注释
    第六行注释
    """

#### 行与缩进
python最具特色的就是使用缩进来表示代码块，不需要使用大括号{} 。缩进的空格数是可变的，但是同一个代码块的语句必须包含相同的缩进空格数。

缩进有利有弊。好处是强迫你写出格式化的代码，缩进的另一个好处就是强迫你写出缩进较少的代码。坏处就是“复制粘贴”功能失效了，这样当你想重构代码时，需要重新检查缩进是否正确。
```python
if True:
    print ("True")
else:
    print ("False")
```
#### 多行语句
python通常是一行写完 一条语句的，但是如果语句很长，我们可以使用反斜杠(\)来实现多行语句。例如
```python
total = item_one + \
        item_two + \
        item_three
```
在 [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)，例如：
```python
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```

## 数据类型与变量
#### 整数
python可以处理任意大小的整数，写法和数学上的写法一样，如1,100，-10,0等<br>
也可以使用十六进制表示整数，十六进制用0x前缀和0-9，a-f表示，如0xddff99,0x9ab4c3331等

#### 浮点数
浮点数也就是小数，浮点数可以用数学写法，如1.23, 3.14，-9.02；对于很大或者很小的浮点数则采用科学计数法，把10用e代替，如1.23x10^9就是1.23e9，0.00015可以写成1.5e-4。<br>
整数和浮点数在计算机内部存储的方式是不同的。

#### 字符串
字符串是以单引号'或者双引号"括起来的任意字符。比如"abc"，'123'。要注意''和""都不是字符串的一部分。如果字符串里面包含只""，则可以使用''表示字符串如"I want to 'play'"，如果只包含''则可以使用""包含字符串如'I want to "play"'。<br>
那么现在问题来了，如果字符串中同时包含'和"怎么办呢？可以使用转义字符\来标识，比如：

    'I\'m playing \"football\"!'

表示的字符串内容是：

    I'm playing "football"!

转义字符\可以转义很多字符串，比如\n表示换行，\t表示制表符，字符\本身也要转义，\\表示的字符就是\<br>
如果不行转义怎么办，python使用r''表示内部的字符串默认不转义。

#### 布尔值
一个布尔值只有True、False两种值，要么是True，要么是False(注意大小写)；在Python中，可以直接用True、False表示布尔值，也可以通过布尔值计算出来。
布尔值可以用逻辑运算符and、or和not进行运算。

#### 空值
空值是Python里一个特殊的值，用None表示。None不能理解为0，因为0是有意义的，而None是一个特殊的空值。

#### 变量
什么是变量，变量的概念跟代数的方程变量是一致的，只是在计算机中，变量不仅仅是数字，还可以是任意数据类型。变量在计算机中可以用标识符表示。
```python
a = 123 # a是整数
print(a)
a = 'ABC' # a现在是字符串
print(a)
```
这种变量本身类型不固定的语言称之为动态语言，反之则为静态语言。python 是动态语言，相比于静态语言更加灵活。

猜一下下面这个打印出来的是什么？
```python
a = 'ABC'
b = a
a = 'XYZ'
print(b)
```
    ABC # 为什么？
    
对变量赋值x = y是把变量x指向真正的对象，该对象是变量y所指向的。随后对变量y的赋值不影响变量x的指向。

#### 常量
常量就是不能改变的变量，比如常用的数学常量π就是一个常量。在Python中，通常用全部大写的变量名表示常量：

    PI = 3.14159265359
    
但事实上PI仍然是一个变量，Python根本没有任何机制保证PI不会被改变，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量PI的值，也没人能拦住你。
    