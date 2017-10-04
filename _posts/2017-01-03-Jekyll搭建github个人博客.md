---
layout: post
title: "Jekyll搭建github个人博客"
date: 2017-01-03
tags: 博客
---

[TOC]



### 一、安装ruby环境

Windows下载地址: http://rubyinstaller.org/

Ruby官网: http://www.ruby-lang.org/zh_cn/

下载安装即可

#### 环境配置

安装好之后必须配置环境变量

在环境变量中`PATH`字段中增加 `你的安装路径\Ruby22-x64\bin` 比如 : `D:\ProgramFiles\Ruby22-x64\bin`

除此之外还必须新增一个字段 `SSL_CERT_FILE`，内容为`cacert.pem`所在路径，比如`D:\ProgramFiles\Ruby22-x64\cacert\cacert.pem`

cacert.pem下载地址: https://curl.haxx.se/ca/cacert.pem

#### 检测安装

`ruby -v`

`gem -v`

如果顺利打印出版本号则安装成功

#### 替换rubyGem库地址

如果使用gem安装软件较慢，可以尝试替换gem源为国内的源

```
gem sources --remove https://rubygems.org/
gem sources -a https://gems.ruby-china.org/
gem sources -l
```

淘宝源:

```
gem sources -a https://ruby.taobao.org/
```

##### 遇到 SSL 证书问题

如果遇到 SSL 证书问题，你又无法解决，请直接用 http://gems.ruby-china.org 避免 SSL 的问题

```
D:\MyBlog\RubyDevKit>gem sources -l
*** CURRENT SOURCES ***

http://gems.ruby-china.org/
```

### 二、安装RubyDevKit

> DevKit是windows平台下编译和使用本地C/C++扩展包的工具。它就是用来模拟Linux平台下的make,gcc,sh来进行编译。但是这个方法目前仅支持通过RubyInstaller安装的Ruby

下载地址: http://rubyinstaller.org/downloads/

双击运行解压到D:\MyBlog\RubyDevKit (路径可自行定义)。

打开终端cmd，输入如下命令进行安装：

```
cd D:\MyBlog\RubyDevKit
ruby dk.rb init
```

接下来需要在D:\MyBlog\RubyDevKit\config.yml 这个文件里面配置Ruby的路径，如下：

```
- D:/ProgramFiles/Ruby22-x64/bin
```

在cmd中执行如下命令进行安装：

```
ruby dk.rb install
```

### 三、安装必要的gem包

```
gem install jekyll
gem install rails
gem install bundler
```

### 四、创建jekyll博客

```
jekyll new myBlog
cd myBlog
jekyll server
```

![](http://ondh71tpt.bkt.clouddn.com/img/posts/github.pages/08.jpg)

在浏览器输入http://127.0.0.1:4000/

![](http://ondh71tpt.bkt.clouddn.com/img/posts/github.pages/09.jpg)

##### 报错排查

###### 端口被占用

```
D:\GarryBlog>jekyll server
Configuration file: D:/GarryBlog/_config.yml
Configuration file: D:/GarryBlog/_config.yml
            Source: D:/GarryBlog
       Destination: D:/GarryBlog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.252 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/GarryBlog'
Configuration file: D:/GarryBlog/_config.yml
jekyll 3.3.1 | Error:  Permission denied - bind(2) for 127.0.0.1:4000
```

解决方案:

这个错误是告诉我们4000端口被占用，解决方法是：
在_config.yml文件的末尾加上port: 5000，改为5000端口即可。
这样在浏览器中输入[http://127.0.0.1:5000/](http://127.0.0.1:5000/) 就可以看到自己的博客了。

### 五、安装git环境

 下载地址: https://git-for-windows.github.io/
安装完成后运行Git Bash。在打开的窗口中输入如下命令设置你的git用户名和邮箱：

```
$ git config --global user.name "{username}"          // 用你的用户名替换{username}
$ git config --global user.email "{name@site.com}"    // 用你的邮箱替换{name@site.com}
```

SSH配置：
为了和Github的远程仓库进行传输，需要进行SSH加密设置。

```
$ ssh-keygen -t rsa -C"{name@site.com}"    // 用你的邮箱替换{name@site.com}
```

一路敲回车即可，在C:\Users\admin.ssh 目录下会生成id_rsa 和 id_rsa.pub 两个文件，其中 id_rsa 是私钥，需要保密， id_rsa.pub 是公钥，无需保密。
在浏览器中登录你的github帐号，点击右上角的Setting-SSH and GPG keys，在SSH Key中添加 id_rsa.pub里的内容，然后点击addkey即可，这样SSH配置就完成了。

### 六、部署到GitHub Pages

建议基于jekyll的个人博客有两种路线：

- 自己学习Jekyll教程和网页设计，设计绝对自我基因的网页。

- Fork已有的开源博客仓库，在巨人的肩膀上进行符合自我的创作。

  ​

#### 路线一

1. 新建一个与自己github用户名相同的仓库，仓库名为 {github用户名}.github.io

   如 : quanzaiyu.github.io

![](http://ondh71tpt.bkt.clouddn.com/img/posts/github.pages/01.jpg)

2. 配置github pages

   ![](http://ondh71tpt.bkt.clouddn.com/img/posts/github.pages/02.jpg)

   ![](http://ondh71tpt.bkt.clouddn.com/img/posts/github.pages/03.jpg)

3. 选择自己喜欢的github主题

   ![](http://ondh71tpt.bkt.clouddn.com/img/posts/github.pages/04.jpg)

4. 在浏览器输入 {github用户名}.github.io 即可访问自己的个人博客

   比如: https://quanzaiyu.github.io/

5. 如果想要使用jekyll搭建个人博客，先将此仓库clone到本地，使用上述方法创建个人博客再发布到github即可



#### 路线二

1. 在网上搜索jekyll 网站模版，挑选一个你看上的，比如

   https://github.com/leopardpan/leopardpan.github.io

    [https://github.com/mzlogin/mzlogin.github.io](https://github.com/mzlogin/mzlogin.github.io) 

   https://github.com/dapengyou/dapengyou.github.com

   http://jekyllthemes.org/

2. 点击链接进入后，点击左上角的fork

3. 在你的主页中点击刚fork的分支，点击进入

4. 点击“Settings”，将“Repository name”改为 {你的Github用户名}.github.io，点击“Rename”。

   * 注意: 一定必须是自己的Github用户名

   ![](http://ondh71tpt.bkt.clouddn.com/img/posts/github.pages/07.jpg)

   此时你会发现已经可以通过 https://{你的Github用户名}.github.io

比如我的: https://quanzaiyu.github.io/

    

### 七、同步博客

#### 同步仓库

在Git Bash中切换到你想存放blog文件的目录下：

```
cd D:\GarryBlog
```

输入如下命令，将远程仓库克隆到本地：

```
git clone https://github.com/Garry2016/garry2016.github.io.git
```

#### 撰写博文

打开本地仓库的 _posts 文件夹，你的所有博文都将放在这里，写新博文只需要新建一个标准文件名的文件，在文件中编写文章内容。 比如我们fork的模版中 _posts 文件夹里有一篇 2016-03-23-hello-world.markdown，你的文件命名也要严格遵循 年-月-日-文章标题.文档格式 这样的格式，尤其要注意月份和日期一定是两位数。
推荐使用Markdown语言写文章，windows下推荐MarkdownPad这个软件编写Markdown文本。
最开始写可以直接模仿别人的博文语法，更多Markdown语法可参考 [认识与入门Markdown](http://sspai.com/25137)。

#### 提交修改

当你使用Git Bash对你的本地仓库进行操作时，先用 cd 命令将你的工作目录设置到你要操作的本地仓库

```
$ cd {你刚才clone下来的项目文件夹路径}
```

每当你对本地仓库里的文件进行了修改，只需在Bash中依次执行以下三个命令即可将修改同步到Github，刷新网站页面就能看到修改后的网页：

```
$ git add .
$ git commit -am "statement"   //此处statement填写此次提交修改的内容，作为日后查阅
$ git push origin master
```

#### 实时查看本地修改的内容

我们本地已经安装好了jekyll环境，我们可以输入 jekyll server启动服务，然后在浏览器中查看本地修改内容，方便快捷！

------

好了，使用jekyll搭建个人博客就写完了，还涉及一些内容这里没讲，比如评论，分享等功能。我们fork的博客里面已经实现了这些功能，大家去看源码应该就可以知道了。