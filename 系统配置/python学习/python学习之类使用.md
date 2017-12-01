---
title: python学习之类使用
date: 2017-03-11 14:34:34
tags: python
categories:
    - 基础
    - 
password: 
---

既然也是面向对象编程的语言，自然也就跟`java`相似，它也有类的概念。今天就简单学习下。看如下代码

```python
#!/usr/bin/python
class cl_test:
    test = 0
    def _init_(self):
        print("my frist class")

    def display(self):
        print("you are display")

    def add_test(self):
        cl_test.test+=1

    def dis_test(self):
        print(cl_test.test)

    def dis_test(self,aa=2):
        print(self.test)


cc = cl_test()
cc.display()
for i in range(20):
    cc.add_test()
cc.dis_test()

cc1 = cl_test()
cc1.dis_test(4)
```

**知识点**

知识点

1.创建格式 class name(parent):

括号里面的是继承父类（可以缺省）

2.它有一个初始化函数

_init_(固定格式)

3.它里面的方法定义

def name(self,....):

类里面的方法，必须含有self参数（像当于java中的this），而且必须放在第一个位置

4.在calss中定义的成员变量是共有的

5.class的引用，只需要

aa = name(..)就行了，参数跟_init_里的参数一致（self省略）

看下输出结果

```
you are display
20
20
```
