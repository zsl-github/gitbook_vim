---
title: ubuntu下安装网易云音乐
date: 2017-04-06 16:44:59
tags: 系统配置
categories:
    - 电脑环境
    - 工具
password: 
---

从[官网](http://music.163.com/#/download)下载安装包，选择`linux`版本

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/netease_download.png)

直接使用`dpkg`安装的话，会有很多依赖，我按照提示，尝试逐个安装依赖，折腾了很久也没有成功，最终还是选择了网上的建议，使用`gdebi-core`安装

```
 sudo apt install gdebi-core
```
注意安装前要把网易音乐卸载，卸载命令如下

```
udo apt-get remove --purge netease-cloud-music
sudo apt-get autoremove --purge netease-cloud-music
dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P
```
然后使用`gdebi`来直接安装

```
sudo gdebi ~/Downloads/netease-cloud-music_1.0.0-2_amd64_ubuntu16.04.deb
```
用这个命令安装的时候，它会自动去安装缺少的依赖，真的方便很多。

然后你就可以使用了


![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/netease_suss.png)

