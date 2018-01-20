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
##### `ViewPager`   
###### `setCurrentItem(int item)`
- 在ViewModel创建`public ObservableInt mCurrentIndex = new ObservableInt();`
- 在xml关联`app:currentItem="@{viewModel.mCurrentIndex}"`
- 通过设置mCurrentIndex的值，即可改变ViewPager的显示位置
``` bash
public void setCurrentItem(int currentIndex) {
  mCurrentIndex.set(currentIndex);
}
```
###### `setAdapter(PagerAdapter adapter)`
- 在ViewModel创建`public FragmentPagerAdapter mAdapter;`
- 在`setViewModel`前，需要初始化`mAdapter`
- 在xml关联`app:adapter="@{viewModel.mAdapter}"`

###### `setOnPageChangeListener(OnPageChangeListener listener)`
- 在ViewModel实现接口`implements OnPageChangeListener`
- 重写`OnPageChangeListener`的方法
``` bash
public void onPageScrollStateChanged(int state) {

}

public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

}

public void onPageSelected(int position) {
    mCurrentIndex.set(position);
}
```
- 在xml关联`app:onPageChangeListener="@{viewModel}"`

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
##### Include布局
Include这个布局标签在DataBinding布局里面使用有点特殊, 因为它需要我们传递绑定的方法和数据对象。比如我们有以下的include布局:
``` bash
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <import type="android.view.View" />
        <variable
            name="viewModel"
            type="com.zhhx.fragment.IbmsMainFragment" />
    </data>
</layout>
```
那么我们在另一个xml引用时,就需要传递这个include需要绑定的方法和数据:
``` bash
<include
   layout="@layout/include_sub_system_airconditioner"
   app:viewModel="@{viewModel}" />
```						