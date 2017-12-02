---
title: vim状态保存跟恢复
date: 2017-05-05 14:59:07
tags: vim
categories:
    - 高级知识
    - 
password: 
---

当我们结束了一天的工作的时候，可能手头的工作仅仅进行了一半，比如我们正在用vim修改一个android 问题，我们定位了问题关键，牵扯到了好几个类，如果这时候我们直接把vim关闭了，那我们下次还要重新打开，很麻烦。其实，这时候我们可以通过如下的两条命令来保存当前的状态，并在需要的时候重新打开。

> mksession filename

进入命令行模式执行此命令，备份当前状态，缺省生成文件名为Session.vim

> source sessionfile

在vim命令行模式下，执行此命令,恢复备份

# 补充

其实有一个插件-`sessionman`，只需要记住如下两个命令，就够了

```
"查看所有的会话列表
nmap <Leader>ma :SessionList<CR>
"保存一个会话列表
nmap <Leader>ms :SessionSave<CR>
```

