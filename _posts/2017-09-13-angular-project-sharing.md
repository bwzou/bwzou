---
layout: post
title: "angular项目分享"
description: 基于angular1.x的一个项目的分享，主要涉及整个项目结构、模块、功能等！
date:   2017-09-13 9:50:00 +0530
categories: code
author: bwzou
---

### 背景
正式入职已经有两个多月，在这两个月内基于Angular1.x完成了项目的1.0版本。今天刚好有时间，可以对项目好好梳理总结一番，一来是可以学到东西，二来也可以为项目的2.0版本开发拓展新思路。

### 项目结构
首先展示整个项目结构：
```js
├─assets        # 资源目录，存放静态资源
│  └─img
│      ├─bg
│      └─logo
├─css           # 自定义样式文件      
├─fonts         # 资源文件，字体
│  └─sourcesanspro
├─framework     # 所有angular相关文件
│  ├─directives       # 自定义directive
│  ├─libs             # 项目引入第三方库
│  │  ├─angular
│  │  ├─angular-animate
│  │  ├─angular-bootstrap
│  │  ├─angular-cookies
│  │  ├─angular-resource
│  │  ├─angular-sanitize
│  │  ├─angular-touch
│  │  ├─angular-translate
│  │  ├─angular-translate-loader-static-files
│  │  ├─angular-translate-storage-cookie
│  │  ├─angular-translate-storage-local
│  │  ├─angular-ui-router
│  │  │  ├─0.2.15
│  │  │  └─release
│  │  ├─angular-ui-utils
│  │  ├─angularjs-toaster   # 弹出提示框
│  │  ├─animate.css
│  │  ├─bootstrap      # bootstrap前端框架
│  │  │  └─dist
│  │  │      ├─css
│  │  │      ├─fonts
│  │  │      └─js
│  │  ├─bootstrap-filestyle
│  │  │  └─src
│  │  ├─font-awesome
│  │  │  ├─css
│  │  │  └─fonts
│  │  ├─footable
│  │  ├─jquery
│  │  │  └─dist
│  │  ├─nestable
│  │  ├─ngstorage
│  │  ├─node_modules
│  │  │  └─highcharts
│  │  │      └─modules
│  │  ├─oclazyload       # 懒加载
│  │  │  └─dist
│  │  ├─qrcode           # 生成二维码 
│  │  ├─select2          # select控件
│  │  │  └─css
│  │  └─simple-line-icons
│  │      ├─css
│  │      └─fonts
│  ├─services     # 自定义angular service服务
│  └─tpl
├─l10n       # 字体语言，如en.js、cn.js等
└─modules    # 自己开发的功能模块
    ├─。。。。
	├─。。。。
```

详细来说，