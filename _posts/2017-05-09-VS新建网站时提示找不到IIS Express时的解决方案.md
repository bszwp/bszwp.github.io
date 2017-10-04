---
layout: post
title: "VS新建网站时提示找不到IIS Express时的解决方案"
date: 2017-05-09
tags: IIS
---

装好了Visual Studio，也装好了IIS，但是启动VS新建项目的时候却提示"Visual Studio初始化applicationhost.config文件失败，找不到IIS Express"

解决方案:

下载Web Platform Installer，安装对应的IIS Express

下载地址:

https://www.microsoft.com/web/downloads/platform.aspx

运行Web Platform Installer，搜索IIS Express

![](http://ondh71tpt.bkt.clouddn.com/img/posts/IIS/05.png)

安装即可

启动VS，新建网站成功

![](http://ondh71tpt.bkt.clouddn.com/img/posts/IIS/07.png)





参考资料:

http://www.cnblogs.com/leleroyn/archive/2011/02/25/1965016.html

