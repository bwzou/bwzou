---
layout: post
title: "如何搭建个人博客"
description: 根据本人搭建博客的经历，整理一篇如何搭建个人博客的文章，内容涉及选取框架、搭建过程、发布等！
date:   2017-09-09 10:50:00 +0800
categories: design
author: bwzou
---

## 前言
自己一直都想搭建一个自己的博客，出来工作以后觉得这件事更加有必要去实行了，因为根据之前的经验做好记录对提高工作效率太有帮助了。零碎的整理对于量少的记录还行，但是记录的量一旦多了就很不方便查找，所以还是要进行系统的记录。正巧刚刚看完《web全栈工程师的自我修养》（作者是腾讯的一位UI工程师），了解到使用Github Pages搭建博客是一个不错的选择。

### 选取框架
本人选取的是Git+GitHub Pages+Jekyll+MarkDown。

### 搭建步骤
1、安装ruby

在Windows下，从[官网]({{http://rubyinstaller.org/downloads/}}){: target="_blank"}下载对应操作系统的rubyinstaller.exe 和 Devkit，两个要对应版本。<br>
执行rubyinstaller.exe安装ruby<br>
解压缩Devkit到一个指定文件夹<br>
管理员权限打开cmd，输入：

	ruby dk.rb init
    ruby dk.rb install

从[官网]({{https://rubygems.org/pages/download/}}){: target="_blank"}下载RubyGems,解压，管理员权限打开cmd，输入：

	cd rubygems的解压目录
	ruby setup.rb

2、配置ruby

由于国内一些众所周知的原因，可能需要修改ruby的源为国内的镜像：淘宝镜像[(https://ruby.taobao.org/)]({{https://ruby.taobao.org/}})或者ruby中国镜像[(https://gems.ruby-china.org/)]({{https://gems.ruby-china.org/}})。由于本人有一些科学上网的渠道，所以就没有做配置，如果有需要，请自行Google。

安装bundler，bundler是一款ruby工具，可以通过配置Gemfile文件来快速安装gems包或者解决已安装包的依赖。

	gem install jekyll

3、安装Jekyll

	gem install jekyll

常用jekyll命令
	
	jekyll new <dir>               # 初始化一个站点目录
	jekyll build -s <dir> -d <dir> # 生成编译后的静态站点文件
	jekyll serve                   # 用于本地预览和调试，地址是127.0.0.1:4000

4、建立站点仓库
我是直接使用别人[模板]({{https://github.com/sharu725/ashwath}}){: target="_blank"}的，也可以参考[官方提供模板]({{}}){: target="_blank"}

	git clone git@github.com:someone/jekyll-theme.git # 克隆一个的模板
	
删除git记录
初始化本地站点仓库，并建立与远程仓库的链接

	git init
	git remote add origin git@github.com:userName/blog.git # 与你的github blog仓库建立联系

5、修改_config.yml
```
# Site settings
title: you name
description: > # this means to ignore newlines until "baseurl:"
 这是本人的博客，记录自己的点点滴滴！
baseurl: "/log" # the subpath of your site, e.g. /blog
url: "http://xxxx.com"  #你自己的域名
permalink: :title/
full-url: "http://xxxx.com/" # the base hostname & protocol for your site

# CSS
sass:
  style: compressed

# Comments - Go to Disqus.com > sign up > get the short-name > insert it here
disqus-shortname: 

# Site version
version: 0.9

# socket 本地调试端口号
port: 4001

# Build settings
markdown: kramdown

include: [_pages] # Includes files inside _pages directory [Do not remove this]

author:
  name: younane
  image: younane.jpg # The author image will be shown on the header. Place this image in /images/ folder.
  about: younane是一个帅帅的前端工程师！！
  twitter: younane  # https://twitter.com/ is automatically added
  facebook: younane # https://facebook.com/ is automatically added
  email: younane@gmail.com # mailto: is automatically added
  
gems: 
  - jekyll-seo-tag
  - jekyll-paginate
```

6、撰写博文

jekyll采用kramdown来解析markdown,与常见的markdown略有区别，详情请参看[kramdown官方语法]({{https://kramdown.gettalong.org/syntax.html}}){: target="_blank"}，当然你也可以在_config.yml文件中设置其他解析器。

博文源文件一般存放在_post文件夹下，命名格式为：2017-09-10-如何搭建个人博客.md

博文结构主要是指头部，头部结构如下：
```
layout: post  # 默认是default，跟_layouts目录下的default.html文件对应，可以通过_config.yml的defaults字段配置默认设置
author: 作者名  # 可以_config.yml默认配置
title: 博文标题 
date: yyyy-mm-dd  #也可写成yyyy-mm-dd hh:mm:ss +0530
categories:category1 category1  #可以写多个
tags:tag1 tag2 #可以写多个
comments: true # 开启第三方评论模块，可以_config.yml默认配置
share: true # 开启第三方分享模块，可以_config.yml默认配置
```

7、做好版本控制

	git add --all
	git commit -m 'some revisions'
	git push origin source

8、本地预览

	jekyll serve

这里如果端口号报错，请在_config.yml中配置新的端口，如：port: 4001。成功后输入127.0.0.1:4001即可预览

9、设置GitHub Pages静态展示

目前GitHub支持两个方式的展示，一种是直接使用默认gh-pages分支，即将项目上传到gh-pages分支而不是主分支；另一种是将项目上传到主分支，但是需要做配置:在setting中GitHub Pages选择如下![]({{site.baseurl}}/images/master_show.png){: .shadow}

以上两种方法都可以通过http://userName.github.io/repoName访问

10、以后每次发布新文章，需要重复6-8。推荐windows markdown编辑器MarkdownPad2。

11、补充：添加disqus评论功能

- 创建一个disqus账号，新建一个admin：
Disqus admin->Your Sites->+new，完成表单，提交
- 在站点页面，插入disqus代码：
Universal Code install instructions


<style>.shadow{
    box-shadow: 2px 2px 5px #aaa;
    border-radius: 0;
    margin-bottom: 3em;
}</style>