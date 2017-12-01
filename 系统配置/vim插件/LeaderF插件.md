---
title: LeaderF插件使用
date: 2017-01-30 19:44:15
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---

# 如何安装LeaderF插件
1. 直接下载模块[LeaderF](https://github.com/Yggdroot/LeaderF)

2. 直接在`vundle`中添加如下内容进行安装
```vim
Bundle 'Yggdroot/LeaderF'
```
# 常用设置

此插件使用默认配置就行了，唯一需要更改的可能就是定义快捷键了

```vim
" 打开文件搜索窗口
let g:Lf_ShortcutF = '<C-P>'

" 打开buffer 列表
let g:Lf_ShortcutB = '<C-B>'
```
# 基本命令
通过配置上面的快捷键， 此插件就有两个最基本的命令

**1.C-P**
执行此命令以后，会列表显示当前目录下的文件

**2.C-B**
在所有打开的`buffer`文件中进行搜索

# 辅助命令

就是在索引结果中的功能键

```
<C-C> : quit from LeaderF.
<C-R> : switch between fuzzy search mode and regex mode.
<C-F> : switch between full path search mode and name only search mode.
<ESC> : switch to normal mode.
<C-V>, <S-Insert> : paste from clipboard.
<C-U> : clear the prompt.
<C-J>, <Down>, <C-K>, <Up> : navigate the result list.
<2-LeftMouse> or <CR> : open the file under cursor or selected(when
        multiple files are selected).
<C-X> : open in horizontal split window.
<C-]> : open in vertical split window.
<C-T> : open in new tabpage.
<F5>  : refresh the cache.
<C-LeftMouse> or <C-S> : select multiple files.
<S-LeftMouse> : select consecutive multiple files.
<C-A> : select all files.
<C-L> : clear all selections.
<BS>  : delete the preceding character in the prompt.
<Del> : delete the current character in the prompt.
<Home>: move the cursor to the begin of the prompt.
<End> : move the cursor to the end of the prompt.
<Left>: move the cursor one character to the left.
<Right> : move the cursor one character to the right.
<C-P> : preview the result.
```

# 常用技巧

1. 在提示符面板内可以使用 <c-f> 来切换搜索模式
- 按照文件名搜索
- 按照文件路径搜索

2. 使用<c-b>可以用来查看当前打开的所有文件，对于跟踪代码非常便利，因为经常需要再各个文件中跳转

3. 在提示符面板内可以使用 <c-r> 来切换是否使用正则表达是搜索
- 顺序搜索
- 正则表达式搜索


# 加快速度

**使用ag搜索**
这个插件调用的是系统的搜索工具，默认的使用顺序是`rg` `pt` `ag` `find` 
所以，系统安装一个`rg`或者`ag`，那简直是如虎添翼，速度会提升很多。


# 配合插件
此插件在看源码的时候，主要是用来在某个模块目录下进行文件查找,这时候可以配合插件`CtrlSF`,这个插件是用来搜索的，它支持搜索当前文件或者指定路径下的字串，可以实现
方法等的搜索，虽然效率比不上`cscope`，但是还是比来回切到终端搜索要快捷的多

