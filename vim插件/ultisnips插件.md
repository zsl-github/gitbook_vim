---
title: ultisnips插件的安装跟使用
date: 2017-01-12 21:09:18
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


# 插件安装
1. 直接下载插件，然后放到`.vim`下就可以了
下载地址 [ultisnips](https://github.com/SirVer/ultisnips)

2. 直接使用管理插件`bundle`,在`.vimrc`中添加如下内容那

```vim
Bundle 'SirVer/ultisnips'
```

# 使用方法

仅仅是安装了此插件还不够，还需要进一步预制模板。预制的方法如下
1. 在`.vim`下创建`UltiSnips`文件
```sh
mkdir UltiSnips
```

2. 在此文件夹下根据文件类型添加对应的模板，比如创建`java`的模板
```sh
java.snippets
```

3. 在`java.snippets`中添加想要的模板

```java
snippet wh 
while ($1)`!p nl(snip)`{
	$0
}
endsnippet
```
其中，`wh`是你模板快捷符号，`snippet`跟`endsnippet`中间的部分，是模板内容
${1:000}，其实就是选中000

4. 然后我们在`java`文件中，输入`wh`,然后按下`TAB`就可以了

# 配置

里面有四个方法可以映射快捷键，具体如下

```vim

" 扩展为代码块
let g:UltiSnipsExpandTrigger="<tab>"
let g:UltiSnipsListSnippets="<c-tab>"

" 下一个占位符
let g:UltiSnipsJumpForwardTrigger="<c-j>"

" 上一个占位符
let g:UltiSnipsJumpBackwardTrigger="<c-k>"
```

# 配合插件

这个插件实现的是功能，代码块需要自己添加。不过，有一个插件专门汇总了各种语言的常用代码块供我们使用
[snippets](https://github.com/honza/vim-snippets/tree/master/snippets),这个插件里有很多已经写好的代码块，我们直接用即可

# 语法讲解

> $n

这个表示光标占用符，光标默认在`$1`，然后通过`g:UltiSnipsJumpForwardTrigger` `g:UltiSnipsJumpBackwardTrigger`在其他`$n`位置进行跳转，最后跳到
`$0`结束。如果没有`$1`,那初始位置在最小值处，如果没有`$0`，则可以一直跳转

> ${n}

单独使用的时候跟`$n`是一样的，如果结合${n:aaa}使用，那么`$n`的值跟`${n:}`的值一致，`${n}`不会，它仍是一个占位符

> ${n:str}

它也是一个占位符，会选择模式选中`str`内容

> ``

执行命令或方法，比如`!v indent(.)`-调用`vim`方法。`!p`-调用`python`方法


