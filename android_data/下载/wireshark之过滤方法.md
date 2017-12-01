---
title: wireshark过滤方法
date: 2017-11-27 10:37:18
tags: wireshark
categories:
    - 过滤
    - 
password: 
---

在使用`wireshark`的过程中，经常需要在整份`LOG`中筛选出我们关心的信息，比如跟某一地址的交互，某一端口的数据信息等，这里就记录下具体的筛选方法。

# 说明

`wireshark`的筛选是有一定的标签的，只要在如下输入框中

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/wire_shark_filter.png)

输入一定的过滤条件即可

# 常用过滤

## 针对IP过滤

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/wire_shark_filter.png)

```
ip.src == 192.168.0.168
```
对源地址为192.168.0.1的包的过滤

```
ip.dst == 192.168.0.1
```
对目的地址为192.168.0.1的包的过滤

```
ip.src == 192.168.0.1 or ip.dst == 192.168.0.168
```
源或者目的地址的ip地址是192.168.0.1的包

## 针对协议的过滤
直接输入协议名即可
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/wire_shark_filter_2.png)

仅仅需要捕获某种协议的数据包，表达式很简单仅仅需要把协议的名字输入即可。
```
http
```
需要捕获多种协议的数据包，也只需对协议进行逻辑组合即可。
```
http or telnet
```
排除某种协议的数据包
```
not arp      !tcp
```

## 对端口的过滤

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/wire_shark_filter_3.png)

捕获某一端口的数据包
```
tcp.port == 80
```

捕获多端口的数据包，可以使用and来连接，下面是捕获高端口的表达式
```
udp.port >= 2048
```
