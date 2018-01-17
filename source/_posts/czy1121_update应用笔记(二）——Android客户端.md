---
title: czy1121_update应用笔记(二）——Android客户端
date: 2018-1-16 19:48:44
tags: [update]
categories: 第三方库
---
最近做的一个APP需要检测更新，以前都是自己写的，这次决定使用别人封装好的一个库。

[czy1121/update](https://github.com/czy1121/update)
清晰灵活简单易用的应用更新库

第一步，需要搭建一个检测更新的服务。详见
[czy1121_update应用笔记(一）——搭建服务](https://chenzhenlindx.github.io/2018/01/16/czy1121_update%E5%BA%94%E7%94%A8%E7%AC%94%E8%AE%B0(%E4%B8%80%EF%BC%89%E2%80%94%E2%80%94%E6%90%AD%E5%BB%BA%E6%9C%8D%E5%8A%A1/)

第二步，就是编写Android客户端代码了。
- 创建Android工程；
- 导入相关的库
 - [czy1121/update](https://github.com/czy1121/update)
 ``` bash
 repositories { 
    maven { url "https://jitpack.io" }
 }
 dependencies {
    implementation 'com.github.czy1121:update:1.1.1'
 }
 ```
 - [afollestad/material-dialogs](https://github.com/afollestad/material-dialogs)
 ``` bash
 dependencies {
   implementation 'com.afollestad.material-dialogs:core:0.9.5.0'
   implementation 'com.afollestad.material-dialogs:commons:0.9.5.0'
 }
 ```
 - [Blankj/AndroidUtilCode](https://github.com/Blankj/AndroidUtilCode)
 ``` bash
 dependencies {
   implementation 'com.blankj:utilcode:1.9.2'
 }
 ```
 需要设置Application并且初始化Utils
 ``` bash
 public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Utils.init(this);
    }
 }
 ```
 ``` bash
 <application
        android:name=".MyApplication"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
 </application>
 ```
 - 修改app下的build.gradle
 
 ``` bash
 defaultConfig {
        applicationId "cn.czl.updatedemo"
        minSdkVersion 16
        targetSdkVersion 22
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
 }
 ```
 ``` bash
 compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
 }
 ```
 - 修改xml布局文件,新增一个TextView用于显示版本，新增一个Button用于主动更新
 
 ``` bash
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="cn.czl.updatedemo.MainActivity">

    <TextView
        android:id="@+id/tv_version"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="30dp"
        android:text="检查更新"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/tv_version" />

</android.support.constraint.ConstraintLayout>
 ```
 - 在MainActivity中编写更新方法，自定义下载提示框、进度框。
 ``` bash
    /**
     * 根据 agent.getInfo() 显示更新版本对话框，具体可参考 {@link UpdateAgent.DefaultUpdatePrompter}
     *
     * @param isManual
     */
    public void update(boolean isManual) {
        // 设置默认更新接口地址与渠道
        UpdateManager.setUrl("http://10.129.51.27:8080/app/check", "yyb");
        // 进入应用时查询更新
        UpdateManager.create(this)
                //自定义更新提示框
                .setPrompter(agent -> {
                    UpdateInfo updateInfo = agent.getInfo();
                    String size = Formatter.formatShortFileSize(MainActivity.this, updateInfo.size);
                    new MaterialDialog.Builder(MainActivity.this)
                            .title("应用更新")
                            .content(R.string.update_content, updateInfo.versionName, size, updateInfo.updateContent)
                            .negativeText("以后再说")
                            .negativeColor(getResources().getColor(R.color.blue))
                            .positiveText("立即更新")
                            .positiveColor(getResources().getColor(R.color.blue))
                            .onPositive((dialog, which) -> agent.update())
                            .show();
                })
                //更新失败给出提示
                .setOnFailureListener(error -> ToastUtils.showShort(error.toString()))
                //设置下载进度条
                .setOnDownloadListener(new OnDownloadListener() {
                    private MaterialDialog dialog;

                    @Override
                    public void onStart() {
                        dialog = new MaterialDialog.Builder(MainActivity.this)
                                .content("下载中")
                                .progress(false, 100, true)
                                .cancelable(false)
                                .show();
                    }

                    @Override
                    public void onProgress(int progress) {
                        if (null != dialog) {
                            dialog.setProgress(progress);
                        }
                    }

                    @Override
                    public void onFinish() {
                        if (dialog != null) {
                            dialog.dismiss();
                            dialog = null;
                        }
                    }
                })
                .setManual(isManual)
                //开始更新
                .check();
    }
 ```
 - 配置网络权限ACCESS_NETWORK_STATE和INTERNET
 
 ``` bash
 <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
 <uses-permission android:name="android.permission.INTERNET" />
 ```
 -安装运行，即可看到如下2个界面：
 ![更新弹窗](/images/UpdateDemo/device-2018-01-17-090844.png)
 ![下载进度](/images/UpdateDemo/device-2018-01-17-091102.png)
 
注意：** 本地apk和服务器apk需要签名相同，都需要release版本才能正常更新。 **
[apk旧版本](/images/UpdateDemo/apk1.0.apk)
[apk新版本](/images/UpdateDemo/apk2.0.apk)