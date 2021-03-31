---
title: 计算机图形学11 ——  贝塞尔曲线和贝塞尔曲面
tags: [计算机图形学, 几何, 贝塞尔曲线, 贝塞尔曲面]
categories: [GAMES101]
index_img: /images/graphics11/graphics11_banner.png
date: 2020-11-18 23:34:13
math: true
---

曲线和曲面是图形学中重要的组成部分，在前面的笔记中介绍了隐式曲面和显式曲面，它们都是表示曲面（线）的方法。本笔记即将介绍的贝塞尔曲面和贝塞尔曲线是一种隐式曲面的表示方法，它可以使用简单的控制点高效地表示曲面（线）。

## 贝塞尔曲线
给定 n+1个数据点，[$p_0 (x0 , y0) … p_n(xn , yn)]$，生成一条曲线，使得该曲线与这些点所描述的形状相符。

如果要求曲线通过所有的数据点，则属于插值问题；如果只要曲线逼近这些数据点，则属于逼近问题。

逼近在计算机图形学中主要用来设计美观的或符合某些美学标准的曲线。为了解决这个问题，有必要找到一种用小的部分即曲线段构建曲线的方法，来满足设计标准。下图为一个贝塞尔曲线的示例：
![](/images/graphics11/bezier_define.png)

其中 $p_0、p_1、p_2、p_3$为控制点，蓝色所表示曲线正是非常著名的贝塞尔曲线了，可以从图中观察到，曲线会与初始与终止端点相切，并且经过起点$p_0$与终点$p_3$。那么这样一条曲线究竟是怎么得到的呢？

其实贝塞尔曲线的定义很像参数方程，给定一个参数$t \in  [0, 1]$就能确定贝塞尔曲线上的一点，倘若取完所有t值，就能得到完整的贝塞尔曲线。首先我们先从最简单的三个控制点的情况进行讨论（n + 1个控制点得到的就是n阶贝塞尔曲线，三个控制点则为二阶贝塞尔曲线）。正如一开始所说，第一步选定一个参数 $t \in  [0, 1]$，在$b_0b_1$线段之上利用t值进行线性插值:
![](/images/graphics11/bezier_process1.png)

由此可以得到 $b_0^1 = b_0 + (b_1 - b_0) * t$，同理可以在$b_1b_2$线段使用线性插值的方式得到点$b_1^1$：

![](/images/graphics11/bezier_process2.png)

之后再基于这个原理继续在$b_0^1b1^1$上进行线性插值，得到点$b_0^2$
![](/images/graphics11/bezier_process3.png)

重复以上插值过程即可得到贝塞尔曲线，**其核心所在就是多次的线性插值，并在生成的新的顶点所连接构成的线段之上递归的执行这个过程，直到得到最后一个顶点。**

当我们通过二阶贝塞尔曲线掌握这一过程之后，更高阶的贝塞尔曲线也是手到擒来，下图是三阶贝塞尔曲线的求解过程：

![](/images/graphics11/bezier_in_three.png)

实际上也是迭代的对存量线段线性插值，最后得到曲线上的一个点。

那么我们为何称由n个控制点的贝塞尔曲线为n-1次呢？同样以一开始的3个控制点为例，将贝塞尔曲线方程完全展开看看

![](/images/graphics11/bezier_math.png)

其实看到这就已经非常清楚了，最终得到的贝塞尔曲线方程恰好就是一个关于参数 t 的二次方程，如果细心观察的话，其实可以发现控制点的系数是非常有规律的，很像二项系数，因此可以总结规律得到一个任意控制点组成的贝塞尔曲线的方程如下:

![](/images/graphics11/bezier_bonstein.png)

对于这样一个特殊系数其实也有一个多项式与之对应，正是伯恩斯坦多项式，其定义如图中下方所示。

至此就是贝塞尔曲线原理的所有内容了，相信显现在对于任意的控制点，都能很快的画出对应的曲线，最后我们对贝塞尔曲线的几点性质做一个概括:
1. 必定经过起始与终止控制点
2. 必定经与起始与终止线段相切
3. 具有仿射变换性质，可以通过移动控制点移动整条曲线
4. 凸包性质，曲线一定不会超出所有控制点构成的多边形范围

回想一下PS等具有画图功能的软件中的钢笔工具所运用的便是贝塞尔曲线了，除了这个例子之外许多字体或是矢量图都广泛运用了贝塞尔曲线。但对于高阶贝塞尔曲线它有一个严重的缺陷:
![](/images/graphics11/bezier_n_10.png)

对于上图所示的由11个控制点所得到的10次贝塞尔曲线，由于控制点众多，很难控制局部的贝塞尔曲线形状，因此为了解决该问题，有人提出了分段贝塞尔曲线，即将一条高次曲线分成多条低次曲线的拼接，其中用的最多的便是用很多的3次曲线来拼接，如下图:
![](/images/graphics11/bazier_concat.png)
如果想要使得拼接的点看起来较为光滑的话，就要满足一些连续条件如一阶连续(连接点导数的左右极限相等)，二阶连续等等，这些都是高数的基础知识，在此不多做赘述。
## 贝塞尔曲面