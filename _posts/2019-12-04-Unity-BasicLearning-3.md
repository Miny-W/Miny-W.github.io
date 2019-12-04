---
title: "Unity教程-基础篇（三）-创建基础游戏对象"

categories: unity

tags:  unity 

author: Miny
---

## 游戏对象（GameObject）

这个词在上篇教程中提到了，但是并没有细讲。

Unity中，GameObject可代表的内容太多了，模型、UI、代码工具、特效等，类似于一个强制拥有Transform的载体。Hierarchy视图里所有展示的都是独立的GameObject。

创建一个GameObject非常简单，仅需在Hierarchy视图点击鼠标右键，就能看到Unity整合的很多基础模块，下面会逐类介绍一下。

### 3D Object

物如其名，三维物体。

|    Name    |  名称   |                     说明                     |
| :--------: | :-----: | :------------------------------------------: |
|    Cube    | 立方体  |           可xyz任意单轴或多轴缩放            |
|   Sphere   |  球体   |           可xyz任意单轴或多轴缩放            |
|  Capsule   |  胶囊   |           可xyz任意单轴或多轴缩放            |
|  Cylinder  |  圆柱   |           可xyz任意单轴或多轴缩放            |
|   Plane    |  平面   |       可xz轴缩放，单面，三角面数为200        |
|    Quad    | 四边形  |        可xy轴缩放，单面，三角面数为2         |
| Ragdoll... | 布偶... |   通过绑定各物体的transform，实现骨骼连接    |
|  Terrain   |  地形   |      用于制作地形，可高低形变，种树种草      |
|    Tree    |   树    |         就是树，可自定义树枝、树叶等         |
| Wind Zone  |  风区   | 模拟风的效果，可使地形上的树草按设定参数摆动 |

### UI

|     Name     |    名称    |                 说明                 |
| :----------: | :--------: | :----------------------------------: |
|     Text     |    文本    |             用来展示文字             |
|    Image     |    图像    |             用来展示图片             |
|  Raw Image   |  原始图像  | 用来展示某个画面（图片、视频、相机） |
|    Button    |    按钮    |           基本点击交互单元           |
|    Toggle    |    切换    |      基本勾选单元，选和不选两态      |
|    Slider    |   滑动条   |    用来表示进度，滑动条存在填充图    |
|  Scrollbar   |   滚动条   |      用来表示位置、不存在填充物      |
|   Dropdown   | 下拉列表框 |           用来展示下拉选项           |
| Input Field  | 文本输入框 |             用来输入文字             |
|    Canvas    |    画布    |         UI的容器，用来展示UI         |
|    Panel     |    面板    |   类似于Image，但常当作遮罩或透底    |
| Scroll View  |  滚动视图  |   用来展示大量内容，自带Scrollbar    |
| Event System |  事件系统  |         UI功能触发的依赖单元         |

### Light

|       Name        |     名称     |                             说明                             |
| :---------------: | :----------: | :----------------------------------------------------------: |
| Directional Light |    定向光    |     一种在场景中只有角度没有位置概念的光，通畅用作太阳光     |
|    Point Light    |    点光源    |            以一个点向外散射的光源，不能用作实时光            |
|     Spotlight     |    聚光灯    |        以一个点向一小于180°角照射的光，不能用作实时光        |
|    Area Light     |    区域光    | 以一平面矩形或平面圆形区域向某一方向照射的光，只能用作烘焙光 |
| Reflection Probe  |  反射探测器  |                 用于控制场景中光线的反射信息                 |
| Light Probe Group | 光照探测器组 |                  在规定区域内捕捉静态光效果                  |

### Camera（摄像机）

为捕捉并显示场景画面的组件。可设置背景（Background）、剔除遮罩（Culling Mask）、视野范围（Field of view）、近点（Near）、远点（Far）等属性。

