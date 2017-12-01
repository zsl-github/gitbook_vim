---
title: ubuntu下安装Beyond Compare
date: 2017-03-26 09:02:12
tags: 系统配置
categories:
    - 系统配置
    - 工具
password: 
---

之前在`window`下工作的时候，移植代码的时候使用的对比工具是`Beyond Compare`，现在`ubuntu`下工作，也希望使用此工具，刚好此工具也支持`ubuntu`的，先记录下安装方法

# 下载

在网上下载`ubuntu`版本的[Beyond Compare 4](http://www.scootersoftware.com/download.php)，根据自己的系统选择相应的版本，如下

![下载版本](https://github.com/zsl-github/blog/raw/master/source/picture//beyond_compare_download.png)

# 安装

我下载的的是`deb`格式的软件包，直接执行如下命令即可

```
sudo dpkg -i bcompare-4.2.3.22587_amd64.deb
```
即可

# 使用

直接在命令行执行`bcompare`即可，或者搜索`be`，找到此软件打开即可

# 注意

此软件是商业软件，是收费的，普通用户只能试用30天

# 17.11.15补充

现在对比工具主要是用`meld`，感觉有它就够了，不需要使用`Beyond`了，而且前者是免费的。

