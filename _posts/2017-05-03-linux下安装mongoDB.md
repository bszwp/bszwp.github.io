---
layout: post
title: "linux下安装mongoDB"
date: 2017-05-03
tags: Linux
---

## 方法一

### 1.编写repo文件

```
[mongodb-org-3.0]
name=MongoDB Repository
baseurl=http://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.0/x86_64/
gpgcheck=0
enabled=1
```

### 2.安装

`yum install -y mongodb-org`

### 3.使用

配置文件在：/etc/mongod.conf  数据文件在：/var/lib/mongo  日志文件在：/var/log/mongodb  mongodb服务使用

```
#启动
service mongod start
#停止
service mongod stop
#重启
service mongod restart
#增加开机启动
chkconfig mongod on
```



## 方法二

### 1.下载

`wget https:``//fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-3.2.6.tgz`

### 2.解压

`tar -zxvf mongodb-linux-x86_64-3.2.6.tgz`

### 3.安装

```
mv mongodb-linux-x86_64-3.2.6.tgz mongodb
cd mongodb
mkdir db
touch logs
cd bin
vi mongodb.conf
```

填写

```
dbpath=/usr/local/mongodb/db
logpath=/usr/local/mongodb/logs
port=27017
fork=true
nohttpinterface=true
```

其中/usr/local/mongodb是软件安装位置，如果安装在root目录下则改为

```
dbpath=/root/mongodb/db
logpath=/root/mongodb/logs
port=27017
fork=true
nohttpinterface=true
```

### 4.启动

启动MongoDB服务

`cd bin`

`./mongod -dbpath=/usr/local/mongodb/db -logpath=/usr/local/mongodb/logs`

或

`./mongod -dbpath=/root/mongodb/db -logpath=/root/mongodb/logs`

```
重新绑定mongodb的配置文件地址和访问IP

/usr/local/mongodb/bin/mongod --bind_ip localhost -f /usr/local/mongodb/bin/mongodb.conf
或
/root/mongodb/bin/mongod --bind_ip localhost -f /root/mongodb/bin/mongodb.conf
```



### 5.后台启动

开机自动启动mongodb

```
vi /etc/rc.d/rc.local
```

填入
```
/root/mongodb/bin/mongod --config /root/mongodb/bin/mongodb.conf
```
重启一下系统测试下能不能自启


后台服务启动

```
/usr/local/mongodb/bin/mongod -dbpath=/usr/local/mongodb/db -logpath=/usr/local/mongodb/logs --fork
或
/root/mongodb/bin/mongod -dbpath=~/mongodb/db -logpath=/root/mongodb/logs --fork
```

后台权限启动

```
/usr/local/mongodb/bin/mongod -dbpath=/usr/local/mongodb/db -logpath=/usr/local/mongodb/logs --fork --auth
或
/root/mongodb/bin/mongod -dbpath=/root/mongodb/db -logpath=/root/mongodb/logs --fork --auth
```



### 6.查看mongdb是否正常启动

netstat -ntulp \| grep 27017

![](http://ondh71tpt.bkt.clouddn.com/img/posts/service/04.png)



### 7.使用mongdb

```
#进入mongodb的shell模式 
/root/mongodb/bin/mongo
#查看数据库列表 
show dbs
#当前db版本 
db.version();
```

![](http://ondh71tpt.bkt.clouddn.com/img/posts/service/05.png)





参考资料:

http://www.centoscn.com/CentosServer/sql/Mariadb/2015/0503/5342.html

http://www.cnblogs.com/weiweictgu/p/5517717.html