---
title: 《Android群英传》读书笔记
date: 2015-11-15 09:55:05
tags: [Android,群英传]
categories: 读书笔记
---
#### 第1章 Android体系与系统架构

* `Android`四大组件：`Activity`、`BroadcastReceiver`、`ContentProvider`和`Service`。 <sub>(P5)</sub>
* [androidxref.com-Android源代码网站](http://androidxref.com/)<sub>(P6)</sub>

#### 第2章 Android开发工具新接触

* [AndroidDevTools-Android镜像网站](http://www.androiddevtools.cn/)<sub>(P15)</sub>
* **ADB命令使用技巧**<sub>(P24)</sub>
	* 获取adb版本 
	  ``` bash
	  adb version
	  ```
	  得到 
	  ``` bash
	  Android Debug Bridge version 1.0.39
	  ```
	* 进入Shell命令 
	  ``` bash
	  adb shell
	  ```
	* 退出Shell命令 
	  ``` bash
	  exit
	  ```
	* 显示设备列表 
	  ``` bash
	  adb devices
	  ```
	* 安装Apk程序之Install 
	  ``` bash
	  adb install -r F:\Test.apk
	  ```
	* 安装Apk程序之Push 
	  ``` bash
	  adb push F:\Test.apk /data/app/
	  ```
	* 输出所有已经安装的应用 
	  ``` bash
	  adb shell pm list package -f
	  ```
	* 模拟按键输入
	  ``` bash
	  adb shell input keyevent 3
	  ```
	  一些常用的Code
	  ``` bash
		input keyevent 82 menu
		input keyevent 3  home
		input keyevent 19 up
		input keyevent 20 down
		input keyevent 21 left
		input keyevent 22 right
		input keyevent 66 enter
		input keyevent 4  back
	  ```
	* 模拟滑动输入
	  ``` bash
	  adb shell input touchscreen swipe <x1> <y1> <x2> <y2>
	  adb shell input touchscreen swipe 18 665 300 665
	  ```
	* 重新启动
	  ``` bash
	  adb reboot
	  ```