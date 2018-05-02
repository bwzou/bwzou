---
layout: post
title: "Matplotlib"
description: Matplotlib是python科学计算中经常用到的绘图工具包。
date: 2018-04-30 08:00:00 +0800
categories: python
author: bwzou
---
## 什么是Matplotlib
简单来说，Matplotlib 是 Python 的一个绘图库。它包含了大量的工具，你可以使用这些工具创建各种图形，包括简单的散点图，正弦曲线，甚至是三维图形。Python 科学计算社区经常使用它完成数据可视化的工作。

在学习matplotlib之前，我们先浏览一下它的[图库](https://matplotlib.org/gallery.html)，看看它都能绘制写什么样的图形。使用matplotlib将变得很简单，因为你可以看到所有图片的代码，然后替换成你的数据即可。

还是那句话，[官方文档](https://matplotlib.org/contents.html)绝对是最好的学习资源。这里也只是抛砖引玉。

## 使用Matplotlib
在使用前，我还是要强调一下的是：遵循使用matplotlib规范，采用如下方式引入matplotlib：
```python
import matplotlib.pyplot as plt
```
我们来画一个简单的正弦图片：
```python
import matplotlib.pyplot as plt
import numpy as np

t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)

plt.figure()    # 定义一个图像窗口
plt.plot(t, s)  # 画曲线
plt.grid()      # 是否显示网格
plt.show()      # 显示绘制图像
```
我们可以在一个窗口里绘制多个图像：
```python
import matplotlib.pyplot as plt
import numpy as np

t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)
y = 2*t

plt.figure(num=1, figsize=(8, 5),)
plt.plot(t, s)
plt.grid()
plt.plot(t, y, color='red', linewidth=1.0, linestyle='--')
plt.show()
```
使用plt.figure定义一个图像窗口：编号为1；大小为(8, 5)。使用plt.plot画(t ,s)曲线； 使用plt.plot画(t ,y)曲线，曲线的颜色属性(color)为红色；曲线的宽度(linewidth)为1.0；曲线的类型(linestyle)为虚线。使用plt.show显示图像。

```python
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt
import numpy as np

t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)
y = 2*t

plt.figure(num=1, figsize=(8, 5),)
plt.plot(t, s)
plt.grid()
plt.plot(t, y, color='red', linewidth=1.0, linestyle='--')

plt.xlim((-1, 3))
plt.ylim((-2, 5))
plt.xlabel(u'时间（s）')
plt.ylabel(u'值')
plt.show()
```
我们使用plt.xlim设置x坐标轴范围：(-1, 3)； 使用plt.ylim设置y坐标轴范围：(-2, 5)； 使用plt.xlabel设置x坐标轴名称：u'时间（s）'； 使用plt.ylabel设置y坐标轴名称：u'值'；如果你正常运行上面代码，毫无疑问会遇到中文乱码问题。记得加上下面代码：
```python
import matplotlib.pyplot as plt
import numpy as np
plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号
```
使用plt.xticks设置x轴刻度：范围是(-1,2)，个数是5。使用plt.yticks设置y轴刻度以及名称：刻度为[-2, -1.8, -1, 1.22, 3]；对应刻度的名称为[‘really bad’,’bad’,’normal’,’good’, ‘really good’]。使用plt.show显示图像。
```python
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt
import numpy as np
plt.rcParams['font.sans-serif'] = ['SimHei']  # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号

t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)
y = 2*t

plt.figure(num=1, figsize=(8, 5),)
plt.plot(t, s)
plt.grid()
plt.plot(t, y, color='red', linewidth=1.0, linestyle='--')

new_ticks = np.linspace(-1, 2, 5)
print(new_ticks)
plt.xticks(new_ticks)
plt.yticks([-1, 0, 0.5, 1.22, 3],[r'$really\ bad$', r'$bad$', r'$normal$', r'$good$', r'$really\ good$'])
plt.show()
```
如果我们要添加图例，首先我们设置两条线的类型等信息（蓝色实线与红色虚线)。legend将要显示的信息来自于绘制线条时的label。 
```python
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt
import numpy as np

t = np.arange(0.0, 2.0, 0.01)
s = 1 + np.sin(2 * np.pi * t)
y = 2*t

plt.figure(num=1, figsize=(8, 5),)
plt.plot(t, s, label='linear line')
plt.plot(t, y, color='red', linewidth=1.0, linestyle='--', label='square line')
plt.legend(loc='upper right')   

plt.show()
```
参数 loc='upper right' 表示图例将添加在图中的右上角。其中’loc’参数有多种，’best’表示自动分配最佳位置，其余的如下：
```
 'best' : 0,          
 'upper right'  : 1,
 'upper left'   : 2,
 'lower left'   : 3,
 'lower right'  : 4,
 'right'        : 5,
 'center left'  : 6,
 'center right' : 7,
 'lower center' : 8,
 'upper center' : 9,
 'center'       : 10,
```
#### 一个figure里有多个绘图区
我们可以使用subplot在一个窗口创建多个绘图区绘制。``subplot(nrows, ncols, index, **kwargs)`` nrows，ncols表示创建几行几列，绘图区个数=nrows*ncols；index表示使用哪一个子绘图区，默认从1开始。
```python
import numpy as np
import matplotlib.pyplot as plt


x1 = np.linspace(0.0, 5.0)
x2 = np.linspace(0.0, 2.0)

y1 = np.cos(2 * np.pi * x1) * np.exp(-x1)
y2 = np.cos(2 * np.pi * x2)

plt.subplot(2, 1, 1)
plt.plot(x1, y1, 'o-')
plt.title('A tale of 2 subplots')
plt.ylabel('Damped oscillation')

plt.subplot(2, 1, 2)
plt.plot(x2, y2, '.-')
plt.xlabel('time (s)')
plt.ylabel('Undamped')

plt.show()
```
#### 读取一张图片
我们使用 matplotlib.image 读取图片。
```python
import matplotlib.pyplot as plt
import matplotlib.image as mpimg

lena = mpimg.imread('./lena.jpg')   #  此时 lena 就已经是一个 np.array 了，可以对它进行任意处理

fig, ax = plt.subplots()   
ax.imshow(lena) 
ax.axis('off')  # clear x- and y-axes
plt.show()
```
r如果我们想只显示某个通道
```python
lena_1 = lena[:,:,0]   # 显示图片的第一个通道
plt.imshow('lena_1')
plt.show()
```
此时会发现显示的是热量图，不是我们预想的灰度图，可以添加 cmap 参数，有如下几种添加方法：
```python
plt.imshow('lena_1', cmap='Greys_r')  
plt.show()
img = plt.imshow('lena_1')
img.set_cmap('gray')   # 'hot' 是热量图
plt.show()
```
#### 绘制直方图
```python
import numpy as np
import matplotlib.pyplot as plt

np.random.seed(19680801)

mu = 100  # 分布的均值
sigma = 15  # 分布的标准差
x = mu + sigma * np.random.randn(437)

num_bins = 50

fig, ax = plt.subplots()

# the histogram of the data
n, bins, patches = ax.hist(x, num_bins, density=1)

# add a 'best fit' line
y = ((1 / (np.sqrt(2 * np.pi) * sigma)) *
     np.exp(-0.5 * (1 / sigma * (bins - mu))**2))
ax.plot(bins, y, '--')
ax.set_xlabel('Smarts')
ax.set_ylabel('Probability density')
ax.set_title(r'Histogram of IQ: $\mu=100$, $\sigma=15$')

# Tweak spacing to prevent clipping of ylabel
fig.tight_layout()
plt.show()
```
#### 绘制三维图型
```python
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
import numpy as np


fig = plt.figure()
ax = fig.gca(projection='3d')

# Make data.
X = np.arange(-5, 5, 0.25)
Y = np.arange(-5, 5, 0.25)
X, Y = np.meshgrid(X, Y)
R = np.sqrt(X**2 + Y**2)
Z = np.sin(R)

# Plot the surface.
surf = ax.plot_surface(X, Y, Z, cmap=cm.coolwarm,
                       linewidth=0, antialiased=False)

# Customize the z axis.
ax.set_zlim(-1.01, 1.01)
ax.zaxis.set_major_locator(LinearLocator(10))
ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))

# Add a color bar which maps values to colors.
fig.colorbar(surf, shrink=0.5, aspect=5)

plt.show()
```
#### 彩色映射散点图
这里我们会根据数据的大小给每个点赋予不同的颜色和大小，并在图中添加一个颜色栏。
```python
import matplotlib.pyplot as plt
import numpy as np

x = np.random.rand(1000)
y = np.random.rand(1000)
size = np.random.rand(1000) * 50
colour = np.random.rand(1000)
plt.scatter(x, y, size, colour)
plt.colorbar()
plt.show()
```
上面的代码大量的用到了 np.random.rand(1000)，原因是我们绘图的数据都是随机产生的。我们用到了 scatter() 函数，传入四个参数分别为x、y坐标和所绘点的大小和颜色。然后我们用 colorbar() 函数添加了一个颜色栏。

