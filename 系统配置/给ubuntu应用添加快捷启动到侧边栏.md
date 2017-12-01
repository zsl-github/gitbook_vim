---
title: 给ubuntu应用添加快捷启动到侧边栏
date: 2017-01-23 09:02:39
tags: 系统配置
categories:
    - 应用
    - 
password: 
---


在`ubuntu`下使用`android studio`的时候，只能通过
```
./studio.sh
```
的方式来启动应用，而且启动以后会占用一个终端窗口，很不方便。之前按照网上的方法，一直添加不成功。按照网上的方法，具体步骤如下

**1.创建desktop文件**
Ubuntu的快捷方式都在`/usr/share/applications/` 下，需在此文件夹下创建 `.desktop文件`

```
sudo vim /usr/share/applications/android_studio.desktop
```

添加内容如下

```
[Desktop Entry]
Type=Application
Name=Android Studio
Exec=/opt/studio/android-studio/bin/studio.sh %F
Icon=/opt/studio/android-studio/bin/studio.png
Categories=development;IDE;
Terminal=false
StartupNotify=true
StartupWMClass=jetbrains-android-studio
```
其中`Exec` `Icon`的路径需要根据自己应用存放的路径进行修改。我的`studio`存放路径是

`/opt/studio/android-studio/`

**2.给创建的desktop添加权限**

执行如下操作

```
sudo chmod +x /usr/share/applications/android_studio.desktop
```
**3.添加到Launcher**

打开`desktop`所在文件目录

```
sudo nautilus /usr/share/applications
```

然后把对应的图标拖到`Launcher`上就行了

## 问题来了

通过上面的步骤，发现在第三步的时候，根本没法把图标拖到`Launcher`上.如果通过命令行打开应用，然后在固定到`Launcher`上，结果点击图标没反应。这个问题纠结了很久，
最后结果各种尝试，终于发现了问题所在，原来还需要给`Exec`中添加的启动文件添加可执行权限

```
sudo chmod a+x /opt/studio/android-studio/bin/studio.sh
```

问题就可以解决了

## Desktop 文件内容解释

**Name**
必选
该数值指定了相关应用程序的名称,快捷方式的显示名称由该键字的数值所决定

**Icon**
必选
快捷方式所使用的图标由该关键字的数值来决定

**Comment**
可选
该数值是对当前`Desktop Entry`的简单描述。

**Type**
必选
关键字`Type`定义了`Desktop Entry`文件的类型。常见的`Type`数值是`Application`和`Link`
`Type = Application`表示当前`Desktop Entry`文件指向了一个应用程序
`Type = Link`表示当前`Desktop Entry`文件指向了一个`URL`

**Exec**
可选
此关键字只有在`Type`类型是`Application`时才有意义，它的数值定义了启动指定应用程序所要执行的命令，在此命令是可以带参数的

**Terminal**
可选
是否在终端中运行，当`Type`为`Application`，此项有效

**Categories**
可选
注明在菜单栏中显示的类别
