---
title: au BufWinLeave 报错
date: 2017-01-20 12:40:14
tags: vim
categories:
    - 常用配置
    - 
password: 
---


今天在公司配置`vim`的时候，发现如果我执行如下命令
```
vim
:q
```
会提示
```
Error detected while processing BufWinLeave Auto commands for "*"
```
这个问题纠结了好久，最后终于发现了解决方案。问题主要出现在`.vimrc`中的配置
```vim
au BufWinLeave . silent mkview 
au BufRead . silent loadview    
```
根据网上的方法，修改成如下方式就不会有问题了


```vim
au BufWinLeave *.* silent mkview 
au BufRead *.* silent loadview    
```

# 总结
其实，当时看到这个错误，也应该能想到是因为`BufWinLeave`导致的错误。但是因为自己遇到错误不会冷静下来，仔细分析错误原因，就会无脑的用尝试的方法进行排除，往往走错了方向，耽误了问题的解决。所以，遇事不要着急立刻去解决，而是先要分析问题，尽可能多的收集问题信息跟细节，抽丝剥茧，最终解决问题。
