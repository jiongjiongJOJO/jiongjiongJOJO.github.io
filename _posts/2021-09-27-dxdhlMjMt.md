---
title: WSL2环境下安装Docker保姆级教程
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [Docker, 教程, 入门, 笔记]
date: 2021-09-27 00:32:12 +0800
img:
coverImg:
password:
summary:

---

## 1 环境检测

要在WSL2环境下安装Docker，需要先安装WSL2，而WSL2有系统环境的要求，需要监测自己的系统版本是否符合环境要求。

根据[官方的要求](https://docs.microsoft.com/zh-cn/windows/wsl/install#prerequisites)：`You must be running Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11.`，您的系统必须是Windows
10的2004或2004以上的版本才可以，或者使用Windows 11。

### 1.1 查询系统版本号

键盘点击`Win图标键 + R`
，接着输入`winver`，会自动打开一个窗口，在窗口中可以看到自己系统的版本号。
![image-20210926221939973](/assets/images/dxdhlMjMt/1632674292432.png)

例如上图所示：Windows11 版本 21H2（OS 内部版本 22000.194）

表示我的系统为Windows11，版本号为：21H2，内部版本号为：22000.194

### 1.2 是否符合要求

`You must be running Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11.`

官方要求为Windows10 的 版本号 2004 或
2004以上，也就是内部版本号大于等于19041的系统。或者Windows
11的所有版本系统。

如果你符合这个要求，可以继续往下看，本篇教程适合你使用。

如果你不符合这个要求，可以尝试升级系统后继续往下看，或者点击窗口上方小叉叉关闭本篇教程。

## 2 安装WSL2

在写教程的时候发现Windows安装WSL2的方法与我之前的不太一样，更简单了一些。当然也可以使用之前的方法安装。

官方教程：

新方法：https://docs.microsoft.com/zh-cn/windows/wsl/install#prerequisites

 旧方法：https://docs.microsoft.com/zh-cn/windows/wsl/install-manual

博主的理解（新方法）：

因为官方教程暂时还没出中文版本，我就把我如何安装的大致说一下，仅供参考。

### 2.1 打开终端窗口：

 右键点击`Windows图标键`，选择`命令提示符(管理员)`（Windows 11
版本是`Windows 终端（管理员）`）。
![image-20210926222002121](/assets/images/dxdhlMjMt/1632674324001.png)
注意：一定要选择<font color=red>管理员</font>，不然无法安装。

### 2.2 输入安装指令

新方法只需要在上一步打开的终端里面输入如下指令即可：

``` powershell
wsl --install
```
![image-20210926224259671](/assets/images/dxdhlMjMt/1632674342914.png)
这个指令会自动安装WSL2，并且下载最新版的Ubuntu作为子系统。

等待安装完后<font color=red>重启电脑</font>，之后就会自动启动Ubuntu子系统。到这里就安装完成了WSL2。

### 2.3 设置子系统

重启后会自动弹出Ubuntu的窗口，此时会配置Ubuntu，按照提示输入信息即可。
![image-20210926232955141](/assets/images/dxdhlMjMt/1632674374814.png)
首先输入用户名，
![image-20210926233050742](/assets/images/dxdhlMjMt/1632674393441.png)
接着设置一个子系统的密码（输入过程不显示内容），密码需要输入两遍。
![image-20210926233201079](/assets/images/dxdhlMjMt/1632674407313.png)
![image-20210926233255149](/assets/images/dxdhlMjMt/1632674419627.png)
出现如上图所示的绿色字符，即表示子系统配置并启动成功。

## 3 安装Dokcer到WSL2

### 3.1 设置超级用户(root)密码

密码可以与前面设置的一样，也可以不一样。

终端输入：

``` bash
sudo passwd root
```
![image-20210926234442001](/assets/images/dxdhlMjMt/1632674435424.png)
记住这个密码，以后会经常用到。

### 3.2 安装Dokcer

首先进入超级用户模式，命令行输入如下指令：

``` bash
su
```

输入超级用户(root)密码后可以进入。

进入后，输入如下指令到Ubuntu窗口：

``` bash
apt-get update
```

中途可能会有询问，一路输入`Y`即可。

接着输入：

``` bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

安装时间比较长，请耐心等待，中途遇到报错请无视。
![image-20210926235303573](/assets/images/dxdhlMjMt/1632674463209.png)
看见如上图所示，表示已经安装完毕。

## 4 如何启动？

### 4.1 启动终端

- 方法一

  打开`开始菜单`，输入`Terminal`，运行。
  ![image-20210926235742659](/assets/images/dxdhlMjMt/1632674476614.png)
 在标签栏新建一个Ubuntu命令窗口：
  ![image-20210926235848832](/assets/images/dxdhlMjMt/1632674506536.png)
  ![image-20210927000209342](/assets/images/dxdhlMjMt/1632674529601.png)

  *如果你搜索不到`terminal`或者terminal里没有Ubuntu标签，可以往下看方法二。*

- 方法二

  打开`开始菜单`，输入`Ubuntu`，运行。
  ![image-20210927000135808](/assets/images/dxdhlMjMt/1632674550596.png)
  ![image-20210927000156127](/assets/images/dxdhlMjMt/1632674585670.png)

### 4.2 启动Docker

因为WSL2环境不能自动启动服务，所以每次打开终端后要手动启动。

首先进入超级用户（root）：

``` bash
su
```

输入超级用户（root）的密码，回车即可。

接着输入如下指令，启动Dokcer：

``` bash
service docker start
```
![image-20210927000604749](/assets/images/dxdhlMjMt/1632674606407.png)
看见`[ OK ]`字样，表示Docker服务已启动。接下来就可以正常使用Docker了。

后面提供了一些可选的教程，不是必须的，但是建议继续操作。

## 5 设置Docker镜像加速（可选）

首先进入超级用户（root）模式，并输入超级用户（root）密码：

``` bash
su
```

接着编辑镜像文件(不存在这个文件的话，会自动创建)：

``` bash
cat >> /etc/docker/daemon.json<<EOF{"registry-mirrors":["https://reg-mirror.qiniu.com/","https://docker.mirrors.ustc.edu.cn/","https://hub-mirror.c.163.com/"]}EOF
```

接着重启Dokcer：

``` bash
service docker restart
```

后面就会使用镜像加速拉取文件了。

## 6 设置开机自动启动Docker服务（可选）

首先进入超级用户（root）模式，并输入超级用户（root）密码：

``` bash
su
```

接着创建并编辑文件：`/etc/init.wsl`

``` bash
cat >> /etc/init.wsl<<EOF#! /bin/sh/etc/init.d/docker $1EOF
```

添加自动启动脚本：

点击键盘 `Win徽标键 + R`，输入：`shell:startup`

在打开的文件夹里创建一个名为`Docker.vbs`的文件，文件内容如下：

``` vbscript
Set ws = CreateObject("Wscript.Shell")ws.run "wsl -d debian -u root /etc/init.wsl start", vbhide
```

这样，在你下一次启动Windows的时候，就可以自动自动Docker服务。

## 7 参考资料

-   [WSL 服务自动启动的正确方法](https://zhuanlan.zhihu.com/p/47733615)
-   [微软官方：WSL2安装教程](https://docs.microsoft.com/zh-cn/windows/wsl/install)
-   [runoob教程的一键安装命令和镜像加速](https://www.runoob.com/docker/ubuntu-docker-install.html)
-   [WSL2下启动Docker指令](https://www.cnblogs.com/yunfeifei/p/13158845.html)
