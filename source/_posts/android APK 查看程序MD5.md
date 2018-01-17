---
title: android APK 查看程序MD5
date: 2016-2-19 14:36:02
tags: [APK,MD5]
categories: 工具
---
为了避免注册时弄错签名，建议直接用打包出来的apk查看签名，具体如下：  
1. 将apk修改后缀为 .rar文件后解压；  
2. 进入解压后的META-INF目录，该目录下会存在文件CERT.RSA  
3. 在该目录下打开cmd，输入命令 ：
``` bash
keytool -printcert -file CERT.RSA
```
这里将会显示出MD5和SHA1签名。

![](/images/android APK查看程序MD5.png)
  