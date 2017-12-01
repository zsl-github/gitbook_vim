---
title: Markdown 解释语言(二)
date: 2017-02-11 20:18:32
tags: tools
categories:
    - markdown
    - 基础知识
password: 
---


现在我们继续学习Markdown语法

# 行内使用代码快

两个反引号表示块引用
现在我们开始学习\`markdown\`
现在我们开始学习`markdown`

# 代码块

四个空格表示代码块
    四个空格表示代码块

    for i in [1,3,5]:
        print("aaa")

# 插入图片

使用\!\[描述](图片路径)

# 删除线

使用\~~删除内容~~

~~删除线~~

输出目录结构
[TOC]

# 标签

标签: 你好吗

# 注脚
这是一个注脚[^footnote]

# LaTex公式
\$表示行内公式

 hha $E=mc^2$ hhh

\$$表示整行公式

$$E=mc^2$$

# 加强代码块

```
sudo apt install vim
```

```python
for i in [1,2,3]:
    print(i)
```

```java
private int getInt() {
    return 3;
}
```

# 流程图

```flow

```
单独学习语法

# 序列图
```seq
```
单独学习

# 甘特图
```gantt
```

后续学习

# Mermaid 流程图
```graphLR
```
后续学习

# Mermaid 序列图
```sequence
```
后续学习
# 表格
|哈哈　｜呵呵｜你好|
| -------- | ----: | :----:  |

[^footnote]: 这是一个注脚的文本

- [ ] **Cmd Markdown 开发**
    - [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
    - [ ] 支持以 PDF 格式导出文稿
    - [x] 新增Todo列表功能


