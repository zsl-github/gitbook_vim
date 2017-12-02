---
title: vim帮助文档汉化
date: 2017-05-07 15:15:30
tags: vim
categories:
    - 基础知识
    - 
password: 
---

我们在使用vim 的时候，经常会碰到一些陌生的命令，这时候我们可以通过

```
:help **
```

来查询一些命令的使用方法。不过呢，我们安装的vim默认的都是英文的，看起来很费劲，尤其是向我这样英语比较差的人。今天在网上查找了一个安装中文帮助文档的方法，记录如下进入下面网址

[vim帮助文档汉化](http://vimcdoc.sourceforge.net/)

下载中文文档

下载界面如下

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/vim_help_cn.png)

在这里面选择相应系统的文档，我电脑是ubuntu系统，选择的就是第三个(tarball)那个，然后进入下载界面下载就好了

然后我们解压一下，解压以后的文件夹里面有一个vimcdoc.sh 文件，然后我们执行这个文件就行了

```
sudo ./vimcdoc.sh -i
```

我们再次打开帮助的时候，就是中文了
