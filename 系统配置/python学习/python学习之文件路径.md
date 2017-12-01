---
title: python学习之文件路径
date: 2017-03-06 10:30:27
tags: python
categories:
    - 基础
    - 
password: 
---

今天开始接触到了文件目录、路径方面的知识点。记录如下

先看代码

```python
#!/usr/bin/python
# -*- coding: utf-8 -*-
import os
import sys
current_path = os.getcwd()
path_test = "/home/zhangshuli/PROJECT/PROJECTL/32_kk"
path_test2 = "/home/zhangshuli/PROJECT/PROJECTL/32_kk/packages/apps/Calculator"

print(current_path)

os.chdir(path_test)
print(current_path)

current_path = os.getcwd()
print(current_path)

current_path = os.listdir(path_test)
print(current_path)

current_path = os.path.split(path_test)
print(current_path)

os.chdir(path_test2)
current_path = os.path.abspath(os.curdir)
print(current_path)

os.chdir(path_test2)
current_path = os.path.abspath('.')
print(current_path)


current_path = os.path.abspath('..')
print(current_path)

current_path = sys.argv[0]
print(current_path)
```

运行结果如下

```
/home/zhangshuli/desktop/python_test
/home/zhangshuli/desktop/python_test
/home/zhangshuli/PROJECT/PROJECTL/32_kk
['ndk', 'makeMtk.ini', 'mediatek', 'bionic', 'dalvik', 'libcore', 'vanzo_custom_base', 'bootable', 'hardware', 'mbldenv.sh', 'system', 'mk', 'external', 'makeMtk', 'Makefile', 'checkenv.log', 'packages', 'apks', 'out', 'development', 'recommend_apks', 'pdk', 'r1.txt', 'auto_sync_android.log', 'art', 'sdk', 'abi', 'docs', 'libnativehelper', 'log', 'kernel', 'update_overlay_files.py', '.repo', 'frameworks', 'device', 'vanzo_common2.pyc', 'build', 'vendor', 'vanzo_common2.py', 'prebuilts']
('/home/zhangshuli/PROJECT/PROJECTL', '32_kk')
/home/zhangshuli/PROJECT/PROJECTL/32_kk/packages/apps/Calculator
/home/zhangshuli/PROJECT/PROJECTL/32_kk/packages/apps/Calculator
/home/zhangshuli/PROJECT/PROJECTL/32_kk/packages/apps
/home/zhangshuli/desktop/python_test/path.py
```

知识点总结

> os.chdir("path")

切换到path目录

> os.getcwd()

获得当前工作目录

如果你使用`os.chdir()`切换了目录，`os.getcwd()`会跟着改变

> os.path.split(path_test)

把path_test进行拆分，得到的是当前目录名跟它的父类路径

> os.path.abspath(os.curdir)

获得当前工作路径

也可以使用这个方法

> os.path.abspath('.')

. 代表当前路径

> os.path.abspath('..')

获得当前目录的父目录路径

> ..
代表上一级目录

> sys.argv[0]

获得运行脚本你本身所在的目录。

