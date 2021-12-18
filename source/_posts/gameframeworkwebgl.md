---
title: GameFrameWork下WebGL打包注意事项
date: 2021-12-18 15:00:30
tags: [GameFramework,WebGL]
---

# GameFrameWork下WebGL打包

Webgl下打包和其他模式并无太大区别 只要注意 两点

1. GameFramework 下 AB包 加载方式 需要设置为 `LoadFromMemory` 

   <img src="https://tva1.sinaimg.cn/large/e1b1a94bly1gxi35s46jfj20f50m6777.jpg"/>

2. AB包不能使用虚拟文件系统 VFS 可以在 `ResourceCollection.xml` 中修改 去掉FileSystem

   <img src="https://tva1.sinaimg.cn/large/e1b1a94bly1gxi38khp3fj20m70330u3.jpg"/>



之后按照正常打包操作 打包完成后部署webgl即可

