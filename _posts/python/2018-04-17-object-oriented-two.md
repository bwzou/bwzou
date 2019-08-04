---
layout: post
title: "面向对象（二）"
description: 在Python中，面向对象还有很多高级特性，允许我们写出非常强大的功能。
date:   2018-04-21 16:00:00 +0800
categories: python
author: bwzou
---
正常情况下，当我们定义了一个class，创建了一个class的实例后，我们可以给该实例绑定任何属性和方法，这就是动态语言的灵活性。先定义class：
```python
from types import MethodType

class Student(object):
    pass

s = Student()
s.name = 'Michael'  # 动态给实例绑定一个属性
print(s.name)

def set_age(self, age):  # 定义一个函数作为实例方法
    self.age = age

s.set_age = MethodType(set_age, s)  # 给实例绑定一个方法(创建一个link指向外部的方法)
s.set_age(22)  # 调用实例方法
print(s.age)
```
但是要注意是的给一个实例绑定方法和属性，对另一个是不起作用的。如果要给所有实例都绑定方法，可以给class绑定方法：
```python
class Student(object):
    pass

s = Student()

def set_score(self, score):
    self.score = score

# Student.set_score = MethodType(set_score, Student)
Student.set_score = set_score

s2 = Student()
s.set_score(80)
s2.set_score(80)
print(s.score)
print(s2.score)
```
## 使用__slots__
如果我们想要限制实例的属性,我们可以定义一个特殊的__slots__变量，来限制该class实例能添加的属性：
```python
class Student(object):
    __slots__ = ('name', 'age')  # 用tuple定义允许绑定的属性名称

ss = Student()
ss.name = 'ben'
ss.score = 90
ss.age = 22   # AttributeError: 'Student' object has no attribute 'age'
```
使用__slots__要注意，__slots__定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：
```python
class PostgraduateStudent(Student):
    pass

g = PostgraduateStudent()
g.score = 9999
```
除非在子类中也定义__slots__，这样，子类实例允许定义的属性就是自身的__slots__加上父类的__slots__。
```python
class Student(object):
    __slots__ = ['name', 'score']

class PostgraduateStudent(Student):
    __slots__ = ['age']

ps = PostgraduateStudent()
ps.age = 22
ps.score = 90
print(ps.score)
```
## 多重继承
上一节我们其实也讲了一点继承的概念。继承是面向对象编程的一个重要方式，子类可以通过继承扩展父类的功能。

举一个例子：Dog狗、Bat蝙蝠、Parrot鹦鹉、Ostrich鸵鸟。

如果安装哺乳动物和鸟类归类，我们可以设计出这样的类的层次：
![]({{site.baseurl}}/images/20180417_python_inherit.png){: .shadow}<br>
但是如果按照“能跑”和“能飞”来归类，我们就应该设计出这样的类的层次：
![]({{site.baseurl}}/images/20180417_python_inherit2.png){: .shadow}<br>
如果要把上面的两种分类都包含进来，我们就得设计更多的层次：哺乳类：能跑的哺乳类，能飞的哺乳类；鸟类：能跑的鸟类，能飞的鸟类。这么一来，类的层次就复杂了：
![]({{site.baseurl}}/images/20180417_python_inherit3.png){: .shadow}<br>
如果要再增加“宠物类”和“非宠物类”，这么搞下去，类的数量会呈指数增长，很明显这样设计是不行的。

正确的做法是采用多重继承。首先，主要的类层次仍按照哺乳类和鸟类设计：
```python
class Animal(object):
    pass

# 大类:
class Mammal(Animal):
    pass

class Bird(Animal):
    pass

# 各种动物:
class Dog(Mammal):
    pass

class Bat(Mammal):
    pass

class Parrot(Bird):
    pass

class Ostrich(Bird):
    pass
```
现在，我们要给动物再加上Runnable和Flyable的功能，只需要先定义好Runnable和Flyable的类：
```python
class Runnable(object):
    def run(self):
        print('Running...')

class Flyable(object):
    def fly(self):
        print('Flying...')
```
对于需要Runnable功能的动物，就多继承一个Runnable，例如Dog;对于需要Flyable功能的动物，就多继承一个Flyable，例如Bat;
```python
class Dog(Mammal, Runnable):
    pass

class Bat(Mammal, Flyable):
    pass
```
通过多重继承，一个子类就可以同时获得多个父类的所有功能。

在设计类的继承关系时，通常，主线都是单一继承下来的，例如，Ostrich继承自Bird。但是，如果需要“混入”额外的功能，通过多重继承就可以实现，比如，让Ostrich除了继承自Bird外，再同时继承Runnable。这种设计通常称之为MixIn。

为了更好地看出继承关系，我们把Runnable和Flyable改为RunnableMixIn和FlyableMixIn。类似的，你还可以定义出肉食动物CarnivorousMixIn和植食动物HerbivoresMixIn，让某个动物同时拥有好几个MixIn：
```python
class Dog(Mammal, RunnableMixIn, CarnivorousMixIn):
    pass
```
MixIn的目的就是给一个类增加多个功能，这样，在设计类的时候，我们优先考虑通过多重继承来组合多个MixIn的功能，而不是设计多层次的复杂的继承关系。

## 定制类
上一节我们讲到python有很多类专有方法，其实这些方法都是有特殊用途的，可以帮助我们定制类。
#### \_\_str__
\_\_str__方法用于定义返回的字符串
```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __str__(self):
        return 'Student object (name: %s)' % self.name

print(Student('Yisa'))
```
#### \_\_iter__
如果一个类想和list或tuple那样被用于for ... in循环，就必须实现一个__iter__()方法，该方法返回一个迭代对象，然后Python的for循环就会不断调用该迭代对象的__next__()方法拿到循环的下一个值，直到遇到StopIteration错误时退出循环。

我们以斐波那契数列为例，写一个Fib类，可以作用于for循环：
```python
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1   # 初始化两个计数器a，b

    def __iter__(self):
        return self   # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b   # 计算下一个值
        if self.a > 100000:   # 退出循环的条件
            raise StopIteration()
        return self.a   # 返回下一个值

for i in Fib():
    print(i)
```
#### \_\_getitem__
Fib实例虽然能作用于for循环，看起来和list有点像，但是把它当成list来使用还是不行，比如我们无法通过下标索引。如果要表现得像list那样按照下标取出元素，需要实现__getitem__()方法：
```python
class Fib(object):
    def __getitem__(self, n):
        a, b = 1, 1
        for x in range(n):
            a, b = b, a + b
        return a

print(Fib[0])
```
如果我们还想实现list的切片特性，可以这样：
```python
class Fib(object):
    def __getitem__(self, n):
        if isinstance(n, int): # n是索引
            a, b = 1, 1
            for x in range(n):
                a, b = b, a + b
            return a
        if isinstance(n, slice): # n是切片
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L

print(f[:10])
```
## 枚举类
python为枚举类型定义一个class类型，然后每个常量都是class的一个唯一实例。比如月份1-12。我们可以这样定义：
```python
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```
这样我们就获得了Month类型的枚举类，可以直接使用Month.Jan来引用一个常量，或者枚举它的所有成员：
```python
for name, member in Month.__members__.items():
    print(name, '=>', member, ',', member.value)
```
value属性则是自动赋给成员的int常量，默认从1开始计数。

如果需要更精确地控制枚举类型，可以从Enum派生出自定义类：
```python
from enum import Enum, unique

@unique             # @unique装饰器可以帮助我们检查保证没有重复值
class Weekday(Enum):
    Sun = '00'      # Sun的value被设定为0
    Mon = '11'
    Tue = '22'
    Wed = '33'
    Thu = '44'
    Fri = '55'
    Sat = '66'

for name, member in Weekday.__members__.items():
    print(name, '=>', member, ',', member.value)
```

<style>.shadow{
    box-shadow: 2px 2px 5px #aaa;
    border-radius: 0;
    margin-top: 1em;
    margin-bottom: 1em;
}</style>
