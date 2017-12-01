---
title: interestingwords 插件的使用
date: 2017-01-05 12:53:13
tags: vim-plugin
categories:
    - vim
    - plugin
---

# 插件安装
- 插件管理器安装
直接在`.vimrc`插件管理段落中中添加如下内容
```vim
Bundle "lfv89/vim-interestingwords"
```
然后运行插件管理工具安装即可
- 下载插件资源安装
直接在[vim-interestingwords](https://github.com/lfv89/vim-interestingwords)中下载插件，按照普通方法即可

# 常用指令

**高亮光标下单词**
`,k`(如果已经高亮，则会取消高亮)

**取消所有高亮**
`,K`(注意是大小写不同)

**在高亮单词间跳转**
`n`下一个
`N`上一个

# 自定义快捷键
默认使用的高亮功能键是k,感觉不太好，换成h感觉更贴切，可以使用如下方法进行自定义
```vim
nnoremap <silent> <leader>h :call InterestingWords('n')<cr>
nnoremap <silent> <leader>H :call UncolorAllWords()<cr>

nnoremap <silent> <leander>n :call WordNavigation('forward')<cr>
nnoremap <silent> <leander>N :call WordNavigation('backward')<cr>
```


## 不便之处

1. 只能再最新的高亮单词上跳转，之前的要想跳转，必须使用vim自带选中\*来跳转

2. 重新定义按键以后，之前的功能快捷键仍然存在，这点需要修改

3. 没法罗列所有高亮的单词，类似marks标签那样

## 5.13补充
此插件目前已经没有在用了，主要是为了清理不常用插件。这个插件的功能有点鸡肋，没有完全达到自己的预期。

