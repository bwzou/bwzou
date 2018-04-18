---
layout: post
title: "Anaconda安装和Pycharm配置"
description: Anaconda是一个把管理器，里面安装了很多用于科学计算的包
date: 2018-04-18 00:00:00 +0800
categories: python
author: bwzou
---
接下来我们进入重要工具包的学习，在此之前需要安装一个软件Anaconda。直接[官网](https://www.anaconda.com/download/)下载合适的版本,这里我们下载python3.6对应的版本。

## 安装Anaconda
双击exe文件，进入到如下界面：<br>
![]({{site.baseurl}}/images/20180411_anaconda_setup.png){: .shadow}
直接next，进入下一步：<br>
![]({{site.baseurl}}/images/20180411_anaconda_setup2.png){: .shadow}
同意，进入下一步：<br>
![]({{site.baseurl}}/images/20180411_anaconda_setup3.png){: .shadow}
这是问是否为所有用户安装，我们选择为所有用户安装吧。<br>
![]({{site.baseurl}}/images/20180411_anaconda_setup4.png){: .shadow}
默认，开始安装。安装完毕之后如果是Windows用户会有下面提示：<br>
![]({{site.baseurl}}/images/20180411_anaconda_setup5.png){: .shadow}
这个VS如果你觉得有需要的话就安装，我因为电脑里面已经有Pycharm了所有你不需要就没有装了。好的，到此我们已经把Anaconda装好了，此时查看你的软件你会发现安装上了anaconda prompt和anaconda navigator。

我们来运行anaconda navigator，得到如下界面：
![]({{site.baseurl}}/images/20180411_anaconda_started.png){: .shadow}
我们可以看到applications栏里面装了很多软件，我们之后用到的Jupyter 和 Spyder都已经装上了。

## Pycharm配置
如果我们想在Pycharm上用Anaconda怎么办呢。毕竟用来那么久的Pycharm还是不舍得放弃的。嗯那我们就根据[官网](https://docs.anaconda.com/anaconda/user-guide/tasks/integration/pycharm.html)来配置吧。

第一步，进入设置项：<br>
![]({{site.baseurl}}/images/20180411_pycharm_setting.png){: .shadow}
第二步，找到project interpreter，新增一个project interpreter:<br>
![]({{site.baseurl}}/images/20180411_pycharm_setting2.png){: .shadow}
输入anaconda安装路径下的python：<br>
![]({{site.baseurl}}/images/20180411_pycharm_setting3.png){: .shadow}
第四步，应用：<br>
![]({{site.baseurl}}/images/20180411_pycharm_setting4.png){: .shadow}



<style>.shadow{
    box-shadow: 2px 2px 5px #aaa;
    border-radius: 0;
    margin-top: 1em;
    margin-bottom: 1em;
}</style>

