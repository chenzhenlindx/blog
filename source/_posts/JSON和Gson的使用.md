---
title: Json和Gson的使用
date: 2018-1-21 07:54:34
tags: [Json,Gson]
categories: Json
---
##### 泛型的使用
通常使用`new TypeToken<XXX>(){}`来获取Type之后转换
``` java
Type type = new TypeToken<Subsys<FireData>>(){}.getType();
subsys.Fire=new Gson().fromJson(obj.optString("Fire"),type);
```	