---
title: 切屏操作之tmux
date: 2017-01-13 19:51:21
tags: 工具
categories:
    - tmux
    - 
password: 
---

---

# 背景
经常使用`vim`,少不了在`vim`跟终端之间进行切换，虽然`Ctrl+z`跟`fg`可以实现此目的，但是还是很麻烦。尤其是当你需要根据终端的搜索结果去在`vim`中进行操作的时候，往往要来回切好几次，才能记住自己所需要的信息。如果可以显示为左边是编辑窗口，右边是终端，那么将极大的提高我们的工作效率

# TMUX

tmux可以实现分屏操作，完全能满足自己上述的需求

**安装**

ubuntu下直接使用命令行安装就可以了

```bash
sudo apt-get install tmux
```

# 配置

**更改前缀**

执行`tmux`相关的操作，都需要一个前缀引导，更改前缀引导的方法如下
在配置文件`.tmux.conf`中添加如下内容(如果没有，请在根目录下创建此文件)

```bash
#设置前缀为Ctrl + a
set -g prefix C-a

#解除Ctrl+a 与前缀的对应关系
unbind C-b
```
**设置vim模式**
在`tmux`中如果想使用`vim`模式，主要是用在复制、粘贴的时候，可以使用`vim`那样移动跟选中

```bash
#copy-mode 将快捷键设置为vi 模式
setw -g mode-keys vi
```
**更改底部状态栏显示**

默认的底部显示如下

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/tmux_bottom_bar.png)

添加如下内容更改样式

```bash

# 背景跟字体颜色
set -g status-bg black
set -g status-fg white

# 对齐方式-窗口名居中显示
set-option -g status-justify centre

# 左下角-会话名称显示颜色跟距离
set-option -g status-left '#[bg=black,fg=green][#[fg=cyan]#S#[fg=green]]'
set-option -g status-left-length 20

# 窗口名称显示颜色
setw -g automatic-rename on
# 默认颜色
set-window-option -g window-status-format '#[dim]#I:#[fg=dim]#W#[fg=grey,dim]'
# 当前窗口颜色
set-window-option -g window-status-current-format '#[fg=cyan]#I#[fg=cyan]:#[fg=cyan]#W#[fg=green]'

# 右下角时间显示
set -g status-right '#[fg=green][#[fg=cyan]%Y-%m-%d#[fg=green]]'

```
修改后的效果图
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/tmux_bottom_bar_change.png)


# 常用命令

## 会话操作
session指的是按下tmux命令后 存在的连接便是session

**tmux new -s**
这个命令是用来创建一个会话，后面跟名称。一个终端窗口可以创建多个会话，但是必须在非会话来创建新会话。在会话中可以使用`ctrl+a`来引导命令，然后进行相关操作

**Ctrl+a :kill-session**
用来关闭当前会话的，跟`exit`作用差不多

**ctrl+a d**
临时退出会话，会话并没有被销毁

**tmux a -t**
进入已经存在的会话，后面跟会话名

**tmux ls**
列出所有会话

**tmux kill-session -t**
删除指定会话，后跟会话名

**tmux kill-server**
删除所有会话

**ctrl+a s**
列表切换会话

## 窗口
window在session里，可以有N个window，并且window可以在不同的session里移动

**Ctrl+a c**
创建window

**Ctrl+a &**
删除window

**Ctrl+a n**
下一个window

**Ctrl+a p**
上一个window

**Ctrl+a ,**
重命名window

**Ctrl+a f**
在多个window里搜索关键字

**Ctrl+a l**
在相邻的两个window里切换

**Ctrl+a .**
修改窗口编号，可以间接调换窗口的顺序

## 面板

pane可以分割窗口，方便进行多窗口工作

**Ctrl+a z**
最大化当前面板，其余面板会隐藏。当再次运行此命令的时候，恢复之前状态

**Ctrl+a x**
删除当前面板

**Ctrl+a %**
建立竖向分割面板

**Ctrl+a "**
建立横向分割面板

**Ctrl+a o**
在面板之间移动

**Ctrl+a 方向键**
面板间切换

# 遇到问题

**brew报错**
在进行使用系统粘贴板操作的时候，需要执行一个命令
```bash
brew install reattach-to-user-namespace
```
会有如下的提示报错信息

```
gcc-5: error: unrecognized command line option ‘-arch’
```
**解决方法**
后来才发现，这个安装是`mac`系统下才需要的，`ubuntu`下根本不需要
