---
layout: post
title: "如何绑定自己的域名到GitHub pages"
date: 2017-03-20
tag: github
---

[TOC]

首先，感谢 [康力兄](https://github.com/LiuKangli) 的帮助，没有折腾很久! 

### 1.域名解析

登录自己的域名备案管理系统，根据自己申请域名的服务提供商进行对应操作，我的是阿里云，都大同小异。

添加域名解析记录，新增一个CNAME记录，主机记录随便填写，如blog，记录值输入自己GitHub pages地址即可，

如图:

![](http://ondh71tpt.bkt.clouddn.com/img/posts/CNAME/01.jpg)

### 2.新建CNAME文件

在GitHub pages仓库的根目录新建一个CNAME的文件，填写上对应解析出来的地址即可，解析出的地址为:

主机记录.自己的域名，如我的: blog.xiaoyulive.top

![](http://ondh71tpt.bkt.clouddn.com/img/posts/CNAME/02.jpg)

![](http://ondh71tpt.bkt.clouddn.com/img/posts/CNAME/03.jpg)

### 3.完成

访问 http://blog.xiaoyulive.top 即可访问自己的GitHub个人博客

![](http://ondh71tpt.bkt.clouddn.com/img/posts/CNAME/04.jpg)