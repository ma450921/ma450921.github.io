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
以上方程式表示了空间中满足几何曲面的关系的点的几何，它代表了我们常见的球面体。一般来说，我们会把隐式曲面表示为方程式的形式：$f(x, y, z) = 0$，球面的隐式方程为： $f(x, y, z) = x^2 + y^2 + z^2 - 1$
### 显式曲面介绍