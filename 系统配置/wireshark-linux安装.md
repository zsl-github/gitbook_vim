---
title: linux下安装wireshark
date: 2017-03-23 19:06:54
tags: 系统配置
categories:
    - 系统配置
    - 工具
password: 
---

之前在`windows`下用过`wireshark`，现在换了`ubuntu`，虽然有安装虚拟机，但是不想把常用工具安装到它里面，因为操作起来太麻烦。就在网上看了下是不是有方法，最好直接有
`linux`版本。很遗憾，官网上只有`win` `IOS`,还有就是源码。网上说的都是通过源码安装，但是在安装的过程中依赖太多，一直在报错，没办法，想着有没有更简单的方法。还真别说，抱着试试看的想法，在终端执行`wireshark`，竟然提示要你命令行安装
```
sudo apt install wireshark
```
即可。然后通过命令`wireshark`就能启动了。

