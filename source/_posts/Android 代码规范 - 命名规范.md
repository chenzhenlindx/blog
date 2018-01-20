---
title: Android 代码规范 - 命名规范
date: 2018-1-20 07:20:00
tags: [代码规范]
categories: 命名规范
---
## Layout
Layout的命名规则需要和使用他们的组件对应，方便查找和维护，比如我们在创建一个用户信息的`UserInfoActivity`，对应的Layout的命名就应该是`activity_user_info.xml`。对应Andorid组件的`Layout`命名规则:
- Activity  ->   `activity_user_info.xml`
- Fragment  ->   `fragment_sign_up.xml`
- Dialog    ->   `dialog_change_password.xml`
- AdapterView Item  ->  `item_user.xml`
- include  ->    `include_stats_bar.xml`
  