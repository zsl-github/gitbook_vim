---
title: python学习之文件访问
date: 2017-03-03 09:50:04
tags: python
categories:
    - 基础知识
    - 
password: 
---

今天继续学习`python`，因为是根据网上的教程，里面用到了一些例子，包含有后面的知识点。但是，因为自己稍微有点c、java等语言基础，所以并没有严格按照教程来学习，反而是遇到知识点就记录下来。

代码如下

```python
#!/usr/bin/python3.2

import sys

file_name = sys.argv[1]
file_finish = "finish"
file_text = ""
try:
  # open file stream
  file = open(file_name, "a")
except IOError:
  print("There was an error writing to"+file_name)
  sys.exit()
print("Enter '"+file_finish+"' When finished")
while 1==1:
  file_text = raw_input("Enter text: ")
  if file_text == file_finish:
    # close the file
    file.close
    break
  file.write(file_text)
  file.write("\n")
file.close()
file_name = raw_input("Enter filename: ")
if len(file_name) == 0:
  print("Next time please enter something")
  sys.exit()
try:
  file = open(file_name, "r")
except IOError:
  print("There was an error reading file")
  sys.exit()
file_text = file.read()
file.close()
print(file_text)
```
**知识点总结**

> string = sys.argv[i]

获得命令行的参数

> file = open("file_name","style")

打开一个文件,`style`是文件操作类型
打开文件有三种样式
- r-只读文件（如果文件不存在，会在当前目录下创建）

```python
string = file.read()
```
- w-写文件(如果文件不存在，会抛异常，它每次成功打开文件，都会把原文件清空以后重新写入)

```python
file.write(string)
file.write("/n")#换行
```

- a-a-追加写文件（在源文件末尾，追加内容，语法上跟w一样)

```python
try:
    open(file_name,style)
exception IOError:
    chuli
```
每次操作完以后，要关闭文件

```python
file = open(file_name,style)
file.close()
```
> exit()

退出程序

1. 执行完代码以后，自动推出

2. 遇到问题中途推出

```python
sys.exit()
```

> while ** :
    pass

进行循环操作
