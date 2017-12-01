---
title: android CTS 测试 -Linux
date: 2017-03-22 16:29:15
tags: 工作
categories:
    - 工具
    - android
password: 
---

之前一直听别人提到`CTS`测试，今天更好有机会要进行相关测试，就学习了下，现记录下来，方便以后温故知新

# CTS 介绍

`CTS` 全称是`Compatibility Test Suite`,翻译过来就是`兼容性测试套件`，它的作用就是测试系统兼容性.CTS是开源的测试框架，使用它来测试你的设备是否具备兼容性。CTS主要包含两个组件：

- 运行在PC上的测试框架组件

主要用来管理测试用例（test case）的执行。

- 运行在设备或模拟器上的测试用例

这些用例用Java写成的APK文件。

** 1. 为什么需要CTS 测试 ? **

1. 保证系统兼容更多的APP 

2. 让APP适配更多的设配

另外，CTS是免费的，而且很简单。


# 安装

## 1. 下载

从[官网](https://source.android.com/compatibility/cts/downloads)上下载自己合适的版本(电脑系统、安卓版本等),我下载的是`Android 7.1 R9 Compatibility Test Suite (CTS) - ARM`

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/android_cts_1.png)

## 2. 安装

此工具不用安装，解压后可直接使用

## 3. 执行
进入目录

> android-cts/tools

里面有一个`cts-tradefed`文件，我们直接执行就可以了


![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/android_cts_2.png)

```
./cts-tradefed
```
**注意**
`CTS`需要`adb` `aapt`的支持，所以要确保二者都已经成功安装且环境变量配置完好。我在执行脚本以后，就遇到了问题，如下

```
34:checkPath aapt
```

就是说缺少`aapt`，直接安装下就可以了

```
sudo apt install aapt
```

# 使用

执行
```
./cts-tradefed
```
后，就进入了`CTS`测试模式，在测试模式中执行对应的命令，即可进行测试，常用命令如下(我的是CTS V2)

**help**

显示帮助信息

**list modules**

显示所有可用模块

**list plans**

显示可用设置

**run cts**

执行`CTS`测试
