---
title: 重构——引入Databinding
date: 2018-1-20 09:55:20
tags: [重构,Databinding]
categories: 重构
---
##### 添加DataBinding支持
在Android Studio上使用，需要在module级别的build.gradle上添加对DataBinding的支持
``` bash
android {
    ....
    dataBinding {
        enabled = true
    }
}
```
##### 布局文件
DataBinding的layout files和普通的非DataBinding布局文件是有一些区别的，下面是一个基础的使用了DataBinding的布局文件
``` bash
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">

    <data>
        <variable
            name="user"
            type="com.example.User" />
    </data>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.firstName}" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@{user.lastName}" />
    </LinearLayout>
</layout>
```
##### 事件处理
###### 点击事件`onClick`
`android:onClick="@{()->viewModel.logout()}`
``` bash
<Button
   android:id="@+id/log_out"
   android:layout_width="268dp"
   android:layout_height="32dp"
   android:layout_centerHorizontal="true"
   android:layout_marginTop="40dp"
   android:onClick="@{()->viewModel.logout()}"
   android:background="@drawable/round_corner_logout"
   android:text="@string/log_out"
   android:textColor="#ffffff"
   android:textSize="14sp" />
```
###### `ViewPager`   
- `setCurrentItem(int item)`
- 在ViewModel创建`public ObservableInt mCurrentIndex = new ObservableInt();`然后在xml关联
``` bash
<android.support.v4.view.ViewPager
   android:id="@+id/id_viewpager"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   app:currentItem="@{viewModel.mCurrentIndex}" />
```
	
- 通过设置mCurrentIndex的值，即可改变ViewPager的显示位置
``` bash
public void setCurrentItem(int currentIndex) {
  mCurrentIndex.set(currentIndex);
}
```
##### View的可见状态控制
- 引入View `<import type="android.view.View" />`
- 通过`?:`来控制显示
``` bash
android:visibility="@{viewModel.isVisible?View.VISIBLE:View.GONE}"
```
##### 颜色控制
- 通过`?:`来控制显示
``` bash
android:textColor="@{viewModel.isBlue?@color/blue:@color/gray}"
```

  