---
layout: post
title: "Scrapy(二)"
description: Scrapy自带了提取数据的机制。它们称为选择器，因为它们“选择”由XPath或CSS表达式指定的HTML文档的某些部分。
date: 2018-05-10 08:00:00 +0800
categories: python
author: bwzou
---
当我们抓取网页时，我们需要做的最常见的一步就是从HTML源中提取数据。本节我们就来讲scrapy自带的提取数据的机制，也称为选择器。因为它们是由XPath或CSS表达式指定的HTML文档的某些部分。

## XPath
#### XPath简介
XPath是一门在XML文档中查找信息的语言。XPath 用于在 XML 文档中通过元素和属性进行导航。

#### XPath语法
下面列出了最有用的路径表达式：

  表达式   |  说明
-----------|-----------------------------------------
 nodename  | 选取此节点的所有子节点
 /         | 从根节点选取
 //        | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。
 .         | 选取当前节点
 ..        | 选取当前节点的父节点
 @         | 选取属性
 
 这里我们列出了一些路径表达式即其结果：
 
 路径表达式       | 结果
 -----------------|----------------------------------------
 article          | 选取article元素的所有子节点。
 /article         | 选取根元素article。
 article/title    | 选取属于article的子元素的所有title元素
 //title          | 选取所有title子元素，而不管它们在文档中的位置
 article//title   | 选择属于article的后代的所有title元素，而不管它们位于article之下的什么位置
 //@lang          | 选取名为lang的所有属性
 
**谓语(Predicates)** 用来查找某个特定的节点或者包含某个指定的值得节点。谓语被嵌在方括号中。

路径表达式              | 结果
------------------------|-------------------------------------------
/article/title[1]       | 选取属于article子元素的第一个title元素
/article/title[last()]  | 选取属于article子元素的最后一个title元素
/article/title[last()-1]| 选取属于article子元素的倒数第二个title元素
/article/title[position()<3] | 选取属于article子元素的前两个title元素
//title[@lang]          | 选取所有拥有名为lang的属性的title元素
//title[@lang='eng']    | 选取所有拥有名为lang的属性且其值为eng的title元素
/article/title[author!='']   | 选取article元素的所有title元素，且title元素中的author值不为空
/article/title[author!='']/time| 选取article元素中title元素的所有time，且title元素中的author值不为空

XPath通配符可用来选取未知的XML元素。

通配符            | 描述
------------------|-------------------------------------------
*                 | 匹配任何元素节点
@*                | 匹配任何属性节点
node()            | 匹配任何类型的节点

我们可以看一些例子：

路径表达式        | 结果
------------------|---------------------------------------------
/article/*        | 选取article元素的所有子元素
//*               | 选取文档中的所有元素
//title[@*]       | 选取所有带有属性的title元素

## CSS选择器
我们列出一些常用的CSS选择器语法

表达式          | 说明
----------------|-----------------------------------------------------
#container      | 选取id为container的元素
.container      | 选取所有class属性值为container的元素
*               | 选取所有元素
div a           | 选取所有div下所有a元素
ul + p          | 选取ul后面的第一个p元素
ul ~ p          | 选取与ul相邻的所有p元素
div#container > ul | 选取id为container的div的第一个ul子元素
div.container > ul | 选取所有class属性值为container的div的第一个ul子元素
div:not(#container)|选取所有id为非container的div
a[title]        | 选取所有拥有title属性的a元素
a[href="url"]  | 选取所有href属性为url的a元素
a[href*="url"] | 选取所有href属性值中包含url的a元素
a[href^="http"] | 选取所有href属性值中以http开头的a元素
a:nth-child(2)   | 选取下面第二个标签，如果是a的话则选取，不是则不取
a:nth-child(2n)  | 选取第偶数个a元素
a:nth-child(2n+1)| 选取第奇数个a元素

