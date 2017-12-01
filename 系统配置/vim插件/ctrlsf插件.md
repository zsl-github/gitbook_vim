---
title: ctrlsf插件
date: 2017-01-25 23:38:05
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---
title


# 插件介绍

这个插件是用来搜索字串的，正如作者的介绍，这个插件是`ack.vim` `ag.vim`的升级替代品。先不说搜索速度，它的搜索结果显示更加友善合理，这也是我最关注的一点，毕竟这个插件的使用场景主要是文件较少的模块以及当前文件中查找

# 插件安装

**1.下载安装**

直接下载[ctrlsf](https://github.com/dyng/ctrlsf.vim)插件，拷贝到插件对应目录下

**2.vundle安装**

```
Bundle 'dyng/ctrlsf.vim'
```

# 常用配置

** 1.ctrlsf_contxst **
这个插件是用来设置搜索结果显示上下行数的，比如你设置为

```
let g:ctrlsf_context = '-B 5 -A 3'
```

搜索结果就会同时显示匹配行前5行跟后三行

** 2.ctrlsf_extra_backend_args **

设置调用搜索工具的搜索参数，例如

```
let g:ctrlsf_extra_backend_args = {
    \ 'pt': '--home-ptignore'
    \ }
```

** 3.ctrlsf_ignore_dir **

设置要忽略的搜索目录

```
let g:ctrlsf_ignore_dir = ['bower_components', 'node_modules']
```

** 4.ctrlsf_position **

设置搜索结果界面的显示位置，有四个值 `top` `right` `bottom` `left`

```
let g:ctrlsf_position = 'bottom'
```

** 5. ctrlsf_selected_line_hl **

高亮匹配行

```
let g:ctrlsf_selected_line_hl = 'op'
```

** 6.ctrlsf_winsize **

设置搜索结果框高度，默认是屏幕高度一半，可以通过百分比修改

```
let g:ctrlsf_winsize = '70%'
```

# 常用命令

命令格式
CtrlSF [arguments] {pattern} [path]

## :CtrlSF string 
搜索string

## :CtrlSF {pattern} /path/to/dir
搜索指定目录内容

## :CtrlSFOpen
打开上次的搜索结果

##  CtrlSF -I {pattern}
忽略大小写

## CtrlSF -A 3 -B 1 {pattern}
上下文行数

## CtrlSF -G .cpp {pattern}
指定搜索文件格式

## CtrlSF -W foo
全词匹配

```
<Plug>CtrlSFPrompt      Input 'CtrlSF' in command line and waiting, just a handy shortcut.

<Plug>CtrlSFVwordPath    Input current visual selected word in command line and waiting for any other user input.

<Plug>CtrlSFVwordExec    Similar to above, but execute it immediately.

<Plug>CtrlSFCwordPath    Input word in the cursor in command line and waiting.

<Plug>CtrlSFCwordExec    Similar to above, but execute it immediately.

<Plug>CtrlSFCCwordPath   Similar to <Plug>CtrlSFCwordPath but will add word boundary around searching word.

<Plug>CtrlSFCCwordExec   Similar to above, but execute it immediately.

<Plug>CtrlSFPwordPath    Input last search pattern in command line and waiting.

<Plug>CtrlSFPwordExec    Similar to above, but execute it immediately.

Maps by default in CtrlSF window:

<Enter>, <o>, <2-LeftMouse> Open file which contains the line under cursor.

<C-O>          Open file in a horizontally split window.

<p>            Open a preview window to view file.

<P>            Open a preview window to view file and switch focus to it.

<O>            Like <o>, but always leave CtrlSF window open.

<T>            Like <t>, but focus CtrlSF window instead of opened new tab.

<q>            Quit CtrlSF. Also close preview window if any.

<C-J>          Move cursor to next match.

<C-K>          Move cursor to previous match.

Maps by default in preview window:

<q>            Quit preview mode.

```

# 快捷键及配置

```vim

" 进入搜索面板
nmap     <C-F>f <Plug>CtrlSFPrompt
" 进入搜索面板，并预先输入选择的内容
vmap     <C-F>F <Plug>CtrlSFVwordPath
" 进入搜索面板，直接搜索选择的内容
vmap     <C-F>f <Plug>CtrlSFVwordExec
" 进入搜索面板，并预先输入光标下的内容
nmap     <C-F>N <Plug>CtrlSFCwordPath
" 进入搜索面板，并搜索光标下的内容
nmap     <C-F>n <Plug>CtrlSFCwordExec
" 打开上次的搜索结果
nnoremap <C-F>o :CtrlSFToggle<CR>
" 面板显示位置
let g:ctrlsf_position = 'bottom'
" 高亮匹配行
let g:ctrlsf_selected_line_hl = 'op'
" 面板高度
let g:ctrlsf_winsize = '70%'

```


