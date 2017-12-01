---
title: autojump的安装跟使用
date: 2017-02-27 20:59:28
tags: 系统配置
categories:
    - 工具
    - 
password: 
---


# 背景

最近在看`vim`插件的时候，看到了一个命令`autojump`，看这个命令介绍，说是`cd`的增强版。它最大的特点，就是可以根据用户平时操作目录的次数，提高此
目录的权重，下次用户执行`autojump`命令的时候就可以直接进入权重最高的匹配目录，而不用像`cd`那样层层进入

# 安装

ubuntu下可以直接用命令行安装，也可以下载源码编译
**1.命令行安装**
执行命令
```
sudo apt install autojump
```

**2.源码安装**

下载源码
```
git clone git://github.com/joelthelion/autojump.git
```

编译

```
cd autojump
chmod 755 install.py
./install.py
```

加入`.bashrc`

```
echo '. /usr/share/autojump/autojump.sh'>>~/.bashrc
source .bashrc

```
# 使用

j 是autojump的别名

1. 跳转到某一目录(A)
```
autojump A
```
2. 跳到子目录(A/B)

```
jc A
```
3. 打开一个文件管理器

```
jo A
```
4. 文件管理器中打开一个子目录
```
joc A
```

5. 权重的统计数据

```
j --stat
```
