---
layout: post
title: "调试移动端页面方法"
description: 做前端开发时，特别是做移动端开发，要想快速找到问题，就必须掌握一些开发调试的方法！
date:   2017-09-14 7:50:00 +0530
categories: design
author: bwzou
---

接触前端多一些发现，在开发移动端页面的时候，常常会出现下面问题：

- 我在PC端测试没有问题，在手机上打开就不行，或者是有一些手机可以有一些不行，但是又看不到报什么错；
- 上线后，无法重现用户出现的页面问题，这是十分令人懊恼的事情。

以上情况对于开发者而言都需要看到log才能快速定位问题，好在两种情况都有自己的处理办理，针对第一种可以使用USB连接到电脑解决，第二种可以使用开源库vConsole。
## 使用Chrome Developer Tools进行调试
下载Android Chrome，在PC端进行调试。要求：

- 开发计算机上已安装 Chrome 32 以上版本。
- 开发计算机上已安装 USB 驱动程序（如果使用Windows）。 
- 拥有一根可以将 Android 设备连接至开发计算机的 USB 线。
- Android 4.0 或更高版本。
- Android 设备上已安装 Chrome（Android 版）。

#### 连接 Android 设备
1. 在 Android 设备上，选择 Settings > Developer Options > Enable USB Debugging。 在运行 Android 4.2 及更新版本的设备上，Developer options 默认情况下处于隐藏状态。不同设备打开方式不一样，具体请查询设备生产厂商官网。
2. 在开发计算机上打开 Chrome。需要用一个 Google 帐户登录到 Chrome。 远程调试在隐身模式或访客模式下无法运行。
3. 打开 DevTools
4. 在 DevTools 中，点击 Main Menu 主菜单，然后选择 More tools > Remote devices。并确保已启用 Discover USB devices。
![]({{site.baseurl}}/images/DevTools Inspect1.png){: .shadow}
5. 使用一根 USB 线将 Android 设备直接连接到开发计算机。
6. 看到 devices 显示连接到正确的 Android 设备之后，在 Android Chrome 上打开网页，这时可以在 DevTools 上看到 Android Chrome 打开的标签。
![]({{site.baseurl}}/images/DevTools Inspect2.png){: .shadow}
7. 点击要调试页面右边的Inspect按钮即进入调试。
![]({{site.baseurl}}/images/DevTools Inspect3.png){: .shadow}

## 直接使用vConsole库，在手机端进行显示
如果觉得前面介绍的那种方式比较麻烦，那么可以用这种比较简单的方式。vConsole:一个轻量、可拓展、针对手机网页的前端开发者调试面板。

#### 特性：
- 查看 console 日志
- 查看网络请求
- 手动执行 JS 命令行
- 自定义插件
#### 安装：
- 下载最新版本的[vConsole]({{https://github.com/WechatFE/vConsole/tree/dev/dist}})。
- 引入 dist/vconsole.min.js 到项目中：

一个好的调试工具真的太重要了，以至于我刚用完之后就想把它们记录下来，写到我的博客去。如果你们还有其他方式，一起分享咯。

<style>.shadow{
    box-shadow: 2px 2px 5px #aaa;
    border-radius: 0;
    margin-bottom: 3em;
}</style>