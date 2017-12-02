---
title: vim备份插件之sessionman
date: 2017-05-06 15:03:54
tags: vim
categories:
    - plugin
    - 
password: 
---

这个插件是用来对编辑界面进行备份的，比如你打开的`buffer`，添加的`marks`等。备份后下次你就而已直接通过命令来打开。这对于我们经常进行的源码分析作用比较大，比如你正跟踪通话流程，然后忽然有一个紧急的工作，需要你暂停当前的动作，去重新打开终端，跟踪另外一段代码。这时候你就可以把现在的工作进行备份，等有时间的时候，直接打开备份，就可以继续之前的工作了。

# 插件安装

这个插件比较简单，直接下载或者使用`vundle`都行

```vim
Bundle 'sessionman.vim'
```

# 常用命令

这个插件一共有如下命令

```
:SessionOpen 打开备份

:SessionOpenLast 打开备份列表

:SessionClose 关闭备份

:SessionSave 保存备份

:SessionSaveAs 重命名

:SessionShowLast 显示上个备份
```

我们只需要掌握如下两个基本命令即可

```vim
"查看所有的会话列表
nmap <Leader>ma :SessionList<CR>
"保存一个会话列表
nmap <Leader>ms :SessionSave<CR>

```

