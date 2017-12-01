---
title: QXDM怎么抓取LOG
date: 2017-04-17 11:39:54
tags: android
categories:
    - 工具
    - QXDM
password: 
---

接触高通两个月了，说起来都很尴尬，因为到现在还不知道怎么通过`QXDM`抓取`LOG`呢。今天就总结学习下抓取`LOG`的方法

# 连接QXDM

我用的是`ubuntu`系统，直接安装的`ubuntu`版本的`QXDM`。官网上给出的连接`QXDM`需要`QPST`

> QXDM connects to a device through QPST Server using a serial or USB cable to a COM port on your PC.

不过，我看官网上`QPST`是没有`ubuntu`版本的，就抱着试试看的态度，尝试直接插上`USB`进行连接


## 打开QXDM

我的`QXDM`是在`ubuntu`下通过命令行来打开的

```
sudo qxdm
```

## 连接手机

然后点击`QXDM`的连接按钮

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qxdm_connect_phone_1.png)

通过`usb`连接手机，就会发现识别到了手机
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qxdm_connect_phone_2.png)

直接点击连接按钮即可

**注意**
手机连接`USB`的时候，手机会弹出一个提示界面，类似

> 是否使用USB进行文件传输

如果你点击了`YES`，上面识别到的设备就会消失，导致连接不上。所以，要选择`NO`

# 抓取LOG

手机成功连接后，我们点击下图的图标就可以开始抓取`LOG`了。想要保存的话，点击`qxdm->file>save item`即可
![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/qxdm_connect_phone_3.png)

# 汇总

总的方法汇总如下

```
1)restart phone.
2)qxdm->file->load configuration, load the GPS log config file like gnss_new.dmc.//这一步如果没有gnss_new.dmc文件，可以选默认configuration即可
3)connect with your phone
4)do the test
5)wait for some additional time like 60s
6)save qxdm log: qxdm->file>save item
```

