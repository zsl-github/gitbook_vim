---
title: Solarized 安装
date: 2017-01-29 19:42:44
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


# 插件安装
solarized　其实算不上严格的插件，它只是一个主题，这个主题看起来还是很不错的

我们可以从[solarized](https://github.com/altercation/vim-colors-solarized) 下载资源，然后放到.vim对应目录下即可

或者我们通过vunble来管理这个主题插件，在.vimrc中添加如下的内容
```vim
    Bundle 'altercation/vim-colors-solarized'
```

插件安装完成以后，我们还需要在`.vimrc`中设置一下主题。如下
```vim
    if has('gui_running')
        set background=light
    else
        set background=dark
    endif
    colorscheme solarized
```

到了这一步，关于vim方面的配置已经全部完成，可是，如果你这时候看vim效果的话，并没有达到预期的效果。那是什么原因呢？这时候上网查了一下，原来还需要
在Ubuntu终端进行一下设置。具体方法如下

# 更改终端的配色为Solarized

下载终端配置资源文件

可以直接下载[终端配色solarized](https://github.com/seebi/dircolors-solarized)

或者使用git 下载

> git clone git://github.com/seebi/dircolors-solarized.git

资源下载完以后（里面有几种颜色资源，我选择的是`dircolors.256dark`），然后执行如下命令

> cp dircolors-solarized/dircolors.256dark ~/.dircolors
> eval 'dircolors ~/.dircolors'

`.dircolors`是用来设置终端颜色显示的，比如文件名的显示颜色等

接着我们在.bashrc中添加如下内容

> export TERM=xterm-256color

执行命令

> source ~/.bashrc

最后我们需要下载Gnome-Terminal颜色

我们可以直接到网页[终端配色](`https://github.com/Anthony25/gnome-terminal-colors-solarized`)

下载资源，或者使用如下git命令获取

> git clone git://github.com/sigurdga/gnome-terminal-colors-solarized.git

下载完成以后，我们进入到资源文件目录中，执行命令

> ./set_dark.sh

到这里，我们终端已经配置完成了。风格如下

执行上个命令的时候，我们会碰到提示说要我们选择一个主题文件，我们选择提示中给的序号就可以了

> 1) :b1dcc9dd-5262-4d8d-a863-c897e6d979b9 (No name)也就是1


这时候我们再进入vim，效果如下
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/sz_vim.png)

# 2017.11.30补充

使用了一段时间`dark`风格，感觉用久了屏幕模糊，对眼睛不好，现在尝试下使用`ligth`风格


