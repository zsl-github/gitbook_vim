---
title: python学习之数据类型转换
date: 2017-03-08 11:38:52
tags: python
categories:
    - 基础
    - 
password: 
---

有时候，我们需要对数据内置的类型进行转换，数据类型的转换，你只需要将数据类型作为函数名即可。

以下几个内置的函数可以执行数据类型之间的转换。这些函数返回一个新的对象，表示转换的值。

```python
#!/usr/bin/python
print(int("123"))
print(long("123"))
print(float("123"))
print(complex(10,20))
print(str(123))
print(str(["heha",1234]))
print(repr({"1":1,"2":2}))
print(eval("3*2"))
print(eval("3+9*5"))
print(tuple((3,4,6,6,7))[1])
print(list((1,2,3,4,5)))
print(set("zhang"))
s = set("zhangshuli")
print(s)
print(dict(((1,2),(3,4),(5,6))))
print(frozenset("zhangshuli"))
s = frozenset("shuli")
print(s)
#s[0] =1
print(chr(98))
print(unichr(20))
print(ord("b"))
print(hex(15))
print(oct(15))
```

结果如下

```
123
123.0
(10+20j)
['heha', 1234]
{'1': 1, '2': 2}
48
[1, 2, 3, 4, 5]
set(['a', 'h', 'z', 'g', 'n'])
set(['a', 'g', 'i', 'h', 'l', 'n', 's', 'u', 'z'])
{1: 2, 3: 4, 5: 6}
frozenset(['a', 'g', 'i', 'h', 'l', 'n', 's', 'u', 'z'])
frozenset(['i', 'h', 's', 'u', 'l'])
b

0xf
```
具体转换方法如下表

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/py_study_2.png)

