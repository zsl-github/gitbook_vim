---
title: wildfire 插件
date: 2017-02-16 20:44:27
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


这个插件是用来快速选中对接符内容的，我们经常会遇到这种场景－选中括号中的内容，虽然我们可以直接使用`vi(`，不过，操作还是有点麻烦，使用了这个
插件，我们就可以直接更加快捷的选中对接符中的内容了

# 插件安装

直接在`.vimrc`中添加如下内容即可

```vimrc
Bundle "gcmt/wildfire.vim"
```

# 配置
此插件不需要额外的配置，直接使用即可。不过，里面还是有两个可以客制化的东西的。

**1.添加适用对接符**

let g:wildfire_objects = ["i'", 'i"', "i)", "i`", "i]", "i}", "i>", "ip"]

**2.更改快捷键**
map <SPACE> <Plug>(wildfire-fuel)
vmap <S-SPACE> <Plug>(wildfire-water)

# 使用

直接把光标放到两个对接符中间，按下空格键就可以了
