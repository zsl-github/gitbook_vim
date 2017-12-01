---
title: python打开网页操作
date: 2017-03-16 16:53:24
tags: python
categories:
    - 脚本
    - 
password: 
---

最近一直想通过python来实现网页的操作。因为想把自己vimrc的配置直接通过脚本来实现。比较理想的情况下，就是通过一个脚本，把自己需要的一些资源直接从网上下载下来，然后再对下载的资源进行安装等操作。不过，一直没办法实现下载功能。从网上找了一个例子，虽然可以进行下载，但是，这个下载只能针对特定的资源来下来。如果你仅仅是进入一个网页，然后需要点击下载操作，这个方法就不行了。无论如何，先记录下来吧，以后掌握的好了，再慢慢的完善


```python
#!/usr/bin/python
#encoding:utf-8
import urllib
import os
import webbrowser
def Schedule(a,b,c):
    '''''
    a:已经下载的数据块
    b:数据块的大小
    c:远程文件的大小
   '''
    per = 100.0 * a * b / c
    if per > 100 :
        per = 100
    print '%.2f%%' % per
#url = 'http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tar.bz2'
url = 'http://pan.baidu.com/s/1sj4ZfTb/javascript:void(0)'
#这个方法有三个参数，第一个是地址
#第二个下载以后文件的存储路径跟文件名
#第三个参数是下载过程中的回调函数
#如果地址文件不存在，也不会报错
urllib.urlretrieve(url,'pan.zip',Schedule)
#打开一个网页
webbrowser.open(url)
```

