---
layout: post
title: "前端工程化"
description: 前端拥有着及其庞大的生态，在此先做一个记录吧！
date:   2018-03-21 23:00:00 +0800
categories: code
author: bwzou
---
### 前言
前端工程化必须要了解的东西有：

- （1）JavaScript背后的规范，学习ES2015/2016
- （2）学习Node.js基础。
- （3）学会使用和配置babel。
- （4）学习webpack，掌握搭配使用babel。

目前分一步一步来学习。前端的生态系统太庞大了，新的规范和特性、新的框架、各种技术方案层出不穷。不具备很强的学习能力根本hold不住的。

### JavaScript规范。
2015年6月，ECMAScript 6正式发布，并且更名为“ECMAScript 2015”，简称ES6。这是因为TC39委员会计划，以后每年发布一个ECMAScirpt的版本，下一个版本在2016年发布，为“ECMAScript 2016”。参考[JavaScript语言的历史](https://javascript.ruanyifeng.com/introduction/history.html)

ES2015/2016的学习是一个累积的过程，不断需要通过编程去提高，之后会在github上给一些自己学习的DEMO。

目前[ES2015/2016新特性](http://www.css88.com/archives/6963)：

 - let & const
 - destructuring 解构赋值
 - Proxies
 - Promises 对象
 - Symbols
 - arrows
 - classes
 - enhanced object literals
 - template strings
 - default & rest & spread
 - iterators & for..of
 - generator 构造器
 - unicode
 - modules
 - module loaders
 - map & set & weakmap + weakset
 - subclassable built-ins
 - math & number & string & array & objectAPIs
 - binary & octal Literals
 - reflect api
 - tail calls

### Node.js学习
[null]

### Babel使用及配置
Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。也就是说你现在可以使用ES6语法编写代码而不需要担心执行环境是否支持。

在升级到了Babel6.x之后，所有的插件都是可插拔的。这也意味着你安装了Babel之后，是不能工作的，需要配置对应的.babelrc文件才能发挥完整的作用。下面就参考[Git上的中文翻译](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/user-handbook.md#toc-babel-cli)。对.babelrc做一个简单解析。

.babelrc文件的基本格式如下：
```json
{
  "presets": [],
  "plugins": []
}
```
*尽管也可以用其他方式给 Babel 传递选项，但 .babelrc 文件是约定也是最好的方式。*

##### 预设(presets)

presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装。
```
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015

# react转码规则
$ npm install --save-dev babel-preset-react

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```

然后将这些规则安装需要加入.babelrc文件中
```json
{
   "presets": [
      "es2015",
      "react",
      "stage-2"
   ],
   "plugins": []
}
```

**env**
```json
{
	"presets": ["env", options]
}
```
env是最近新增的一个选项，有以下options选择。

targets:{[string]:number}，默认{}
需要支持的环境，可选例如：chrome, edge, firefox, safari, ie, ios, node，甚至可以制定版本，如node: 6.5。也使用node: current代表使用当前的版本。

browsers: Array | string，默认[]
浏览器列表，使用的是browserslist，可选例如：last 2 versions, > 5%。

loose: boolean，默认false
是否使用宽松模式，如果设置为true，plugins里的插件如果允许，都会采用宽松模式。

debug: boolean，默认false
编译是否会去掉console.log。

whitelist: Array，默认[]
设置一直引入的plugins列表。
##### 插件(plugins) 
其实presets，也就是一堆plugins的预设，起到方便的作用。如果你不采用presets，完全可以单独引入某个功能，比如以下的设置就会引入编译箭头函数的功能。
```json
{
  "plugins": ["transform-es2015-arrow-functions"]
}
```
那么，还有一些方法是presets中不提供的，这时候就需要单独引入了，下面介绍几个常见的插件。

1、transform-runtime
```json
{
  "plugins": ["transform-runtime", options]
}
```
特点：解决编译中产生的重复的工具函数，减小代码体积；非实例方法的poly-fill，如Object.assign，但是实例方法不支持，如"foobar".includes("foo")，这时候需要单独引入babel-polyfill

2、transform-remove-console
```json
{
	"plugins":["transform-remove-console"]
}
```
使用这个插件，编译后的代码都会移除console.*。

### Webpack学习

##### 为什么要使用Webpack

我的这个只要理解webpack的核心就可以——[一切皆模块]。webpack就是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其他浏览器不能直接运行的扩展语言（Scss,TypeScript等）,并将其转换和打包为浏览器可理解的格式。

##### 通过配置文件来使用webpack

webpack拥有很多比较高级的功能（比如说本文后面会介绍的loaders 和 plugins），这些功能都可以通过命令行模式实现，但是这样不方便且容易出错，更好的办法是定义一个配置文件webpack.config.js，这个配置文件其实也是一个简单的JavaScript模块，将配置文件其置于跟目录下。

我们可以把所有与打包相关的信息放在里面。
```js
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
```
*注：“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。*
 
有了这个配置之后，再打包文件，只需在终端里运行webpack(非全局安装需使用node_modules\.bin\webpack)命令就可以了，这条命令会自动引用webpack.config.js文件中的配置选项。

参考[官网](https://webpack.js.org/guides/getting-started/)

