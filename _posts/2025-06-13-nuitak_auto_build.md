---
title: Windows下使用Nuitka自动编译Python脚本
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [Docker, CI, Nuitka, Python, Windows]
date: 2025-6-13 14:15:12 +0800
img:
coverImg:
password:
summary:
---

## 问题描述

从官方的[issue](https://github.com/Nuitka/Nuitka/issues/2787)和[commit](https://github.com/Nuitka/Nuitka/commit/8d4147e7da1fdcee540dff33b05c91a5a384d19c)可知，现阶段Nuitka不再非官方指定的编译器版本。即便使用官方指定的同版本gcc，也会抛出异常，必须通过Github下载对应的zip包才能使用。

> Read the changelog, Nuitka does't accept anymore wingcc versions that are not the ones it downloaded.

> Scons: Fix, do not accept any gcc on Windows version that is not ours
> * There are way too many issues, broken ccache recently,
>   broken clang potentially, broken binutils, etc. that
>   are all hard to recognize.

我尝试了下载`15.1.0`和`14.2.0`，解压出来后，将对应的bin目录添加到`PATH`中，编译时仍然会报错。

~~此报错来自issue的日志，实际与我本地的异常一致，这里直接引用了issue的日志。~~
```bash
Nuitka-Options: Used command line options: --standalone --mingw64 ..\..\test.py
Nuitka: Starting Python compilation with Nuitka '2.1.2' on Python '3.11' commercial grade 'not installed'.
Nuitka: Completed Python level compilation and optimization.
Nuitka: Generating source code for C backend compiler.
Nuitka: Running data composer tool for optimal constant value handling.
Nuitka: Running C compilation via Scons.
Nuitka-Scons: Non downloaded winlibs-gcc 'D:\WebTools\WinLibs_x64_Nuitka\bin\gcc.exe' is being ignored, Nuitka Nuitka-Scons: is very dependent on the precise one.
Nuitka will use gcc from MinGW64 of winlibs to compile on Windows.

Is it OK to download and put it in '.\Nuitka\Nuitka\Cache\downloads\gcc\x86_64\13.2.0-16.0.6-11.0.1-msvcrt-r1'.


Fully automatic, cached. Proceed and download? [Yes]/No : 
```

从日志的提示可以看出：
```
Non downloaded winlibs-gcc 'D:\WebTools\WinLibs_x64_Nuitka\bin\gcc.exe' is being ignored, Nuitka Nuitka-Scons: is very dependent on the precise one.
```
Nuitka会忽略未下载的winlibs-gcc，且非常依赖精确的版本。
```
Is it OK to download and put it in '.\Nuitka\Nuitka\Cache\downloads\gcc\x86_64\13.2.0-16.0.6-11.0.1-msvcrt-r1'.
```
Nuitka会下载并放置在`.Nuitka\Nuitka\Cache\downloads\gcc\x86_64\13.2.0-16.0.6-11.0.1-msvcrt-r1`目录下。(我这边使用的是2.7.7的Nuitka，它提示的路径为：`C:\Users\ContainerAdministrator\AppData\Local\Nuitka\Nuitka\Cache\downloads\gcc\x86_64\14.2.0posix-19.1.1-12.0.0-msvcrt-r2`)

一般情况下，我们直接在日志弹出的位置回车就会自动下载对应的包，但是我这里有些特殊情况，所以，无法从网络（Github）进行下载。
我这边是一个Docker容器（Windows镜像），用来Gitlab CI/CD编译Python脚本的，容器内没有非常好的网络环境下载Github上的文件，而且也没必要每次编译都重新下载一遍。
所以，这里的解决方案是修改原有的Docker镜像，将需要的zip包提前放到容器内的指定目录下，这样在编译时就不会触发下载的提示了。

## 解决方案
从日志描述可以看出，Nuitka会自动下载并放置在`C:\Users\ContainerAdministrator\AppData\Local\Nuitka\Nuitka\Cache\downloads\gcc\x86_64\14.2.0posix-19.1.1-12.0.0-msvcrt-r2`目录下。
那我们就提前把对应的文件放到这个目录下，在编译的时候，它就会自动解压该文件，就不会触发下载的提示了。

另外，这边在运行时，也收到了需要Cache的提示，也是需要下载，这里解决方案也是一样，把下载的zip包放到`C:\Users\ContainerAdministrator\AppData\Local\Nuitka\Nuitka\Cache\downloads\depends\x86_64\`目录下即可。

注意，上文提到的路径需要自己根据实际情况进行调整。