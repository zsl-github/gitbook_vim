---
title: python学习之编程规范
date: 2017-03-04 10:02:52
tags: python
categories:
    - 基础
    - 
password: 
---

今天继续学习`python`，内容主要就是`python`编程过程中的一些规范，包括注释、换行等

**注释**

所有的注释都是以`#`开头，注释可以单独占有一行，也可以放到语句的末尾。因为`python`没有多行注释，所以，如果你注释多行，必须每一行都以`#`开头进行注释

**多行语句**

`python`的语法规则是，通过另起一行表示语句结束，如果我们想连接多行，例如字符串过长，需要多行显示。我们可以使用（\）来连接多行显示。代码如下

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
#注释的用法，采用‘#’开头，可在语句
#开始或者后面，如果是多行注释，也必须使用多个‘#’开头的语句
#也就是没有java中的块注释
print("#")
print("hello\
        word\
nihaoma")
```
需要注意的是，连接的另起一行，如果你有空格，显示的时候，空格也是存在的，执行结果如下

```
hello        wordnihaoma
>>>
```
再看如下的多行字符串

```python
string = "hello world + \
         i am comming + \
         haha"

print(string)

string = "hello world" + \
         "i am commint" + \
         "again"
print(string)
```
运行结果

```
#
hello        wordnihaoma
hello world +          i am comming +          haha
hello worldi am commintagain
>>>
```

如果语句中包含{},[],(),这时候就不再需要添加（\），如下

```python
string = {"hehe","hello"
          "world"}
print(string)
```
打印结果如下

```
set(['hehe', 'helloworld'])
>>>
```

**空行**

这个主要是为了代码的管理跟维护方便。如，我们在函数结束以后，留下一个空行在开始一个新的语句等
