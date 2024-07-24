---
layout: post
title: OMSCS-CV课程笔记05-Stereo Geometry
date: 2021-09-08
description: 立体视觉
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍立体视觉的相关内容。
{: .block-preface }


## Stereo Geometry

在相机的成像过程中我们去掉了空间点的z坐标，也就是说我们无法只通过单一图像来获知空间点的深度信息。更严重的问题是如果存在两个点P1和P2与投影中心共线，在投影后它们会具有相同的坐标而且我们没有办法对它们进行区分。

<div align=center>
<img src="https://i.imgur.com/n8RlZjm.png" width="50%">
</div>

因此仅通过单张图像去还原物体的空间信息是非常困难的。要解决这个问题最简单的方法是添加一个额外的相机，用两个相机来还原物体的空间信息。实际上人眼感知空间也是利用了类似的原理，当我们用双眼观察物体时两只眼睛的观察结果会有微小的差异，这个微小的差异就产生了物体的空间感。

最基本的立体视觉系统由两个相机组成，我们称为**双目视觉(stereo vision)**。对于空间中的点P假设我们知道它在两张图像上的位置以及两个相机的位姿，我们就可以利用图像还原出P点在空间中的坐标。

<div align=center>
<img src="https://i.imgur.com/UEBdY5o.png" width="70%">
</div>

双目视觉的经典应用是空间点的**深度估计(depth estimation)**。假设我们有两个平行放置焦距为$$f$$的相机，它们投影中心的距离为B称为**基线(baseline)**。此时空间点P与两个投影中心构成了一个平面如图下图所示：

<div align=center>
<img src="https://i.imgur.com/XfX5Yv0.png" width="40%">
</div>

记P的深度为$$Z$$，它在两张图像上的横坐标分别为$$x_l$$和$$x_r$$。根据相似三角形可以得到几何关系：

$$
\frac{B}{Z} = \frac{B-x_l+x_r}{Z-f} = \frac{x_l-x_r}{f}
$$

因此深度可以表示为：

$$
Z = \frac{B}{x_l-x_r} f
$$

其中$$x_l-x_r$$表示P点在两张图像上横坐标的差异，称为**视差(disparity)**。对于同一个双目相机，视差越小表明该点到相机的距离越远。描述图像上每一点视差的图片称为**视差图(disparity map)**。

<div align=center>
<img src="https://i.imgur.com/oVJhB8U.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/PxpRpoM.png" width="80%">
</div>

## Epipolar Geometry

在大部分实际应用中我们很难保证两个相机相互平行，此时利用图像还原空间信息会相对困难一些。

<div align=center>
<img src="https://i.imgur.com/C24pXGH.png" width="80%">
</div>

设空间点P在两张图像上的投影分别为$$p$$和$$p'$$。P点与两个投影中心构成了一个平面称为**极平面(epipolar plane)**，极平面与投影平面的交线称为**极线(epipolar line)**，基线与投影平面的交点称为**极点(epipole)**。根据几何关系不难发现极平面上的点会被投影到成像平面的极线上，此外同一投影平面上所有的极线都交于极点。极点、极线和极平面之间的关系称为**对极约束(epipolar constraint)**。

<div align=center>
<img src="https://i.imgur.com/0opuZb6.png" width="60%">
</div>

对极约束的一个直接应用是加速图像上点的匹配：对于图像上的任一点在寻找匹配时无需在另一张图像上进行遍历，根据对极约束我们只需要在极线上进行搜索即可，这样大大加快了匹配的过程。

<div align=center>
<img src="https://i.imgur.com/lhbFEwV.png" width="70%">
</div>

当两个相机相互平行时，极线会变成图像中相互平行的水平直线。此时极点位于成像平面的无穷远处。

<div align=center>
<img src="https://i.imgur.com/fF306IR.png" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/N6tFdY1.png" width="70%">
</div>

## Stereo Correspondence

利用对极约束可以加速对应点匹配但并没有解决如何匹配的问题，因此我们仍然需要对应点的匹配算法来计算视差图。为了便于讨论这里假定空间中的点都能够在两个相机中观察到，而且极线均为水平直线。

<div align=center>
<img src="https://i.imgur.com/9mWVECF.png" width="60%">
</div>

最基本的匹配方法是利用相似性进行匹配。对于空间中的点可以认为它在两张图像中的投影是相似的，因此可以利用滑窗的方式来计算视差。

<div align=center>
<img src="https://i.imgur.com/1JJyoTQ.png" width="70%">
</div>

显然对于左边图像给定的窗口，在右边图像中匹配代价最小、相似性最高的位置即为该窗口的匹配点。

<div align=center>
<img src="https://i.imgur.com/K4RbgFe.png" width="70%">
</div>

对于具有显著特征的区域，上述基于相似性的匹配算法基本可以得到良好的匹配结果；但对于特征不明显的低纹理区域直接进行匹配可能无法得到令人满意的结果。

<div align=center>
<img src="https://i.imgur.com/Qtj0U3D.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/A3ZFrEE.png" width="70%">
</div>

同时窗口的尺寸对于匹配的结果也会产生一定的影响。如果窗口尺寸过小，窗口可能无法捕捉到图像的纹理信息导致很多区域上无法进行匹配；而如果窗口尺寸过大则可能会在匹配时失去一些细节导致结果过于平滑。

<div align=center>
<img src="https://i.imgur.com/6m8TWrv.png" width="70%">
</div>

立体匹配时的另一个常见问题是**遮挡(occlusion)**。此时场景中的某些点仅能在一个相机中观测到，在这样的情况下进行匹配是无法得到合理的匹配结果的。

<div align=center>
<img src="https://i.imgur.com/ZSskp9W.png" width="60%">
</div>

在一些匹配算法中可能会使用匹配点的顺序来优化匹配结果，此时我们假定匹配点在左右两张图像中必须按照相同的顺序出现。

<div align=center>
<img src="https://i.imgur.com/F7uVJVn.png" width="60%">
</div>

当然这样的假定在某些情况下也是不能得到满足的。

<div align=center>
<img src="https://i.imgur.com/FiGSUO8.png" width="40%">
<img src="https://i.imgur.com/fsAw1Bv.png" width="40%">
</div>

如果直接使用基于滑窗的匹配方法，即使每一点都得到了最佳匹配最终得到的视差图可能也与实际情况相差甚远。

<div align=center>
<img src="https://i.imgur.com/RsiisNW.png" width="70%">
</div>

在一些更现代的方法中往往还会结合一些全局的信息来辅助和优化匹配的过程，比如说在对每一行进行匹配的时候可以利用动态规划来计算当前行的最优匹配。

<div align=center>
<img src="https://i.imgur.com/S3Vl6dm.png" width="70%">
</div>

不过使用动态规划得到的匹配结果可能会出现一些条纹状的伪影，这是由于每一行的计算的相互独立的；同时需要注意的是动态规划这样的方法不能直接在二维图像上使用。

<div align=center>
<img src="https://i.imgur.com/5VUjPN2.png" width="40%">
</div>

使用全局信息来进行立体匹配的思路是将匹配问题转化为最优化问题。我们可以定义系统总体的能量包括每个点匹配后的具有的能量$$E_\text{data}$$以及匹配结果的光滑性$$E_\text{smooth}$$：

$$
E_\text{data} = \sum_i (W_1(i) - W_2(i + D(i)))^2
$$

$$
E_\text{smooth} = \sum_i \sum_{\text{neighbors} \ j} \rho(D(i) - D(j))
$$

<div align=center>
<img src="https://i.imgur.com/wR0b97x.png" width="70%">
</div>

$$E_\text{data}$$描述了每个点单独匹配的结果，匹配效果越好能量越小；而$$E_\text{smooth}$$则描述了视差图的光滑程度，匹配的结果越光滑能量越小。显然一个良好的全局匹配应该最小化系统的总体能量，一般可以通过graph-cut等算法来求解。使用全局信息优化后的匹配结果如下图所示，可以发现此时匹配得到的视差图已经非常接近真实的视差。

<div align=center>
<img src="https://i.imgur.com/3Av27LE.png" width="70%">
</div>

最后需要说明的是立体匹配仍然是一个没有完全解决的问题，目前立体匹配的难点包括：

1. 低纹理的区域很难进行匹配；
2. 遮挡问题还没有很好的解决办法；
3. 场景中光照发生改变或者表面反光的区域很难进行匹配；
4. 两个相机的位置差异过大时拍摄的图片会有很大的区别，此时很难进行匹配；
5. 如果极线的计算存在误差，有时可能无法进行匹配。