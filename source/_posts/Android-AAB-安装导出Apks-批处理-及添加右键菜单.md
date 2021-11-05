---
title: Android AAB 安装导出Apks 批处理 及添加右键菜单
date: 2021-06-03 17:34:17
tags: [Android,Bat]
---

# 什么是Android APP Bundles

​	一个Android应用程序包，包括所有程序的编译代码和资源，但推迟APK的生成。该程序包在安装时可以根据用户设备的屏幕分辨率、cpu架构以及语言为其生成特定的apk，而不用将所有的资源文件包含在apk中，有效减少apk的大小。

​	Android App Bundle是扩展名为.abb的文件

# 使用bundletool生成apk

​	得到Android应用程序包后，有两种方法可以生成特定设备的apk：在本地使用 bundletool命令行工具或者上传到Google Play，由于无法使用Google Play，所以使用bundletool进行apk生成。

​	bundletool，[下载地址](https://github.com/google/bundletool/releases) 下载bundletool-all-x.x.x.jar文件。

​	使用方法：[bundletool  | Android 开发者  | Android Developers (google.cn)](https://developer.android.google.cn/studio/command-line/bundletool)

# 使用批处理安装aab

 - 安装aab

   ```shell
   @echo off ::关闭echo打印
   chcp 65001 ::设置编码为UTF-8 支持中文输出
   set aabFile=%~nx1 ::获得当前右键的文件
   set apksFile=%aabFile:.aab=.apks% :: 根据aab得到apks的文件名
   ::判断当前是否有设备链接 如果无设备取消安装并退出
   for /F %%i in ('adb devices') do ( set adbDeviceResult=%%i)
   if %adbDeviceResult% neq List (echo AAB文件正在安装中...)^
   else (
   	echo 当前没有链接设备无法安装! 
   	pause 
   	exit
   )
   ::删除本地可能存在的apks文件 防止报错
   if exist %apksFile% (del temp.apks echo 删除%apksFile%文件成功) 
   :: 设置打包apks命令
   set buildApksCmd=java -jar bundletool.jar build-apks
   set buildApksCmd=%buildApksCmd% --connected-device
   set buildApksCmd=%buildApksCmd% --bundle=%aabFile%
   set buildApksCmd=%buildApksCmd% --output=%apksFile%
   set buildApksCmd=%buildApksCmd% --ks=KeyStore
   set buildApksCmd=%buildApksCmd% --ks-pass=pass:密码
   set buildApksCmd=%buildApksCmd% --ks-key-alias=别名
   set buildApksCmd=%buildApksCmd% --key-pass=pass:密码
   :: 执行打包apks命令
   %buildApksCmd%
   echo 导出%apksFile%成功
   :: 设置安装命令
   set installCmd=java -jar bundletool.jar install-apks --apks=%apksFile%
   %installCmd%
   echo 安装成功
   pause
   ```

   

 - 生成apks

   ```shell
   @echo off
   set aabFile=%~nx1
   chcp 65001
   echo 正在导出apks文件...
   set apksFile=%aabFile:.aab=.apks%
   if exist %apksFile% (del temp.apks) 
   set buildApksCmd=java -jar bundletool.jar build-apks
   set buildApksCmd=%buildApksCmd% --bundle=%aabFile%
   set buildApksCmd=%buildApksCmd% --output=%apksFile%
   set buildApksCmd=%buildApksCmd% --ks=KeyStore
   set buildApksCmd=%buildApksCmd% --ks-pass=pass:密码
   set buildApksCmd=%buildApksCmd% --ks-key-alias=别名
   set buildApksCmd=%buildApksCmd% --key-pass=pass:密码
   %buildApksCmd%
   echo 导出%apksFile%成功!
   pause
   ```

 - 安装apks

   ```shell
   @echo off
   chcp 65001
   set apksFile=%~nx1
   for /F %%i in ('adb devices') do ( set adbDeviceResult=%%i)
   if %adbDeviceResult% neq List (set isHaveDevice=1) else set isHaveDevice=0
   if %isHaveDevice%==1 (echo apks文件正在安装中...)^
   else (
   	echo 当前没有链接设备无法安装! 
   	pause 
   	exit
   )
   set installCmd=java -jar bundletool.jar install-apks --apks=%apksFile%
   %installCmd%
   pause
   ```

# 为aab和apks文件类型注册右键菜单 

这里推荐一个软件可以很方便的为windows 注册右键菜单

软件下载地址：[Releases · BluePointLilac/ContextMenuManager (github.com)](https://github.com/BluePointLilac/ContextMenuManager/releases/)

**操作：**

![软件主页](/mydata/imgs/article/AndroidAAB-01.png)

![软件主页](/mydata/imgs/article/AndroidAAB-02.png)

![软件主页](/mydata/imgs/article/AndroidAAB-03.png)

![软件主页](/mydata/imgs/article/AndroidAAB-04.png)

![软件主页](/mydata/imgs/article/AndroidAAB-05.png)

这样就可以将上面的批处理注册到具体文件类型的右键菜单了  其他两个批处理脚本操作同理

