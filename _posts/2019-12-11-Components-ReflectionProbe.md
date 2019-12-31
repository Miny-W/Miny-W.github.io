---
title: "Unity教程-组件篇-Reflection Probe"
categories: unity
tags: unity component
author: Miny
---

# Reflection Probe（反射探测器）

#### 说明

反射探测器（又叫反射探针），用于控制场景中光线的反射信息

提到反射效果，自然逃不开shader，其可以做出实时真实反射，但（性能）代价比较大

Reflection Probe就提供了对周围真实环境的（伪）反射

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-11-Components-ReflectionProbe.assets/04516CD4.jpg" alt="img" style="zoom:50%;" />

#### 属性

先来介绍一下这个组件的Inspector面板

![image-20191211182214596](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-11-Components-ReflectionProbe.assets/image-20191211182214596.png)

和Light Probe Group一样，一上来就是个编辑态的钮，按下去后，可以将反射范围展示出来（需要打开Gizmos），拖动时可以改变各个面的位置，从而改变反射探针的捕捉范围；

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-11-Components-ReflectionProbe.assets/image-20191211182651297.png" alt="image-20191211182651297" style="zoom:80%;" />

右侧的编辑态按钮，是用来移动探针位置的（这里区分一下，探针位置是指捕捉周围反射信息的点，并不是指transform.position），这个位置必须在反射捕捉范围内变化；

Type是反射探针类型，3种，Baked、Custom、RealTime：

Baked，其作用就是烘焙出反射贴图（cubemap）。烘焙的时候注意如果想把某物体烘焙进去，该物体应该勾选reflectionProbe static。当你需要反射贴图时，可以自己生成一个；这种类型，反射贴图是不会有变化的，简单理解，把一个新物体拿到反射区域内，光滑物体上不会出现该物体的映象；

Custom类型，是用你指定的cubemap来反射周围环境，营造氛围不错……

Realtime类型，实时反射，多了两个选项，Refresh Mode和Time Slicing，Refresh Mode分三类：On Awake/Every Frame/Via Scripting；On Awake 就是程序开始运行时获取周围环境生成的一张反射贴图，之后你再改变外部环境，反射贴图不会有变化，还是这个样……（当然如果你是改变Reflection Probe的某些参数还是会变的）；Every Frame ，顾名思义就是每一帧都会获取周围环境来生成反射贴图；via scripting 这个没发现有啥特别的……在Time Slicing应用于Every Frame模式下，有三种类型：All faces at once/Individual faces/No time slicing，效果上区别不大，性能上有些许区别。比如当你移动反射探头的时候，No time slicing如丝滑般过渡，All face at once 就有一点卡顿的感觉，Individual faces 卡顿感更强……；

Importance代表了重要程度，比如有两个反射探针，范围内存在同一个物体，其受影响的优先级就由Importance决定，若两个反射探针的值相等则权重评分，若有一个更大，则只受其影响（这个权重是会随着物体在两个探针范围边界过渡时线性变化的）；

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-11-Components-ReflectionProbe.assets/image-20191211185831068.png" alt="image-20191211185831068" style="zoom:80%;" />

Intensity ：在反射探头的范围内控制环境亮度大小，如同光源一样照亮周围；

Box Projection :最上面的动态图中，从不同角度看镜面有不同的景象，但是你把摄像头拉远，在反射平面上看到的景象大小并不会改变。如果你想让反射的景象随距离发生变化，那么久勾选box projection;

Box Size : 反射探头影响范围大小；

Box Offset :Box的偏移；

Resolution:值越大，反射的景象越清晰；

HDR:是否高动态范围图像

Shadow Distance :反射景象中阴影的有或无，明显后者不明显；

Clear Flags ：Skybox则反射天空盒，Solid Color 反射背景颜色；

Background ：Solid Color模式下的背景色

Culling Mask :指定反射哪一层的物体；

Use Occlusion Culling :使用遮挡剔除

Clipping Planes ：控制反射探头反射环境的远近，Near仅Far远；

#### 应用

对于光滑度（Smoothness）很高的物体来讲，其表面的反射映象应该更为清晰，Unity默认的光烘焙设定里，反射环境为天空盒（Skybox），室内或其他封闭环境比较不适用，而Reflection Probe解决了这种问题

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-11-Components-ReflectionProbe.assets/image-20191212085831885.png" alt="image-20191212085831885" style="zoom:80%;" />

甚至在大多数情况，需要设置多个反射探针，因为从不同角度看，物体的反射映象应该是不同的，若全局仅用一个探针，会出现映象与真实环境不符的情况

<img src="https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-11-Components-ReflectionProbe.assets/image-20191212090235267.png" alt="image-20191212090235267" style="zoom:80%;" />

如上图，关闭了选中的Reflection Probe，铁架的反射映象出现了错误

这个就要根据我们所需要展示的区域去调整工作量了，毕竟看不到的地方，错了也无妨

![img](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-11-Components-ReflectionProbe.assets/2A874730.jpg)