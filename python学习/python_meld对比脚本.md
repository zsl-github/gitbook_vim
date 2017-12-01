---
title: python meld脚本
date: 2017-03-18 17:05:05
tags: python
categories:
    - 脚本
    - 
password: 
---

今天模仿着别人的脚本，结合网上的资料，摸索着写了一个简单的脚本，用来打开meld 工具,代码如下

```python
#/use/bin/python3.2
import os
import sys
j_name = sys.argv[1]
j_mode = sys.argv[2]
j_vtrunk_normal_path ="/home/zhangshuli/PROJECT/32_kk/packages/apps/Email/"
j_vtrunk_change_path = "/home/zhangshuli/PROJECT/35_vtrunk/packages/apps/Email/"
if "32" in j_mode:
    j_vtrunk_normal_path ="/home/zhangshuli/PROJECT/32_kk/packages/apps/Email/"
else:
    j_vtrunk_normal_path ="/home/zhangshuli/PROJECT/35m_new/packages/apps/Email/"
j_vtrunk_change = j_vtrunk_change_path +j_name
j_vtrunk_normal = j_vtrunk_normal_path +j_name
def wrap_meld():
    meld = "meld"+" "+j_vtrunk_change+" "+j_vtrunk_normal
    os.system(meld)
print("begin meld")
wrap_meld()
#os.chdir("/home/zhangshuli/desktop")
#path = os.getcwd()
#print(path)
#os.chdir("/home/zhangshuli/PROJECT/35_vtrunk/packages/apps/Email");
#os.system("meld")
```

