---
title: ubuntu下安装数据库工具
date: 2017-03-25 18:49:24
tags: 系统配置
categories:
    - 系统配置
    - 工具
password: 
---

最近在处理工作上的问题的时候，需要查看数据库-db文件。因为不想在虚拟机中安装相关的工具，所以就想在`ubuntu`下看看有没有什么好用的工具可以直接查看数据库文件，这里就记载下
具体方法

# 安装mysql

执行如下命令进行安装

```
1. sudo apt-get install mysql-server

2. apt-get install mysql-client

3.  sudo apt-get install libmysqlclient-dev
```
安装过程中会提示你给数据库添加密码，我最开始就是忘了设置，直接点的OK，导致一直无法登录

安装好后，发现这个只是安装本地数据库，不能用来看相关的`db`文件

# 安装 sqlite3

直接执行如下命令即可

```
apt-get install sqlite sqlite3
```

这个也是数据库，跟`mysql`相似，只是它是轻量级的数据库。两者的差别主要就是

> 简单来说，SQLITE功能简约，小型化，追求最大磁盘效率；MYSQL功能全面，综合化，追求最大并发效率。如果只是单机上用的，数据量不是很大，需要方便移植或者需要频繁读/写磁盘文件的话，就用SQLite比较合适；如果是要满足多用户同时访问，或者是网站访问量比较大是使用MYSQL比较合适。

具体差别可以看下如下博客
[SQLite和MySQL数据库的区别与应用](http://blog.csdn.net/zbw1185/article/details/47975965)

# 图形化工具sqlitebrowser
这个工具才是可以直接图形化打开`db`文件的工具

执行命令

```
sudo apt-get install sqlitebrowser
```

执行`sqlitebrowser`即可

效果如下

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/db_brower_sq.png)

# 备注
上面的工具还不太熟悉，等以后慢慢掌握了再补充

