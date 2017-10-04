---
layout: post
title: "linux下安装nginx"
date: 2017-05-03
tags: Linux
---

### 一、使用源码安装

#### 1.1下载安装包

> wget http://nginx.org/download/nginx-1.5.9.tar.gz
>

#### 1.2 解压 

> tar -zxvf nginx-1.5.9.tar.gz 

#### 1.3 设置

> 设置一下配置信息 ，或者不执行此步，直接默认配置
>
> ./configure --prefix=/usr/local/nginx 
>

#### 1.4 编译及安装

> make #编译
>
> make install #安装 （make install是把这些编译出来的可执行文件和库文件复制到合适的地方）



### 二、yum安装

由于centos没有默认的nginx软件包，需要启用REHL的附件包

> rpm -Uvh http://download.Fedora.RedHat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm 
>
> rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
>
> yum -y install nginx



### 三、错误及解决

错误提示：./configure: error: the HTTP rewrite module requires the PCRE library.

解决办法：安装pcre-devel解决问题

> yum -y install pcre-devel



错误提示：./configure: error: the HTTP cache module requires md5 functions
from OpenSSL library.   You can either disable the module by using
--without-http-cache option, or install the OpenSSL library into the system,
or build the OpenSSL library statically from the source with nginx by using
--with-http_ssl_module --with-openssl=\<path> options.

解决办法：

> yum -y install openssl openssl-devel



### 四、启动与关闭

安装后在linux下启动和关闭nginx：

**启动操作**

> /usr/local/nginx/sbin/nginx 启动
>
> /usr/local/nginx/sbin/nginx -t 查看配置信息是否正确
>
> netstat -ntlp 查看服务是否启动

 **停止操作**

停止操作是通过向nginx进程发送信号（什么是信号请参阅linux文 章）来进行的
步骤1：查询nginx主进程号

> ps -ef \| grep nginx

在进程列表里 面找master进程，它的编号就是主进程号了。
步骤2：发送信号
**从容停止Nginx**

> kill -QUIT 主进程号

**快速停止Nginx**

> kill -TERM 主进程号

**强制停止Nginx**

> pkill -9 nginx

另外， 若在nginx.conf配置了pid文件存放路径则该文件存放的就是Nginx主进程号，如果没指定则放在nginx的logs目录下。有了pid文 件，我们就不用先查询Nginx的主进程号，而直接向Nginx发送信号了，命令如下：
kill -信号类型 '/usr/nginx/logs/nginx.pid'
**平滑重启**
如果更改了配置就要重启Nginx，要先关闭Nginx再打开？不是的，可以向Nginx 发送信号，平滑重启。
平滑重启命令：
kill -HUP 住进称号或进程号文件路径或者使用/usr/nginx/sbin/nginx -s reload  注意，修改了配置文件后最好先检查一下修改过的配置文件是否正 确，以免重启后Nginx出现错误影响服务器稳定运行。判断Nginx配置是否正确命令如下：
nginx -t -c /usr/nginx/conf/nginx.conf或者/usr/nginx/sbin/nginx -t 

**设置开机启动**

chkconfig nginx on



### 五、配置

查看nginx的安装目录

> rpm -ql nginx



参考资料:

http://www.cnblogs.com/kunhu/p/3633002.html