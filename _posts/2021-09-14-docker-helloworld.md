---
title: Docker实验一:Docker入门
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [Docker, 教程, 入门]
date: 2021-09-14 20:56:24
img:
coverImg:
password:
summary:

---

<!--more-->

# 1.实验内容

学习`Docker`的入门级命令。

# 2.实验要点

掌握`Docker`的入门级命令

# 3.实验环境

- Windows11

# 4.注意事项

如果`docker pull`由于网络原因无法使用，可使用`docker load`命令直接从桌面的`images`目录中加载相应的镜像。

例如对于命令：

```bash
docker pull ubuntu:latest
```

若实验环境无法访问互联网或访问dockerhub服务不稳定，可使用如下命令代替：

```bash
docker load < images/ubuntu_latest.tar.gz
```

上述命令假设了当前的工作目录为桌面（`/headless/Desktop`）。

如果当前工作目录不是桌面（可使用`pwd`命令查看），可使用如下命令代替：

```bash
docker load < /headless/Desktop/images/ubuntu_latest.tar.gz
```

# 5.实验步骤

### 5.1 HelloWorld

首先使用docker pull从镜像仓库Docker Hub下载hello-world镜像。

```bash
docker pull hello-world:latest
```

![image-20210914195230031](/assets/images/docker-helloworld/1631624951060.png)

使用 docker run在容器内运行应用程序，输出Hello world

![image-20210914195549162](/assets/images/docker-helloworld/1631624970360.png)

接下来，使用Ubuntu:16.04镜像生成容器，并输出hello world，首先下载ubuntu:16.04镜像

```bash
docker pull ubuntu:16.04
```

![image-20210914195907176](/assets/images/docker-helloworld/1631625004768.png)

接着，运行`docker run`命令使用Ubuntu容器输出hello world，命令如下：

```bash
docker run ubuntu:16.04 /bin/echo "hello world"
```

![image-20210914195946594](/assets/images/docker-helloworld/1631625017180.png)

### 5.2 运行交互式容器

通过docker的两个参数`-i -t`，让docker运行的容器实现"对话"的能力：

![image-20210914200042232](/assets/images/docker-helloworld/1631625029055.png)

各个参数解析：

- -t: 在新容器内指定一个伪终端或终端。
- -i: 允许你对容器内的标准输入 (STDIN) 进行交互。

注意上图中的root@0529c96c034f:/#，此时我们已进入一个 ubuntu 16.04 系统的容器

在容器中使用命令 `cat /proc/version `查看当前系统的版本信息

```bash
cat /proc/version
```

![image-20210914200223477](/assets/images/docker-helloworld/1631625041216.png)

在容器中使用命令`ls`查看当前目录下的文件列表

![image-20210914200243864](/assets/images/docker-helloworld/1631625054254.png)

通过`exit`命令或者使用`CTRL+D`退出容器。

![image-20210914200300831](/assets/images/docker-helloworld/1631625072220.png)

注意第三行中 PS C:\Users\JOJO> 表明已退出当前的容器，返回到当前的主机中。

### 5.3 启动容器（后台模式）

使用以下命令创建一个以进程方式运行的容器

```bash
docker run -d ubuntu:16.04 /bin/bash -c "while true;do echo hello world; sleep 1; done"
```

![image-20210914200402686](/assets/images/docker-helloworld/1631625084626.png)

在输出中，没有看到期望的 "hello world"，而是一串长字符：

```bash
470cf4dc561cacc67f00a6f5d35d202858b90cb347bda516a875169c2580e9ff
```

这个长字符串叫做容器ID，对每个容器来说都是唯一的。可以通过容器ID操作、查看对应的容器。
首先，我们需要确认容器有在运行，可以通过`docker ps`来查看：

![image-20210914200450281](/assets/images/docker-helloworld/1631625097622.png)

输出详情介绍：

- CONTAINER ID：容器 ID。
- IMAGE：使用的镜像。
- COMMAND：启动容器时运行的命令。
- CREATED：容器的创建时间。
- STATUS：容器状态，共包含7种状态。
  - created：已创建
  - restarting：重启中
  - running：运行中
  - removing：迁移中
  - paused：暂停
  - exited：停止
  - dead：死亡
- PORTS：容器的端口信息和使用的连接类型（tcp\udp）。
- NAMES：自动分配的容器名称。

在宿主主机内使用`docker logs`命令，查看容器内的标准输出：

```bash
docker logs 470cf4dc561c
```

![image-20210914200646285](/assets/images/docker-helloworld/1631625111042.png)

使用容器名称查看日志：

```bash
docker logs eager_cannon
```

![image-20210914200759983](/assets/images/docker-helloworld/1631625123585.png)

### 5.4 停止容器

使用`docker stop`命令来停止容器:

```bash
docker stop 470cf
```

![image-20210914200950223](/assets/images/docker-helloworld/1631625141666.png)

通过`docker ps`查看，容器已经停止工作。 注意，在上述命令中，容器ID只使用了`470cf`，而没有写上完整的“470cf4dc561c”，这是由于docker会自动按照前缀搜索，如果某个容器ID可以和这个前缀匹配，则会定位到该容器。因此可以使用`470cf`这种简写的方式，当然，如果确定只有一个容器以`4`开头，也可以直接用命令`docker stop 4`。

![image-20210914201319884](/assets/images/docker-helloworld/1631625151551.png)

同样地，在停止容器时，也可以使用容器名称来操作:

![image-20210914201540600](/assets/images/docker-helloworld/1631625159089.png)

<font color=#FFFFFF>实验报告来自：囧囧JOJO</font>
