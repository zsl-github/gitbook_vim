---
title: EasyMotion插件安装跟使用
date: 2017-02-06 20:11:35
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


# 安装插件

1. 下载插件安装
我们点击[easyMotion](https://github.com/Lokaltog/vim-easymotion/)下载此插件，然后copy到.vim对应的目录下即可

2. 通过vundle进行安装

直接在.vimrc中添加如下内容
```vim
Bundle 'easymotion/vim-easymotion'
```

# 插件的使用

这个插件的命令很简单，有如下几个命令

**,,f+**

表示你想要查询的字符。执行这个命令以后，匹配的字符会被标记，然后你选择标记标签，就可以跳转到匹配项了

对于如下内容:
```md
SelectLines and SelectPhrase are not actually *motions*, so I've moved them into
separate plugins.

- https://github.com/haya14busa/vim-easyoperator-line
- https://github.com/haya14busa/vim-easyoperator-phrase
```

我们执行如下命令
> ,,fs

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/find_s_result.png)

如果你执行`,,F+*`，那么会向前匹配

**,,w**

这个命令是用来给光标后的单词进行标记，然后你选择相应的标记，就会跳转到对应的单词处。如果单词比较多，后使用两个字母标记，如fa,fb...这时候你选择ｆ，接下来你就可以继续选择a,就会跳转到fa处


同样是上面的文本
```md
SelectLines and SelectPhrase are not actually *motions*, so I've moved them into
separate plugins.

- https://github.com/haya14busa/vim-easyoperator-line
- https://github.com/haya14busa/vim-easyoperator-phrase
```

我们执行如下命令
> ,,w

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/w_find_result.png)

**,,t ,,T**
这个命令跟**,,f ,,F**相似，只是它会跳转到匹配字符的前一个字符位置

**,,b ,,B**
这个命令跟**,,w ,,W**相似,b是匹配单词，B是匹配字串

**,,e ,,E**
这个命令跟**,,b ,,B**相似,只是它匹配的是单机结尾

**,,ge ,,gE**
这个命令跟**,,e ,,E**相似,只是它向上匹配

**,,j ,,k**
这两个命令是用来标记行的，然后可以选择标记，跳转到对应的行。如下面这段内容

```

    <Leader>f{char}      | Find {char} to the right. See |f|.
    <Leader>F{char}      | Find {char} to the left. See |F|.
    <Leader>t{char}      | Till before the {char} to the right. See |t|.
    <Leader>T{char}      | Till after the {char} to the left. See |T|.
    <Leader>w            | Beginning of word forward. See |w|.
    <Leader>W            | Beginning of WORD forward. See |W|.
    <Leader>b            | Beginning of word backward. See |b|.
    <Leader>B            | Beginning of WORD backward. See |B|.
    <Leader>e            | End of word forward. See |e|.
    <Leader>E            | End of WORD forward. See |E|.
    <Leader>ge           | End of word backward. See |ge|.
    <Leader>gE           | End of WORD backward. See |gE|.
    <Leader>j            | Line downward. See |j|.
    <Leader>k            | Line upward. See |k|.
```

执行`,,j`以后的命令是

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/row_j_result.png)

**,,n**
这个命令是要结合搜索命令来使用的，首先找到匹配结果，然后使用此命令对匹配结果在进行标记跳转，这个命令使用起来并不方便，不需要掌握

**,,s**
这个命令其实是**,,f ,,F**的结合，可以上下搜索

