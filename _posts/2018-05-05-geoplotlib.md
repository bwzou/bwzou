---
layout: post
title: "Geoplotlib"
description: Geoplotlib是一个用于制作地图和地理相关数据的工具箱。
date: 2018-05-01 08:00:00 +0800
categories: python
author: bwzou
---
Geoplotlib可以用来制作多种地图，比如等值区域图， 热度图，点密度图。

必须安装 Pyglet （一个面向对象编程接口）和 Numpy 才能使用geoplotlib。 

## 安装
在anaconda prompt命令行下输入：
```
pip install geoplotlib
```
这时我们还需要安装Pyglet，所以也输入：
```
pip install pyglet
```
如果是开发环境是Mac的话，请安装Pyglet1.24以上。Numpy 前面我们已经安装了，这里就不需要再进行安装了。

## 使用
我们先介绍一些常用简单图形的绘制：

#### 点密度图
bus.csv数据：name,lat,lon，例：Rådhuspassagen,55.7439334696163,12.4939206032287
```python
import geoplotlib
from geoplotlib.utils import read_csv


data = read_csv('data/bus.csv')
geoplotlib.dot(data)
geoplotlib.show()
```
#### 直方图
opencellid_dk.csv数据：lon,lat，例：11.3536345358484,55.396806865594705
```python
import geoplotlib
from geoplotlib.utils import read_csv, BoundingBox


data = read_csv('data/opencellid_dk.csv')
geoplotlib.hist(data, colorscale='sqrt', binsize=8)
geoplotlib.set_bbox(BoundingBox.DK)
geoplotlib.show()
```
#### 核密度估计
opencellid_dk.csv数据：lon,lat，例：11.3536345358484,55.396806865594705
```python
import geoplotlib
from geoplotlib.utils import read_csv, BoundingBox

data = read_csv('data/opencellid_dk.csv')
geoplotlib.kde(data, bw=5, cut_below=1e-4)

geoplotlib.set_bbox(BoundingBox.KBH)
geoplotlib.show()
```
#### K-means
这是一个自定义图层，要继承BaseLayer这个基类，然后再重写其中的部分方法，比如，我们这次要重写invalidate(self, proj)这个方法，这个方法的作用，就是在每次geoplatlib相机高度改变，需要重绘的时候调用的。
```python
from geoplotlib.colors import create_set_cmap
import pyglet
from sklearn.cluster import KMeans
import geoplotlib
from geoplotlib.layers import BaseLayer
from geoplotlib.core import BatchPainter
from geoplotlib.utils import BoundingBox
import numpy as np


class KMeansLayer(BaseLayer):

    def __init__(self, data):
        self.data = data
        self.k = 4


    def invalidate(self, proj):
        self.painter = BatchPainter()
        x, y = proj.lonlat_to_screen(self.data['lon'], self.data['lat'])

        k_means = KMeans(n_clusters=self.k)
        k_means.fit(np.vstack([x,y]).T)
        labels = k_means.labels_

        self.cmap = create_set_cmap(set(labels), 'hsv')
        for l in set(labels):
            self.painter.set_color(self.cmap[l])
            self.painter.convexhull(x[labels == l], y[labels == l])
            self.painter.points(x[labels == l], y[labels == l], 2)
    
            
    def draw(self, proj, mouse_x, mouse_y, ui_manager):
        ui_manager.info('Use left and right to increase/decrease the number of clusters. k = %d' % self.k)
        self.painter.batch_draw()


    def on_key_release(self, key, modifiers):
        if key == pyglet.window.key.LEFT:
            self.k = max(2,self.k - 1)
            return True
        elif key == pyglet.window.key.RIGHT:
            self.k = self.k + 1
            return True
        return False
  

data = geoplotlib.utils.read_csv('data/bus.csv')
geoplotlib.add_layer(KMeansLayer(data))
geoplotlib.set_smoothing(True)
geoplotlib.set_bbox(geoplotlib.utils.BoundingBox.DK)
geoplotlib.show()
```

