---
layout: post
title: "使用docker部署spring-boot项目"
description: 使用docker可以极大的方便部署项目！
date:   2017-12-04 14:15:00 +0800
categories: docker
author: bwzou
---
最近在开发后台管理项目，使用的spring-boot，接口写得差不多了，听测试说使用docker会方便部署。从前端刚转过来的我一脸懵逼，但是做开发就是这样，怎么简单怎么来，能偷懒就要偷懒。花了一下午和晚上终于搞定了。

### 安装步骤
我是使用公司的内部服务器部署测试的，系统Ubuntu的Xenial 16.04 (LTS)，去docker[官网](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/)找了一下，里面有详细的安装教程。大概步骤如下：

```
SET UP THE REPOSITORY
1、 sudo apt-get update

2、 $ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

3、 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

4、 sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22

5、 sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

INSTALL DOCKER CE
1、 sudo apt-get update

2、 sudo apt-get install docker-ce

3、 sudo docker run hello-world
```

安装完之后，运行docker run hello world，如果输出成功表明安装成功。

然后是构建jar包build-》assemble，使用xftp将jar包上传至内部服务器。然后运行容器：
```
docker run --name first_project \
-p 8080:8080 \
--log-driver json-file --log-opt max-size=100m \
-e "TZ=Asia/Shanghai" \
-v /root/bwzou/webapp:/webapp \
-v /root/bwzou/workspace:/workspace \
-v /root/bwzou/data:/data
-d openjdk:8-jre java -jar -Dspring.profiles.active=test /webapp/hello-kitty.jar
```

这里使用了现成的openjdk:8-jre镜像，就不用自己写dockerfile构建了，可以省事。-v是服务器与docker之间的映射。

介绍一些简单的常用docker命令:
```
1、查看现在运行容器
docker ps

2、查看所有容器
docker ps -a

3、启动停止容器
docker start|stop|restart [id]

4、停止全部运行的容器
docker stop `docker ps -q`

5、删除一个容器
docker rm [容器id]

6、docker rm `docker ps -a -q`

```