---
title: 用Lobe来部署Lobe？——Docker Compose部署Lobe杂谈
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [Docker, GPT, LLM, Gemini]
date: 2024-12-13 18:24:47 +0800
img:
coverImg:
password:
summary:
---

## 前情提要

从[使用Docker Compose安装Lobe数据库版本](../docker_compose_install_lobe_database_version)开始谈起。

最初，我安装了简易版本的Lobe，用起来不错，但是使用时间一长，就会有新的问题了。

我很想体验一下它的知识库功能，再加上我之前是在本机搭建的，换台电脑不在同一个网络下（比如公司/家），就不能访问了。

根据以上的契机，我决定尝试在服务器上部署Lobe，并且这次将会尝试使用数据库版本进行部署。

开始部署前，最近Gemini比较火，因为它的新模型`gemini-2.0-flash-exp`，质量提升了很高，也支持了联网功能，所以，这次就尝试在Gemini的帮助下试试能不能完成部署吧。

## 部署

直接参考以下全文的对话内容：

<iframe src="/assets/files/2f2bf5b1-27f7-4ee8-ba82-5879cd930a80.pdf" width="800" height="600">
  <p>您的浏览器不支持内联框架。</p>
</iframe>

挺长，但是挺专业，几乎没有任何提示，完成了Lobe的部署，真的很强。
