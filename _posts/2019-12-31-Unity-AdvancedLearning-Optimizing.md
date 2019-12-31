---
title: "Unity教程-进阶篇-性能优化"
categories: unity
tags: unity optimization
author: Miny
---

## 前提

### Mono虚拟机

Mono可以实现强大的跨平台功能，Unity打包时会将Mono一起打包进来，所以哪怕空场景打包，安装包也会有二三十兆的大小。

### DrawCall

Draw Call，是CPU对图形绘制接口的调用，CPU通过调用图形库(DirectX/OpenGL)接口，命令GPU进行渲染操作。
因为每次绘制时，CPU都需要调用DrawCall，而每个DrawCall都需要很多准备工作，检测渲染状态，提交渲染数据，提交渲染状态等，而GPU本身具有很强大的计算能力，可以很快处理完渲染任务。 但是当DrawCall过多时，CPU就会产生很多额外的开销用于准备工作，CPU就可能处于负载状态。

### Unity Profiler

Profiler是用来分析游戏运行性能，内存使用等情况的工具，可以准确定位到影响游戏性能的脚本方法，内存过高的资源等等。可以在Window菜单中打开。

### Unity Statistics

![20180923215814828](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-31-Unity-AdvancedLearning-Optimizing.assets/20180923215814828.png)

Statistics是unity的渲染统计窗口，可以快速了解当前场景中关于声音、图形渲染、网络状况等多种统计信息

FPS：每秒传输帧数(Frames Per Second)，表示引擎渲染一个游戏帧所需要的时间，主要受到场景中渲染物体数量和GPU性能的影响，游戏画面每秒帧数至少要保证在30以上，后面括号中的5.2ms(1S = 1000ms)是指处理一帧需要的时间

CPU：获取到当前占用CPU进行计算的时间绝对值，和时间点。如果主进程处于挂断或休眠的状态时，CPU time将会保持不变

Render thread：GPU渲染线程处理图像所花费的时间，具体数值由GPU的性能决定

Batches：是Unity内置的Draw Call Batching技术。引擎每对一个物体进行一次DrawCall，就会产生一个Batch，Batch中包含了该物体的所有网格和顶点数据。当渲染另一个相同的物体时，引擎会直接调用Batch里的信息，将相关的顶点数据直接送到GPU，从而让渲染过程更加高效，即Batching技术是将所有材质相近的物体进行合并渲染。

Tris、Verts：指摄像机视野范围内的三角面数量和顶点数量，即Camera中的fieldOfView所有取值下的Tris和Verts，即使在game视图中看不到，但只要在这个范围内，都会被渲染。而且哪怕某个模型上只有一个顶点在范围内，整个模型都要被渲染出来

Screen：表示当前屏幕分辨率的大小，后面的值表示总内存的使用数值

Setpass Calls：每当GPU运行一个Pass之前，就会产生一个“SetPass Call”,可以用来描述渲染性能的开销

Shadow Casters：表示场景中有多少可以投射阴影的物体，一般这些物体都作为场景中的光源

Visible Skinned Meshed：渲染皮肤网格的数量

Animations：表示正在播放的动画数量

## 资源优化

### 轻量端优化标准

模型单位统一
相机视野内渲染模型面数或顶点数推荐不高于50万
场景中灯光光源数量需要控制在50个以内
尽量使用效率较高的Shader，不要使用涉及反射，屏幕UV坐标，屏幕后处理的Shader
单个模型材质数小于3个
贴图实际尺寸小于1024，根据实际情况选择大小
长时间音乐（背景音乐）压缩格式 mp3
短时间音乐（音效）非压缩格式 wav ogg
减少冗余资源和重复资源，不同目录下的相同资源，如果都被引用，那么都会被打进包中

### 模型、贴图优化

尽可能减少模型中三角形的数目
尽可能合并顶点
合理合并网格
合理合并贴图
可以使用插件Mesh Beaker

## 渲染优化

### CPU/GPU分工

CPU负责逻辑运算、数值处理等
GPU负责场景渲染、灯光处理等

CPU和GPU采用渲染流水线的方式进行并行工作和交互，两者相互独立互不依赖。

### 批处理

Unity中有两种批处理的方式，分别是动态批处理和静态批处理。

动态批处理是Unity自动完成的。不需要我们自己操作，但是有很多限制，一不小心破坏，就能导致Unity无法处理使用相同材质的物体。
动态批处理条件如下：
使用同一种材质的物体
顶点数量小于900
所以使用相同材质的对象必须使用同一个缩放尺寸
使用LightMap的物体不会批处理
使用过多Pass的shader的对象会中断批处理
接受实时阴影的对象也不会批处理
所以是否能动态批处理，随缘吧~

静态批处理，自由度高，限制少，但不能移动。
只需要勾选物体的Static属性即可，Unity在运行的时候会合并所有标识为“Static”的物体。

### LOD

层次细节(Leves of Detail)是根据物体在游戏中所占视图的百分比来调节不同复杂度模型的。当摄像机距离某物体较远的时候，采用低模，当摄像机距离较近的时候，采用高模。可有优化游戏渲染效率，但会占用更多的内存。

### Occlusion Culling

遮挡剔除(Occlusion Culling)在对象被其他静态物体遮挡，摄像机无法看到的时候，禁用对象的渲染。遮挡剔除和视椎体剔除(Frustum Culling)不同，视椎体剔除只禁用摄像机视野外的对象渲染，不会禁用视野内被遮挡对象的渲染。在使用遮挡剔除的时候，任然受益于视椎体剔除。

未开启遮挡剔除的场景渲染效果

![2018092322010839](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-31-Unity-AdvancedLearning-Optimizing.assets/2018092322010839.png)

开启遮挡剔除的场景渲染效果，可以发现不在视野内和被遮挡的物体被禁用渲染

![20180923220133668](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-31-Unity-AdvancedLearning-Optimizing.assets/20180923220133668.png)

在大场景中，为了能更好使用遮挡剔除的功能，应该将物体合理分离，不要将大量模型合并成一个物体。例如，单个屋顶，单个椅子，单面墙，单个房间等，一个整体作为一个对象即可，不要分离的过于细碎

### Lightmapping

实时光照对于轻量平台是个非常昂贵的操作。如果只有一个平行光还好，但如果场景中包含了太多光源并且使用了过量多Passes的shader，那么很有可能会造成性能下降。灯光贴图技术可以预先计算场景中的灯光信息，并且生成灯光贴图

### 合并Mesh

将多个模型合并成一个物体

#### 提高运行性能

统一材质，创建统一的材质，方便控制、分配所有物体
方便控制人物动画，将不同衣服、武器、裤子、人物骨架等结合成一个物体，再由同一个骨架控制运动，就能实现不同组合角色，却可以做一样的动作

#### Mesh Beaker

简介：用于合并网格、贴图，将场景中细碎的模型合并，可以有效提高性能
基础操作：

GameObject->CreateOther->MeshBaker->Mesh And MaterialBaker 创建MeshBeaker对象
点击Open Tools For Adding Objects
选中需要合并的物体，点击Add Selected Meshes，注意需要合并的物体材质必须使用相同的Shader
MeshBaker0的ObjectsToBeCombined里就是刚才选择的物体，当然，自己手动拖进去也是可以的

![20180923220554451](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-31-Unity-AdvancedLearning-Optimizing.assets/20180923220554451.png)

接下来点击CreateEmptyAssetsForCombinedMaterial，主要是用来储存合并材质和合并信息的
点击Bake Materials Into Combined Material
等待操作完成后，选择子物体，点击Bake生成一个 CombinedMesh-MeshBaker0-mesh 的对象，
然后点击Disable Renderers on Source Objects，可以隐藏原来物体的Render组件

## 其他优化

### 优化工具：Amplify Impostors

简介

Amplify Impostors（我没用过）是一种一键式解决方案，可以极大的减少你的多边形面数和DrawCall的调用次数。使用这种技术的目的就是为了能够使用非常低的多边形数来表示遥远的物体，例如：树木、灌木丛、岩石、遗迹、建筑物等。
Impostors(冒牌者)是面向摄像机的扁平的四边形，或者是简单的多边形形状。通过渲染一个原始资源的虚假替代品，来替换复杂的高多边形对象，可以在运行时进行编辑或者创建。目前只支持在编辑器状态下烘焙对象，之后会添加实时生成的功能。
Impostors在运行时，可以移动、旋转、缩放，并且可以接受和投放阴影，甚至可以和其他对象或Impostor交互。Impostors可以直接和LOD或第三方的LOD系统搭配使用，可以在近距离显示实际的网格，一定的距离使用Impostor。

快速上手

选择场景中的游戏对象或者项目中的预制体
为你选择的对象在Inspector窗口添加“Amplify Impostor”组件
点击“Bake Impostor”按钮，并且选择你想要保存Impostor Asset的文件夹
The Inspector

![20180923220811466](https://raw.githubusercontent.com/Miny-W/Miny-W.github.io/master/_posts/2019-12-31-Unity-AdvancedLearning-Optimizing.assets/20180923220811466.png)

Impostor Asset：这是对AmplifyImpostorAsset的引用，如果您有多个相同对象的Impostor，可以共享该数据
LOD Group：当添加LOD组件时，会自动赋值
References：这是被用于烘焙Impostor的对象引用
+：打开文件夹，创建一个Impostor Asset并且自动赋值给组件
Bake Impostor：依据当前的数据和路径来烘焙Impostor
当Impostor Asset赋予到该组件时，你可以配置高级烘焙设置

Bake Type：以效果对比来说 Spherical < Octahedron < HemiOctahedron
Texture Size：设置烘焙图像的纹理大小
Axis Frames：每个轴向的帧数，如果值为16，则代表对于一个Impostor有256(16*16)个不同的摄像机，帧数越大，效果越好，当然数据量也越大
Pixel Padding：设置图像边缘像素溢出的大小
Billboard Mesh：设置更适合的形状
Max Vertices：设置最大顶点数
Outline Tolerance：允许顶点更加贴合形状
Normal Scale：根据形状的法线，展开顶点。当值为1时，可以快速生成四边形
