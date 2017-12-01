---
title: git命令别名设置
date: 2017-01-03 08:54:06
tags: git
categories:
    - 常用配置
    - 
password: 
---


今天在电脑上装了`git`，装完以后，使用`git st` `git add`  `git df` (这个是上家公司的工作习惯)，结果发现不能用。后来感觉到，也许是这些缩写都是需要重新配置的。第一反映就是在.bashrc里面就行配置。但是，我的bashrc就是从公司拷贝过来的，那就是说，应该不是在这里面配置的。然后我就从网上需找答案，最终找到了，原来git 本身提供了这种快捷设置命令，格式如下

```
git config --global alias.* #

```

上面的那个`*`代表的是你想要使用的替换指令`#`代表的则是系统原来自带的命令

```
git config --global alias.st status

git config --global alias.co checkout

git config --global alias.ct commit

git config --global alias.df diff

git config --global alias.br branch
```

这样就可以实现快捷命令了。

但是，另外的一个问题又出现了，就是我的`git st .` 命令以后，结果都是黑色的，也就是说，`add` 状态跟没有`add`的状态是没有区别的，然后继续网上寻找答案，找到了如下设置命令

```
git config --global color.diff auto
```

这个的意思就是，显示diff 的结果，然后是默认的。我进行了如下的默认设置

```
git config --global color.diff auto

git config --global color.status auto

git config --global color.branch auto
```

这样下来，`git`的工作环境就跟原来的一样了

进行了上面的操作以后，其实你可以在`git` 的配置文件`.gitconfig`中看到如下信息

```git
[user]
        email = zhangshuli@huaqin.com
        name = zsl
[color]
        ui = auto
[alias]
        st = status
        br = branch
        co = checkout
```

所以，我们也可以直接在`.gitconfig`中通过添加上述内容来进行快捷命令设置

