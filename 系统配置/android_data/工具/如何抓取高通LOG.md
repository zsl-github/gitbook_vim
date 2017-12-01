---
title: 如何抓取高通LOG
date: 2017-04-16 19:00:48
tags: android
categories:
    - 高通
    - 方法
password: 
---

接触高通项目有两个月了，说来惭愧，现在竟然还不知道怎么抓取高通`LOG`.当然，使用`adb logcat`就算了.现在总结下如何抓取高通`LOG`

# 高通LOG

## 通过QXDM抓取

具体文章请参考[QXDM怎么抓取LOG]()

## 通过手机直接抓取

**进入LOG抓取界面**
直接输入暗码`*#*#446622*#*#`进入`LOG`界面

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qc_log_1.png)

**勾选相应LOG**

勾选自己需要的`LOG`即可,勾选后已经开始自动抓取了

**导出LOG**

`LOG`保存在内部存储器中，使用命令
```
adb pull sdcard/diag_logs
adb pull sdcard/logs
```
即可

