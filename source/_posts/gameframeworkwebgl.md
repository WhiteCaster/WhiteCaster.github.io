---
title: GameFrameWork下WebGL打包注意事项
date: 2021-12-18 15:00:30
tags: [GameFramework,WebGL]
---

# GameFrameWork下WebGL打包

Webgl下打包和其他模式并无太大区别 只要注意 两点

1. GameFramework 下 AB包 加载方式 需要设置为 `LoadFromMemory` 

   ![image-20230222192540199](https://scz-texture.oss-cn-beijing.aliyuncs.com/texture/image-20230222192540199.png)

   如果有大量需要修改 可以直接修改`ResourceCollection.xml` `LoadType="1"`

2. AB包不能使用虚拟文件系统 VFS 可以在 `ResourceCollection.xml` 中修改 去掉FileSystem

   ![image-20230222192741550](https://scz-texture.oss-cn-beijing.aliyuncs.com/texture/image-20230222192741550.png)



 3. ResourceComponent  的ResourceMode  要设置为`Packeage`

    ![image-20230222192848221](https://scz-texture.oss-cn-beijing.aliyuncs.com/texture/image-20230222192848221.png)

之后按照正常打包操作 打包完成后部署webgl即可

PS: GameFrameWork打包推荐使用群友扩展的规则编辑器 

[规则编辑器地址 https://github.com/northWolf/GameFramework.ResourceRuleEditor](https://github.com/northWolf/GameFramework.ResourceRuleEditor)

