---
title: Inspect Code常见问题
date: 2018-1-24 14:23:18
tags: [重构]
categories: 重构
---
#### `Empty 'catch' block` 空的catch代码块
问题：发生错误程序没有任何反应，不方便定位问题
``` java
try {
	...
} catch (Exception e) {

}
```	

#### `Unused import` 
问题：Ctrl+Alt+O 处理即可
