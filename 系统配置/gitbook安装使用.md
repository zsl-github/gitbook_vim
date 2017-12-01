---
title: gitbook安装使用
date: 2017-11-24 10:48:43
tags: 系统配置
categories:
    - gitbook
    - 
password: 
---

今天忽然想起来继续体验下之前接触过的`gitbook`工具了，主要是看它跟我博客能否有效结合起来.

# 安装

执行如下命令即可

```
sudo npm install -g gitbook-cli
```
执行完以后，执行
```
gitbook -v
```
并没有什么反应。

然后执行

```
sudo npm install -g gitbook
```
又提示如下错误

```
You need to install "gitbook-cli" to have access to the gitbook command anywhere on your system.
If you've installed this package globally, you need to uninstall it.
>> Run "npm uninstall -g gitbook" then "npm install -g gitbook-cli"
```

最终决定卸载了重新安装

