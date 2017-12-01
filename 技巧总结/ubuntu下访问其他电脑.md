---
title: ubuntu下访问其他电脑
date: 2017-04-24 14:57:03
tags: 技巧
categories:
    - ubuntu
    - 远程访问
password: 
---

最近工作过程中，经常需要共享其他同事电脑上的资源，之前一般的做法是对方建立贡献文件，然后这边去进行访问。这有个弊端，一是经常出现访问失败的现象，另外就是每次对方都需要把
资源拷贝到共享中去，这时候如果是想要使用对方的源码，那么大的文件放到共享中也很麻烦。后来就在网上看到了如下两个比较有用的命令，一个是`ssh`，一个是`scp`

# SSH

这个命令是用来访问远程电脑的，`ssh`分为客户端`openssh-client`和服务器端`openssh-server`

## 安装SSH

要想给其他机器提供`ssh`远程登录，则必须安装服务器端`Server`，并保证`ssh`服务正常运行。

要想通过`ssh`登录其他机子，则必须安装客户端`Client`。

可执行下面命令进行安装。
```
sudo apt-get install openssh-client openssh-server
```

## 启动SSH

ssh服务启动

```
sudo /etc/init.d/ssh start
```

ssh停止服务

```
sudo /etc/init.d/ssh stop
```

ssh重启服务
```
sudo /etc/init.d/ssh restart
```

## 连接远程SSH

通过IP访问

```
ssh username@192.168.1.112 
```
比如

```
ssh 1989@10.100.90.240
```

接下来需要你输入访问用户的弥漫

# SCP

`scp` 是`secure copy`的简写，用于在`Linux`下进行远程拷贝文件的命令，和它类似的命令有`cp`，不过`cp`只是在本机进行拷贝不能跨服务器，而且`scp`传输是加密的

## 常用命令

这个命令比较多，当前就掌握最基本的，也就是从远程拷贝以及拷贝到远程

**从远程拷贝**

```
scp remote_username@remote_ip:remote_folder local_file
```
比如

```
scp 1989@10.100.90.240:/home/zsl1989/test ~
```

**拷贝到远程**
```
scp local_file remote_username@remote_ip:remote_folder 
```
比如

```
scp test 1989@10.100.90.240:/home/zsl1989/test
```
