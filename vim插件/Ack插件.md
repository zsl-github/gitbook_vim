---
title: vim插件之ACK
date: 2017-02-02 20:05:38
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


# 插件介绍

这个插件主要是用来进行字串搜索的，把搜索结果在`quickfix`中显示结果

# 插件安装

1. 直接使用`vundle`插件
在`.vimrc`中添加如下内容

```vim
Bundle "mileszs/ack.vim"
```

2. 直接下载
[ack](https://github.com/mileszs/ack.vim)

# 插件使用

1. :Ack
此命令是用来查找某个内容，如果后面没有指明路径，默认是`vim`工作目录
此命令的搜索结果显示在`quickfix`列表中，并跳转到第一个匹配项。
还有另两个命令：
- `Ack!`，跟前个命令的区别就是，它不会跳转到第一个匹配项
- `AckAdd`，跟前个命令的区别就是，它不会清空之前的`quickfix`列表，而是追加到其中

2. :LAck LAckAdd
跟`Ack`的区别是，存在在`location-list`中

3. :AckFile
查找匹配文件

4. :AckWindow
在当前`tab`中的所有`buffer`中搜索
LAckWindow跟次类似

# 插件配置
> let g:ackhighlight=1
这个开关比较有用，高亮显示匹配结果

> let g:ack_autofold_results = 1
是否按照文件名折叠匹配结果

> let g:ack_qhandler = "botright copen 25"
设置`quickfix`高度

> let g:ack_use_cword_for_empty_search = 0
此设置比较有用，设置为１的话可以默认搜索光标下内容

# 使用场景
这个插件目前也就发现两个可以使用的场景:
- 显示当前文件中某个字串使用处
- 判断缓存区某个字串使用处

# 备注
此插件目前使用频率比较低，因为它的搜索结果实在太糟糕了。

**2017-9-6 补充**

这个插件因为其糟糕的搜索结果显示，目前已经摒弃不用，被另一个插件`ctrlsf`取代。另一个跟`ack`相似的插件是`ag`，也不再使用

