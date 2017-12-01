---
title: 分屏下编辑短信出现ANR
date: 2017-04-19 08:53:00
tags: android
categories:
    - ANR
    - 短信
password: 
---

# 问题描述
在分屏情况下,点击`编辑`按钮，短信出现`ANR`

# 原因分析

在`LOG`中搜索关键字`ANR`，发现如下LOG

```
10-21 16:33:36.547 3683 3683 W ActivityThread: handle msg : 110 took time = 858 
10-21 16:33:36.557 1414 1467 E ActivityManager: ANR in com.baidu.input_****
10-21 16:33:36.557 1414 1467 E ActivityManager: PID: 2824 
10-21 16:33:36.557 1414 1467 E ActivityManager: Reason: executing service com.baidu.input_****/.ImeService 
10-21 16:33:36.557 1414 1467 E ActivityManager: Load: 0.0 / 0.0 / 0.0 
10-21 16:33:36.557 1414 1467 E ActivityManager: CPU usage from 12106ms to 1755ms ago (2017-10-21 16:33:24.199 to 2017-10-21 16:33:34.551): 
10-21 16:33:36.557 1414 1467 E ActivityManager: 60% 516/surfaceflinger: 35% user + 25% kernel / faults: 6004 minor 
10-21 16:33:36.557 1414 1467 E ActivityManager: 29% 1414/system_server: 18% user + 11% kernel / faults: 1329 minor 
1 

10-21 16:33:42.414 1414 1467 E ActivityManager: ANR in com.android.mms (com.android.mms/.ui.BootActivity) 
10-21 16:33:42.414 1414 1467 E ActivityManager: PID: 3443 
10-21 16:33:42.414 1414 1467 E ActivityManager: Reason: Input dispatching timed out (Waiting to send non-key event because the touched window has not finished processing certain input events that were delivered to it over 500.0ms ago. Wait queue length: 6. Wait queue head age: 6146.3ms.) 
10-21 16:33:42.414 1414 1467 E ActivityManager: Load: 7.3 / 2.41 / 0.85 
10-21 16:33:42.414 1414 1467 E ActivityManager: CPU usage from 2008ms to -5357ms ago (2017-10-21 16:33:34.551 to 2017-10-21 16:33:42.351): 
10-21 16:33:42.414 1414 1467 E ActivityManager: 123% 3443/com.android.mms: 115% user + 8% kernel / faults: 22781 minor 38 major 
10 
```
可以初步看到发生`ANR`的原因可能跟输入法有关，接下来就让输入法的人去分析了。当然，这个只是初步定位，真的要找到根因，这些肯定是远远不够的。

