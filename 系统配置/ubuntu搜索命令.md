---
title: ubuntu搜索命令-rg、ag、ack、grep对比
date: 2017-01-22 09:58:40
tags: 工具
categories:
    - 搜索
    - 
password: 
---


之前写过一个博客，对`ag` `ack` `grep` 的搜索速度做过对比，自从使用了自己搭建的博客以后，之前的那个博客就不用了，先把之前的博客内容附上

# 2015-11-15 内容

今天在网上搜索关于grep的用法的时候，忽然看到其他更加高效的搜索方式。当然它们应该不存在所谓的谁更好。关键的是，在不同的场合使用不同的搜索命令，可以提高效率。其实，因为自己也没有使用过其他两种方法，目前还不能妄下结论。也就是参考网上的介绍，初步体验下罢了，等过一段时间，有了自己的心得体会再说吧。

这三个搜索命令，在终端下都是可以使用命令行进行安装的。其中，grep是随着ubuntu发布的，本身就有。其他两个，我们只需要在终端操作ack ag,系统就会给我们提示安装包跟安装命令，我们只需要按着提示操作就行了。至于使用方法，还是需要以后自己多多发掘的，可以借助help命令。

现在，我主要是想根据网上的介绍，研究一下这三个命令在庞大操作目录下的执行效率。验证的环境是在～ 目录下，执行的操作是寻找所有java字符串出现的地方。验证结果如下

1. ag

```
time ag java
```

结果

```
real    1m14.938s
user    0m2.410s
sys    0m2.757s
```

2. ack

```
time ack-grep java
```
结果

```
real    1m6.235s
user    0m18.153s
sys    0m1.766s
```

3. grep

```
time grep -rn java .
```

结果

```
real    2m20.380s
user    0m6.482s
sys    0m3.779s
```

结论：

看起来ack跟ag执行效率差不多（网上别人做的实验，ag要比ack块好多，不知道为什么），而且，他们的结果显示更加清晰明了。

grep就比较慢了

# 2017-01-22

最近又在网上看到，说`ripgrep`的搜索速度比`ag`更快，很多插件后台依赖的系统搜索工具默认顺序也是先`ripgrep`后`ag`，所以就安装了下做下对比。结果如下


![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/rg_ag_search.png)

不过，从上的对比结果来看，并没有快多少。还是先使用一段再说吧


**安装方法(ubuntu 16.06)**

1. 确保系统已经安装了`rustc`

如果没有安装，可以直接使用命令安装

```
sudo apt install rustc
```

2. 确保系统已经安装了`cargo`

如果没有安装，可以直接使用命令安装

```
sudo apt install cargo
```

3. 安装

```
cargo install ripgrep
```

4. 拷贝到`bin`目录

执行完步骤3，会在~/下生成一个`.cargo`目录，里面有个`bin/rg`文件，把这个文件拷贝到`~/bin`目录下即可

```
cp rg ~/bin
```

这时候就可以在任意目录执行`rg`命令进行搜索了

## 2017-10-25补充

安装了`rg`跟`ag`，但是没有感觉两者谁更快，反而感觉还是`ag`更习惯些，毕竟使用的时间更久，命令也更熟悉些。今天又整理了下两者常用命令，这里记录下

### AG

#### 忽略搜索文件目录

> ag --ignore-dir out string

`--ignore-dir`是用来忽略所有目录的，执行这条命令以后，会忽略所有目录包括其他目录下子目录的`out`。
比如有如下三个目录
- out/out1/out1/test1.txt
- out2/out2/test2.txt
- out3/out3/out/test3.txt

执行命令后，会同时忽略整个`out`以及`out3/out33/out`这两个目录

如果你仅仅想忽略当前目录下的`out`目录，可以使用如下方式

```
ag --ignore-dir ./out string
```

对于忽略文件夹，还有另一种方式，就是在`home`下添加`.agignore`文件，里面添加你想要屏蔽掉的文件夹目录

```
vim .agignore
./out //忽略文件夹
./filename //忽略文件
:wq
```
这时候你执行`ag string`就会自动忽略`out`目录了


#### 搜索制定类型文件

> ag --xml string

这个搜索的话，只会搜索`xml`类型的文件。如果想要看之所的文件类型，可以执行命令

```
ag --list-file-types
```
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/ag_file_types.png)

### RG

#### 忽略搜索文件目录

需要在搜索目录添加文件`.rgignore`，具体规则如下

- 不加！表示忽略
- 加！表示不忽略

```

/*            //忽略整个目录
!/source      //不忽略此文件目录
!_config.yml  //不忽略此文件
!.gitignore
next_config.diff //忽略此文件
```

### 17-11-07补充

最近在源码下搜索的时候，发现在几百万个文件下搜索还是有点吃力的，虽然`rg`跟`ag`已经非常快了，但还是满足不了秒搜的要求,后来就考虑只能靠忽略无效目录跟无效文件了实现了.
因为两者的速度没有明显的差别(至少在我使用过程中没有明显感到谁比谁更快)，就只能看谁对筛选的过滤更有优势来进行选择了。对比了下，发现`ag`能做的`rg`都能做，但是有一点`rg`
比`ag`更有优势，就是在忽略文件中，`rg`可以同时配置忽略某文件或目录，不忽略某文件或内容，`ag`似乎只能做到忽略某文件或目录，这样，`rg`就能够实现一个功能--只搜索指定的文件目录。比如，在源码目录下，我只想搜索`packages`目录，就可以在根目录下配置如下内容

```
vim .rgignore
/*
!/packages

```
这样就可以明显提高搜索速度，而且，当你进入二级目录搜索时，这个`.rgignore`文件就没有作用了，非常灵活。以后应该就只用`rg`了，相似的工具，熟练掌握其中一个就行了

**rg常用命令**

> rg string

在当前目录下搜索`string`内容

> rg -i string
在当前目录下搜索`string`内容，忽略大小写

> rg string -An 
> rg string -Bn
> rg string -Cn

显示匹配行上下n行内容

```
zsl1989@zsl1989:~/test$ ag eee -A2
aa.txt
5:eee
6-fff
7-ggg
zsl1989@zsl1989:~/test$ ag eee -B2
aa.txt
3-ccc
4-ddd
5:eee
zsl1989@zsl1989:~/test$ ag eee -C2
aa.txt
3-ccc
4-ddd
5:eee
6-fff
7-ggg
zsl1989@zsl1989:~/test$

```

> rg string -c

显示文件中匹配行总数

```

zsl1989@zsl1989:~/test$ rg aaa -c
aa.txt:1
zsl1989@zsl1989:~/test$ rg zzz -c
aa.txt:2
zsl1989@zsl1989:~/test$

```
> rg string -l

只显示匹配文件名

```
zsl1989@zsl1989:~/test$ rg zzz -l
aa.txt
zsl1989@zsl1989:~/test$

```

>
