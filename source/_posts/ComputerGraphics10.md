---
title: 计算机图形学10 - 几何介绍
tags: [计算机图形学, 几何, 隐式曲面, 显式曲面]
categories: [GAMES101]
index_img: /images/graphics10/graphics10_banner.png
date: 2020-11-11 23:45:22
math: true
---
## 几何介绍
几何形体是计算机图形学当中十分重要的一部分内容，无论是人物，风景，建筑，都离不开几何，如何表示好各种各样的模型是几何部分的主要研究内容之一。在一般物体的渲染过程中，几何充当着非常重要的作用，它是物体的骨架。通常来说几何可以被分为两大类——隐式和显式。

### 隐式曲面介绍
所谓隐式曲面指的是并不会告诉你任何点的信息，只会告诉你该曲面上所有点满足的关系。来举个具体的隐式曲面的例子:
$$
x^2 + y^2 + z^2 = 1
$$
以上方程式表示了空间中满足几何曲面的关系的点的几何，它代表了我们常见的球面体。一般来说，我们会把隐式曲面表示为方程式的形式：$f(x, y, z) = 0$，球面的隐式方程为： $f(x, y, z) = x^2 + y^2 + z^2 - 1$。

隐式方程的表示虽然方便，直接可以用数学公式来进行表示，但是对于光栅化过程确比较难。因为对于一个隐式方程而言，很难对一个特定的点进行采样，比如在光栅化过程中介绍地对一次函数进行采样就遇到一些难题。因此对于隐式方程来说因为没有给出任何点的信息，因此如何采样到曲面上具体的点是一个很难的问题，如下图这样一个例子:
![](/images/graphics10/sampling.png)

很难找到圆环上的一个点，甚至看到此隐式方程不知道它是什么图形。

但是隐式方式有个非常大的优点，在于可以十分方便地判断点、线与曲面的关系。
![](/images/graphics10/inside.png)
利用这个特性可以很方便地判断光线与物体是否相交，在光线追踪的过程中，这个特性会非常有用。（但是可惜渲染过程中很少使用隐式曲面来渲染物体）

### 显式曲面介绍
显示曲面实际上是一种空间中所有点的集合，曲面中所有的点都会被直接给出或者是可以通过映射的方式获取。
![](/images/graphics10/uv_mapping.png)

上图中的马鞍图形就是基于一种图形映射方式，将坐标由$(u, v)$ 向 $(x,y,z)$坐标进行转换即可得到显式曲面。

显式曲面相较于隐式曲面的采样难的缺点而言是非常由优势的，比如下图中圆环的显式曲面的表示方式中：
![](/images/graphics10/uv_plug.png)

想要得到圆环表面的所有点，带入所有的uv值即可得到这些点。

但是凡事都有两面性，显示曲面对于判断点或者光线的关系来说是非常复杂的。比如下图中判断点与物体的关系：
![](/images/graphics10/in_or_not.png)

这个过程往往需要大量的计算的匹配，在后续的光线追踪的笔记中会详细地介绍这一个过程。

**总体而言，没有“最好的”方法来表示几何，根据具体的任务来选择隐式还是显式才是合理的做法。**

## 隐式曲面详解

### 代数曲面(Algebraic Surfaces)
对于该类隐式曲面来说其实正是在第一章中举例说明所运用到的，通过代数表达式可以得到许许多多不同的几何曲面:
![](/images/graphics10/algebraic.png)
但似乎单纯代数表达式的曲面都颇具有规则性，而且其表示过程也很复杂，那么对于更复杂的几何形体怎么办呢？ 

### Constructive Solid Geometry(CSG)
CSG就是解决普通代数曲面无法表示复杂图形的解决方案。CSG过程简单来说就是对简单的几何物体进行交、并、差计算之后得到的一种曲面过程。
![](/images/graphics10/csg1.png)

上图表示了一个CSG的过程，对两个物体求交、并、差后可分别得到其他图形。如下图所示：
![](/images/graphics10/csg2.png)

### 符号距离函数(Signed Distance Function)
除了对于几何的布尔操作，还可以通过距离函数来得到几何形体混合的效果，如下图:
![](/images/graphics10/sdf1.png)
如何得到这种blend的效果，就要从SDF即符号距离函数说起了(这里的符号是指距离可以有正有负)。

首先对于符号距离函数来说本质上就是一种定义距离的函数。如有空间任意一点到各个几何物体表面的距离，对这些距离做各种各样的运算操作最后得到的一个函数就是最终的距离函数了，举一个简单的例子:
![](/images/graphics10/sdf2.png)
对于这样一个二维平面的例子，定义空间中每一个点的SDF为该点到阴影区域右边界的垂直距离，在阴影内部为负，外部为正，因此对于A和B两种阴影来说的SDF分别如上图下半部分所示。

有了$SDF(A)$和$SDF(B)$，之后对这两个距离函数选择性的做一些运算得到最终的距离函数，这里采用最简单的$SDF = SDF(A)+SDF(B)$来举例，最终得到的SDF为零的点的集合即为blend之后曲面，对该例子来说，就是两道阴影之间中点的一条线。

### 水平集(Level Set)
水平集的方法其实与SDF很像(像是SDF的一种特殊形式)，也是找出函数值为0的地方作为曲线，但不像SDF会空间中的每一个点有一种严格的数学定义，而是对空间用一个个格子去近似一个函数，如下:
![](/images/graphics10/level_set1.png)
对该面内的每一个点利用已经定义好的格子值进行双线性插值(在纹理映射一节已讲解)就可以得到任意一点的函数值，找出所有=0的点作为曲面。

该方法的好处是对于SDF，我们可以更加显示的区空间曲线的形状。该方法广泛的运用在医学成像和物理模拟之中:
![](/images/graphics10/level_set2.png)

### 分型几何(Fractals)
分型几何是指许许多多自相似的形体最终所组成的几何形状。
![](/images/graphics10/fractals.png)
如雪花是一个六边形，放大之后会发现每一个边上又是一个六边形，再放大六边形边上的六边形边上又是六边形，就这样无限套娃，有点递归的意思。

以上就是对隐式曲面的具体例子的一些介绍了，总的来说隐式曲面具有形式简单，轻易判断点与曲面关系等优点，但难以采样曲面点和模拟过于复杂的形状。


## 显式曲面详解
除了在第一节中介绍的参数方程的显式曲面表示方法外，还有其他的现实曲面的表示方法。
### 点云(Point Cloud)
![](/images/graphics10/point_cloud.png)
顾名思义，就是很多很多的点构成的曲面，直接有着所有点的信息，每个点包含了点的位置和点的颜色信息。

### 多边形网格(Polygon Mesh)

![](/images/graphics10/mesh.png)

多边形网格是在计算机图形学中一种比较常见的一种技术方案，即使用规定好的规范来顶底物体的顶点、法线、纹理坐标以及这些数据的关联等信息。
![](/images/graphics10/obj.png)

比如在渲染中经常使用的obj文件来存储加载模型。