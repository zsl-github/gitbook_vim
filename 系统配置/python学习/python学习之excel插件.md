---
title: python插件之excel
date: 2017-03-12 16:33:08
tags: python
categories:
    - 插件
    - 
password: 
---

最近需要对比两个表格的内容，然后修改其中的某列内容。因为工作量太大，所以想通过`python`来实现。上网查了相关的操作，其中牵扯到两个功能模块，`xlrd xlwt`。这两个功能模块分别是对`excel`进行读写的操作。那么，我们应该如何实现功能模块的安装呢，这里我就记录下我的安装过程。

前提条件：确保你的电脑上已经安装了python

# window
**下载两个功能模块**

可以在地址[xlutils](https://pypi.python.org/pypi/xlutils/1.7.1)

**解压你的文件到某个目录**
比如我的是在`C:\Users\zwx318792\Downloads\xlutils-1.7.1\xlutils-1.7.1`。然后进入`cmd`模式，进入到这个目录下，执行如下命令 

```
setup.py install
```

# ubuntu

直接执行如下命令即可

```
sudo pip install xlrd(xlwt)
```
