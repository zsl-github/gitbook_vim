---
title: python学习之选择语句
date: 2017-03-10 11:52:44
tags: python
categories:
    - 基础
    - 
password: 
---

python的条件选择语句跟其他语言的及其相似，这里就不做详细记录，仅仅是看个例子好了

```python
#!/usr/bin/python
if 1 in [1,2,3,"4"]:
    print('1 in [1,2,3,"4"]')
elif 2 in [1,2,3,"4"]:
    print('2 in [1,2,3,"4"]')
else:
    print("not number in")

if not 1 in [2,3,4]:
    print('1 not in [1,2,3]')
else:
    print('1 in [1,2,3]')
```

结果

```
1 in [1,2,3,"4"]
1 not in [1,2,3]
```
一个选择

if ：

两个选择

if :

else:

多个选择

if :

elif :

......

else:
