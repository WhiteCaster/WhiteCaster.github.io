---
title: Linux下Svn的搭建
date: 2021-2-24 21:02:45
tags: [Linux,Svn]
---

# 安装svn

* **安装命令**

  ```
  yum install subversion 或 sudo apt install subversion
  当然也可以下载源码自行编译
  ```

* **查看是否安装/版本号**

  ```
  svnserve --version
  ```

* **查看安装目录**

  ```
  whereis subversion
  ```

# 创建仓库

1. **创建仓库目录**

   ```
   mkdir 目录
   ```

2. **创建仓库**

   ```
   svnadmin create 目录
   ```

   创建成功后，在目录下可以看到以下文件：
   
   ![标题效果](/mydata/imgs/article/Linux下Svn搭建-01.png)

# 配置

​	conf目录下是svn的配置目录，其中包括：

​	![标题效果](/mydata/imgs/article/Linux下Svn搭建-02.png)

1. **权限配置**(conf/authz)

   ```
   #用户分组(admin->管理员,development ->开发 other->其他)
   [groups]
   admin = test1 ``#管理员用户test1
   development = test2,test3 ``#开发用户test2，test3
   other = test4,test5,test6  ``#其他用户test4,,test5,test6
   #权限配置
   [/]
   @admin = rw ``#管理员读写权限
   @development = rw ``#开发读写权限
   @other = r    ``#其他读权限
   test7 = rw    ``#test7用户读写权限
   ```

2. **密码配置** (/conf/passwd)

   ```
   #密码配置,格式为用户名=密码，密码为明文
   [users]
   test1 = test1
   test2 = test2
   test3 = test3
   test4 = test4
   test5 = test5
   test6 = test6
   test7 = test7
   ```

3. **服务进程配置**(/conf/svnserve.conf)

   ```
   [general]
   anon-access = none  #匿名用户无权访问 不要设置为read 会导致日志读取问题
   auth-access = write  #认证用户可读写
   password-db = passwd #指定用户认证密码文件
   authz-db = authz  #指定权限配置文件
   ```

   **PS:最后说一点一定要注意的是 所有配置前面不能有空格**

# 启动svn服务

* **默认方式启动**

  ```
  svnserve -d -r 工作目录 
  -d 服务后台运行 -r 指定工作目录，
  例如 仓库地址 (/var/project/test)
  svnserve -d -r /var/project
  注意不能指定仓库名地址仓库名 (/var/project/test)
  ```

* **指定端口号启动**

  ```
  svnserve -d -r /var/project --listen-port 3691
  ```

# 防火墙配置

* **关闭防火墙**

  ```
  systemctl stop firewalld
  ```

* **开启指定防火墙**

  ```
  #开启3690端口 svn默认端口为3690 如果使用自定义端口 这里配置为自己定义的端口
  firewall-cmd --zone=public --add-port=3690/tcp --permanent
  #刷新配置
  firewall-cmd --reload
  ```

# 客户端连接

 **svn地址为** : svn://IP:指定端口号/仓库名  

 **注意： 这里的仓库名不是/var/project/test而是/test**

