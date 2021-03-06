---
title: vim 跨文件复制
date: 2017-05-10 15:34:26
tags: vim
categories:
    - 高级知识
    - 
password: 
---

我们都知道，当我们在一个文件之间进行复制粘贴的时候，vim提供给我们的方法非常多，三个模式下都有方法实现字符、句子、段落之间的复制粘贴。当时，如果我们想要在两个文件之间进行复制粘贴，这就有点麻烦了。我之前常用的方法是分窗口实现。这个的缺点就是，每次都打开连个文件。更关键的是，如果两个文件>目录相差太多，打开的时候非常的麻烦。而且，这个方法，不适合操作那些需要root权限的文章。还有就是使用ctrl+shift+c跟ctrl+shift+c来进行复制粘贴操作>。这个方法的弊端就更清楚了，因为会存在格式缩进等问题。

今天我在网上看到了一个方法，虽然这个方法操作起来也挺麻烦的。不过，应该会比上面的两个方法好些吧。首先看实现方法。这个方法我只是在ubuntu下的终端>上操作成功，至于window上的gvim就不知道了

1. 保证你的电脑上安装了vim-gnome.如果没有安装，你执行gvim命令，系统会提示你可能需要的几个vim相关软件。你直接使用

``` 
sudo apt-get install vim-gnome
```
安装就行了

2. 然后我们就可以下面的命令进行复制粘贴了

```
"+y（依次点"->+->y）-复制
"+d（依次点"->+->d）-剪切
```

上面两个是在可视模式下。如果是普通模式下，那么就要用"+yy了。

```
"+p（依次点"->+->p）-复制
```

其实，这个命令跟我们的yy p等当个文件内复制是很相似的。如果我们把"+这个操作当做是选择寄存器就比较好理解了。yy是使用了默认的寄存器，我们这个则是使用了+寄存器。选择寄存器以后跟着的就是功能操作，所以，我们在当个文件内的操作，也可以在"+上使用，只是前面增了了一个寄存器而已。+这个寄存器，感觉应该是系统寄存器

当然，上面每次都要去引用下系统剪切板很麻烦，我们可以在`.vimrc`中进行快捷键定义，如下

```vim
" 设置快捷键将选中文本块复制至系统剪贴板
vnoremap <Leader>y "+y

" 设置快捷键将系统剪贴板内容粘贴至 vim
nmap <Leader>p "+p
```
