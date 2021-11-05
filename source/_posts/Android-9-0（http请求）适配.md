---
title: Android 9.0（http请求）适配
date: 2021-03-03 18:09:24
tags: Android
---

问题原因:Android P 限制了明文流量的网络请求,非加密的流量请求都会被系统禁止掉

**解决方案：**

#### 方法一：

1. res目录下新建xml目录，xml目录下新建network_security_config.xml文件

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <network-security-config>
       <base-config cleartextTrafficPermitted="true" />
   </network-security-config>
   ```

2. 修改AndroidManifest.xml

   ```xml
   <application
       android:networkSecurityConfig="@xml/network_security_config">
       <!--android9.0适配-->
       <uses-library
           android:name="org.apache.http.legacy"
           android:required="false" />
   </application>
   ```

#### 方法二：

​	AndroidManifest.xml添加：

```xml
<application 
    android:usesCleartextTraffic="true"
>
```



