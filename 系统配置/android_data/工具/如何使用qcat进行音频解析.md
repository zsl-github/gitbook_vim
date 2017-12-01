---
title: 如何使用qcat解析音频文件
date: 2017-04-26 12:40:46
tags: android
categories:
    - 工具
    - qcat
password: 
---

最近在分析外场问题的时候，有很多通话杂音问题。这类问题往往需要解析音频文件来确认是接收端问题`RX`还是发送端问题`TX`，具体可以通过`qcat`解析获得的音频资源来确认，具体方法
如下

# 打开modem log

这个就不用多少了，用过`qcat`的都知道

# 解析音频文件

具体通过如下步骤解析音频文件

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qcat_voice_1.png)
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qcat_voice_2.png)
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qcat_voice_3.png)

# 查看音频文件
解析后的音频文件如下所示
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qcat_voice_4.png)

我们直接选择音频文件打开即可听到具体内容

