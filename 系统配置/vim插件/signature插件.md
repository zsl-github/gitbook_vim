---
title: signature插件
date: 2017-01-15 23:22:58
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---


这个插件是用来进行标签操作的，因为在看代码的时候，跳转比较多，这个插件还是很需要的

# 安装

直接在`vundle`中添加如下内容进行安装

```
Bundle "kshenoy/vim-signature"
```

# 常用配置

这个插件几乎不需要配置，直接使用即可
不过，有一点不太满意的就是书签的高亮颜色不太满意，红色字体看不清楚，这个可以通过如下方式进行修改
在`.vimrc`中添加如下内容

```
hi Marks_zsl term=reverse ctermfg=23 ctermbg=255 guibg=Grey60
function! HG_Color(lineno)
  return "Marks_zsl"
endfunction
let g:SignatureMarkTextHL = function("HG_Color")
```
其中`g:SignatureMarkTextHL`的作用就是可以自定义自己获取颜色的方法，当然，方法的返回值必须是`highlight group`

# 常用命令

```
mx           Toggle mark 'x' and display it in the leftmost column
dmx          Remove mark 'x' where x is a-zA-Z

m,           Place the next available mark
m.           If no mark on line, place the next available mark. Otherwise,
           remove (first) existing mark.
m-           Delete all marks from the current line
m<Space>     Delete all marks from the current buffer
]`           Jump to next mark
[`           Jump to prev mark
]'           Jump to start of next line containing a mark
['           Jump to start of prev line containing a mark
`]           Jump by alphabetical order to next mark
`[           Jump by alphabetical order to prev mark
']           Jump by alphabetical order to start of next line having a mark
'[           Jump by alphabetical order to start of prev line having a mark
m/           Open location list and display marks from current buffer

m[0-9]       Toggle the corresponding marker !@#$%^&*()
m<S-[0-9]>   Remove all markers of the same type
]-           Jump to next line having a marker of the same type
[-           Jump to prev line having a marker of the same type
]=           Jump to next line having a marker of any type
[=           Jump to prev line having a marker of any type
m?           Open location list and display markers from current buffer
m<BS>        Remove all markers

```

