---
title: github添加秘钥
date: 2017-01-19 18:02:23
tags: 工具
categories:
    - 知识积累
    - 
password: 
---


# 什么是SSH?

`SSH` 为 `Secure Shell` 的缩写，由 `IETF` 的网络小组（Network Working Group）所制定；`SSH` 为建立在应用层基础上的安全协议。`SSH` 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。

# SSH 公钥
为什么`GitHub`需要`SSH Key`呢？
因为`GitHub`需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而`Git`**支持SSH协议**，所以，`GitHub`只要知道了你的公钥，才能允许你对相关内容进行修改

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

# 生成公钥

使用如下命令
```
ssh-keygen -t rsa -C "邮箱地址"
```
可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是`SSH Key`的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥.

# 添加公钥

打开github帐号管理中的添加SSH key界面的步骤如下：
1. 登录github
2. 点击右上方的Accounting settings图标
3. 选择 SSH key
4. 点击 Add SSH key
在出现的界面中填写`SSH key`的名称，填一个你自己喜欢的名称即可，然后将上面拷贝的`~/.ssh/id_rsa.pub`文件内容粘帖到key一栏，在点击`add key`按钮就可以了。

# 添加多个公钥

如果你想用多台电脑操作同一个仓，这时候需要给`github`添加多台电脑的`key`。只需要点击`add`按钮进行添加即可

# 拓展

有时候一台电脑需要访问不同的`git`，可能需要不同的`key`,如何在一台电脑上使用不同的`ssh key`呢？方法如下

1. 执行命令生成`key`

```
ssh-keygen -t rsa -C "your_email@example.com"
```
第一次输入的是文件名，输入一个别名如:id_rsa

2. 把生成的文件拷贝到`.ssh`目录下

3. 在 ~/.ssh 目录下创建 config 文件

4. 按照以下格式配置网站对应的`key`

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_a

```

# 疑问

如果直接添加公钥似乎不行，还要执行如下命令
```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```
不知道这个是不是必须的
