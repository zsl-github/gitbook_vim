---
title: EARFCN基础概念
date: 2017-11-23 14:21:20
tags: 通讯
categories:
    - 基础概念
    - 
password: 
---

这个是频点编号，用来鉴别特殊射频通道的编号方案。频点间隔是5MHZ,我们可以看如下的截图

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/earfcn_1.png)

通过它可以查看对应的频率以及所处的band

频点跟频率换算公式为

NDL=10×(DL-DL_low)+NDL_low

其中 NDL 是计算所得频点，DL是频率 DL_low是当前频段频率起点，NDL_low 是当前频段频点起点

例如，对于F段频段 1890MHZ的频点就是

10×(1890-1880)+38250
