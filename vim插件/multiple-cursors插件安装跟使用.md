---
title: multiple cursors插件安装跟使用
date: 2017-02-14 20:42:45
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


# 插件安装

1. 通过下载模块来安装

点击下载地址[multiple-cursors](https://github.com/terryma/vim-multiple-cursors)，然后拷贝到`.vim`下对应的目录即可

2. 通过插件管理工具`vundle`进行安装

在`.vimrc`中添加如下内容

```vim
Bundle 'terryma/vim-multiple-cursors'
```

然后执行`BundleInstall`，就可以成功安装此插件了


# 插件的使用

## 常用命令

此插件使用起来也比较方便，唯一知道的就是，如果你想更改某个单词或者字串，你只需要选中这个字串，然后点击`C-n`就可以了，等你把想要更改的内容
全部选中以后，就可以使用删除、编辑等命令，进行批量修改了

比如，如下的内容
```java
zsl-lien1
zsl-lien2
zsl-lien3
zsl-lien4
```

然后我们更改上面的zsl为zhangshuli

我们就可以这么做

- 光标放到zsl上
- 按下`C-n`
- 按下C
- 输入zhangshuli
- ESC

常用的几个命令

- C-n 向下选中内容
- C-p 向上取消选中内容
- C-x 跳过当前选中内容，选择下一个内容

## 常用设置

如果想要设置上面的几个快捷键，可以在`.vimrc`中进行设置，例如

```vim
    let g:multi_cursor_next_key='<C-n>'
    let g:multi_cursor_prev_key='<C-p>'
    let g:multi_cursor_skip_key='<C-x>'
    let g:multi_cursor_quit_key='<Esc>'
```

还有其他设置项，其实没什么太大用处的，知道上面的就已经够了
