---
title: Android String.xml 相关
date: 2018-1-22 11:33:04
tags: [重构,Databinding]
categories: 重构
---
#### 动态显示
> %n$ms：代表输出的是字符串，n代表是第几个参数，设置m的值可以在输出之前放置空格
> %n$md：代表输出的是整数，n代表是第几个参数，设置m的值可以在输出之前放置空格
> %n$mf：代表输出的是浮点数，n代表是第几个参数，设置m的值可以控制小数位数，如m=2.2时，输出格式为00.00
##### %1$s
``` xml
<string name="loading">离下班回家还剩%1$s分钟</string>
```
``` java
String temp = getResources().getString(R.string.loading);
String timeTip = String.format(temp,38);
```
结果：离下班回家还剩**38**分钟
##### %1$3s
``` xml
<string name="loading">离下班回家还剩%1$4s分钟</string>
```
``` java
String temp = getResources().getString(R.string.loading);
String timeTip = String.format(temp,38);
```
结果：离下班回家还剩**  38**分钟
注：m设置为4只有2个空格
##### %1$2.2f
``` xml
<string name="loading">离下班回家还剩%1$2.2f分钟</string>
```
``` java
String temp = getResources().getString(R.string.loading);
String timeTip = String.format(temp,1234.123);
```
结果：离下班回家还剩**1234.12**分钟

#### %的显示
使用%%来显示一个%
``` xml
<string name="loading">离下班回家还剩%1$2.2f%%分钟</string>
```
``` java
String temp = getResources().getString(R.string.loading);
String timeTip = String.format(temp,34.123);
```
结果：离下班回家还剩**34.12%**分钟

#### 空格的显示
##### `&#8194;`半个中文字
``` xml
<string name="num">今日&#8194;&#8194;&#8194;&#8194;工单总数</string>
```
结果：
![](/images/string显示空格.png)