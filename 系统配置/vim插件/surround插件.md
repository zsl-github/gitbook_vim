---
title: surround 插件使用
date: 2017-02-09 20:15:28
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


这个可以快捷键更改对接符

# 插件安装


直接在`.vimrc`中添加如下内容即可

```vimrc
Bundle 'tpope/vim-surround'
```

# 配置

这个插件也不需要配置，默认即可以满足需求

# 使用
只要记住如下几个命令就可以了

> ds+char 

直接删除包裹光标的对接符

> cs+char1+char2

把包裹光标的char1替换为char2

> ys[aw]+char

添加对接符
这里把ys当做v来看待，是一个命令，后面跟执行动作，最后添加对接符

> yss+char

整行添加对接符，忽略中间空格

上面两个命令跟`yS` `ySS` 区别就是后者还能让包围内容单独一行并且加上缩进

可视模式下，选中想要添加对接符的内容，按下`S`即可，然后输入对接符

