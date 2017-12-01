---
title: 配置hexo流程图插件
date: 2017-02-26 20:58:39
tags: 系统配置
categories: 
    - 博客
    - 
password: 
---


# 问题背景
使用`github`、`hexo`搭建好博客平台以后，发现不能够识别`markdown`中的流程图，就在网上百度了一下，发现可以通过安装插件来解决，现在记录一下具体的方法

# 具体安装

直接在博客目录`Hexo`下执行如下命令即可
```bash
sudo npm install --save hexo-filter-flowchart
```

# 验证结果

创建文档，然后在文档中添加如下内容

```
st=>start: 开始
ed=>end: 结束
op1=>operation: 考试
cond=>condition: 是否达标？
op2=>operation: 补考
op3=>operation: 通过

st->op1->cond(yes)->ed
cond(no)->op2(right)->op1
```
显示结果如下

```flow
st=>start: 开始
ed=>end: 结束
op1=>operation: 考试
cond=>condition: 是否达标？
op2=>operation: 补考
op3=>operation: 通过

st->op1->cond(yes)->ed
cond(no)->op2(right)->op1
```
