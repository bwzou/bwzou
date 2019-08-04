---
layout: post
title: "需要知道的CSS知识（三）"
description: 前端开发中css是极其重要的一部分，需要有一个更全面的学习！
date:   2017-10-18 20:13:00 +0800
categories: frontend
author: bwzou
---
续上一篇，接下来记录的是字体和文本。

#### 第4章 字体和文本
网页设计中很多时候都要处理文本，包括段落、标题、列表、菜单和表单中的文本。因此，理解本章要讲的字体和文本属性，对于创造出专业水准的网页排版非常重要。

难道字体和文本不是一回事？

字体是“文字的不同体式”或者“字的形体结构”。对于英文而言，每种字体都是由一组具有独特样式的字母、数字和符号组成的。根据外观，字体可以分为不同的类别（font collection），包括衬线字体（serif）、无衬线字体（sans-serif）和等宽字体（monospace）。每一类字体可以分成不同的字体族（font family），比如Times和Helvetica。而字体族中又可以包含不同的字型（font face），反映了相应字体族基本设计的不同变化，例如Times Roman、Times Bold、Helvetica Condensed和Bodoni italic。

文本就是一组字或字符，比如章标题、段落正文等等，跟使用什么字体无关。

**字体**

网页中的字体有三个来源。

- 用户机器中安装的字体。（直到最近，这些字体还是能在网页中放心使用的唯一一批字体。）
- 保存在第三方网站上的字体。最常见的是Typekit和Google，可以使用link标签把它们链接到你的页面上。
- 保存在你的Web服务器上的字体。这些字体可以使用@font-face规则随网页一起发送给浏览器。

与字体样式相关的6个属性:font-family、font-size、font-style、font-weight、font-variant和font（简写属性）。

如果你想缩进段落，实现106中的上角标效果，在标题字符间增加一些间距，以及其他一些排版效果，就要用到CSS的文本属性。

与文本样式相关的6个属性:text-indent、letter-spacing、word-spacing、text-decoration、text-align、line-height、text-transform、vertical-align。




