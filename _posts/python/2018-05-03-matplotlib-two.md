---
layout: post
title: "Matplotlib(二)"
description: Matplotlib是python科学计算中经常用到的绘图工具包。
date: 2018-05-03 18:00:00 +0800
categories: python
author: bwzou
---
我发现一篇讲matplotlib讲的很好的文章，在此把它翻译过来，原文地址[点这里](https://heartbeat.fritz.ai/introduction-to-matplotlib-data-visualization-in-python-d9143287ae39)

## 绘图区剖析
Plot中有两个关键组件; 即Figure和Axes。

Figure是顶级容器，充当绘制所有内容的窗口或页面。 它可以包含多个独立的figures, multiple Axes, a subtitle  (figure的标题), a legend, a color bar等等

Axes是我们绘制数据的区域以及与之关联的任何标签/标记。 每个轴都有一个X轴和一个Y轴。

## 绘制图形的两种方法
#### 函数方法
使用基本的matplotlib命令，我们可以轻松创建一个图。 让我们使用两个Numpy数组x和y绘制一个示例：
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.linspace(0, 10, 20)
y = x ** 2

plt.plot(x,y)
plt.show()
```
现在我们绘制一个图形，让我们继续使用.xlabel（）,. ylabel（）和.title（）命名x轴，y轴，和添加标题：
```python
plt.plot(x,y)
plt.title('Our first Plot')
plt.xlabel('X Label')
plt.ylabel('Y Label')
```
如果我们需要在画布上显示不止一个绘图区，Matplotlib允许我们使用.subplot（）方法在同一图上轻松创建多个图。 .subplot（）方法接受三个参数，即：

- nrows：窗口应该具有的行数。
- ncols：窗口应该具有的列数。
- plot_number：指的是窗口中的特定绘图区。

使用.subplot（）我们将在同一个画布上创建两个图：
```python
plt.subplot(1, 2, 1)
plt.plot(x, y, 'red')

plt.subplot(1, 2, 2)
plt.plot(y, x, 'green')
```
注意这两个图有不同的颜色。 这是因为我们需要能够区分这些图。 这可以通过简单地将颜色属性设置为“红色”和“绿色”来实现，如上所示。

#### 面向对象的接口
这是创建绘图的最佳方式。 这里的想法是创建图对象并从中调用方法。 让我们使用.figure（）方法创建一个空白Figure对象。
```python
fig = plt.figure()
```

现在我们需要使用.add_axes（）方法为它添加一组轴。 add_axes（）方法接受一个包含四个参数的列表（left，bottom，width和height —— 这些轴应该放置的位置），范围从0到1.这是一个示例：
```python
fig = plt.figure()
ax = fig.add_axes([0.1, 0.2, 0.8, 0.9])
```

如你所见，我们有一组空白的轴。 现在让我们在它上绘制我们的x和y数组：
```python
fig = plt.figure()
ax = fig.add_axes([0.1, 0.2, 0.8, 0.9])
ax.plot(x, y, 'purple')
```
我们可以进一步添加x和y标签以及我们的绘图标题，但这里和在函数方法中略有不同。

使用.set_xlabel（）,. set_ylabel（）和.set_title（）让我们继续为我们的绘图区添加标签和标题：
```python
fig = plt.figure()
ax = fig.add_axes([0.1, 0.2, 0.8, 0.9])

ax.plot(x, y, 'purple')
ax.set_xlabel('X Lable')
ax.set_ylabel('Y Lable')
ax.set_title('Our First Plot using Object Oriented Approach')
```

还记得吗，我们注意到一个窗口可以包含多个绘图区。 让我们尝试在一个画布上放入两组绘图区：
```python
fig = plt.figure()
axes1 = fig.add_axes([0.1, 0.2, 0.8, 0.9])
axes2 = fig.add_axes([0.2, 0.5, 0.4, 0.3])
```

现在让我们在我们创建的轴上绘制x和y数组：
```python
fig = plt.figure()
axes1 = fig.add_axes([0.1, 0.2, 0.8, 0.9])
axes2 = fig.add_axes([0.2, 0.5, 0.4, 0.3])

axes1.plot(x, y)
axes2.plot(y, x)
```

快速练习：现在我们已准备好绘图，看看你是否可以设置标题，两个轴的x和y标签。

就像我们在函数方法中所做的那样，我们也可以使用.subplots（）方法和NOT .subplot（）在面向对象的方法中创建多个图。 .subplots（）方法接受nrows，这是图应该具有的行数，ncols是图应该具有的列数。

例如，我们可以创建一个3乘3的子图，如下所示：
```python
fig, axes = plt.subplot(nrows=3, ncols=3)
```

我们刚刚做的是我们使用解构赋值从图形对象中抓取轴，这给了我们3个3的子图。 如我们所见，在我们创建的子图中存在重叠问题。 我们可以通过使用.tight_layout（）方法来解决这个问题：
```python
fig, axes = plt.subplot(nrows=3, ncols=3)
plt.tight_layout()
```
plt.figure（）和plt.subplots（）之间的唯一区别是plt.subplots（）根据你指定的行数和列数自动执行.figure（）的.add_axes（）方法。

现在我们知道如何创建子图，让我们看看如何在它们上绘制x和y数组。 我们想分别在索引位置（0,1）的轴上绘制x，y，在位置（1,2）的轴上绘制y，x：
```python
fig, ax = plt.subplot(nrows=3, ncols=3)

ax[0, 1].plot(x, y)
ax[1, 0].plot(y, x)
plt.tight_layout()
```

快速练习：看看你是否可以为两个轴设置标题和x和y标签。

## Figure size, aspect ratio, and DPI
Matplotlib允许我们通过简单地指定figsize和dpi参数来指定图形大小，宽高比和DPI来创建自定义图。figsize是图形宽度和高度的元组（以英寸为单位），dpi是dots-per-inch（每英寸像素数）。

在前面的示例中，我们没有指定figsize和dpi，因此matplotlib采用了默认值。 现在，让我们继续并指定我们想要一个宽度= 8，高度= 2，dpi = 100的图形。
```python
fig = plt.figure(figsize=(8, 2), dpi=100)
ax = fig.add_axes([0, 0, 1, 1])
ax.plot(x, y)
```
我们可以用subplots（）做同样的事情：
```python
fig, ax = plt.subplot(nrows=2, ncols=1, figsize=[8, 4], dpi=100)

ax[0, 1].plot(x, y)
ax[1, 0].plot(y, x)
plt.tight_layout()
```
现在我们已经学会了如何创建绘图，让我们学习如何保存它们以供将来使用。

## 如何保存图片  
我们可以使用Matplotlib生成高质量的图形并以多种格式保存它们，例如png，jpg，svg，pdf等。使用.savefig（）方法，我们将上图保存在名为my_figure.png的文件中
```python
fig.savefig(‘my_figure.png’)
```

## 