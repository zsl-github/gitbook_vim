---
title: git 中文显示乱码
date: 2017-02-25 20:57:28
tags: 系统配置
categories:
    - 知识累积
    - 
password: 
---

# git 中文显示乱码

今天在使用git 的过程中，发现git仓库下，如果有中文文件名，这时候执行
> git status .

会出现如下的内容

```
"Markdown/MD\346\217\222\344\273\266\345\256\211\350\243\205.md"
"YCM/YCM\347\232\204\345\256\211\350\243\205.md"

```

后来在网上查找了解决方法，只要进行如下操作就可以了
> git config --global core.quotepath false

当然，你也可以直接在.gitconfig中添加如下内容

```
[core]
	quotepath = false
```

这时候在执行

> git status .

结果如下
```
    Markdown/Markdown.md -> Markdown/MD插件安装.md
    YCM/YCM.md -> YCM/YCM的安装.md
```

