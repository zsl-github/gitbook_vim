---
title: git基础知识
date: 2017-01-28 19:40:54
tags: git
categories:
    - 基础知识
    - 
password: 
---


今天继续学习`git`的基本知识，主要是存储结构

我们先创建一个文件夹，然后创建`git`管理

```bash
mkdir gitTest
cd gitTest
git init
```

然后我们进入到`.git`看看都有什么

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/git_jichu_init_1.png)

然后我们创建一个文件，并添加到缓存区，看看变化

```bash
echo "aaaaaaaaaa" > test.txt

git add test.txt
```

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/git_jichu_add.png)

这个里面的`69/dd9b9dc4d109a772ad9c6fc66548f9130cd487`是一个哈希值，它表示的是文件`test.txt`中的内容，而不是文件名。为了防止在一个目录下
存储文件过多，所以`git`在保存哈希值的时候，把前两位用一个斜杠分开。
我们可以使用`git cat-file -p`来查看哈希值对应的具体内容

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/git_jichu_cat_file.png)

我们也可以通过命令 `git ls-files -s`查看当前仓库中的文件跟对应的 哈希值
```bash
zsi1989u@zsi1989u:~/gitTest$ git ls-files -s
100644 69dd9b9dc4d109a772ad9c6fc66548f9130cd487 0       test.txt
```
我们可以通过`git hash-object`来查看一个文件对应的哈希值

```
zsi1989u@zsi1989u:~/gitTest$ git hash-object test.txt
69dd9b9dc4d109a772ad9c6fc66548f9130cd487

```
