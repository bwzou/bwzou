---
layout: post
title: "面向对象（一）"
description: Object Oriented Programming，OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。
date:   2018-04-21 10:00:00 +0800
categories: python
author: bwzou
---
面向对象最重要的概念就是类（Class）和实例（Instance），必须牢记类是抽象的模板，比如Student类，而实例是根据类创建出来的一个个具体的“对象”，每个对象都拥有相同的方法，但各自的数据可能不同。

在Python中，所有数据类型都可以视为对象，当然也可以自定义对象。自定义的对象数据类型就是面向对象中的类（Class）的概念。

## 类的定义
定义类的一般格式为：
```python
class Student(object):
    pass
```
class后面是类名字，即Student，类名通常是大写开头的单词，紧接着是(object)，表示该类是从哪个类继承下来的，继承的概念我们后面再讲，通常如果没有合适的继承类，就使用object类，这是所有类最终都会继承的类。

定义好类之后，就可以根据Student类创建实例，方式如下：
```python
yisa = Student()
print(yisa)
```
我们可以给类绑定属性（属性——可以理解是类里面定义的变量），例如：
```python
yisa = Student()
yisa.name = 'YiSha'
print(yisa.name)
```
类提供__init__方法，类的实例化操作会自动调用__init__方法，我们可以通过__init__把一些我们认为必须绑定的属性强制填写进去。
```python
class Student(object):
    
    def __init__(self, name, score):
        self.name = name
        self.score = score
```
注意：有了__init__方法，在创建实例的时候，就不能传入空的参数了，必须传入与__init__方法匹配的参数，但self不需要传。

类的方法与普通的函数有一个特别的区别，它们的第一个参数永远是self，表示创建的实力本身；。但self不需要传，python解析器自己会把实例变量传进去

## 类的属性
#### 1、直接在类中定义属性
定义类的属性，当然最简单最直接的就是在类中定义，例如：
```python
class Student(object):
    name = 'yisa'
```
#### 2、在构造函数中定义属性
就是在构造对象的时候，对属性进行定义。
```python
class Student(object):
    
    def __init__(self, name, score):
        self.name = name
        self.score = score
```
#### 3、属性的访问限制
在Class内部，可以有属性和方法，而外部代码可以通过直接调用实例变量的方法来操作数据，这样，就隐藏了内部的复杂逻辑。一般来说，类有几个级别的访问控制，分别为公开属性、私有属性、和特殊属性。举个例子：
```python
class Student(object):
    def __init__(self, name, score)
        self.name = 'yisa'
        self._score = '90'
        self.__age = '25'
```
`name表示公开属性；_score表示私有属性，但是我们想访问还是可以访问到的，这不是python语法上规范，只是编程规范而已；__age表示私有属性，在创建这个类的时候，是不可以直接访问到两个下划线的属性的，但是还有有方法可以访问到，不能直接访问__age是因为Python解释器对外把__age变量改成了_Student__age，所以，仍然可以通过_Student__age来访问__age变量；形如__xxx__的变量是特殊变量，只有当文档有说明时使用，不要自己定义这类变量，特殊变量是可以直接访问的，不是私有变量，所以不能用__name__、__score__这样的变量名。`

## 类的方法
#### 1、类专有的方法

方法            |	说明
----------------|-----------------------
__init__	    |构造函数，在生成对象时调用
__del__	        |析构函数，释放对象时使用
__repr__	    |打印，转换
__setitem__     |按照索引赋值
__getitem__	    |按照索引获取值
__len__	        |获得长度
__cmp__	        |比较运算
__call__	    |函数调用
__add__	        |加运算
__sub__	        |减运算
__mul__	        |乘运算
__div__	        |除运算
__mod__	        |求余运算
__pow__	        |乘方

这里我们举一个例子：其他以此类推
```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def __add__(self, other):
        print(self.score + other.score)

    def print_score(self):
        print('%s: %s' % (self.name, self.score))


Yisa = Student('Yisa', 90)
Linc = Student('Linc', 90)

Yisa + Linc
```

#### 2、方法的访问控制
其实我们也可以把方法看成是类的属性的，那么方法的访问控制也是跟属性是一样的，也是没有实质上的私有方法。一切都是靠程序员自觉遵守 Python 的编程规范。
```python
class Student(object):
    def getName(self):
        pass
     
    def _getScore(self):
        pass
       
    def __getAge(self):
        pass
```
#### 3、方法的装饰器
- @classmethod 调用的时候直接使用类名类调用，而不是某个对象
- @property 可以像访问属性一样调用方法

```python
class Student(object):
    gender = 'female'

    def __init__(self, name, score):
        self.name = name
        self.score = score

    @classmethod
    def print_name(cls):         # 通常以cls开头
        return cls.gender

    @property
    def print_score(self):
        return '%s: %s' % (self.name, self.score)

if __name__ == '__main__':
    Yisa = Student('Yisa', 90)

    print(dir(Yisa))   # 打印所有属性

    print(Yisa.__dict__)   # 打印构造函数中的属性

    print(Yisa.gender)  # 直接访问属性

    print(Student.gender)   # 直接使用类名类调用，而不是某个对象

    print(Yisa.print_score)     # 像访问属性一样调用方法（注意看get_age是没有括号的）
```
## 类的继承
#### 1、定义类的继承
```python
class ClassName(BaseClassName):
    <statement-1>
    .
    .
    .
    <statement-N>
```
当然上面的是单继承，Python 也是支持多继承的，具体的语法如下：
```python
class ClassName(Base1,Base2,Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```    
多继承有一点需要注意的：若是父类中有相同的方法名，而在子类使用时未指定，python 在圆括号中父类的顺序，从左至右搜索 ， 即方法在子类中未找到时，从左到右查找父类中是否包含方法。

继承的子类的好处：

- 会继承父类的属性和方法
- 可以自己定义，覆盖父类的属性和方法

#### 2、调用父类的方法
一个类继承了父类后，可以直接调用父类的方法的，举个例子：
```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        return '%s: %s' % (self.name, self.score)


class CollegeStudent(Student):
    pass


if __name__ == '__main__':
    Yisa = CollegeStudent('Yisa', 100)
    print(Yisa.print_score())
```
#### 3、父类方法的重写
子类还可以修改父类中定义的方法。
```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        return '%s: %s' % (self.name, self.score)


class CollegeStudent(Student):

    def print_score(self):
        if self.score >= 90:
            return 'A'
        elif self.score >= 80:
            return 'B'
        elif self.score >= 60:
            return 'C'
        else:
            return 'D'


if __name__ == '__main__':
    Yisa = CollegeStudent('Yisa', 90)
    print(Yisa.print_score())
```
#### 4、子类的类型判断
对于 class 的继承关系来说，有些时候我们需要判断 class 的类型，可以使用 isinstance() 函数。
```python
class Student(object):
    pass

class Student2(Student):
    pass

class Student3(Student2):
    pass

if __name__ == '__main__':
    student3 = Student3()

    print(isinstance(student3, Student3))  # isinstance()就可以告诉我们，一个对象是否是某种类型
    print(isinstance(student3, Student2))
    print(isinstance(student3, Student))
```
## 类的多态
多态的概念其实不难理解，它是指对不同类型的变量进行相同的操作，它会根据对象（或类）类型的不同而表现出不同的行为。

比如，我们经常用到多态的性质，比如：
```python
print(1+2)
print('a'+'b')
```
可以看到，我们对两个整数进行+操作，会返回他们的和，对两个字符串进行相同的+操作，会返回拼接后的字符串，还有之前我们对两个字符串进行+操作，会执行我们定义在__add__函数的内容。也就是说，不同类型的对象对同一消息会作出不同的响应。
```python
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        return '%s: %s' % (self.name, self.score)


class CollegeStudent(Student):

    def print_score(self):
        if self.score >= 90:
            return 'A'
        elif self.score >= 80:
            return 'B'
        elif self.score >= 60:
            return 'C'
        else:
            return 'D'


class PostgraduateStudent(Student):

    def print_score(self):
        if self.score >= 90:
            return 'perfect'
        elif self.score >= 80:
            return 'great'
        elif self.score >= 60:
            return 'good'
        else:
            return 'come on!'


def printScore(student):
    return student.print_score()


if __name__ == '__main__':
    cs = CollegeStudent('Bwzou', 90)
    print(printScore(cs))
    ps = PostgraduateStudent('Huan', 90)
    print(printScore(ps))
```
CollegeStudent和PostgraduateStudent是两个不同的对象，对他们调用printScore方法，它们会自动调用实际类型的printScore方法，作出不同的响应。这就是多态的魅力。
