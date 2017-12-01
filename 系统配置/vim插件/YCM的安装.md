---
title: YCM的安装跟应用
date: 2017-01-31 20:03:32
tags: vim-plugin
categories:
    - vim
    - plugin
password: 
---

# YCM 安装以及应用

折腾了两天，现在终于可以成功使用YCM了，现在记录下安装此插件的过程

# 前提条件

vim保证7.3以上
ubuntu 下可以直接通过`sudo apt-get install vim`进行安装

# 安装cmake

CMake是一个比make更高级的编译配置工具，它可以根据不同平台、不同的编译器，生成相应的Makefile或者vcproj项目
ubuntu 下可以通过`sudo apt-get install cmake`进行安装

# 安装YCM插件

## 通过vundle 安装

直接添加 `Bundle 'Valloric/YouCompleteMe'` 到.vimrc中,然后运行`BundleInstall`

安装完以后，你打开`vim`，可能会有如下提示

```
YouCompleteme unavailable : no module named future
```

在你执行如下操作

```
cd ~/.vim/bundle/YouCompleteMe
./install.py --clang-completer //支持C语言
```

这时候会报错，如下

```
ERROR: some folders in /home/zsl1989/.vim/bundle/YouCompleteMe/third_party/ycmd/third_party are empty; you probably forgot to run:
        git submodule update --init --recursive

```

上面两个错误，我们按照上面的提示，直接执行

```
git submodule update --init --recursive
```
然后再执行

```
./install.py --clang-completer //支持C语言
```
就可以了

![pic alt](https://github.com/zsl-github/blog/raw/master/source/picture/ycm_install_1.png)


# 安装LLVM-Clang、Clang 标准库

## clang

Clang是一个C语言、C++、Objective-C、C++语言的轻量级编译器可以通过`sudo apt-get install clang` 进行安装

## llvm

一种编译器构架系统

安装的话，直接下载预编译的文件，解压以后，然后拷贝到本地目录`/usr/local/`即可
```
http://llvm.org/releases/download.html->Download LLVM 3.9.0->Pre-Built Binaries->Clang for x86_64 Ubuntu 16.04 (.sig)
```

# 编译构建 ycm_core 库

## 创建一个目录放编译过程中产生的文件

```
mkdir ~/.ycm_build
cd ~/.ycm_build
```
然后执行如下命令

```
cmake -G "Unix Makefiles" -DPATH_TO_LLVM_ROOT=~/ycm_temp/llvm_root_dir . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp

```
这时候报错了，报错信息如下

```

Using libclang to provide semantic completion for C/C++/ObjC
CMake Error at ycm/CMakeLists.txt:341 (message):
      Using Clang completer, but no libclang found.  Try setting
        EXTERNAL_LIBCLANG_PATH or revise your configuration


      -- Configuring incomplete, errors occurred!
      See also "/home/zsl1989/.ycm_build/CMakeFiles/CMakeOutput.log".

```
原来是发现上面的`DPATH_TO_LLVM_ROOT=~/ycm_build/llvm_root_dir`弄错了，它的意思是引用`llvm`。所有，要把上面下载的`llvm`当道这个目录下。把下载的`llvm`解压到`.ycm_temp`即可，然后执行

```
cmake -G "Unix Makefiles" -DPATH_TO_LLVM_ROOT=~/.ycm_temp/clang+llvm-4.0.0-x86_64-linux-gnu-ubuntu-16.04 . ~/.vim/bundle/YouCompleteMe/third_party/ycmd/cpp

```

执行成功

```
-- The C compiler identification is GNU 5.4.0
-- The CXX compiler identification is GNU 5.4.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Found PythonLibs: /usr/lib/x86_64-linux-gnu/libpython2.7.so (found suitable version "2.7.12", minimum required is "2.6")
Using libclang to provide semantic completion for C/C++/ObjC
Using external libclang: /home/zsl1989/.ycm_build/llvm/lib/libclang.so.4.0
-- Configuring done
-- Generating done
-- Build files have been written to: /home/zsl1989/.ycm_build

```

## 构建 ycm_core

执行如下命令


```
cmake --build . --target ycm_core --config Release

```
她会在ycm下生成一个`ycm_core.so`

至此，YCM插件就算完全弄好了。

# 配置

1. 复制 .ycm_extra_conf.py

```
cp ~/.vim/bundle/YouCompleteMe/third_party/ycmd/examples/.ycm_extra_conf.py ~/.vim/
```

2. 配置.vim

在.vimrc中添加如下内容

```vim
let g:ycm_server_python_interpreter='/usr/bin/python'
let g:ycm_global_ycm_extra_conf='~/.vim/.ycm_extra_conf.py'
```
至此此插件已经完全安装成功

# YCM使用
YCM目前主要作用其实是智能联想，也就是在写C等语言的时候，可以自动补充。除此功能以外，还有就是代码的跳转，主要是有如下几个方法

## GoToInclude

此命令是用来跳转到头文件

比如，我们光标位于如下代码处
```C
#include <stdio.h>
```
这时候执行命令`YcmCompleter GoToInclude`就会跳转到stdio.h文件中去。当然，目前就发现仅仅是适用于C、C++，而且只能寻找系统的一些库文件

## GoToDeclaration

这个命令是用来跳转到变量声明的地方
比如，我们如果光标位于如下代码的zsl处，执行命令`YcmCompleter GoToDeclaration`，就会跳到`int zsl`的地方
```C
int zsl;
zsl = 4;
```
可惜的是，仍然只支持C、CPP、python，不支持java

## GoTo

感觉跟GoToDeclaration差不多啊，都是跳转到定义处

这个插件唯一可惜的就是，对java的支持实在太少了，大都是对C等的支持。等后续在补充吧

