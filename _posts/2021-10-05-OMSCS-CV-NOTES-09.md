---
layout: post
title: OMSCS-CV课程笔记09-Lightness and Brightness
date: 2021-10-05
description: 光照和着色
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍光照和着色的相关内容。
{: .block-preface }


## Photometry

**光度学(photometry)**是研究光强弱的学科。对于三维空间中的物体，我们之所以能够看见它们是因为物体反射了光。反射光的强弱决定了我们看到的物体的颜色，而反射光则由物体法线、物体的材质、光照条件以及观察的角度所决定。

<div align=center>
<img src="https://i.imgur.com/ZOer74o.png" width="60%">
</div>

### Radiometry

如果想要更严格地描述反射的过程则需要借助**辐照度量学(radiometry)**的相关知识。我们定义**辐射率(radiance)**为单位立体角和**垂直**此方向的单位面积上的辐射通量，记为$$L$$(W⋅sr$$^{−1}$$⋅m$$^{−2}$$)；**辐照度(irradiance)**单位面积上接收到的辐射通量，记为$$E$$(W⋅m$$^{−2}$$)。radiance和irradiance可通过入射方向与法向的夹角$$\theta$$来进行转换：

$$
E(\theta, \varphi) = L(\theta, \varphi) \cos \theta \ d\omega
$$

<div align=center>
<img src="https://i.imgur.com/ZpYW7Oh.png" width="30%">
</div>

当光线照射到物体上时，物体反射出的radiance由**双向反射分布函数(bidirectional reflectance distribution function, BRDF)**控制，它定义为反射出的radiance与接收到的irradiance的比值：

$$
f(\theta_i, \varphi_i; \theta_r, \varphi_r) = \frac{L(\theta_r, \varphi_r)}{E(\theta_i, \varphi_i)}
$$

<div align=center>
<img src="https://i.imgur.com/az7L7iY.png" width="70%">
</div>

BRDF需要满足Helmholtz互异性(Helmholtz reciprocity)，即交换入射和出射方向函数值不变。同时BRDF还满足旋转对称性，即BRDF仅与入射和出射方向在平面上的相对夹角$$\varphi_i - \varphi_r$$有关，当入射方向和出射方向同时绕法向旋转时BRDF保持不变。

$$
f(\theta_i, \varphi_i; \theta_r, \varphi_r) = f(\theta_r, \varphi_r; \theta_i, \varphi_i)
$$

$$
f(\theta_i, \varphi_i; \theta_r, \varphi_r) = f(\theta_i, \theta_r, \varphi_i - \varphi_r)
$$

### Reflection Models

对于表面比较粗糙的物体，我们可以认为光线到达物体表面后会向各个方向发生反射，这种类型的反射称为**漫反射(diffuse reflection)**。

<div align=center>
<img src="https://i.imgur.com/nvhk9eG.png" width="50%">
</div>

另一种常见的反射类型是**镜面反射(surface reflection)**，此时反射会集中到某些指定的方向使得物体表面出现反光的效果。

<div align=center>
<img src="https://i.imgur.com/PNfmEhs.png" width="50%">
</div>

当然大多数情况下物体表面会同时发生漫反射和镜面反射。

<div align=center>
<img src="https://i.imgur.com/YvrBSi4.png" width="52%">
</div>

#### Diffuse Reflection and Lambertian BRDF

漫反射的一个重要特征是无论观察的角度如何，漫反射的强度都是一样的。换句话说漫反射材质的BRDF是一个常数，我们称之为**反照率(albedo)：**

$$
f(\theta_i, \varphi_i; \theta_r, \varphi_r) = \rho_d
$$

我们将化简后的BRDF带入反射过程得到漫反射情况下反射的radiance：

$$
L = \rho_d I \cos \theta_i = \rho_d I (n \cdot s)
$$

<div align=center>
<img src="https://i.imgur.com/SFRrA3O.png" width="60%">
</div>

#### Specular Reflection and Mirror BRDF

对于镜面反射的BRDF，我们可以利用$$\delta$$函数来描述只在给定方向上存在radiance的情况：

$$
f(\theta_i, \varphi_i; \theta_v, \varphi_v) = \rho_s \delta (\theta_i - \theta_v) \delta (\varphi_i + \pi - \varphi_v)
$$

类似地，镜面反射出的radiance为：

$$
L = \rho_s I \delta(m - v)
$$

<div align=center>
<img src="https://i.imgur.com/9G7olsF.png" width="67%">
</div>

### Phong Reflection Model

我们把漫反射和镜面反射结合到一起就得到了图形渲染中非常常用的Phong反射模型，它将漫反射和镜面反射相加来模拟真实材质的BRDF。

<div align=center>
<img src="https://i.imgur.com/qqB029q.png" width="50%">
</div>

## Lightness

在上一节中我们介绍了物体的反射模型。对于相机来说物体的反射光决定了图片上接收到的颜色，但人眼对于颜色的感知是不完全由接收到的光线决定的。以下图为例，A和B两块棋盘格上的颜色是完全相同的，但人眼往往会认为A处的颜色会更深一些。

<div align=center>
<img src="https://i.imgur.com/I0a6Vlo.png" width="30%">
<img src="https://i.imgur.com/QGAiVSi.png" width="30%">
</div>

类似地，图像的空间感也会干扰人眼对颜色的认知。

<div align=center>
<img src="https://i.imgur.com/NhNL6oN.png" width="50%">
</div>

实际上人眼会把接收到的光线进行分解，具体而言人的视觉系统会把光分解成illumination和reflectance两部分。当我们看到某个物体时，大脑会尽可能将我们看到的颜色还原成它在白光下的颜色。因此对于不同光照条件下的同一个物体，人眼往往会看到相同的颜色；相应地，人眼对于光源的变化则没有那么敏感。

从前一节的内容中我们知道物体的颜色可以表示为光照$$E$$与反射方程$$R$$的乘积：

$$
L(x, y) = R(x, y) \cdot E(x, y)
$$

<div align=center>
<img src="https://i.imgur.com/uNIsSqc.png" width="50%">
</div>

那么对于具有不同反照率的平面物体在变化的光源下会得到类似于下面的图像：

<div align=center>
<img src="https://i.imgur.com/g0AE3LE.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/eusF6q4.png" width="50%">
</div>

如果假定光照是缓慢变化的，那么我们可以从照射得到的图像上恢复物体的反照率。具体而言需要只对图像取对数并计算导数：

<div align=center>
<img src="https://i.imgur.com/DzEtWNP.png" width="50%">
</div>

由于光照是缓慢变化的，在取对数后光照对应导数中非常小的部分，我们可以使用阈值化来过滤掉它。这样我们对过滤后的函数进行积分就能够重建出物体的albedo(和真实值只相差一个常数)。

<div align=center>
<img src="https://i.imgur.com/PyEj30h.png" width="60%">
</div>

当然这样的方法对于的三维空间物体是不适用的，这是因为通常情况下三维空间中物体接收到的光线不满足缓慢变化的假设。

## Shape from Shading

通过着色我们还可以重建物体的表面。假设空间中的曲面方程为$$z(x, y)$$，定义$$p$$、$$q$$分别为曲面在两个方向上的负导数：

$$
p = -\frac{\partial z}{\partial x}, q = -\frac{\partial z}{\partial y}
$$

对于曲面上的任意点，我们可以利用$$p$$和$$q$$定义出两个切向量：

$$
t_x = (1, 0, -p)^T, t_y = (0, 1, -q)^T
$$

因此该点的曲面法向为：

$$
n = \frac{t_x \times t_y}{\Vert t_x \times t_y \Vert} = \frac{1}{\sqrt{p^2 + q^2 + 1}} (p, q, 1)^T
$$

我们可以把法向$$n$$移动到单位球上并将它延长到$$z=1$$的平面上，这个平面称为gradient space。显然对于任意方向的法向我们总能在gradient space上找到法向与平面的交点，且交点坐标恰为$$(p, q, 1)$$。

<div align=center>
<img src="https://i.imgur.com/21ftIF2.png" width="50%">
</div>

类似地，我们把光线入射方向也映射到Gradient Space上，得到入射方向的单位向量：

$$
s = \frac{1}{\sqrt{p_S^2 + q_S^2 + 1}} (p_S, q_S, 1)^T
$$

此时入射方向与法向的夹角为：

$$
\cos \theta_i = n \cdot s = \frac{p \cdot p_S + q \cdot q_S + 1}{\sqrt{p^2 + q^2 + 1} \cdot \sqrt{p_S^2 + q_S^2 + 1}}
$$

<div align=center>
<img src="https://i.imgur.com/AMS0xIk.png" width="50%">
</div>

假设物体表面是Lambert面，对应的albedo为$$\rho$$；同时假定来自光源的入射光强度为$$k$$。那么曲面上任意点的反射光强度为：

$$
I = \rho \cdot k \cdot \cos \theta_i = \rho \cdot k (n \cdot s)
$$

不妨设$$\rho \cdot k = 1$$，此时反射光可以化简为：

$$
I = n \cdot s = \frac{p \cdot p_S + q \cdot q_S + 1}{\sqrt{p^2 + q^2 + 1} \cdot \sqrt{p_S^2 + q_S^2 + 1}} = R(p, q)
$$

我们称$$R(p, q)$$为Lambert面的Reflectance Map。曲面的法向一定在$$R(p, q) = I$$所定义的曲线上，如下图所示。同时$$R(p, q)$$仅在$$(p_S, q_S)$$处取最大值$$(p, q) = 1$$，此时光线入射方向与法向重合；$$R(p, q)$$在直线$$p \cdot p_S + q \cdot q_S + 1 = 0$$上取最小值$$(p, q) = 0$$，此时入射方向与法向相互垂直。

<div align=center>
<img src="https://i.imgur.com/R0VwyQQ.png" width="60%">
</div>

因此对于平面图像上的任意一点我们可以取出该点的像素值并在gradient space绘制出对应的曲线，该点在曲面上的法向一定位于这条曲线上。尽管如此，我们还是无法确定法向的具体方向。

<div align=center>
<img src="https://i.imgur.com/4e0Kl7w.png" width="60%">
</div>

想要确定具体地法向一般有两种做法。第一种方法是引入额外的约束，比如说假定已知曲面的边界而且曲面比较光滑，再通过一系列复杂的优化是就解出具体的法向。当然这样的方法在实际中的效果并不好，工程上更常用的方法是利用多张不同光源下的图像来重建曲面，这样的方法称为**光度立体(photometric stereo)**。

具体来说，我们需要固定相机和物体的位置然后利用3个不同角度的光源来拍摄图像。假设入射光的强度均为1，在每个光源下曲面上的反射光满足方程：

$$
I_i = \rho \ n \cdot s_i
$$

联立3个光源可以得到矩阵方程：

$$
\begin{bmatrix}
I_1 \\ I_2 \\ I_3
\end{bmatrix}
=
\begin{bmatrix}
s_1^T \\ s_2^T \\ s_3^T
\end{bmatrix}
\rho n
$$

记$$\hat{n} = \rho n$$，通过求解线性方程组可以得到：

$$
\hat{n} = \rho n = S^{-1} I
$$

上式对于包含多组不同光源的图像仍然适用。由于法向$$n$$是单位向量，我们只需要对$$\hat{n}$$进行规范化即可得到曲面法向，同时我们还可以得到该点的反照率：

$$
\rho = \vert \hat{n} \vert, n = \frac{\hat{n}}{\rho}
$$

<div align=center>
<img src="https://i.imgur.com/ujRHsB8.png" width="37%">
<img src="https://i.imgur.com/FdKsPgK.png" width="15%">
<img src="https://i.imgur.com/eoV5j1W.png" width="15%">
<img src="https://i.imgur.com/bahr9yZ.png" width="15%">
</div>

光度立体的本质是在gradient space上求曲线交点。对于每个给定的光源，我们都可以在gradient space上画出相应的曲线，且待求的法向一定位于这些曲线的交点上。由于每条曲线都是二次曲线，我们至少需要3条曲线才能确定这个交点。

<div align=center>
<img src="https://i.imgur.com/L2zTZgL.png" width="50%">
</div>

利用光度立体的方法我们可以重建曲面的法线并对曲面的albedo进行估计，如下图所示。

<div align=center>
<img src="https://i.imgur.com/3ni3ROf.png" width="70%">
</div>

当然光度立体的方法也存在一些缺陷，比如说它基本无法处理反光和半透明的材质，同时对于阴影和相互反射(inter-reflections)的情况也没有特别好的解决方法。此外光度立体一般需要保证光源和相机距离物体比较远，而且必须要知道光线的方向和强弱。这些缺陷都限制了光度立体的应用场景。