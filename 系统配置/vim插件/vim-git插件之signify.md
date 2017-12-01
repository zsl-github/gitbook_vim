---
title: vim git相关插件之signify
date: 2017-02-13 20:42:01
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


# 背景

今天在网上查找相关插件的时候，偶然发现自己的配置中竟然没有`git`相关的的配置。当然，没有此相关配置的原因，是因为自己还没有遇到需要此插件的场景。
不过，今天在网上看到了`signify`插件，插件功能中描述说它可以显示文件的变化，因为自己在修改代码的时候，可能会想知道自己更改了哪些内容，所以，
这个插件应该还是有些功能可以使用的，就此安装体验下

**补充**
这个插件也用了有两个多月了,平日里使用主要是做界面提示用,并没有用到对`git`的操作.不过,这也正是这个插件的核心功能,它并没有像另一个插件
`fugitive`，这个插件主要功能就是在`vim`中进行`git`的操作，二者结合起来基本上就能满足`git`的操作了

# 插件signify

## 如何安装

**直接下载安装**

直接在网上下载安装 [signify](https://github.com/mhinz/vim-signify),然后拷贝到`vim`插件目录即可

**通过vundle安装**

在`.vimrc`中添加如下内容

```vim
Bundle 'mhinz/vim-signify'
```

## 如何配置

此插件几乎不用什么配置，默认配置就能满足需求
不过，里面还是有两个属性比较重要

** 1.signify_line_highlighti **

这个属性是用来设置变更的行是否高亮，默认情况下，更改行只会在侧边列添加符号标识，设置了这个属性后，整行就会高亮

** 2.signify_vcs_list **

打开文件，都要判断此文件的版本控制类型(比如是git还是svn)，默认情况是各个版本控制工具进行遍历，导致的结果就是影响载入时间。用这个属性可以
指定判断类型，除了制定的版本控制工具，其他的不在尝试去判断

## 常用命令

使用这个插件主要是用到它的标识功能.当处于`git`跟踪的文件发生改动的时候,修改的地方会自动添加符号标识

修改前
![修改前](https://github.com/zsl-github/blog/raw/master/source/picture/git_signify_xiugai_q.png)

修改后
![修改后](https://github.com/zsl-github/blog/raw/master/source/picture/git_signify_xiugai_h.png)

各个字符的意义

**+**
新增行

**!**
修改行

**_1**
删除的行数

**99**
than 9, the `_` will be omitted. For numbers larger than 99, `_>`

**_>**
will be shown instead.

**!1**
This line was modified and a number of lines below were deleted.

**!>**
It is a combination of `!` and `_`. If the number is larger than 9,

**!>**
will be shown instead.

**‾**
The first line was removed. It's a special case of the `_` sign.

如下几个命令提供了变更位置跳转指令

```

]c   Jump to next hunk.
[c   Jump to previous hunk.

]C   Jump to last hunk.
[C   Jump to first hunk.

```
## vimrc 配置

```vim

" 高亮变更行
let g:signify_line_highlight = 1
" 检查版本控制工具
let g:signify_vcs_list = [ 'git', 'svn' ]

```
