---
layout: post
title: "如何将nodejs做成Linux服务"
date: 2017-05-06
tags: node.js
---

### 1 用forever  进行管理

```
npm install -g forever
forever start index.js

```

如果提示

```
warn:    --minUptime not set. Defaulting to: 1000ms
warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
```

尝试

```
forever start --minUptime 100 --spinSleepTime 100 -l xxx.log -a index.js 
```



修改/etc/rc.local文件，增加一行：

> tail -n 1 /etc/rc.local  

```
forever start -w /usr/local/server.js  
```

查看forever启动列表：

> [root@IOTSS64x ~]# forever list  
>
> info:    Forever processes running  
>
> data:    uid  command             script    forever pid   logfile                 uptime  
>
> data:    [0] IAHz /usr/local/bin/node server.js 18327   18329 /root/.forever/IAHz.log 0:2:29:27.885  



### 2 用自带的服务nohub

```
nohup node index.js > myLog.log 2>&1 &
```

![](http://ondh71tpt.bkt.clouddn.com/img/posts/service/06.png)



### 3 使用&后台启动

> node index &



参考资料:

http://natumsol.github.io/2016/03/16/nginx-basic/

[upstart封装nodejs应用为系统服务](http://blog.fens.me/linux-upstart-nodejs/) 

[upstart把应用封装成系统服务](http://blog.fens.me/linux-upstart/) 

[Centos下用upstart管理自己的服务程序](http://blog.csdn.net/u011344514/article/details/49863091) 

