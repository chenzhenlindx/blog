---
title: android studio生成aar包并在其他工程引用aar包
date: 2018-1-23 13:19:36
tags: [重构]
categories: 重构
---
1、aar包是android studio下打包android工程中src、res、lib后生成的aar文件，aar包导入其他android studio 工程后，其他工程可以方便引用源码和资源文件
2、生成aar包的步骤：
①.用android studio打开一个工程，然后新建一个Module，新建Module时候选择Android Library,后面按新建普通工程操作；
②.在新建的Module中编写完代码后，接下来编译整个工程后就会自动生成aar包，包的路径在新建的Module ==》 build ===》outputs ==>aar目录下
3、其他androidstudio工程引用aar包
①.将aar包复制到lib目录下
②.配置build.gradle文件：
加入
``` bash
repositories {
    flatDir {
    dirs 'libs'
}
dependencies {
    ...
    compile(name:'auil-debug', ext:'aar') 
    ...
}
```
③.编译一次工程
以上操作成功后可以在扩展包下看到被引用的aar包文件
![](/images/android编译aar引用.png)