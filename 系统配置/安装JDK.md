---
title: 安装配置JDK
date: 2017-01-21 14:22:58
tags: 系统配置
categories:
    - 环境
    - 
password: 
---


# 概念区分

对于`java`中的`JDK` `JRE` `JVM` 一直很混乱，今天配新环境，顺便网上学习了下这几个概念的区别。
以下内容参考自[FlyElephant](http://www.cnblogs.com/xiaofeixiang)的博客

```

JDK（Java Development Kit）简单理解就是Java开发工具包
JRE(Java Runtime Enviroment)是Java的运行环境
JVM( java virtual machine)也就是常常听到Java虚拟机
JDK是面向开发者的，JRE是面向使用JAVA程序的用户
```

我们可以看下`JDK`的目录


![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/jdk_jre.png)

可以看到`JDK`是自带`JRE`的
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/jdk_jvm.png)

在`jre`中能看到`jvm`的信息
综合来看，JDK包含JRE，而JRE包含JVM，JDK是用于java程序的开发,而jre则是只能运行class而没有编译的功能

# 安装JDK

## Windows安装

1. 首先就是下载window 的jdk，百度上搜下就有了

2. 然后直接点击安装

3. 配置环境变量

- 添加环境变量JAVA_HOME =安装地址（我的是c:\Program File\Java\jkd1.8.0_65）

- 在path中添加 %JAVA_HOME%\bin

- 配置classpath（没有可自己创建） classpath= %JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar

然后我们只需要在cmd下，查看java -version就可以了

## UBUNTU安装

**1.下载JDK**

从官网下载所需要的版本，我选择的是[jdk1.8.0_144](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/jdk_down.png)

**2.解压**

在`/usr/bin`下创建`java`文件夹，把解压后的文件夹拷贝到此目录下。

```
cd /usr/bin
sudo mkdir java
sudo move jdk1.8.0_144 .
```
**3.配置环境变量**

在`/etc/profile`中添加`JDK`

```
export JAVA_HOME=/usr/lib/java/jdk1.8.0_144
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

**4.设置软链接**

```
sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk1.8.0_144/bin/java 300
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/jdk1.8.0_144/bin/javac 300
```
重启下电脑，然后通过`ava -version`查看是否成功。如果出现如下内容，说明设置成功了

```
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
```
