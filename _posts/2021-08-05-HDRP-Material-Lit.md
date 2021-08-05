---
title: "Unity HDRP Lit 材质说明"
categories: Unity
tags: Unity
author: Miny
---

**次表面散射**（Subsurface Scattering ）会模拟光线交互和穿透半透明对象时的效果，例如：植物。

**各向异性**（Anisotropy）选项会模拟出根据朝向进行配置修改的表面材质，例如：模拟拉丝铝的效果。

**晕彩**（Iridescence）选项提供参数设置用于在材质表面创建晕彩效应，该效果类似光线在浮油上的效果。

**高光颜色**（Specular Color）用于控制材质上镜面反射的颜色和强度。

**半透明**（Translucent）选项能非常有效地模拟植被的光线交互。

**涂层遮罩**（Coat Mask）会模拟材质上的清漆层效果，提高光滑度。

------

在HDRP中，**遮罩贴图**由以下内容组成：
红色通道：金属参数范围从0到1
绿色通道：环境遮蔽
蓝色通道：细节贴图遮罩
Alpha通道：光滑度

------

细节贴图使用以下通道：
红色：使用叠加混合的灰度值
绿色：法线贴图Y通道
蓝色：光滑度
Alpha：法线贴图X通道

------

**折射模型**（Refraction Model）定义如何模拟透过材质的光线弯曲效果。它具有二个选项：Plane和Sphere。
选择折射模型取决于材质应用对象的形状和大小:
**球体**（Sphere）：对于实心的物体，请使用球体模型。折射厚度（Refraction Thickness）值对应物体的大小来设置。
**平面**（Plane）：对于中空的物体，请使用平面模式。折射厚度（Refraction Thickness）应设置的较小。
**折射率**（Index of Refraction）和折射率厚度（Index of Refraction Thickness）选项允许你控制折射模型的行为。