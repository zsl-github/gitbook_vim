---
title: python入门
date: 2017-03-02 21:14:58
tags: python
categories:
    - 基础知识
    - 
password: 
---

今天正式开始学习python，先写一个最今经典的例子 `helloword`

```python
#!/usr/bin/python3.2
print("hello work!")
```

# 知识点

> #!usr/bin/python3.2

我们知道`python`有两种执行方式

1. 用`python` 命令执行`.py`文件

```
python filename
```

如果你环境下有多个`python`版本，可以选择

```
python3.2 filename
```

2. 给你的`.py`文件添加权限`chmod a+x filename`,利用`./filename` 执行

这时候就是`#!usr/bin/python`发挥作用的时候了。它是告知系统，去这个目录下寻找运行这个`.py`文件的`python`，如果目录下没有相应的`python`，运行就会报错

```
bash: ./helloword.py: usr/ddd/python2.7: bad interpreter: No such file or directory
```

如果你在代码里加入了中文，当你保存的时候，编译器会提示你添加中文编码，如下

```
#!/usr/bin/python2.7
# -*- coding: utf-8 -*-
print ("hello work!")
print ("你好吗")
```
