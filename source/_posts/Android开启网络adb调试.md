---
title: Android开启网络adb调试
date: 2018-1-27 10:45:06
tags: [adb,调试]
categories: 调试
---
- 首先开启手机的USB调试并且使用USB连接电脑
- 在终端执行`adb tcpip 5555`：
``` bash
C:\Users\LENOVO>adb tcpip 5555
restarting in TCP mode port: 5555
```
- 在终端执行`adb connect 192.168.2.106`：
``` bash
C:\Users\LENOVO>adb connect 192.168.2.106
connected to 192.168.2.106:5555
```
- 如果要断开连接
在终端执行`adb disconnect`：
``` bash
C:\Users\LENOVO>adb disconnect
disconnected everything
```