---
title:  "Unity-WebGL的坑"

categories: unity

tags:  unity webgl

author: Miny
---

## 有一种痛，叫作 “我的项目转成**WebGL**环境时报了错”

本篇将对遇到过的或**大**或小的问题进行一系列汇总

愿看过的人不用**熬夜**发布WebGL（真™的~~服~~了）

1. 使用Unity默认的Arial字体无法显示中文，需要更换其他中文字体（微软雅黑/黑体/其他）。建议字体大小设为24（不要加粗），发布出来效果比较好。

2. Unity自带的InputField无法在WebGL上输入中文，使用Unity推出的IME input for Unity WebGL(Assets Store,免费)。如果用到InputField，注意判断空值问题，尽量避免让用户输入很多文字。

3. WebGL发布后，会出现段落部分文字不显示的问题。可以修改发布设置中的ColorSpace将Gamma修改为Linear或直接将文字替换成图片，注意：修改成Linear会导致项目整体颜色变淡。（创建项目后，优先改成Linear）

4. 当文本中需要打空格是可以使用搜狗输入法敲入v1d

5. WebGL不要使用System.IO.File，DLLImport注释掉（会导致黑屏），尽量避免使用WWW加载

6. 当项目资源比较大或浏览器已经提示内存不足时，可修改发布设置的内存设置。（推荐256~512，太小报Out of Memory Size，太大浏览器崩溃，自2019版本已弃用该设置）

7.  打包时设置的路径以及StreamingAssets文件夹中的资源不可以有中文

8. 视频播放要用VideoPlayer组件Url模式（不要用MovieTexture），视频格式需要转换为.ogv，且大小要有一定控制（可用Theora Converter .NET）

9. 部分shader不可用，HDRP不可用，LWRP可用（2018.3不稳定，2019.2已稳定）高光、轮廓闪烁等，部分不支持（Highlighter在编辑器容易出Bug，发布出来无问题），建议使用Highlight Plus（Assets Store有售）

10. 如果涉及漫游、旋转视角等功能，要写鼠标锁死和隐藏

11. 避免Esc键功能，Alt键功能，鼠标滚轮键功能

12. 发布路径建议和工程路径保持同级（例如：G:\Project\和G:\Package\）

13. 尽量减少场景内实时光的使用，减少MeshCollider使用

14. 发布时注意删除Update中的Debug.Log

