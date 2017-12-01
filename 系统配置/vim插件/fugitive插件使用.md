---
title: vim git相关插件之fugitive
date: 2017-01-26 11:20:16
tags: vim-plugin
categories:
    - vim
    - plugin
---

这是我`vim`中的第二个`git`相关插件，第一个插件是`signify`，这两个插件一个在编辑文件中显示文件变化状态(后者)，一个在`vim`中进行`git`操作。


## 如何安装

**直接下载安装**

直接在网上下载安装 [fugitive](https://github.com/tpope/vim-fugitive),然后拷贝到`vim`插件目录即可

**通过vundle安装**

在`.vimrc`中添加如下内容

```vim
Bundle 'tpope/vim-fugitive'
```

## 如何配置

此插件几乎不用什么配置，保持默认配置就可

## 常用命令

### :Git [args] 

执行`git`相关命令，在终端显示结果，按任意键返回编辑界面

### :Git! [args]

执行`git`相关命令，在窗口新开一个文件显示结果，按`q`键返回编辑界面

### :Gcd [directory]

功能跟`:cd`一样，改变工作目录

### :Glcd [directory]

功能跟`:lcd`一样，改变工作目录

### :Gstatus 

在当前界面显示`git-status`结果，之后可以进行如下操作

```
g?    show this help
<C-N> 下一个修改的文件
<C-P> 上一个修改的文件
<CR>  |:Gedit| 打开此文件
-     |:Git| add 相当于 git add
-     |:Git| reset (staged files) 相当于 git reset
a     Show alternative format
cA    |:Gcommit| --amend --reuse-message=HEAD
ca    |:Gcommit| --amend
cc    |:Gcommit|
cva   |:Gcommit| --amend --verbose
cvc   |:Gcommit| --verbose
D     |:Gdiff|
ds    |:Gsdiff|
dp    |:Git!| diff (p for patch; use :Gw to apply)
dp    |:Git| add --intent-to-add (untracked files)
dv    |:Gvdiff|
O     |:Gtabedit|
o     |:Gsplit|
p     |:Git| add --patch
p     |:Git| reset --patch (staged files)
q     close status
r     reload status
S     |:Gvsplit|
U     |:Git| checkout
U     |:Git| checkout HEAD (staged files)
U     |:Git| clean (untracked files)
U     |:Git| rm (unmerged files)
```
### :Gcommit [args]

相当于`git commit`

如果后跟参数`-m` 这时候会直接提交
如果后跟参数`-v` 这时候会打开一个`tab` 进入提交界面

### :Gmerge [args]     

`git merge` 功能没用过,待学习了`git` 相关功能后再看

###:Gpull [args]

执行`git pull` 操作

###:Gpush [args]    

执行`git push` 操作

### :Gfetch [args]      
`git fetch` 功能没用过,待学习了`git` 相关功能后再看

### :Ggrep[!] [args] 

功能不常用，先不关注

### :Glgrep[!] [args]

功能不常用，先不关注

### :Glog [args]  

`git log` 操作

### :{range}Glog [args]

不常用操作，先不关注

:Gllog [args]

不常用操作，先不关注

### :Gedit [revision]

功能不清楚，先不关注

### :Gsplit [revision]
功能不清楚，先不关注

### :Gvsplit [revision]
功能不清楚，先不关注

### :Gtabedit [revision]
功能不清楚，先不关注

### :Gpedit [revision]
功能不清楚，先不关注

### :Gsplit! [args]
功能不清楚，先不关注

### :Gvsplit! [args]
功能不清楚，先不关注

### :Gtabedit! [args]
功能不清楚，先不关注

### :Gpedit! [args] 
功能不清楚，先不关注

### :Gread [revision]
功能不清楚，先不关注

### :{range}Gread [revision]
功能不清楚，先不关注

### :Gread! [args]
功能不清楚，先不关注

### :{range}Gread! [args]
功能不清楚，先不关注

### :Gwrite
功能不清楚，先不关注

### :Gwrite {path}
功能不清楚，先不关注

### :Gwq [path]
功能不清楚，先不关注

### :Gwq! [path]
功能不清楚，先不关注

### :Gdiff [revision]
`git diff` 命令

### :Gsdiff [revision] 
跟`:Gdiff` 功能一样，只是显示的时候是上下分屏

### :Gvdiff [revision]
目前没发现跟`:Gdiff`

### :Gmove {destination}
`git move` 操作

### :Gdelete     
`git mv`操作

### :Gremove
没发现跟`:Gdelete`的差别

### :Gblame [flags]
查看文件每一行的修改记录

```

g?    show this help
A     resize to end of author column
C     resize to end of commit column
D     resize to end of date/time column
q     close blame and return to blamed window
gq    q, then |:Gedit| to return to work tree version
<CR>  q, then open commit
o     open commit in horizontal split
O     open commit in new tab
-     reblame at commit
~     reblame at [count]th first grandparent
P     reblame at [count]th parent (like HEAD^[count])

```

### :[range]Gblame [flags]
范围内查找行变更记录

### :Gbrowse
### :Gbrowse {revision}
### :Gbrowse [...]@{remote} 
### :{range}Gbrowse [args] 
### :[range]Gbrowse! [args]
目前不打算使用，暂不关注

```
                                                *fugitive-c_CTRL-R_CTRL-G*
<C-R><C-G>              On the command line, recall the path to the current
                        object (that is, a representation of the object
                        recognized by |:Gedit|).

                                                *fugitive-y_CTRL-G*
["x]y<C-G>              Yank the commit SHA and path to the current object.

These maps are available in Git objects.

                                                *fugitive-<CR>*
<CR>                    Jump to the revision under the cursor.

                                                *fugitive-o*
o                       Jump to the revision under the cursor in a new split.

                                                *fugitive-S*
S                       Jump to the revision under the cursor in a new
                        vertical split.

                                                *fugitive-O*
O                       Jump to the revision under the cursor in a new tab.

                                                *fugitive--*
-                       Go to the tree containing the current tree or blob.

                                                *fugitive-~*
~                       Go to the current file in the [count]th first
                        ancestor.

                                                *fugitive-P*
P                       Go to the current file in the [count]th parent.

                                                *fugitive-C*
C                       Go to the commit containing the current file.

                                                *fugitive-.*
.                       Start a |:| command line with the current revision
                        prepopulated at the end of the line.

                                                *fugitive-a*
a                       Show the current tag, commit, or tree in an alternate
                        format.

```

功能太多，使用熟练些后再继续总结
