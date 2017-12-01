---
title: rainbow_parentheses 插件
date: 2017-02-15 20:43:54
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


这个插件是彩虹色显示对接符的，看代码的时候，容易区分对接符包裹范围

# 安装

1. 直接下载安装

直接点击[rainbow parenthese](https://github.com/kien/rainbow_parentheses.vim),然后直接拷贝到对应位置即可

2. 使用vundle插件管理器安装

把如下内容添加到`.vimrc`中，然后执行命令`BundleInstall`即可

```vim
Bundle 'kien/rainbow_parentheses.vim'
```

# 配置

一般来说，直接使用此插件的默认设置就可以了

如果你想做客制化，有几个设置项

1. `RainbowParenthesesToggle`

这个是切换高亮的，相当于开关此插件功能，一般是要打开的

2. RainbowParenthesesLoadRound    " (), the default when toggling

加载括号高亮

3. RainbowParenthesesLoadSquare   " []

加载方括号高亮

4. RainbowParenthesesLoadBraces   " {}

加载大括号高亮

5. RainbowParenthesesLoadChevrons " <>

加载尖括号高亮

要想默认上上述对接符高亮，需要在`.vimrc`中配置如下内容

```vim
au VimEnter * RainbowParenthesesToggle
au Syntax * RainbowParenthesesLoadRound
au Syntax * RainbowParenthesesLoadSquare
au Syntax * RainbowParenthesesLoadBraces
```

6. g:rbpt_colorpairs

此配置项是用来客制化对接符颜色显示的，默认即可


**效果图**
此插件的最终效果
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/rain_shot.png)

