---
title: ctrlp插件使用
date: 2017-02-04 20:08:59
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---

# 如何安装CTRLP插件
1. 直接下载模块[ctrlp](https://github.com/ctrlpvim/ctrlp.vim)

2. 直接在`vundle`中添加如下内容进行安装
```vim
Bundle 'ctrlpvim/ctrlp.vim'
```
# 常用设置
1. 使用该选项来改变普通模式 |Normal| 下调用CtrlP的按键绑定:
> let g:ctrlp_map = '<c-p>'

2. 修改该选项为1，设置默认为按文件名搜索（否则为全路径）:
> let g:ctrlp_by_filename = 0

3. 使用当前工作目录作为搜索跟目录
> let g:ctrlp_working_path_mode = 'ra'

4. 包含当前文件
> let g:ctrlp_match_current_file = 1

5. 保存的是之前的操作记录以及载入的搜索结果,再次启动可以缩短时间
> let g:ctrlp_clear_cache_on_exit = 0

6. 如果设置为0,默认模糊匹配，1则是顺序匹配
> let g:ctrlp_regexp = 1

7. 搜索目录级数
> let g:ctrlp_max_depth = 20

8. 搜索文件数量限制
> let g:ctrlp_max_files = 1000000

9. 设置搜索显示位置跟数量
> let g:ctrlp_match_window = 'bottom,order:ttb,min:1,max:1000,results:1000'

10. 新打开的文件位置
> let g:ctrlp_tabpage_position = 'ac'

11. 针对每个会话，是否设置缓存
> let g:ctrlp_use_caching=0

12. 退出vim后是否删除缓存
> let g:ctrlp_clear_cache_on_exit = 1


"忽略显示指定文件夹的匹配结果
set wildignore+=*/.git/*,*/.hg/*,*/.svn/*,*/bbb/*

if executable('ag')
    " Use Ag over Grep
    set grepprg=ag\ --nogroup\ --nocolor\
    " Use ag in CtrlP for listing files.
    let g:ctrlp_user_command = 'ag %s -l --nocolor -g ""'
    " Ag is fast enough that CtrlP doesn't need to cache
    let g:ctrlp_use_caching = 1
endif

# 基本命令
1. 此插件有一个最基本的命令，就是`C-P`，执行此命令以后，会列表显示当前目录下的文件

2. 指定目录中进行搜索
> ctrp 目录

3. 在所有打开的`buffer`文件中进行搜索
> nnoremap <C-b> :CtrlPBuffer<CR>


# 常用技巧

1. 在提示符面板内可以使用 <c-d> 来切换搜索模式
- 按照文件名搜索
- 按照文件路径搜索

2. 使用`CtrlPBuffer`可以用来查看当前打开的搜索文件，对于跟踪代码非常便利，因为经常需要再各个文件中跳转

3. 在提示符面板内可以使用 <c-r> 来切换是否使用正则表达是搜索
- 顺序搜索
- 正则表达式搜索

4. 通过设置工作目录，来合理限制查找范围
> let g:ctrlp_working_path_mode = 'ra'
倾向于设置为`rw`，使用当前工作目录作为搜索目录，可以实现在源码跟目录进行跳转

# 加快速度

**使用ag搜索**
这个插件本身带有一个可以调用外部工具进行搜索的属性 `ctrlp_user_command`。我这边对比了下，如果使用插件自己的搜索工具，在`android`源码下，搜索时间是`4:10`，如果使用`ag`来搜索，搜索时间是50s，完全不是一个级别。另外，`ag`可以
有自己的配置方式，可以过滤某些不必要的文件跟文件夹。鉴于自己使用此插件的场景是数目较小的文件，所以就不再设置了


# 对比LeaderF插件
在网上偶然看到，有一个更新的插件(LeaderF)[https://github.com/Yggdroot/LeaderF/issues)，据说这个插件跟`ctrlp`唯一的区别就是它比后者更高效。此插件我本地也验证了，确认要比`ctrlp`要块，遗憾的是它并没有快到可以满足我在源码根目录使用的程度。
顺便偷偷告诉你，我英语不好，巧的是有人把[ctrlp说明](https://github.com/codepiano/ctrlp.vim)翻译成中文了

**2017-9-6 补充**
现在已经放弃了在源码根目录下使用`ctrlp`的尝试，重新定义了文件浏览插件的使用场景-在文件数目不多的目录下使用。而在看源码的时候，主要是用来在某个模块目录下进行文件查找算是对`lookupfile`功能的补充吧。所以，目前考虑到性能，还是决定放弃`ctrlp`,使用`LeaderF`

