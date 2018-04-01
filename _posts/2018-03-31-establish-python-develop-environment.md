---
layout: post
title: "python开发环境搭建"
description: python学习第一步，搭建适合自己的开发环境！
date:   2018-03-31 12:00:00 +0800
categories: code
author: bwzou
---
## Python起源
python 是著名的“龟叔”Guid van Rossum 在1989年的圣诞节期间，为了打发无聊无聊的圣诞节而编写的一个编程语言。<br>
python的特点是“优雅”、“明确”、“简单”，所以python程序看上去总是简单易懂，初学者学python，不但入门容易，而且将来深入下去，可以编写那些非常非常复杂的程序。<br>
python的缺点是：1、运行速度慢（相对于C,Java等）；2、代码不能加密（解释型语言）

根据[TIOBE](https://hellogithub.com/tiobe/?url=/category/Python%20%E9%A1%B9%E7%9B%AE/)（编程语言排行榜），python处于第四的位置
![]({{site.baseurl}}/images/20180331_TIOBE.png){: .shadow}

## Python安装
要开始学习python编程，首先就要在你的电脑里安装python。成功安装后，你会得到python解析器（负责运行python程序的），一个命令行交互环境，还有一个简单的集成开发环境。

目前python有两个版本，一个是2.x，一个是3.x，这两个版本是不兼容的（不能在python 3.x环境运行2.x代码，反之亦然）。由于3.x版本越来越普及，我们将会以最新的python3.6版本为基础进行学习。

#### 在Mac上安装Python
如果你使用的是mac，系统是OS X 10.8-10.10，那么系统自带的python版本是2.7。要安装最新python3.6，有两种方法：<br>
方法一：从Python官网下载最新的python 3.6.5的[安装程序](https://www.python.org/ftp/python/3.6.5/python-3.6.5-macosx10.6.pkg)，双击运行并且安装<br>
方法二：如果安装了Homebrew，直接通过命令brew install python3安装即可。

#### 在Linux上安装Python
如果你正在使用Linux，那么安装python对你而言肯定不是问题。

#### 在Windows上安装Python
首先根据Windows版本（64位还是32位）下载对应的python3.6.5 对应的64位安装程序或32位安装程序，然后运行exe安装包：

特別要注意勾上Add Python 3.6 to PATH，然后点击Customize installation(如果不想自定义可以点击Install Now即可完成安装)。
![]({{site.baseurl}}/images/20180331_py_install_setup.png){: .shadow}
直接保留默认，点击next
![]({{site.baseurl}}/images/20180331_py_install_setup2.png){: .shadow}
更改安装路径（也可以默认）
![]({{site.baseurl}}/images/20180331_py_install_setup3.png){: .shadow}
如果出现Disable path length limit，点击Disable path length limit
![]({{site.baseurl}}/images/20180331_py_install_setup4.png){: .shadow}
安装完成后，打开cmd，输入python出现上图表明安装成功！开始愉快的python之旅吧！
![]({{site.baseurl}}/images/20180331_py_install_setup5.png){: .shadow}
## 第一个Python程序
在开始编写第一个python程序前，我们先了解什么是命令行模式和python交互模式。

#### 命令行模式
在Windows开始菜单选择“命令提示符”，进入命令行模式，它的提示符类似c:\>

#### Python交互模式
在命令行模式下输入python，就看到类似如下文的一堆文本输出，然后就进入python交互模式，他的提示符是`>>>`。<br>
在python交互模式下输入exit()并回车，就推出来python交互模式，并回到命令行模式。

## Pycharm安装
经过以上安装步骤，你完全可以进行python编程。在Python的交互模式写程序，好处是一下子就能得到结果，坏处是没法保存，下次还想运行的时候，还得再敲一遍。所以，在实际开发的时候，我们需要一个文本编辑器来写代码，写完了保存为一个文件，这样程序就可以反复运行了。

这里推荐的是Jetbrains公司的Pycharm，一款十分优秀的商业IDE，有community版（免费）和professional版（收费）。我们到官网[下载pycharm](https://www.jetbrains.com/pycharm/download/#section=windows)。下载完进行安装，这里两种版本的安装方法都会介绍：
#### community版
1、直接运行exe文件，选择需要安装的位置，点击next
![]({{site.baseurl}}/images/20180331_pycharm_setup.png){: .shadow}
2、勾选箭头指示的复选框，（如果系统是32位，请勾选23 launcher)点击next
![]({{site.baseurl}}/images/20180331_pycharm_setup2.png){: .shadow}
3、直接默认，点击next
![]({{site.baseurl}}/images/20180331_pycharm_setup3.png){: .shadow}
4、此处问是否安装插件，全部点击install吧，但是可能需要等待一会，网络下载需要些时间
![]({{site.baseurl}}/images/20180331_pycharm_setup4.png){: .shadow}
5、点击工具栏help -> about看软件相关信息
![]({{site.baseurl}}/images/20180331_pycharm_setup5.png){: .shadow}

#### professional版
前面四步完全一样，但是启动软件之前会提示需要激活。这一步需要购买pycharm（先别购买），选择试用。
![]({{site.baseurl}}/images/20180331_pycharm_setup7.png){: .shadow}
而软件激活之后的界面是这样的：
![]({{site.baseurl}}/images/20180331_pycharm_setup6.png){: .shadow}

<style>.shadow{
    box-shadow: 2px 2px 5px #aaa;
    border-radius: 0;
    margin-top: 1em;
    margin-bottom: 1em;
}</style>