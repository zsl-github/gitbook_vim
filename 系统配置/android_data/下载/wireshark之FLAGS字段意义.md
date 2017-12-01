---
title: wireshark之FLAGS字段
date: 2017-03-24 17:08:51
tags: android
categories:
    - 工具
    - wireshark
password: 
---

`wireshark`工具是用来查看`网络传输过程`的，在`TCP`层中，有个`FLAGS`字段，这个字段有以下几个标识
```
SYN, FIN, ACK, PSH, RST, URG.
```
下面是用`wireshark`打开手机`LOG`的显示内容

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/wireshark_flags_1.png)

上面的意义分别是

**SYN-SYN表示建立连接**

这个是`TCP`三次握手建立连接的时候的`FLAG`，具体如下


![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/wireshark_flags_2.png)

三次握手的示意图如下

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/wireshark_flags_3.png)


**FIN断开连接**

```
249 2017-11-01 16:07:42.630309  10.100.8.232    121.51.8.119    TCP 56  49104 → http(80) [FIN, ACK] Seq=460 Ack=586 Win=90624 Len=0
```

**ACK数据相应**

```
205 2017-11-01 16:07:41.613502  10.100.8.232    183.232.126.37  TCP 68  43541 → http(80) [ACK] Seq=1 Ack=1 Win=87808 Len=0 TSval=4294940911 TSecr=3939246364
```

**PSH传输数据**

```
274 2017-11-01 16:07:42.767055  121.51.140.141  10.100.8.232    TCP 167 http-alt(8080) → 38291 [PSH, ACK] Seq=1 Ack=167 Win=262140 Len=111
```

**RST重新连接**

```
333 2017-11-01 16:07:44.029264  10.100.8.232    121.51.8.100    TCP 56  44282 → http(80) [RST] Seq=625 Win=0 Len=0
```

**URG 作用待确认**



