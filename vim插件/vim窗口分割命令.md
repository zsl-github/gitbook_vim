---
title: vim 窗口分割命令
date: 2017-04-05 15:33:07
tags: vim
categories:
    - 基础
    - 初级
password: 
---

如何在一个窗口下面同时打开两个以及以上的文件，有横向跟纵向两种方式

# 终端分屏打开两个文件

**横向分割显示**

```
 vim -o filename1 filename2
```

**纵向分割显示**

```
vim -O filename1 filename2
```

# vim窗口中分屏

**横向分割显示**

```
:vs filename
```
**纵向分割显示**

```
:ps filename
```

# 关闭窗口

```
:q 或 :close

//关闭除光标所在的窗口之外的其他窗口
:only

//关闭所有窗口
:qa
```

# 切换窗口

打开了多个窗口，需要在窗口之间切换时
```
ctrl + w w
```
在窗口间移动

```
ctrl + w <h|j|k|l>
```
如果我们在`.vimrc`中添加如下快捷键，移动起来就比较方便了，直接按`Ctrl+移动键`就行了

```
nmap <C-h> <C-W>h
nmap <C-j> <C-W>j
nmap <C-k> <C-W>k
nmap <C-l> <C-W>l
```
