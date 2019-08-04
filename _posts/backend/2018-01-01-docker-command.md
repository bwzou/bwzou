---
layout: post
title: "常用的docker命令"
description: docker是一个很有用的软件，提供一次性的环境。
date: 2018-06-12 00:00:00 +0800
categories: backend
author: bwzou
---
## 常用Docker命令

#### 启动docker服务
```
service docker start
```
或者
```
systemctl start docker
```

#### 关闭docker服务
```
service docker stop
```
或者
```
systemctl stop docker
```

#### 运行docker镜像
```
docker run -t -i ubuntu /bin/bash
```
官网是这么说的：

- docker run: runs a container.
- ubuntu: is the image you would like to run.
- -t: flag assigns a pseudo-tty or terminal inside the new container.
- -i: flag allows you to make an interactive connection by grabbing the standard in (STDIN) of the container.
- /bin/bash: launches a Bash shell inside our container.

#### 启动docker镜像
该容器存在，想再次打开这个container，运行：
```
docker start container_name
```

#### 列举运行中的容器
```
docker ps
```

#### 列举所有的容器
```
docker ps -a
```

#### 进入容器
```
docker attach container_name
```
使用“docker attach”命令进入container有一个缺点，那就是每次从container中退出到前台时，container也跟着退出了。

```
docker exec -it container_name /bin/bash
```

#### 退出容器
```
exit
```

## 常用Docker-Compose命令
#### 启动docker
```
docker-compose up -d
```
-d 后台启动

