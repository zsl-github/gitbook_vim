---
title: fzf插件安装
date: 2017-04-09 18:41:00
tags: vim-plugin
categories:
    - vim
    - vim-plugin
password: 
---

fzf插件效果跟`LeaderF`相似，也是一个命令集成工具，它里面调用了很多其他插件的借口，集成了很多功能，可以看下官网的功能截图

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/vim_plugin_fzf_1.png)

之前有篇文章讨论过`ctrlp` `LeaderF`的优劣比较，最终选择了`LeaderF`。但是，在`android`根目录下，它还是无法满足时时查找的需求，尤其是在搜索的过程中，输入字串容易卡顿，当你尝试粘贴多个字符的时候，更是卡顿的让人抓狂。最近在网上看到了另一个插件`fzf`，据说速度上比`LeaderF`有过之而无不及，现在体验下。


# 环境依赖

## 安装`go`环境

安装`go`语言环境
```
sudo apt install golang-go
```

## 安装系统`fzf`工具

直接从[fzf-github](https://github.com/junegunn/fzf)下载源码，进行命令安装即可。比较简单的方法是使用`git`安装

```
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf ~/.fzf/install
```

这里说明下，安装了`fzf`后，在终端就可以通过`fzf`的命令进行文件查找了，查找指令`fzf [options]`,类似于`ag -g`，只是它会遍历出所有文件，让你进行筛选
另外，它还能够影响到终端历史记录的显示效果，当我们按下`Ctrl+R`的时候，会调出历史列表，我们也可以筛选或上下移动选中.见效果图

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/fzf_search_result.gif)

### 常用命令

命令格式 

```
fzf [options]
```

**fzf -e**
- 全词匹配，默认是模糊匹配的

这个并不会影响搜索列表，列表显示的都是当前目录下的所有文件，它影响的是在列表中进行二次筛选的结果

**fzf --query=STR**
搜索包含STR的文件,在列表中显示，类似文件名关键字的筛选

**fzf --filter=STR**
跟`--query`相似,只是它不会进入交互模式,而是直接返回搜索结果,跟ag -g类似.不过,比较下来,似乎ag更快

**fzf -i**
忽略大小写

# 插件安装

直接通过`vundle`进行安装，在`.vimrc`中添加如下内容

```
Bundle 'junegunn/fzf.vim'
```

# 插件使用

主要就是上面截图中的方法,有一个是我比较喜欢的，就是`Ag!`方法，这个方法几乎解决了我之前一直纠结的在当前文件中查找某个内容显示不友好的问题

**Ag string**
在当前工作目录中进行搜索，然后选中搜索结果点下`enter`就可以打开了

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/fzf_ag_search.gif)

不过，还是不知道要怎么在当前文件下搜索



