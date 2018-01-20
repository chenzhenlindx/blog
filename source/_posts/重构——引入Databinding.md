---
title: 重构——引入Databinding
date: 2018-1-20 09:55:20
tags: [重构,Databinding]
categories: 重构
---
###### 添加DataBinding支持
在Android Studio上使用，需要在module级别的build.gradle上添加对DataBinding的支持
``` bash
android {
    ....
    dataBinding {
        enabled = true
    }
}
```




  