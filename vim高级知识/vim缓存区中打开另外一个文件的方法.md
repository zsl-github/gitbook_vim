---
title: vim-缓存区中打开另外一个文件的方法
date: 2017-05-11 15:39:23
tags: vim
categories:
    - 高级知识
    - 
password: 
---

有这么一种情况：
我现在在ubuntu用户根目录下-～,我根目录下有一个文件夹blogs，这个文件夹下面有两个文件：text1,text2.我现在从～目录下进行如下操作

```
vim ~/blogs/text1
```
然后，我想在已经打开的text1中，使用命令行打开同目录下的text2,那么问题来了，你打算怎么打开呢？
我之前的做法就是，使用命令行，然后用e命令。之后使用../../blogs/text2这样来打开。如果目录层级比较少还好（就像这个例子）,那么，如果我们是在目录层级比较多的情况下，那么我们该怎么做呢，还是这样通过相对路径来打开吗？vim给我们提供了一个方法，这个方法感觉还是很实用的。
其他的方法都不变，我们在需找文件路径的时候，可以使用如下方法

```
:e %:h<Tab>
```
%-代表的是当前打开的文件相对与缓存区工作路径的路径 + 文件名

缓存区工作路径的路径,也就是你使用vim打开一个文件的时候所在的路径
:h-这个操作，就是去掉文件名，仅仅剩下当前文件的路径

如果你感觉每次输入还是太麻烦的话，你还可以进行如下的配置 　
```vim
cnoremap <expr> %% getcmdtype()==':'?expand('%:h').'/':'%%'

````
把上面的这句话加入到vimrc中，然后你在命令行模式下，自动输入两个%%，然后它就会自动转化为跟你输入%:h<Tab>

