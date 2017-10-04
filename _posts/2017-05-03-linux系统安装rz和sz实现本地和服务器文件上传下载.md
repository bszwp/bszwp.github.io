---
layout: post
title: "linux系统安装rz和sz实现本地和服务器文件上传下载"
date: 2017-05-03
tags: Linux
---

### 一：说明 

    通过SecureCRT等SSH登录软件连接服务器，可以通过rz和sz命令上传下载文件，就不需要通过xftp进行文件操作了. 
sz 文件名: 将选定的文件发送(send)到本地机器; 
rz：运行该命令会弹出 一个文件选择窗口, 从本地选择文件上传到服务器(receive). 


### 二：安装

1、获取安装包 
`wget http://www.ohse.de/uwe/releases/lrzsz-0.12.20.tar.gz`

2、解压缩 
`tar zxvf lrzsz-0.12.20.tar.gz`

3、编译安装 
进入解压缩的目录：`cd lrzsz-0.12.20` 
编译安装：`./configure && make && make install` 

### 三：配置 

上面安装过程默认把lsz和lrz安装到了`/usr/local/bin/`目录下，可以创建软链接，并命名为rz/sz（这时是可以通过`./lrz`来运行的，但是我们希望在任何目录下通过sz/rz命令都可以使用，可以配置环境变量 ）： 上面安装过程默认把lsz和lrz安装到了`/usr/local/bin/`目录下，可以创建软链接，并命名为rz/sz（这时是可以通过`./lrz`来运行的，但是我们希望在任何目录下通过sz/rz命令都可以使用，可以配置环境变量 ）： 
（1）进入目录`cd /usr/local/bin` 
（2）创建软链接并命名为rz/sz：`ln -s lrz rz && ln -s lsz sz` 
（3）修改`/ect/profile` 环境变量

```
#lrz and lsz setting
export FILEMANAGE_HOME=/usr/local
export PATH=$FILEMANAGE_HOME/bin:$PATH
```

如果profile文件权限不够，使用 `chmod 666 profile`命令修改文件权限

 
现在我们可以通过命令来进行文件传输工作了

![这里写图片描述](http://ondh71tpt.bkt.clouddn.com/img/posts/service/02.png)
（4）还可以设置接收和发送文件的目录位置

![这里写图片描述](http://ondh71tpt.bkt.clouddn.com/img/posts/service/03.jpg) 

### **四、总结 **

sz +文件名: 将选定的文件发送(send)到本地机器; 
rz：运行该命令会弹出 一个文件选择窗口, 从本地选择文件上传到服务器(receive).



参考资料:

http://blog.csdn.net/u013082989/article/details/52334711