---
layout: post
title: "移动端页面开发常见问题"
description: 移动端网页开发不同于PC端网页开发，不同设备显示略有区别，这就留下了很多的坑！
date:   2017-09-17 15:50:00 +0800
categories: frontend
author: bwzou
---

最近由于移动端项目忙不过来，需要人手，我就转而接触移动端网页开发。移动端项网页开发会遇到一些常见问题，这里做一个记录。

#### 1、部分手机不支持flex布局
直接使用display：flex在部分手机上显示有问题。需要写上所有的。

    display: -webkit-box; /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display: -moz-box; /* 老版本语法: Firefox (buggy) */
    display: -ms-flexbox; /* 混合版本语法: IE 10 */
    display: -webkit-flex; /* 新版本语法: Chrome 21+ */
    display: flex; /* 新版本语法: Opera 12.1, Firefox 22+ */

#### 2、全屏遮罩问题
部分手机出现这个问题，点击时出现一个半透明的遮罩，原以为是兼容性性问题，在网上找到一个普遍的解决方法：
```css
/*
ios用户点击一个链接，会出现一个半透明灰色遮罩, 如果想要禁用，
可设置-webkit-tap-highlight-color的alpha值为0去除灰色半透明遮罩；

android用户点击一个链接，会出现一个边框或者半透明灰色遮罩, 
不同生产商定义出来额效果不一样，可设置-webkit-tap-highlight-color
的alpha值为0去除部分机器自带的效果；

winphone系统,点击标签产生的灰色半透明背景，能通过设置
<meta name="msapplication-tap-highlight" content="no">去掉；

特殊说明：有些机型去除不了，如小米2。对于按钮类还有个办法，
不使用a或者input标签，直接用div标签
*/
a,button,input,textarea { 
	-webkit-tap-highlight-color: rgba(0,0,0,0); 
	-webkit-user-modify:read-write-plaintext-only; 
	/*-webkit-user-modify有个副作用，就是输入法不再能够输入多个字符*/
}   
/* 也可以 */
* { -webkit-tap-highlight-color: rgba(0,0,0,0); }
/* winphone下 */
<meta name="msapplication-tap-highlight" content="no">
```

而我使用荣耀6plus进行测试，发现这样改并没有作用。后来发现问题出处：

{% highlight css %}
/*
 * Remove text-shadow in selection highlight:
 * https://twitter.com/miketaylr/status/12228805301
 *
 * These selection rule sets have to be separate.
 * Customize the background color to match your design.
 */
::selection {
    background: #b3d4fc;   /*问题就出在这里,使用了::selection 选择器*/
    text-shadow: none;
}
{% endhighlight %}

#### 3、修改默认框样式问题
一、使用appearance改变webkit浏览器的默认外观

	input,select { -webkit-appearance:none; appearance: none; }

#### 4、缓存设置
```html
<!-- 清除缓存 -->
<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0">   
```
#### 5、ios上input、textarea等无法输入

{% highlight css %}
*{
    -webkit-user-select: none;
    -ms-touch-select: none;
    -ms-touch-action: none;
}
{% endhighlight %}

将以上代码注释掉或者删除掉