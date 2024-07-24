---
layout: post
title: OMSCS-CV课程笔记02-Hough Transform
date: 2021-08-31
description: 霍夫变换
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍霍夫变换的相关内容。
{: .block-preface }


## Hough Line Transform

**霍夫变换(Hough transform)**是一种经典的图像处理技术，我们可以通过它来检测图像中的各种几何元素。二维平面上最简单的几何元素是直线，对应的检测算法称为**霍夫线变换(Hough line transform)**。

直线检测的难点在于平面上存在无穷多条直线，使用暴力穷举直线的方法是不可行的。因此霍夫线变换使用了**计数(voting)**的方式来统计图像中可能出现的直线，其思想是只考虑图像中可能出现的直线并进行计数，如果计数结果大于一定的阈值则说明图像上存在一条直线。

下面介绍霍夫线变换的基本原理。我们可以用直线方程来表示平面上通过点$$(x, y)$$的任意直线：

$$
y = m_0 x + b_0
$$

其中$$m_0$$和$$b_0$$是直线的参数。如果把直线方程看做是参数的函数，那么也可以在参数平面来表示直线，即图像平面的一条直线对应参数平面上的一个点。

<div align=center>
<img src="https://i.imgur.com/Ztofz6W.png" width="70%">
</div>

这说明图像平面和参数平面存在对偶关系，图像平面上的点也可以用参数平面上的直线来表示：

$$
y = m_0 x + b_0 \Leftrightarrow b_0 = -x m_0 + y
$$

<div align=center>
<img src="https://i.imgur.com/Ezgr9ES.png" width="70%">
</div>

对于图像平面上的两个点$$(x_0, y_0)$$和$$(x_1, y_1)$$，我们可以分别在参数平面上画出对应的两条直线，它们的交点即代表图像平面上通过$$(x_0, y_0)$$和$$(x_1, y_1)$$的直线。

<div align=center>
<img src="https://i.imgur.com/wPdqN4a.png" width="70%">
</div>

因此我们可以利用参数平面来寻找图像上的直线元素。对于图像上的一系列点我们可以在参数平面上画出它们对应的直线，这些直线的交点即为图像平面上通过这一系列点的直线，交点的坐标为直线方程的参数。

<div align=center>
<img src="https://i.imgur.com/rMW5rYq.png" width="70%">
</div>

在此基础上可以得到霍夫线变换的基本流程如下：

1. 对图像进行边缘检测得到边缘点；
2. 将参数平面划分成网格，每个格子对应图像平面的一条直线，同时初始化格子计数为0；
3. 对边缘点进行遍历，在每个点上考虑参数平面通过该点的所有格子并令相应格子的计数加1；
4. 完成遍历后统计参数平面网格上的计数，大于阈值的格子即为图像平面上直线方程的参数。

在实际中通常使用极坐标下的直线方程作为参数：

$$
x \cos \theta + y \sin \theta = d
$$

此时只需要在参数平面上考虑$$\theta$$的所有可能取值并利用$$(x, y)$$反算出剩下的参数$$d$$。

<div align=center>
<img src="https://i.imgur.com/buEoPYd.png" width="30%">
</div>

使用极坐标作为参数平面的霍夫线变换算法流程如下：

<div align=center>
<img src="https://i.imgur.com/7CQ1AiD.png" width="90%">
</div>

因此霍夫线变换将直线检测的问题转化为参数平面的计数问题，所需的空间复杂度为参数平面网格的大小$$O(k^2)$$，时间复杂度为边缘点的数量$$O(n)$$。

在标准流程外我们还可以利用图像梯度的方向显式指定参数$$\theta$$，或是赋予不同参数不同的权重来提升算法的效果。

<div align=center>
<img src="https://i.imgur.com/Kv5MMyH.png" width="90%">
</div>

<div align=center>
<img src="https://i.imgur.com/guV5rpq.png" width="90%">
</div>

## Hough Circle Transform

使用同样的思想进行圆的检测就得到了**霍夫圆变换(Hough circle transform)**，此时我们只需要将直线方程替换为圆方程即可。假设圆心为$$(a, b)$$半径为$$r$$，圆上的每个点$$(x_i, y_i)$$在平面上满足方程：

$$
(x_i - a)^2 + (y_i - b)^2 = r^2
$$

根据对偶性$$(x_i, y_i)$$在参数平面上对应以$$(x_i, y_i)$$为圆心半径为$$r$$的圆，因此我们只需要在参数平面上画出相应的圆并寻找到它们的交点就能得到图像平面上的圆心$$(a, b)$$。

<div align=center>
<img src="https://i.imgur.com/AwAkWEk.png" width="90%">
</div>

在实际应用中由于往往事先不知道圆的半径大小，我们还需要对半径$$r$$进行遍历。此时霍夫圆变换会根据每个可能半径$$r$$在相应的参数平面上画圆：

<div align=center>
<img src="https://i.imgur.com/gFUNweT.png" width="90%">
</div>

显然把半径$$r$$直接加入到参数空间中会极大地降低算法的性能，更常用的方法是结合图像的梯度来筛选可能出现的圆。对于图像上给定点$$(x_i, y_i)$$，圆心只可能出现在该点的梯度方向上。因此在堆半径$$r$$进行遍历时可以显式计算出圆心$$(a, b)$$的位置，无需在参数平面上画圆。

<div align=center>
<img src="https://i.imgur.com/PQ9UXlA.png" width="90%">
</div>

在实践中使用霍夫变换需要特别关注参数空间的离散化，过于粗糙或是精细的参数网格都会对检测结果产生影响。同时霍夫变换的时间复杂度随参数量按指数递增，因此也需要对几何形状的参数化方式加以注意。

## Generalized Hough Transform

在很多情况下一些几何形状不能使用参数化的方式进行表达，此时则需要使用**广义霍夫变换(generalized Hough transform)**。

对于这一类几何形状我们首先在图形的边界点上分别计算边界点的梯度方向$$\theta_i$$和它到图形中心的位移向量$$r_i = c - p_i$$，这样可以建立以梯度方向为key位移向量为value的表格来表示图形，这个表格称为**Hough table**。

<div align=center>
<img src="https://i.imgur.com/eR9FYCu.png" width="40%">
</div>

在图像上利用Hough table进行计数就实现了广义霍夫变换。假设Hough table在图像上不会发生尺度和朝向的变化，此时广义霍夫变换与霍夫线变换的流程基本一致：

1. 对图像进行边缘检测得到边缘点，并在边缘点上计算图像梯度的方向；
2. 对边缘点进行遍历，根据该点的梯度方向在Hough table中查找该方向上所有可能的位移向量$$r_i$$；
3. 每个位移向量$$r_i$$对应一个可能的图形中心点，在参数平面上令该点的计数加1；
4. 完成遍历后统计中心点的计数，大于阈值的即为图像上给定形状出现的位置。

如果图像上Hough table出现了尺度或朝向的变化则需要将尺度因子和角度变化加入到遍历中，同时在计算几何形状中心点坐标时也需要考虑相应的缩放和旋转变换。

## Reference
- [Wikipedia: Hough Transform](https://en.wikipedia.org/wiki/Hough_transform)
- [Wikipedia: Generalized Hough Transform](https://en.wikipedia.org/wiki/Generalised_Hough_transform)