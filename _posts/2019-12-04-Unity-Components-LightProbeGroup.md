---
title: "Unity教程-组件篇-Light Probe Group"
categories: unity
tags: unity component
author: Miny
---

# Light Probe Group（光照探测器组）

### 说明

在规定区域内捕捉静态光效果的组件

![image-20191204112041392](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204112041392.png)

面板布局很简单：

一个编辑态按钮（按下去开启编辑态，弹上来不让编辑）；

一个Show Wireframe（Show the tetrahedron wireframe visualizing the blending between probes.简单来讲，探针之间线的显隐）

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204113408731.png" alt="image-20191204113408731" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204113426118.png" alt="image-20191204113426118" style="zoom:50%;" />

一个Remove Ringing（When enabled,removes visible overshooting often observed as ringing on objects affected by intense lighting at the expense of reduced contrast.当开启时，以降低对比度为代价，移除经常被观察到的受强光影响的物体上的振铃效应）

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204113829222.png" alt="Remove Ringing=true" style="zoom:50%;" />

​                                                                   

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204113911193.png" alt="Remove Ringing=false" style="zoom:50%;" />

> [振铃](https://baike.baidu.com/item/振铃)效应（Ringing effect）是影响复原[图像质量](https://baike.baidu.com/item/图像质量/5133364)的众多因素之一，其典型表现是在[图像灰度](https://baike.baidu.com/item/图像灰度/2983459)剧烈变化的[邻域](https://baike.baidu.com/item/邻域/6323269)出现类[吉布斯](https://baike.baidu.com/item/吉布斯/1039203)（[Gibbs](https://baike.baidu.com/item/Gibbs/10014764)）分布--(满足给定约束条件且熵最大的分布)的振荡。在图像盲复原中，振铃效应是一个不可忽视的问题，其严重降低了复原图像的质量，并且使得难于对复原图像进行后续处理。
>
> 振铃效应是由于在[图像复原](https://baike.baidu.com/item/图像复原)中选取了不适当的图像模型造成的；在图像盲复原中如果[点扩散函数](https://baike.baidu.com/item/点扩散函数)选择不准确也是引起复原结果产生振铃效应的另一个原因，特别是选用的点扩散函数尺寸大于真实点扩散函数尺寸时，振铃现象更为明显；振铃效应产生的直接原因是图像退化过程中[信息量](https://baike.baidu.com/item/信息量/420401)的丢失，尤其是高频信息的丢失。
>
> [振铃](https://baike.baidu.com/item/振铃)效应对复原[图像质量](https://baike.baidu.com/item/图像质量/5133364)影响严重，众多学者对抑制振铃效应的方法进行了广泛研究，然而大多数图像复原方法在这一点上都有所不足，造成了复原过程中的振铃效应几乎不可避免，尤其对于有噪声存在的场合，它会混淆图像的高频特性，使得振铃效应带来的影响更加显著。



一个 Selected Probe Position（用来显示选中探针的坐标）；

四个按钮：Add Probe（增加一个探针）、Select All（选中全部探针）、Delete Selected（删除选中的探针）、Duplicate Selected（复制所选探针）

------

### 用处

Unity有一条硬核规定：烘焙光只会影响静态对象。

Unity还有一条硬核规定：实时光照射到静态对象时，静态对象的阴影面（与实时光方向相反的方向的垂面）亮度为0（当然，不考虑漫反射）。



这两条规定在某些特定情况下，就会让人头皮发麻，例如↓

我有一个封闭空间（潦草一点，用6个cube拼的），墙体均为静态

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204120047890.png" alt="image-20191204120047890" style="zoom:50%;" />

空间里一片漆黑，加个Point Light，烘焙一下，嗯~亮了

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204120248485.png" alt="image-20191204120248485" style="zoom:50%;" />

放个Sphere，不设置静态，再烘培一下，emmm....

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204120348853.png" alt="image-20191204120348853" style="zoom:50%;" />

这不亮还有个球用（别问为什么不给Sphere设静态，我需要它移动）

![img](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/0845F6BB.png)

面对这种情况，萌新们一般会选择硬加实时点光，或者调一调Emission（自发颜色），效果不作介绍

其实这种时候，只要打开一首适合抄作业的歌，创建一个Light Probe Group，扩增探针，在Sphere需要移动的空间范围里码一码（这个就是经验了），再点一下烘焙

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204121551775.png" alt="image-20191204121551775" style="zoom:50%;" />

嗯~亮了

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204121724739.png" alt="image-20191204121724739" style="zoom:50%;" />

移动一下Sphere，光影也会随之改变，妥

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/0851C55F.jpg" alt="img" style="zoom:67%;" />

以上理论也可反其道而行之，在实时光搭配静态对象时，也可通过Light Probe让对象的光信息更加丰富。

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204130809256.png" alt="image-20191204130809256" style="zoom:50%;" />

Light Probe的用法就是在Light Mapping的基础上加上了一些探针来记录光源的信息。探头越多，效果就越明显，当然光信息存储也会越多，相对实时光而言，这是需要权衡的。因此，在大型场景中，Light Probe会用到很多个，而不是单纯扩增探针铺满整个场景，真实应用，只需要在需要的区域内设置探针就可以了。

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-04-Unity-Components-LightProbeGroup.assets/image-20191204130059064.png" alt="image-20191204130059064" style="zoom:50%;" />

探针的摆放位置，也会影响到烘焙出来的光效，这个可以参照Unity官方案例，或者是大佬们的分析文。

