---
title: vunble插件安装跟使用
date: 2017-02-12 20:41:07
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---

# 插件安装

1. 直接点击[vundle](https://github.com/gmarik/vundle)下载`vundle`插件

2. `.vim`下创建一个`bundle`目录，紧接着把`vundle`放进去就行了。还有一种更加方便的方法，就是直接使用如下的命令

```git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim```

3. 安装好以后，在你的`.vimrc`中加入如下内容

```vim
""""""""""""""""""""""""""""""
"vundle.vim
"可以管理、安装、卸载你的插件
""""""""""""""""""""""""""""""

"似乎这里是必须的,后面会重新打开
filetype off

"设置运行跟初始化Vundle的路径
set rtp+=~/.vim/bundle/vundle/

"设置插件的安装目录,默认是vundle
"call vundle#begin('~/some/path/here')
"所有的安装插件都要在begin 跟 end
call vundle#begin()

    "添加Vundle到Vundle管理列表
    Bundle 'gmarik/vundle'

    """""""""""""""""""""""""
    "在Github其他用户下的repos, 需要写出”用户名/repos名
    """""""""""""""""""""""""
    Bundle 'tpope/vim-commentary'

    """""""""""""""""""""""""
    "在Github vim-scripts 用户下的repos,只需要写出repos名称
    """""""""""""""""""""""""
    "查看当前文件目录，不是以侧边栏显示，不过感觉没有nerdtree有用
    "Bundle 'bufexplorer'

    """""""""""""""""""""""""
    "不在Github上的插件，需要写出git全路径
    """""""""""""""""""""""""
    Bundle 'git://git.wincent.com/command-t.git'

    "对应上面的begin
    call vundle#end()

    "对应上面的filetype off
    filetype plugin indent on

```
# 常用的命令
```
    " Brief help
    " :BundleList          - list configured plugins
    " :BundleInstall(!)    - install (update) plugins
    " :BundleSearch(!) foo - search (or refresh cache first) for foo
    " :BundleClean(!)      - confirm (or auto-approve) removal of unused plugins
```
