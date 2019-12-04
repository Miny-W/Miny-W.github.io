---
title: "Unity教程-进阶篇-Shader Graph 节点介绍"

categories: unity

tags:  unity ShaderGraph

author: Miny
---

# ***\*Channel mixer\****

根据输入的RGB值及各个通道的权重，输出权重加成后的RGB值。

# ***\*Contrast\****

根据输入In及Contrast调节对比度。Contrast为1时输出In，Contrast为0时输出In的中值。

# ***\*Hue\****

根据Offset调节色相。
可以根据Degrees调节即（-180，180）
或者根据Normalized调节即（-1，1）

# ***\*Invert Colors\****

反转颜色，可以选择单个或多个反转的通道

# ***\*Replace Color\****

替换颜色

In：输入的颜色
From：要替换的颜色
To：替换成的颜色
Range：类似PS里的容差值
Fuzziness：软化选区的边缘

# ***\*Saturation\****

饱和度。Saturation为1时输出原颜色，Saturation为0时为完全不饱和色。

# ***\*White Balance\****

白平衡

Temperature 让颜色变黄或者变蓝
Tint 让颜色变粉或者变绿

# ***\*Blend节点\****

根据输入的Base、Blend两个值进行混合
Opacity可以设置混合的强度，0=不增强
Mode可以设置混合的模式

# ***\*Dither\****

Dither是一种特定格式的噪波，用于随机量化误差。它用于防止大幅拉伸图片时出现的异常情况，如图像中的色带。Dither节点在屏幕空间抖动来确保图案的均匀分布。可以通过连接另一个节点来输入屏幕位置。此节点通常作为主节点上Alpha Clip Threshold的输入，为不透明对象提供透明的外观。这对于创建透明的对象很有用，而且仍然可以写入深度缓冲区。





# ***\*Channel Mask\****

可以从下拉框中选择输出的通道。可以用来过滤或使用某个或某几个通道。

# ***\*Color Mask\****

从输入颜色与Mask Color相等的颜色的位置创建一个遮罩。
Range：类似PS里的容差值
Fuzziness：软化选区的边缘

# ***\*Normal Blend 法线混合\****

混合两个法线

# ***\*Normal Create 创建法线\****

从一张高度图Texture创建法线贴图。UV和Sampler可以从对应的UV和Sampler State节点连接，如果没有设置将使用默认值。

创建的法线贴图的强度可以用Offset和Strength属性修改。Offset定义了法线细节的最大距离，Strength是结果的系数。

# ***\*Normal Strength\****

修改法线贴图的Strength。Strength为1时返回原图，为0时返回纯黑的法线贴图。

# ***\*Normal Unpack\****

解包一个法线贴图。

# ***\*Colorspace Conversion\****

颜色空间转换。从一种颜色空间转换到另一种颜色空间。

# ***\*Combine 组合\****

可以从RGBA重新组合一个新的向量。

# ***\*Flip 翻转\****

反转各个值（变为相反值），可以选择一个或多个通道进行反转。

# ***\*Split 分割\****

将一个多维向量分割成多个1维的数值。如果输入的向量小于4维，不够的维度会使用默认值分别是 (0, 0, 0, 1)。

# ***\*Swizzle （打乱）\****

从输入的向量创建一个新的向量，但是可以根据下拉参数打乱输出的维度。

# ***\*Boolean\****

创建一个布尔值常量。

# ***\*Color\****

创建一个颜色常量，可选正常颜色或HDR颜色。

# ***\*Constant\****

创建一个无理数常量，可选的有PI（圆周率π）、TAU（2π）、PHI（黄金分割1.618）、E（常数e，2.718）、SQRT2（根号2，2^0.5）

# ***\*Integer\****

创建一个整数常量，类型是Vector 1。通过右键菜单可以转换为一个Integer模式的Vector1属性。

# ***\*Slider\****

创建一个使用滑条作为输入的Vector1常量。通过右键菜单可以转换为一个Slider模式的Vector1属性。

# ***\*Time\****

可以获取Unity中的时间值。

Time 即代码中的Time.time

Sine Time sin(Time.time)

Cosine Time cos(Time.time)

Delta Time Time.deltaTime

Smooth Delta Time.smoothDeltaTime

# ***\*Vector1/2/3/4\****

Vector1/2/3/4的值，如果输入没有连，可以作为常量使用。输入的每一个维度都需要单独连接。

# ***\*Bitangent Vector\****

获取mesh的双切线向量。可选的空间有Object, View, World, Tangent。

# ***\*Normal Vector\****

获取mesh的法线向量。可选的空间有Object, View, World, Tangent。

# ***\*Position\****

获取mesh的顶点或片段的位置。可选的空间有Object, View, World, Tangent。

# ***\*Screen Position\****

获取mesh的顶点或片段在屏幕空间的位置。

### ***\*Default\****

返回屏幕坐标。这个模式屏幕坐标除了clip space position W。

***\*Raw\****
返回屏幕坐标。这个模式屏幕坐标没除clip space position W。对于投影很有用。

***\*Center\****
添加了偏移，让float2(0,0)在屏幕的中心。

***\*Tiled\****
添加了偏移，让float2(0,0)在屏幕的中心并且使用frac进行tiled。

# ***\*Tangent Vector\****

获取mesh的顶点或片段的切线向量。可选的空间有Object, View, World, Tangent。

# ***\*UV\****

获取mesh的顶点或片段的UV。可选的空间有UV0, UV1, UV2, UV3。

# ***\*Vertex Color\****

获取mesh的顶点或片段的顶点颜色。

# ***\*View Direction\****

获取mesh的顶点或片段的视角方向向量。从顶点或片段到相机的方向。可选的空间有Object, View, World, Tangent。

# ***\*Gradient\****

可以设置渐变色的一个节点

# ***\*Sample Gradient\****

经过采样后输出一个Color颜色。

# ***\*Matrix 2x2/3x3/4x4\****

不同维度的矩阵矩阵。

# ***\*Transformation Matrix\****

变换矩阵。变换类型可以在下拉菜单中选择。

# ***\*Dielectric Specular\****

返回一个基于物理材质的高光系数。可以通过Material下拉框选择材质的类型。

· Common 是介于0.024 到 0.048 的sRGB值。可以用于塑料和面料。

· Custom 可以用于自定义。IOR代表材质的折射率。

# ***\*Metal Reflectance\****

金属反射率。

如果PBR主节点使用Specular Workflow，这个输出应该连到PBR节点的Specular。如果PBR主节点使用Metallic Workflow，这个输出应该连到PBR节点的Albedo。

# ***\*Ambient 环境\****

获取场景的环境光设置。

如果场景环境光设置为Gradient，Color/Sky, Equator , Ground 对应 Sky Color, Equator Color 和 Ground Color

如果环境光设置为Color，Color/Sky返回Ambient Color，其他两个返回0。



# ***\*Camera 相机\****

获取当前相机的参数。参数和Camera类似。

其中Z Buffer Sign：-1表示反转Z Buffer，否则是1

# ***\*Fog 雾\****

获取场景的Fog设置。

# ***\*Light Probe 光照探头\****

获取物体位置的光照探头参数。需要传入法线数据进行探头采样。

# ***\*Object\****

获取物体的位置和缩放。

# ***\*Reflection Probe 反射探头\****

访问物体对应的反射探头。需要 Normal和 View Direction两个输入参数进行采样。LOD参数可以设置LOD的级别。

# ***\*Screen 屏幕\****

获取屏幕的宽高，单位是像素。

# ***\*Cubemap Asset\****

定义一个cubemap常量。需要在后面连接Sample Cubemap节点。

# ***\*Sample Cubemap\****

如上图所示，根据输入参数View Direction和Normal进行cubemap采样。可以设置LOD作为LOD采样级别。

# ***\*Sample Texture 2D\****

2D贴图采样。通过这个节点获取贴图中的RGBA数据。

# ***\*Sample Texture 2D Array\****

对2D贴图数组进行采样。

# ***\*Sample Texture 2D LOD\****

为Texture 2D Sample添加了LOD功能。Sample Texture 2D LOD使用与Sample Texture 2D相同的输入和输出栏，但包括通过Vector1输入参数调整细节级别的输入。

# ***\*Sampler State\****

用于Sample Texture 2D节点中的输入结点Sampler(SS)。

# ***\*Texture 2D Array Asset\****

2D贴图数组常量。可以一次输入多次采样。

# ***\*Texture 2D Asset\****

2D贴图常量。可以一次输入多次采样。

# ***\*Texture 3D Asset\****

3D贴图常量。可以一次输入多次采样。

# ***\*PBR Master\****

PBR(physically based rendering)主节点，该节点是基于物理渲染的节点。可以用于金属或高光材质。

# ***\*Unlit Master\****

不受光照的主节点。

# ***\*Absolute 绝对值\****

返回输入值的绝对值。

# ***\*Exponential 指数\****

输入值为In，输出值是Base的In次幂。Base可以通过下拉框选择2或者e。

如Base选择Base2，输入In为3，那么输出Out = 2 ^ 3 = 8

# ***\*Length 模长\****

返回一个向量的模长，即向量的长度。

![img](2019-12-04-Unity-AdvancedLearning-ShaderGraph.assets/wps1.png) 

# ***\*Log 对数\****

输入值为In，输出值以Base为底，In的对数。Base可以通过下拉框选择2、10或者e。

# ***\*Modulo 模数\****

输入为A和B，输出Out = A % B
例如A是10，B是3，那么10 % 3 = 1

# ***\*Negate 相反数\****

输入为In，输出为Out = -1 x In

# ***\*Normalize 单位化\****

单位化输入的向量，即向量的方向不变，但是模长为1。
如下图所示，| v | 代表向量v的模长，u即为单位化后的向量。

# ***\*Posterize 色调分离\****

色调分离是指一幅图像原本是由紧紧相邻的渐变色阶构成，被数种突然的颜色转变所代替。这一种突然的转变，亦称作“跳阶”。

# ***\*Reciprocal 倒数\****

输入为In，输出Out = 1 / In

# ***\*Reciprocal Square Root 平方根倒数\****

输入为In，输出Out = 1 / In^0.5

# ***\*Add 加法\****

输出Out = 输入A + 输入B

# ***\*Divide 除法\****

输出Out = 输入A / 输入B

# ***\*Multiply 乘法\****

输出Out = 输入A * 输入B

# ***\*Power 幂\****

输出Out = 输入A ^ 输入B

# ***\*Square Root 平方根\****

输出Out = 输入In ^ 0.5

# ***\*Subtract 减法\****

输出Out = 输入A - 输入B



# ***\*DDX\****

返回屏幕空间中输入In在X轴上的偏导数。这个节点只能用于像素着色器阶段。

# ***\*DDXY\****

返回屏幕空间中输入In在X轴和Y轴上的偏导数的和。这个节点只能用于像素着色器阶段。

# ***\*DDY\****

返回屏幕空间中输入In在Y轴上的偏导数。这个节点只能用于像素着色器阶段。

# ***\*Lerp 线性插值\****

根据输入A、B和T进行线性插值。T的值会被Clamp到[0, 1]。
公式是：Out = (1-T)*A + T*B

# ***\*Inverse Lerp 反向线性插值\****

根据输入A、B和T求出线性插值的系数。这个操作是Lerp的反向操作。
公式是： Out = (T - A)/(B - A)

# ***\*Smoothstep 平滑阶梯\****

如果输入In在Edge1和Edge2之间，则返回Hermite插值

# ***\*Matrix Construct 构造矩阵\****

根据输入的向量构造矩阵。下拉框可以选择Row或Column。
选择Row时，每个向量代表矩阵的每一行。
选择Column时，每个向量代表矩阵的每一列。M0代表第一列，M1代表第二列……

# ***\*Matrix Determinant 矩阵行列式\****

返回该矩阵的行列式。

# ***\*Matrix Split 分割矩阵\****

将矩阵按行或列分割成向量。

# ***\*Matrix Transpose 矩阵转置\****

返回矩阵的转置。

# ***\*Clamp\****

如果输入In介于A、B之间返回In；若输入In小于Min，返回Min；若大于Max，返回Max

# ***\*Fraction 取小数部分\****

返回输入In的小数部分。比如输入3.14则返回0.14。

# ***\*Maximum 最大值\****

返回两个值的最大值，如果是多维向量则取每个维度的最大值

# ***\*Minimum 最小值\****

返回两个值的最小值，如果是多维向量则取每个维度的最小值

# ***\*One Minus 1减去\****

输出Out = 1 - 输入In

# ***\*Random Range 范围随机\****

根据输入Seed返回一个伪随机数值，介于输入Min和Max之间。

虽然输入种子中的相同值总是会导致相同的输出值，但输出值本身为随机值。Input Seed是一个Vector 2值，方便根据UV输入生成一个随机数，但对于大多数情况，Vector 1输入就足够了。

# ***\*Remap 重映射\****

根据输入In在InMinMax中的插值，计算输出。
比如输入In为0，InMinMax时（-10，10），那么In在InMinMax的位置就是0.5，如果OutMinMax为（0，10），那么输出Out就是5

公式为：Out = OutMinMax.x + (In - InMinMax.x) * (OutMinMax.y - OutMinMax.x) / (InMinMax.y - InMinMax.x)

# ***\*Saturate\****

将输入In Clamp到[0, 1]

# ***\*Ceiling 向上取整\****

比如输入5.4，则返回6
比如输入6，则返回6

# ***\*Floor 向下取整\****

比如输入5.4，则返回5
比如输入6，则返回6

# ***\*Round 四舍五入\****

比如输入5.5，则返回6
比如输入5.4，则返回5

# ***\*Sign 正负符号\****

小于0返回-1
等于0返回0
大于0返回1

# ***\*Step 阶梯\****

如果输入In大于等于输入Edge，返回1，否则返回0

# ***\*Truncate 截取（舍弃小数）\****

比如输入5.5，则返回5.0
比如输入5.4，则返回5.0

# ***\*Arccosine 反余弦arccos\****

输入应该在[-1, 1]

# ***\*Arcsine 反正弦arcsin\****

输入应该是-Pi/2 到 Pi/2

# ***\*Arctangent 反正切arctan\****

输入应该是-Pi/2 到 Pi/2

# ***\*Arctangent2 即Atan2\****

# ***\*Cosine 余弦cos\****

# ***\*Degrees To Radians 角度转弧度\****

比如输入360，输出PI
比如输入60，输出PI/6

# ***\*Hyperbolic Cosine 双曲余弦cosh\****

# ***\*Hyperbolic Sine 双曲正弦sinh\****

# ***\*Hyperbolic Tangent 双曲正切tanh\****

# ***\*Radians To Degrees 弧度转角度\****

比如输入PI/6，输出60

# ***\*Sine 正弦sin\****

# ***\*Tangent 正切tan\****

# ***\*Cross Product 叉乘\****

返回输入A和B的值的叉乘。两个向量的叉乘的结果是一个垂直于两个输入向量的第三个向量。结果的大小等于两个输入向量的模，乘以输入向量之间角度的正弦值，如上图。可以使用“左手法则”确定结果向量的方向。

叉乘法与左手法则
左手展平，四指并拢，拇指与四指呈90度夹角。让第一个矢量的箭头刺向左掌心，并使四指指根到指尖方向与第二个矢量指向相同，拇指指根到指尖的方向就是第三个矢量的方向。

 

# ***\*Distance 距离\****

返回两个向量的欧式距离

# ***\*Dot Product 点乘\****

# ***\*Fresnel Effect 菲涅尔效应\****

 

菲涅耳效应是基于视角的不同对表面反射率的影响，接近掠射角度时会反射更多光线。菲涅耳效应节点通过计算表面法线和视角方向之间的角度。该角度越宽，返回值越大。这种效果通常用于实现边缘照明，这在许多特效中很常见。

Out = pow((1.0 - saturate(dot(normalize(Normal), normalize(ViewDir)))), Power)

# ***\*Projection 投影\****

将输入A投射到向量B上，返回投射后的向量。

Out = B * dot(A, B) / dot(B, B)

# ***\*Rejection\****

# ***\*Checkerboard 检查板\****

创建一个检查板效果，基于输入的Color A和Color B和UV，交替显示两种颜色。检查版的尺寸由输入参数Frequency决定。

# ***\*Gradient Noise 渐变噪点\****

基于输入的UV生成一个渐变噪点图（Perlin噪点）。Scale可以控制噪点图的大小。

# ***\*Simple Noise\****

基于输入的UV生成一个简单噪点图

# ***\*Voronoi 泰森多边形\****

基于输入的UV生成一个泰森多边形噪点图

# ***\*Ellipse 椭圆\****

根据输入的UV生成一个椭圆形状，尺寸由输入参数Width和Height决定。生成的形状可以通过在UV输入之前连接Tiling And Offset节点进行偏移或平铺。请注意，为了保持在UV空间内偏移形状的能力，如果平铺，形状不会自动重复。要实现重复，先通过Fraction节点连接输入。

# ***\*Polygon 多边形\****

根据输入UV生成规则多边形形状，尺寸由输入参数Width和Height决定。多边形的边的数量由输入Sides确定。生成的形状可以通过在UV输入之前连接Tiling And Offset节点进行偏移或平铺。请注意，为了保持在UV空间内偏移形状的能力，如果平铺，形状不会自动重复。要实现重复，先通过Fraction节点连接输入。

# ***\*Rectangle 矩形\****

根据输入的UV生成一个矩形形状，尺寸由输入参数Width和Height决定。生成的形状可以通过在UV输入之前连接Tiling And Offset节点进行偏移或平铺。请注意，为了保持在UV空间内偏移形状的能力，如果平铺，形状不会自动重复。要实现重复，先通过Fraction节点连接输入。

# ***\*Rounded Rectangle 圆角矩形\****

根据输入的UV生成一个矩形形状，尺寸由输入参数Width和Height决定，圆角半径由Radius参数决定。生成的形状可以通过在UV输入之前连接Tiling And Offset节点进行偏移或平铺。请注意，为了保持在UV空间内偏移形状的能力，如果平铺，形状不会自动重复。要实现重复，先通过Fraction节点连接输入。

# ***\*Preview 预览\****

显示一个预览窗口，这个节点不会修改输入值。

# ***\*All\****

所有输入为非0时，返回true，否则返回false。这个节点经常和下面的Branching节点一起使用。

# ***\*And\****

如果输入A和B都为true，则返回true，否则返回false。这个节点经常和下面的Branching节点一起使用。

注意这个节点的输入为布尔类型。

# ***\*Any\****

如果输入值中有任意一个元素为非0值，则返回true，否则返回false。这个节点经常和下面的Branching节点一起使用。

# ***\*Branch 分支\****

如果输入值Predicate为true，则会返回输入值True，否则返回输入值False。注意这里的输入参数名字为True和False。

# ***\*Comparison 比较\****

根据下拉框的选项比较A和B两个值。

# ***\*Is Front Face 是否是正面\****

你可以根据给定片段的朝向标识更改图形输出。如果当前片段是正面的一部分，则节点返回True。对于背面，节点返回False。注意：此功能需要在主节点上启用双面。

# ***\*Is Infinite 是否无穷大\****

输入值中是否包含无穷大值。一般无穷大值出现在除0的情况。

# ***\*Is NaN 是否非数值\****

输入参数中，如果有任意一个元素为非数值，则返回true。

# ***\*Nand 与非\****

如果输入值A和B都为false，则返回true。

# ***\*Not 非\****

将输入值变为相反值。

# ***\*Or 或\****

如果输入值A和B有一个为true，则返回true，如果全为false，则返回false。

# ***\*Flipbook 图象序列\****

创建一个图像像素列，作为UV输入。序列中序列图的数量是由输入的Width和Height决定的。当前显示第一个序列图是由输入参数Tile决定的。

这个节点经常用来创建贴图动画，通常用于粒子效果和sprite。方法是：将Time节点连到输入参数的Tile中，输出数据连接到Texture Sampler节点的UV输入参数中。

UV数据的(0,0)点是左下角。(0,0)点在预览窗口中显示为黑色。但是图像序列通常是从左上角开始，所以Invert Y参数默认是勾中的。可以通过设置Invert X和Invert Y来改变图像序列的方向。

# ***\*Polar Coordinates 极坐标\****

将输入的UV坐标转换成极坐标。在数学中，极坐标系是一种二维坐标系，其中平面上的每个点由参考点的距离和与参考方向的角度确定。

这个节点的结果是将输入参数UV的x通道转换为距输入Center值对应点的距离值，并将y通道转换为围绕该点的旋转角度值。

这些值可以分别通过输入参数Radial Scale和Length Scale进行缩放。

# ***\*Radial Shear 径向剪切\****

将类似于波浪的径向剪切扭曲效果应用于输入参数UV。扭曲效果的中心参考点由输入参数Center决定，效果的整体强度由输入参数Strength决定。输入参数Offset可用于偏移结果的各个通道。

# ***\*Rotate 旋转\****

将输入值UV根据中心点Center进行旋转，旋转的量由输入参数Rotation决定。旋转量的单位可以通过Unit下拉框选择角度Degrees或者弧度Radians。

# ***\*Spherize 球面化\****

将类似于鱼眼镜头的球形扭曲效果应用于输入参数UV。扭曲效果的中心参考点由输入参数Center定义，效果的整体强度由输入参数Strength定义。输入参数Offset可用于偏移结果的各个通道。

# ***\*Tiling And Offset 平铺和偏移\****

这个功能我们会经常用到，一般只要有贴图的材质上都会有Tiling And Offset。

将输入参数UV的值，通过Tiling和Offset改变平铺和偏移。

# ***\*Triplanar\****

Triplanar 是通过世界空间投影生成UV和采样贴图的一种方式。输入贴图Texture会被采样3次，世界空间的每个x，y和z轴上进行一次采样，然后将结果信息平面投影到模型上，并由法线或表面角度进行混合。生成的UV可以通过输入参数Tile进行缩放，最终的混合强度可以通过输入参数Blend进行控制。可以通过输入参数Position和Normal来修改投影。这通常用于大型模型（如地形）的贴图，大型模型使用UV坐标时会出现问题或效率很低。

输入贴图的类型可以使用下拉菜单Type进行切换。如果设置为Normal，法线将转换到世界空间，因此可以构造新的切线，然后在输出之前将其转换回切线空间。

# ***\*Twirl 扭曲\****

将类似于黑洞的旋转扭曲效果应用于输入的UV。扭曲效果的中心参考点由输入参数Center决定，效果的整体强度由输入参数Strength决定。输入Offset*可用于偏移结果的各个通道。