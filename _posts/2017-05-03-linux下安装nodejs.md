---
layout: post
title: "linux下安装nodejs"
date: 2017-05-03
tags: Linux
---

### 获取nodejs 资源

\# 4.x

*curl --silent --location https://rpm.nodesource.com/setup_4.x \| bash -*

\# 5.x

*curl --silent --location https://rpm.nodesource.com/setup_5.x \| bash -*

\# 6.x

*curl --silent --location https://rpm.nodesource.com/setup_6.x \| bash -*

\# 0.10

*curl --silent --location https://rpm.nodesource.com/setup \| bash -*

我这里安装的是 v4.x

### 安装

yum install -y nodejs

### 测试是否安装成功

node -v 

 # v4.4.0

npm -v

\# 2.14.20



参考资料:

http://jingyan.baidu.com/article/dca1fa6f48f478f1a5405272.html

