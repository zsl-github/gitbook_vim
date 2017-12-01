---
title: python学习之运算符
date: 2017-03-09 11:40:37
tags: python
categories:
    - 基础
    - 
password: 
---

Python语言支持以下类型的运算符:

- 算术运算符
- 比较（关系）运算符
- 赋值运算符
- 逻辑运算符
- 位运算符
- 成员运算符
- 身份运算符
- 运算符优先级


**算数运算符**

```python
a = 23.6
b = 14.7
print(a+b)
print(a-b)
print(a*b)
print(a/b)
print(a%b)
print(a//b)
print(a**b)
print("======================")
a = 10
b = 2
print(a+b)
print(a-b)
print(a*b)
print(a/b)
print(a%b)
print(a//b)
print(a**b)
```

结果

```
38.3
8.9
346.92
1.60544217687
8.9
1.0
1.51987000319e+20
======================
8
5
5
```

总结

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/py_study_3.png)

**关系运算符**

```python
a = 2
b = 1
print(a > b)
print(a < b)
print(a <> b)
print(a == b)
print(a != b)
print(a >= b)
print(a <= b)
```

结果

```
True
False
True
False
True
True
False
```

总结

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/py_study_4.png)

**位运算符**

代码

```python
a = 2#00000010
b = 1#00000001
print(a & b)
print(a | b)
print(a ^ b)
print(~a)
print(a>>1)
print(a<<2)
```

结果

```
0
3
3
-3
1
8
```

总结

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/py_study_5.png)

**逻辑运算符**

代码

```python
if 0 and 1:
    print("0 and 1 False")
print("0 and 1",(0 and 1))
print("False and 1",(False and 1))
print("True and 1",(True and 1))
print("\n")
if 2 or 3:
    print("2 and 3 True")
print("2 or 3",(2 or 3))
print("True or 3",(True or 3))
print("False or 3",(False or 3))
print("\n")
if not 0:
    print("not 0 True")

print("not 0",(not 0))
print("not False",(not False))
print("not 3",(not 3))
print("not True",(not True))
```

结果

```
('0 and 1', 0)
('False and 1', False)
('True and 1', 1)
and 3 True
('2 or 3', 2)
('True or 3', True)
('False or 3', 3)


not 0 True
('not 0', True)
('not False', True)
('not 3', False)
('not True', False)
```

总结

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/py_study_6.png)

**成员运算符**

代码

```python
a ="hahaha"
b = "hahaha"
print(a is b)
print(id(a) == id(b))
```

结果

```
a ="hahaha"
b = "hahaha"
print(a is b)
print(id(a) == id(b))
```

总结

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/py_study_7.png)
