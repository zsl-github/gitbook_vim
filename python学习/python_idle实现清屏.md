---
title: python idle 实现清屏
date: 2017-03-17 16:56:28
tags: python
categories:
    - 技巧
    - 
password: 
---

最近在学习python的时候，需要用到ubuntu的python idle。这个工具可以测试python语法。但是呢，在使用的过程中遇到了一个问题。就是随着你的输入，你会发现这个输入会停留在这个界面的最低部分，让我们开起来非常的不舒服。我的理想情况，肯定是像在ubuntu的终端一样，有一个clear命令。不过，在网上查了下，发现idle本身并没有提供清屏的操作。然后就在网上查到了一个解决方法，这里记录下。

# 下载插件
首先我们需要下载一个功能扩展文件[ClearWindow.py](http://bugs.python.org/file14303/ClearWindow.py),个人理解应该是类似于插件吧。进入链接后，是一个文件。

```python
"""

Clear Window Extension
Version: 0.2

Author: Roger D. Serwy
        roger.serwy@gmail.com

Date: 2009-06-14

It provides "Clear Shell Window" under "Options"
with ability to undo.

Add these lines to config-extensions.def

[ClearWindow]
enable=1
enable_editor=0
enable_shell=1
[ClearWindow_cfgBindings]
clear-window=<Control-Key-l>


"""

class ClearWindow:

    menudefs = [
        ('options', [None,
               ('Clear Shell Window', '<<clear-window>>'),
       ]),]

    def __init__(self, editwin):
        self.editwin = editwin
        self.text = self.editwin.text
        self.text.bind("<<clear-window>>", self.clear_window2)

        self.text.bind("<<undo>>", self.undo_event)  # add="+" doesn't work

    def undo_event(self, event):
        text = self.text

        text.mark_set("iomark2", "iomark")
        text.mark_set("insert2", "insert")
        self.editwin.undo.undo_event(event)

        # fix iomark and insert
        text.mark_set("iomark", "iomark2")
        text.mark_set("insert", "insert2")
        text.mark_unset("iomark2")
        text.mark_unset("insert2")


    def clear_window2(self, event): # Alternative method
        # work around the ModifiedUndoDelegator
        text = self.text
        text.undo_block_start()
        text.mark_set("iomark2", "iomark")
        text.mark_set("iomark", 1.0)
        text.delete(1.0, "iomark2 linestart")
        text.mark_set("iomark", "iomark2")
        text.mark_unset("iomark2")
        text.undo_block_stop()
        if self.text.compare('insert', '<', 'iomark'):
            self.text.mark_set('insert', 'end-1c')
        self.editwin.set_line_and_column()

    def clear_window(self, event):
        # remove undo delegator
        undo = self.editwin.undo
        self.editwin.per.removefilter(undo)

        # clear the window, but preserve current command
        self.text.delete(1.0, "iomark linestart")
        if self.text.compare('insert', '<', 'iomark'):
            self.text.mark_set('insert', 'end-1c')
        self.editwin.set_line_and_column()

        # restore undo delegator
        self.editwin.per.insertfilter(undo)

```
直接粘贴复制，命名为一个`ClearWindow.py`文件就行了

# 使用插件
把我们的这个文件复制到你`python`安装目录下的`idlelib`目录下。例如，我的目录如下

```
/usr/lib/python2.7/idlelib
```

# 更改config-extensions.def 内容

在上面的那个目录下，我们可以找到一个 config-extensions.def文件。我们在这个文件下增加如下的内容

```
#引入脚本
[ClearWindow]
enable=1
enable_editor=0
enable_shell=1
[ClearWindow_cfgBindings]
#设置快捷键
clear-window=<Control-Key-l>
```

然后我们重新启动idle。你就会发现在options菜单下，发现一个Clear Shell Window Ctrl l的新选项。这个就可以用来实现清屏了。

一定要注意。当我们在 config-extensions.def 中添加内容的时候，里面的clearwindow一定要跟你拷贝进去的py文件名一样。我就是当时因为没有注意这点，py文件的名字是小写，里面添加的时候，名字是大写，结果导致没有效果。

