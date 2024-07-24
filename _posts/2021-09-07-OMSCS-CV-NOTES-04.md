---
layout: post
title: OMSCS-CV课程笔记04-Camera Models
date: 2021-09-07
description: 相机模型
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍相机模型的相关内容。
{: .block-preface }


## Cameras and Images

在图像处理中我们把图像视为一种二维函数，但现实生活中的图像不仅仅是一个二维函数。实际上图像是空间中的点在二维平面上的投影，我们通过相机将三维空间中的点投影到图像平面上。

### Pinhole Camera

现实生活中最简单的相机是**针孔相机(pinhole camera)**，它通过小孔成像的原理将空间中的点投影到底片上。

<div align=center>
<img src="https://i.imgur.com/B5LMp0N.png" width="70%">
</div>

针孔相机的一个主要问题在于成像不够清晰，这通常是由于针孔的尺寸取得过大所导致的。当针孔比较大时同一点发出的不同光线会投影到底片的不同位置，来自不同物体的光线在同一位置上相互叠加就会导致成像变得模糊。同时需要说明的是针孔的尺寸并不是越小越好，当针孔过于小时由于光的衍射(diffraction)也可能会导致成像的模糊。

<div align=center>
<img src="https://i.imgur.com/YYglSfz.png" width="70%">
</div>

### Lenses

实际中更常用的相机是带镜头的相机，我们利用镜头来将光线汇聚到底片上。通常情况下我们假定镜头足够薄，这样根据几何关系可以得到**成像公式(thin lens equation)**：

$$
\frac{1}{z} + \frac{1}{Z} = \frac{1}{f}
$$

<div align=center>
<img src="https://i.imgur.com/9nNf5pC.png" width="50%">
</div>

当相机和透镜确定时，**光圈(aperture)**的大小控制了**景深(depth of field)**的范围。当光圈比较大时会有较多的光线进入透镜，此时的景深较小；相反的当光圈比较小时只会有少部分的光线进入透镜，此时的景深会增大。

<div align=center>
<img src="https://i.imgur.com/jR2KTF0.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/9BSP6EO.png" width="70%">
</div>

在成像过程中另一个重要参数是**视野(field of view)** $$\phi$$，它与焦距$$f$$以及底片尺寸$$d$$有关。当底片尺寸确定时焦距越大，视野越小。

$$
\phi = \tan^{-1} \bigg( \frac{d}{2 f} \bigg)
$$

<div align=center>
<img src="https://i.imgur.com/NlwWGhp.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/gKarQoK.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/AaMVHWm.png" width="80%">
</div>

直观上理解视野描述了成像区域的大小，视野越大成像的范围就越大。但需要注意的是调整视野和调整成像距离是不完全一致的，这在人像摄影中尤为重要。

<div align=center>
<img src="https://i.imgur.com/Lzrct7A.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/xvp4j9P.png" width="70%">
</div>

此外，焦距还会对透视产生一定的影响。当焦距不断增大时，成像的结果会趋向于平行投影。

<div align=center>
<img src="https://i.imgur.com/4RD9spN.png" width="70%">
</div>

焦距的在成像中的另一个应用是**滑动变焦(dolly zoom)**。只需要改变焦距和成像距离而无需移动物体就可以干扰人眼对空间的认知，这在很多影视作品中都有应用。

<div align=center>
<img src="https://upload.wikimedia.org/wikipedia/commons/c/c7/Contra-zoom_aka_dolly_zoom_animation.gif" width="40%">
</div>

现实生活中的透镜由于制造工艺等因素会存在一定的缺陷，其中最常见的缺陷是**畸变(distortion)**，此时原本的直线在投影后会变成曲线：

<div align=center>
<img src="https://i.imgur.com/WUKSPF8.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/jJvUc0j.png" width="47%">
<img src="https://i.imgur.com/t0NdfnY.png" width="32%">
</div>

另一种常见的缺陷是**色差(chromatic aberration)**，它是指透镜无法将各种波长的色光都聚焦在同一点上的现象。

<div align=center>
<img src="https://i.imgur.com/kYJjhQt.png" width="70%">
</div>

<div align=center>
<img src="https://upload.wikimedia.org/wikipedia/commons/6/66/Chromatic_aberration_%28comparison%29.jpg" width="40%">
</div>

此外还可能会出现**晕影(vignetting)**的现象：由于镜筒的存在倾斜入射的光线比垂直入射的光线要少一些，使得图像的边缘比中心显得更暗。

<div align=center>
<img src="https://photographylife.com/wp-content/uploads/2013/10/Optical-Vignetting.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/tPn2fhx.png" width="70%">
</div>

## Perspective Imaging

对于现代相机而言通过复杂的透镜系统可以去除掉绝大部分的镜头缺陷，使得最终呈现的图像类似于针孔相机的成像结果，因此在进行分析时可以直接假定图像来自于针孔相机模型。

### Modeling Projection

对针孔相机的投影过程进行建模，我们可以在**投影中心(center of projection, COP)**建立坐标系将空间中的点投影到**投影平面(projection plane, PP)**上。根据相似三角形可以得到投影前后坐标的变换公式：

$$
(X, Y, Z) \rightarrow (-d \frac{X}{Z}, -d \frac{Y}{Z}, -d)
$$

在投影平面上可以丢掉z坐标得到平面坐标：

$$
(x', y') = (-d \frac{X}{Z}, -d \frac{Y}{Z})
$$

<div align=center>
<img src="https://i.imgur.com/icZd1oR.png" width="50%">
</div>

需要说明的是这里为了数学上推导的方便把投影平面放到了相机朝向的后方；同时为了保证相机坐标系是右手系还规定z轴的正方向指向相机后方，在其它的资料中的建模方式可能与此不同进而导致坐标变换公式存在一些差异。

### Homogeneous Coordinates

显然针孔相机的投影过程不是一个线性变换，但实际上我们可以利用**齐次坐标(homogeneous coordinates)**的技巧将投影过程表示为一个线性变换。具体来说，齐次坐标是在原有的坐标上额外添加一个维度：

$$
(x, y) \rightarrow (x, y, 1)
$$

$$
(x, y, z) \rightarrow (x, y, z, 1)
$$

同时我们规定齐次坐标向原有坐标的转换方法是除以最后一维的数值：

$$
(x, y, w) \rightarrow (\frac{x}{w}, \frac{y}{w})
$$

$$
(x, y, z, w) \rightarrow (\frac{x}{w}, \frac{y}{w}, \frac{z}{w})
$$

上式说明齐次坐标具有尺度不变性：在齐次坐标上乘以一个非0常数不会改变原有的坐标。

### Perspective Projection

通过齐次坐标就可以把投影过程表示为线性变换的形式：

$$
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & \frac{1}{f} & 0 \\
\end{bmatrix}

\begin{bmatrix}
x \\ y \\ z \\ 1
\end{bmatrix}
=
\begin{bmatrix}
x \\ y \\ \frac{1}{f} z
\end{bmatrix}
\rightarrow
\begin{bmatrix}
\frac{f}{z} x \\ \frac{f}{z} y
\end{bmatrix}
=
\begin{bmatrix}
u \\ v
\end{bmatrix}
$$

其中$$f$$表示投影中心到投影平面的距离，也就是焦距。由于齐次坐标具有尺度不变性，上式可以改写为：

$$
\begin{bmatrix}
f & 0 & 0 & 0 \\
0 & f & 0 & 0 \\
0 & 0 & 1 & 0 \\
\end{bmatrix}

\begin{bmatrix}
x \\ y \\ z \\ 1
\end{bmatrix}
=
\begin{bmatrix}
fx \\ fy \\ z
\end{bmatrix}
\rightarrow
\begin{bmatrix}
\frac{f}{z} x \\ \frac{f}{z} y
\end{bmatrix}
$$

上式定义的投影过程称为**透视投影(perspective projection)**。显然空间中的点经过透视投影后再图像平面上仍然是点，直线和多边形经过投影后仍然是直线和多边形。同时可以证明空间中平行的直线在透视投影后会相交，假设空间中经过点$$(x_0, y_0, z_0)$$的直线为：

$$
x(t) = x_0 + a t \\
y(t) = y_0 + b t \\
z(t) = z_0 + c t \\
$$

投影后的点坐标为：

$$
x'(t) = \frac{f}{z(t)} x(t) = \frac{f \cdot (x_0 + a t)}{z_0 + c t} \\
y'(t) = \frac{f}{z(t)} y(t) = \frac{f \cdot (y_0 + b t)}{z_0 + c t} \\
$$

当$$t \to \infty$$时投影后的点会趋于$$(\frac{fa}{c}, \frac{fb}{c})$$，这说明空间中方向为$$(a, b, c)$$的平行直线经过透视投影后都会相交于投影平面上的一个点，称为**灭点(vanishing point)**。类似地，空间中同一平面上的所有直线经过投影后它们的灭点会位于投影平面的同一直线上。

<div align=center>
<img src="https://i.imgur.com/crwBf0k.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/SRtZixs.png" width="50%">
</div>

### Other Models

除了透视投影外，常用的投影模型还包括**正交投影(orthographic projection)**和**弱透视投影(weak perspective projection)**。在正交投影中我们直接丢掉空间点的z坐标，对应的投影计算公式为：

$$
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}

\begin{bmatrix}
x \\ y \\ z \\ 1
\end{bmatrix}
=
\begin{bmatrix}
x \\ y \\ 1
\end{bmatrix}
\rightarrow
\begin{bmatrix}
x \\ y
\end{bmatrix}
$$

正交投影表示物距$$z$$和焦距$$f$$趋于无穷大情况下相机的投影过程，可以近似望远镜的成像效果。同时空间中平行的直线经过正交投影后仍是平行的，因此正交投影也称为**平行投影(parallel projection)**。

<div align=center>
<img src="https://i.imgur.com/9fdnnPi.png" width="50%">
</div>

另一种常见的投影模型是弱透视投影，此时我们假定空间中的点都位于距离投影中心$$z_0$$远的平面上。根据透视变换的原理得到投影后点的坐标为：

$$
(x', y') = (\frac{f x}{z_0}, \frac{f y}{z_0})
$$

写成矩阵形式为：

$$
\begin{bmatrix}
1 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 \\
0 & 0 & 0 & \frac{1}{s} \\
\end{bmatrix}

\begin{bmatrix}
x \\ y \\ z \\ 1
\end{bmatrix}
=
\begin{bmatrix}
x \\ y \\ \frac{1}{s}
\end{bmatrix}
\rightarrow
\begin{bmatrix}
sx \\ sy
\end{bmatrix}
$$

<div align=center>
<img src="https://i.imgur.com/SOD6Xsa.png" width="70%">
</div>

和透视投影相比弱透视投影要简单一些，但投影的结果不如透视投影精确。