---
title: git-commit预设提交注释
date: 2017-01-02 09:52:04
tags: git
categories:
    - 常用配置
    -
password:
---


做项目提交`gerrit`的时候，有规定的注释格式，如果每次提交的时候都要手动或者复制过来，将会非常都麻烦，`git`已经考虑到了这点，我们可以通过如下方法来预制注释信息，这样当我们执行`git commit`的时候，预制的信息就会自动显示在提交界面了

## 1.创建一个文件，里面添加自己预制信息

```
vim git_msg
```
里面内容加入如下内容

```
test1
test1
test1
```

## 2.在.gitconfig中预制此文件

在`.gitconfig`中添加如下内容

```
[commit]
    template=git_msg
```

然后你在执行`git commit`的时候，提交界面就可以看到
```
test1
test1
test1
```

## 思考

这个方法有点不好，就是注释是固定的，如果我们需要对不同项目添加不同提交，或者注释中有变化的内容，如时间，这就不太方便了。我更加推荐通过`snippet`的方式，通过添加代码块来实现。具体方法

### 1.在~/.vim/UltiSnips 中创建all.snippets

### 2.在上面的文件中添加如下内容

```
snippet ctm "git commit message"
[describe] : ${1:modify}
[project]   : ${2:aaaa}
[owner]    : bbbb
[date]       : `!v strftime("%Y%m%d")`
[bug_id]   : ${0} 
endsnippet
```
这时候在`git`提交界面输入`ctm`就可以快速插入注册信息了

当然，前提是你已经安装了`ultisnips`.至于此插件相关信息，可看另一篇文章[ultisnips插件的安装跟使用](http://zsl1989.com/2017/04/15/vim%E6%8F%92%E4%BB%B6/ultisnips%E6%8F%92%E4%BB%B6/)
