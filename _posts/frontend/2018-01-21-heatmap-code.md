---
layout: post
title: "heatmap.js代码分析"
description: 本文是对heatmap.js代码的研读。涉及立即执行函数、模块化规范
date:   2018-01-21 09:00:00 +0800
categories: frontend
author: bwzou
---
JavaScript是一门很随意的语言，写法上没有很严谨的要求，这就造成一个问题，上手容易深入难。但我个人认为其实是“形散神不散”，看似杂乱无章，其实都是一些高阶的写法，只是我理解还不够深，雾里看花。研读优秀代码库对深入理解一门语言是相当重要的，本文就是讲讲研读heatmap.js 2.0后对js的深层次理解。

### 立即执行函数
函数声明：使用function关键字声明一个函数，再指定一个函数名。如下：

```js	
function fnName(){
	...
}
```
 
函数表达式：使用function关键字声明一个函数，但是没有指定函数名，然后将匿名函数赋予一个变量，叫做函数表达式。如下：

```js
var fnName = function(){
	...
}
```

匿名函数：使用function关键字声明一个函数，但是没有指定函数名，所以叫做匿名函数，匿名函数属于函数表达式。匿名函数有很多作用，赋予一个变量则创建函数，赋予一个事件则成为事件处理程序或者闭包。
```js
function(){
	...
}
```

函数声明和函数表达式的区别：一、JavaScript引擎在解析JavaScript代码时会存在“函数声明提升”,也就是说可以先调用函数，只要后面有该函数声明，而函数表达式必须等到JavaScript引擎执行到它所在行时，才会从上而下一行行解析函数表达式。二、函数表达式后面可以加括号立即调用该函数，函数声明却不能这么做，只能以fnName()形式调用。

立即执行函数有两种写法：(function (){...})()和(function (){...}())。在函数体后面加括号就能立即调用，这个函数必须是表达式，不能是函数声明。例子：

```js
(function(a){
  console.log(a);  //firebug输出123,使用（）运算符
})(123);
 
(function(a){
  console.log(a);  //firebug输出1234，使用（）运算符
}(1234));
 
!function(a){
  console.log(a);  //firebug输出12345,使用！运算符
}(12345);
 
+function(a){
  console.log(a);  //firebug输出123456,使用+运算符
}(123456);
 
-function(a){
  console.log(a);  //firebug输出1234567,使用-运算符
}(1234567);
 
var fn=function(a){
  console.log(a);  //firebug输出12345678，使用=运算符
}(12345678)
```

使用（）、！、+、-、=都可以立即调用函数，但是只有（）是最安全的，其他操作符都有可能对其他行为有影响！

### 打包方式
```js
(function (name, context, factory) {

  // Supports UMD. AMD, CommonJS/Node.js and browser context
  if (typeof module !== "undefined" && module.exports) {
    module.exports = factory();
  } else if (typeof define === "function" && define.amd) {
    define(factory);
  } else {
    context[name] = factory();
  }

})("h337", this, function () {
	...
});
```

这里说明一下：

>commonJS是服务器端模块的规范，Node.js采取了这个规范。根据CommonJs规范，一个单独的文件就是一个模块。加载模块使用require方法，该方法读取一个文件并执行，最后返回文件内部的exports对象。CommoonJs加载是同步的，所以只有加载完成才能执行后面的操作。像Node.js主要用于服务器编程，加载的模块文件一般都已经存在本地硬盘，加载起来比较快，不用考虑异步加载的方式，所以CommonJS规范比较适用。

>AMD是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义"。AMD设计出一个简洁的写模块API：define(id?, dependencies?, factory);第一个参数 id 为字符串类型，表示了模块标识，为可选参数。若不存在则模块标识应该默认定义为在加载器中被请求脚本的标识。如果存在，那么模块标识必须为顶层的或者一个绝对的标识。第二个参数，dependencies ，是一个当前模块依赖的，已被模块定义的模块标识的数组字面量。第三个参数，factory，是一个需要进行实例化的函数或者一个对象。

>UMD是AMD和CommonJS的糅合。AMD模块以浏览器第一的原则发展，异步加载模块。CommonJS模块以服务器第一原则发展，选择同步加载，它的模块无需包装(unwrapped modules)。这迫使人们又想出另一个更通用的模式UMD（Universal Module Definition）。希望解决跨平台的解决方案。UMD先判断是否支持Node.js的模块（exports）是否存在，存在则使用Node.js模块模式。在判断是否支持AMD（define是否存在），存在则使用AMD方式加载模块。






