---
layout: post
title: "Windows下如何将Nginx做成系统服务"
date: 2017-05-03
tags: Windows
---

### 1. 准备工作

下载安装nginx，并记住安装目录 [官网下载](http://nginx.org/en/download.html)

下载winsw，[下载地址](http://www.cr173.com/soft/101797.html)

### 2. winsw设置

将winsw可执行程序复制到nginx安装目录下，并重命名为nginx-service

新建名为nginx-service.xml的文件（注：文件名必须与可执行文件名相同）

并编辑如下，其中name为 服务名，executable为可执行程序路径，logpath为程序运行日志路径

```xml
<service>      
  <id>nginx</id>      
   <name>nginx</name>      
   <description>nginx</description>      
   <executable>E:\phpStudy\nginx\nginx.exe</executable>      
   <logpath>E:\phpStudy\nginx</logpath>      
   <logmode>roll</logmode>      
   <depend></depend>      
   <startargument>-p E:\phpStudy\nginx</startargument>      
   <stopargument>-p E:\phpStudy\nginx -s stop</stopargument>      
</service>  
```

如下：

### 3. 安装服务

在nginx安装目录下运行cmd（快捷方式：shift + 鼠标右键），运行命令：nginx-service.exe install

注：nginx-service.exe uninstall命令可删除对应的系统服务

nginx-service.exe stop命令可停止对应的系统服务

nginx-service.exe start命令可启动对应的系统服务

### 4. 查看服务是否安装成功

计算机管理  -> 服务

如服务为未运行状态，可在此启动服务，或设置为自动启动

注：若服务安装成功，可在cmd（管理员身份）中对服务进行如下操作

启动nginx ：net start nginx

停止nginx：net stop nginx

### 5. 验证nginx是否正常运行

在浏览器中打开网址http://localhost

### 6. 修改端口

如果端口被占用，修改nginx/conf/nginx.conf文件中的listen字段即可