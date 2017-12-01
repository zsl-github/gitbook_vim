---
title: 使用adb操作手机数据库
date: 2017-04-22 15:25:55
tags: android
categories:
    - 数据库
    - 基本操作
password: 
---

最近在做紧急拨号的问题的时候，需要更改手机中的数据库，这里总结下数据库的基本操作

**1. 进入手机数据库所在目录**

通过`adb`指令，进入需要操作的数据库所在目录，前提当然是手机已经获得`root`权限，已操作`qcril.db`数据库为例

```
adb shell
cd data/misc/radio
```

**打开数据库**

通过如下命令可以打开相应的数据库

```
sqlite3 qcril.db
```

之后进入数据库操作模式

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/sqlte_1.png)

**操作数据库**

之后就可以进行数据库操作了，比如，插入一条数据
```
 insert into qcril_emergency_source_mcc_table values('460','110','','limited');
```
注意每条命令必须以分号表示结束

**常用命令**

> .tables
列出所有表格

> delete from tables where

删除表格的某些内容

>  insert into tables values

插入一行内容


