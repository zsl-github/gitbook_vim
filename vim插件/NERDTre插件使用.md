---
title: NERD Tre 插件使用
date: 2017-02-01 20:04:25
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


# 背景
这个插件是用来树状显示文件列表的，因为自己工作的环境是`android`源码，工作目录都是在源码根目录，在根目录树状显示文件层级，几乎没有什么作用，所以
很长一段时间，我都是把这个插件束之高阁的。不过，最近在写总结的时候，因为希望可以快速找到自己之前写过的东西，这个插件的作用就显示出来了。比如，
我想找之前总结的一个文档，我需要在之前汇总的联系人信息文档中查找某个人的号码，项目信息需要确认下等，这时候都需要能快速的找到自己的文档。所以，
这个插件最近用的多了起来，而且发现，如果你仅仅是在一个层级比较少的目录下工作，这个插件还是很有用的

# 插件安装

**直接在网上下载插件**
[nerdtree](https://github.com/scrooloose/nerdtree)


**用vundle安装**

```vim
Bundle 'scrooloose/nerdtree'
```

# 使用

这个插件目前只用到最基本的功能，因为此功能已经能满足我的需求了。在`.vimrc`中添加快捷启动按键

```vim
nmap <Leader>fl :NERDTreeToggle<CR>
```

常用的命令如下

```bash

o.......Open files, directories and bookmarks
go......Open selected file, but leave cursor in the NERDTree
i.......Open selected file in a split window
gi......Same as i, but leave the cursor on the NERDTree
s.......Open selected file in a new vsplit
gs......Same as s, but leave the cursor on the NERDTree
O.......Recursively open the selected directory
x.......Close the current nodes parent
X.......Recursively close all children of the current node
e.......Edit the current dir

<CR>...............same as |NERDTree-o|.

D.......Delete the current bookmark

P.......Jump to the root node
p.......Jump to current nodes parent
K.......Jump up inside directories at the current tree depth
J.......Jump down inside directories at the current tree depth
<C-J>...Jump down to the next sibling of the current directory
<C-K>...Jump up to the previous sibling of the current directory.|NERDTree-C-K|

C.......Change the tree root to the selected dir
u.......Move the tree root up one directory
U.......Same as 'u' except the old root node is left open
r.......Recursively refresh the current directory
R.......Recursively refresh the current root
m.......Display the NERD tree menu
cd......Change the CWD to the dir of the selected node
CD......Change tree root to the CWD

I.......Toggle whether hidden files displayed
f.......Toggle whether the file filters are used
F.......Toggle whether files are displayed
B.......Toggle whether the bookmark table is displayed

q.......Close the NERDTree window
A.......Zoom (maximize/minimize) the NERDTree window
```
