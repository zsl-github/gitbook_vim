---
title: python学习之数据类型
date: 2017-03-07 10:39:28
tags: python
categories:
    - 基础
    - 
password: 
---

因为自己是根据网上教程学习的，所以以下内容参考自[学习](http://www.w3cschool.cc/python/python-variable-types.html)

`python`支持物种数据类型，分别是

- Numbers（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Dictionary（字典）

它们具体划分为

**number类型**

- int（有符号整型）
- long（长整型[也可以代表八进制和十六进制]）
- float（浮点型）
- complex（复数）

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/py_study_1.png)

长整型也可以使用小写"L"，但是还是建议您使用大写"L"，避免与数字"1"混淆。Python使用"L"来显示长整型。
`Python`还支持复数，复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型

**字符串**

字符串或串(String)是由数字、字母、下划线组成的一串字符。

```python
s = "hello world"

s2 = "hello_one_123"
```

**列表**

`List`（列表） 是 `Python` 中使用最频繁的数据类型。

列表可以完成大多数集合类的数据结构实现。它支持字符，数字，字符串甚至可以包含列表（所谓嵌套）。

列表用[ ]标识。是python最通用的复合数据类型。看这段代码就明白

```python
s = ["xiaohong","xiaoli","xiaobai","xiaoming","xiaohua"]
s = ["xiaohong","xiaoli","xiaobai","xiaoming",["nihoa","qiantao"],34.5]
```

**元组**

元组是另一个数据类型，类似于List（列表）。

元组用"()"标识。内部元素用逗号隔开。但是元素不能二次赋值，相当于只读列表。

```python
s = ("xiaohong","xiaoli","xiaobai","xiaoming","xiaohua")
```

**字典**

字典(dictionary)是除列表以外python之中最灵活的内置数据结构类型。列表是有序的对象结合，字典是无序的对象集合。

两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。


```python
s = {"name":"zhangshuli","year":28,"sex":"man"}
```
