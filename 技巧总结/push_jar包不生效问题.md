---
title: push jar包不生效
date: 2017-11-24 10:18:53
tags: 问题
categories:
    - 累计
    - 
password: 
---

最近在做一个需求的时候，需要本地编译telephony-common.jar包，结果通过命令

```
adb push out/target/product/msm8937_64/system/framework/telephony-common.jar system/framework
```
push进手机以后，死活都不生效。后来终于发现原因了，原来，编译的时候除了生成自己的jar包外，还生成了两个文件`arm`、`arm64`。因为手机是64位的，所以还需要把arm64包push进手机才行

```
adb push out/target/product/msm8937_64/system/framework/arm64 system/framework/arm64

```

编译是生效了，但是一插卡手机就会重启，现在正在看是不是跟自己push的命令不对有关，还是因为自己的修改导致。感觉应该跟自己的修改关系不大，即使回退自己的修改，问题还是有的。
