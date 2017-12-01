---
title: 分析问题心得(1)
date: 2017-04-20 08:45:25
tags: 工作
categories:
    - 工作
    - 总结
password: 
---

# 小窍门一

测试提供的`LOG`有时候会提供截图给我们，如果测试是通过手机截图，而且没有更改图片默认名称的话，我们是可以通过图片名称来确认截图的时间点的，也就获得了问题发生的详细时间点
如果用户把默认截图名称修改了的话，那么我们可以通过在`LOG`中搜索关键字`Screenshots`或获取截图时间，如下

```
10-21 19:44:50.642   524   539 V MediaProvider: insertInternal>>>: content://media/external/images/media, value=mime_type=image/png title=20171021-194450.png _data=/storage/emulated/0/Pictures/Screenshots/20171021-194450.png _size=107656 date_added=1508586290 width=720 height=1440 date_modified=1508586290 datetaken=1508586290226 _display_name=20171021-194450.png, match=1
```

