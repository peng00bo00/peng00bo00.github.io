---
layout: post
title: OMSCS-CV课程笔记07-Multiple Views
date: 2021-09-21
description: 多视图几何
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍多视图几何的相关内容。
{: .block-preface }


## Image to Image Projections

在正式介绍多视图几何的相关内容前我们首先来考虑一下平面上的几何变换。平面上的几何变换可以分为**平移变换(translation)**、**刚体(欧式)变换(Euclidean transform)**、**相似变换(similarity transform)**以及**仿射变换(affine transform)**，而它们都可以看做是**投影变换(projective transform)**的特例。

<div align=center>
<img src="https://i.imgur.com/qV6Uush.png" width="70%">
</div>

通过齐次坐标可以把这些变换写成通用的形式：

$$
\begin{bmatrix}
x' \\ y' \\ w'
\end{bmatrix}
=
\begin{bmatrix}
a & b & c \\ d & e & f \\ g & h & i
\end{bmatrix}
\begin{bmatrix}
x \\ y \\ w
\end{bmatrix}
$$

$$
x' = H x
$$

其中矩阵$$H$$称为**单应矩阵(homography)**，它刻画了平面到平面的坐标变换关系。

### Translation

最基本的平面变换是平移变换，它的单应矩阵形式为：

$$
H = 
\begin{bmatrix}
1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1
\end{bmatrix}
$$

<div align=center>
<img src="https://i.imgur.com/n4j0rTX.png" width="50%">
</div>

显然在平移变换前后物体的夹角、长度、面积、朝向等几何量都保持不变。

### Euclidean Transform

在平移变换的基础上加入旋转就得到了刚体变换，对应的单应矩阵形式为：

$$
H = 
\begin{bmatrix}
\cos{\theta} & -\sin{\theta} & t_x \\ \sin{\theta} & \cos{\theta} & t_y \\ 0 & 0 & 1
\end{bmatrix}
$$

<div align=center>
<img src="https://i.imgur.com/QpEdkMr.png" width="50%">
</div>

显然物体的朝向在变换后会发生改变，但夹角、长度、面积保持不变。

### Similarity Transform

如果允许对平移变换后的物体进行缩放就得到了相似变换，它的单应矩阵为：

$$
H = 
\begin{bmatrix}
a \cos{\theta} & -a \sin{\theta} & t_x \\ a \sin{\theta} & a \cos{\theta} & t_y \\ 0 & 0 & 1
\end{bmatrix}
$$

<div align=center>
<img src="https://i.imgur.com/OGWX6Pz.png" width="50%">
</div>

相似变换后的物体只能保证夹角大小不变，物体的面积和长度则会按照相似比例变化。

### Affine Transform

进一步放宽约束条件可以得到仿射变换：

$$
H = 
\begin{bmatrix}
a & b & c \\ d & e & f \\ 0 & 0 & 1
\end{bmatrix}
$$

<div align=center>
<img src="https://i.imgur.com/4407cCM.png" width="50%">
</div>

仿射变换可以保证平行的直线经过变换后仍然保持平行。

### Projective Transform

最后，利用齐次坐标的归一化性质可以投影变换的但应矩阵：

$$
H = 
\begin{bmatrix}
a & b & c \\ d & e & f \\ g & h & 1
\end{bmatrix}
$$

<div align=center>
<img src="https://i.imgur.com/kb4S2bs.png" width="50%">
</div>

投影变换只能保证直线在变换后仍保持直线，但不能保证平行的直线变换后保持平行。

我们把前面介绍的这几种变换的性质进行汇总，可以得到如下的表格：

<div align=center>
<img src="https://i.imgur.com/mIT1Ue6.png" width="60%">
</div>

## Homographies and Mosaics

图像上的齐次坐标实际上可以从三维空间进行理解。此时多出来的一维$$w$$表示空间点的深度，因此图像平面上的点实际上表示的是空间中位于平面$$z=1$$上的点。同时利用齐次坐标的规范化性质，坐标$$(x, y, 1)$$可以理解为从原点出发经过图像平面上点$$(x, y, 1)$$的射线。

<div align=center>
<img src="https://i.imgur.com/Q56tdOx.png" width="30%">
</div>

利用齐次坐标的几何意义可以实现图像平面到图像平面的变换。假设空间中存在2个具有相同成像中心的图像平面PP1和PP2，我们想要计算两个图像平面的变换关系。对于空间中的点P可以做一条成像中心C到P点的连线与图像平面分别交于P1和P2两点，如下图所示：

<div align=center>
<img src="https://i.imgur.com/eMIOhuM.png" width="30%">
</div>

不难发现实际上我们不需要考虑两个平面的空间关系，只需要知道投影点的坐标变换关系就可以实现PP1和PP2之间的转换。

### Panorama

图像平面变换的一个典型应用是获得**全景图(panorama)**。我们可以利用图像平面的变换将不同角度拍摄的图像变换到一个固定的角度，从而获得更大的视野。

<div align=center>
<img src="https://i.imgur.com/vNAPQjy.png" width="70%">
</div>

全景图拼接的本质是把不同的成像平面变换到一个给定的平面上，从而得到空间点在该平面上的投影。因此这样的过程需要保证不同图像的成像中心是同一点，同时视野的角度需要小于180°，否则会产生一些扭曲的问题。

<div align=center>
<img src="https://i.imgur.com/Wl2B40P.png" width="20%">
</div>

### Homography

图像平面到图像平面的变换可以利用投影变换的单应矩阵$$H$$来描述，两幅图像上对应点的坐标满足：

$$
p' = H p
$$

由于单应矩阵$$H$$具有8个自由度，我们需要至少4组对应点才能求解。类似于计算相机矩阵的过程，我们可以假定$$H$$矩阵的最后一个元素为1来求解；而实际中更常用的方式是利用奇异值分解来计算，整个计算流程类似于[Direct Linear Calibration](/2021/09/14/OMSCS-CV-NOTES-06.html#direct-linear-calibration)中介绍的方法。

### 3D Planes

使用单应矩阵来描述图像平面变换的另一个好处在于我们可以任意空间平面到图像平面的投影。假设空间中的点分布于某个平面上，对应的平面方程为：

$$
a X + b Y + c Z + d = 0
$$

我们将平面方程带入到投影过程得到：

$$
\begin{bmatrix}
u \\ v \\ 1
\end{bmatrix}
\simeq
\begin{bmatrix}
m_{00} & m_{01} & m_{02} & m_{03} \\
m_{10} & m_{11} & m_{12} & m_{13} \\
m_{20} & m_{21} & m_{22} & m_{23} \\
\end{bmatrix}

\begin{bmatrix}
X \\ Y \\ \frac{a X + b Y + d}{-c} \\ 1
\end{bmatrix}
$$

由于$$Z$$项是其它三维的线性组合，我们可以把相机矩阵的第3列合并到其它列中，得到新的投影方程：

$$
\begin{bmatrix}
u \\ v \\ 1
\end{bmatrix}
\simeq
\begin{bmatrix}
m_{00}' & m_{01}' & 0 & m_{03}' \\
m_{10}' & m_{11}' & 0 & m_{13}' \\
m_{20}' & m_{21}' & 0 & m_{23}' \\
\end{bmatrix}

\begin{bmatrix}
X \\ Y \\ \frac{a X + b Y + d}{-c} \\ 1
\end{bmatrix}
$$

上式说明当空间中的点位于同一平面上时，我们只需要一个$$3 \times 3$$的矩阵就可以描述投影过程，即单应矩阵$$H$$描述了三维平面到图像平面的投影。

### Image Rectification

我们可以把图像看做空间中的一个平面，这样利用单应矩阵就可以完成对图像的矫正(rectification)。

<div align=center>
<img src="https://i.imgur.com/cdZL4iN.png" width="50%">
</div>

图像矫正的一个常见应用是把透视投影矫正为正交投影，使原本相交于灭点的平行直线在投影后仍保持平行，这样方便我们在矫正后的图像上进行处理。

<div align=center>
<img src="https://i.imgur.com/Zbzgzag.png" width="30%">
<img src="https://i.imgur.com/o5Yvycu.png" width="45%">
</div>

### Image Warping

使用单应矩阵进行图像变换时需要注意的是我们实际上是对变换后的图像进行处理。具体来说对于变换后图像$$g(x, y)$$上的点$$(x', y')$$，我们需要把原始图像$$f(x, y)$$上对应点$$(x, y)$$处的像素拷贝过来。

<div align=center>
<img src="https://i.imgur.com/JYmu3Q0.png" width="70%">
</div>

在大多数情况下原始图像上对应点的坐标都不是整数，因此我们需要通过插值的方式来计算像素值。实践中往往会利用双线性插值来进行计算：

$$
\begin{aligned}
f(x, y) &= (1-a)(1-b) \cdot f(i, j) \\ &+ a(1-b) \cdot f(i+1, j) \\ &+ ab \cdot f(i+1, j+1) \\ &+ (1-a)b \cdot f(i, j+1)
\end{aligned}
$$

<div align=center>
<img src="https://i.imgur.com/JKHbo4b.png" width="30%">
</div>

## Projective Geometry

### Point and Line Duality

对于二维平面，我们可以利用齐次坐标来表示直线方程：

$$
a x + b y + c = 0 \Leftrightarrow 
\begin{bmatrix}
a & b & c
\end{bmatrix}
\begin{bmatrix}
x \\ y \\ 1
\end{bmatrix}
= 0
$$

其中向量$$l = \begin{bmatrix} a & b & c \end{bmatrix}^T$$表示直线方程的参数。从代数的角度上看直线方程与坐标向量并没有本质区别，都只是一个三维的向量。因此平面直线与齐次坐标是对偶的。

如果把齐次坐标看成是经过原点的射线，那么平面直线同样可以理解为经过原点的平面与成像平面的交线。此时直线向量$$l$$表示该平面的法向量。

<div align=center>
<img src="https://i.imgur.com/VxsaaFK.png" width="30%">
</div>

利用对偶性可以方便地计算平面上的直线方程。假设图像平面上存在两个点$$p_1$$和$$p_2$$，经过两个点的直线$$l$$需要满足：

$$
l^T p_1 = 0
$$

$$
l^T p_2 = 0
$$

从空间中来看通过$$p_1$$和$$p_2$$的直线实际上就是射线张成的平面与图像平面的交线，该平面的法向为：

$$
l = p_1 \times p_2
$$

<div align=center>
<img src="https://i.imgur.com/UkgvEpm.png" width="30%">
</div>

类似地，假设图像上存在两条直线$$l_1$$和$$l_2$$且它们的交点为$$p$$。$$p$$点的坐标同样可以由$$l_1$$和$$l_2$$给出

$$
l_1^T p = 0
$$

$$
l_2^T p = 0
$$

$$
p = l_1 \times l_2
$$

<div align=center>
<img src="https://i.imgur.com/LSNdXw7.png" width="30%">
</div>

上面的分析说明在投影空间下直线与点是相互对偶的，利用叉积运算可以方便地求解直线方程和直线交点。

<div align=center>
<img src="https://i.imgur.com/k7ztm6Z.png" width="30%">
<img src="https://i.imgur.com/MkPW4OZ.png" width="37%">
</div>

### Ideal Points and Lines

当齐次坐标的最后一维为0时，坐标向量$$(x, y, 0)$$与成像平面平行。我们称此时的点为**理想点(ideal points)**，它位于成像平面上的无穷远处。

<div align=center>
<img src="https://i.imgur.com/ICZc5Dq.png" width="40%">
</div>

类似地，当直线向量的最后一维为0时，直线向量$$l = (a, b, 0)$$平行于成像平面且在成像平面上该直线通过原点。这样的直线称为**理想直线(ideal line)**，显然无穷远处的理想点都满足理想直线的方程，也就是位于某些理想直线上。

<div align=center>
<img src="https://i.imgur.com/zoGZRLl.png" width="40%">
</div>

## Essential Matrix

在[Epipolar Geometry](/2021/09/08/OMSCS-CV-NOTES-05.html#epipolar-geometry)一节中我们介绍过双目相机的对极约束。空间点$$P$$与两个成像中心构成的平面称为**极平面(epipolar plane)**；$$P$$点在成像平面$$\Pi$$上的投影$$p$$对应$$\Pi'$$平面上的一条直线，称为**极线(epipolar line)**；成像中心$$O'$$在$$\Pi$$平面上的投影称为**极点(epipole)**；同时$$\Pi$$平面上所有的极线都相交于极点。

<div align=center>
<img src="https://i.imgur.com/p0etcdX.png" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/BAK9HMw.png" width="50%">
</div>

几何意义之外，对极约束在代数上也具有非常优雅的形式。假设空间中存在两个成像中心$$O_c$$和$$O_c'$$，且两个相机的相对位姿可以通过平移$$T$$和旋转$$R$$来表示。为不失一般性我们假定世界坐标系与$$O_c$$坐标系重合，此时空间点$$X$$在$$O_c'$$坐标系下的坐标为：

$$
X' = R X + T
$$

<div align=center>
<img src="https://i.imgur.com/9cA77SJ.png" width="50%">
</div>

在等式两端同时叉乘$$T$$得到：

$$
T \times X' = T \times R X + T \times T = T \times R X
$$

然后再同时点乘$$X'$$得到：

$$
X' \cdot (T \times X') = X' \cdot (T \times R X) = 0
$$

我们把向量叉乘改写成矩阵乘法的形式：

$$
T \times R = 
\begin{bmatrix}
0 & -T_3 & T_2 \\
T_3 & 0 & -T_1 \\
-T_2 & T_1 & 0 \\
\end{bmatrix}

\begin{bmatrix}
R_{11} & R_{12} & R_{13} \\
R_{21} & R_{22} & R_{23} \\
R_{31} & R_{32} & R_{33} \\
\end{bmatrix}
=
T_\times R
$$

$$
X' \cdot (T_\times R X) = 0
$$

令$$E = T_\times R$$，上式可化简为：

$$
X'^T E X= 0
$$

其中矩阵$$E$$称为**本质矩阵(essential matrix)**。如果把$$X$$和$$X'$$换成相应的齐次坐标，那么本质矩阵实际上给出了极线的表达式。令$$l' = EX$$，此时对极约束可以改写为$$X'^T l' = 0$$，即$$X$$在$$\Pi'$$平面上的投影$$X'$$位于极线$$l' = EX$$上。

利用基本矩阵还可以从代数角度证明平行放置的双目相机空间点的投影一定在同一水平直线上。由于此时没有相对旋转，旋转矩阵$$R$$退化为单位阵$$I$$，同时位移项$$T$$仅有x方向分量：

$$
R = I
$$

$$
T = (-B, 0, 0)^T
$$

<div align=center>
<img src="https://i.imgur.com/c7UEgWU.png" width="30%">
</div>

我们把旋转和平移带入到本质矩阵的计算公式，得到退化的本质矩阵：

$$
E = T_\times R = T_\times
=
\begin{bmatrix}
0 & 0 & 0 \\
0 & 0 & B \\
0 &-B & 0 \\
\end{bmatrix}
$$

假设$$P$$点在两个投影平面的投影分别为$$p_l = (x, y, f)$$和$$p_r = (x', y', f)$$，带入对极约束可以得到：

$$
p_l^T E p_r = 0 \Leftrightarrow
\begin{bmatrix}
x' & y' & f
\end{bmatrix}

\begin{bmatrix}
0 & 0 & 0 \\
0 & 0 & B \\
0 &-B & 0 \\
\end{bmatrix}

\begin{bmatrix}
x \\ y \\ f
\end{bmatrix}
=
\begin{bmatrix}
x' & y' & f
\end{bmatrix}

\begin{bmatrix}
0 \\ Bf \\ -By
\end{bmatrix}

= 0
$$

$$
y' = y
$$

也就是说同一点投影后在两个成像平面上一定具有相同的y坐标，即位于同一条水平直线上。

## Fundamental Matrix

本质矩阵给出了成像平面上的对极约束。回忆相机的投影过程，相机矩阵可以分解为内参矩阵和外参矩阵：

$$
p_\text{im} = K \Phi P_\text{w} = K P_\text{c}
$$

其中$$P_\text{c} = \Phi P_\text{w}$$表示空间点在相机坐标系下的坐标。由于内参矩阵$$K$$是可逆的，$$P_\text{c}$$也可以通过$$p_\text{im}$$来表示：

$$
P_\text{c} = K^{-1} p_\text{im}
$$

我们把$$P_\text{c}$$带入到本质矩阵中，得到图像坐标表达的对极约束：

$$
(K_r^{-1} p_{\text{im},r})^T E (K_l^{-1} p_{\text{im},l}) = 0
$$

$$
p_{\text{im},r}^T (K_r^{-T} E K_l^{-1}) p_{\text{im},l} = p_{\text{im},r}^T F p_{\text{im},l} = 0
$$

其中矩阵$$F = K_r^{-T} E K_l^{-1}$$称为**基本矩阵(fundamental matrix)**，它给出了图像平面上的对极约束关系。使用基本矩阵的好处在于我们无需事先对相机进行标定，只要有两幅图像上的对应点就可以进行计算。与本质矩阵类似，基本矩阵给出了图像平面上的极线表达式$$l_r = F p_l$$，同时极点为$$F$$矩阵的零空间。

### Rank of Fundamental Matrix

基本矩阵的一个重要性质是矩阵的秩为2。假设有如下图所示的成像环境，$$p$$点在右侧图像上对应的极线方程为：

$$
l' = F p
$$

<div align=center>
<img src="https://i.imgur.com/VprYenA.png" width="40%">
</div>

同时，在两个成像上的投影点可以利用单应矩阵进行变换：

$$
p' = H p
$$

由于极线$$l'$$通过极点$$e'$$，$$l'$$可通过向量叉乘进行计算：

$$
l' = e' \times p' = e' \times H p
$$

联立两个极线方程得到：

$$
F = e' \times H = e'_\times H
$$

上式说明基本矩阵$$F$$具有和反对称矩阵$$e'_\times$$相同的秩，它们的秩都等于2。

### Solving F

我们可以利用图像平面上的对极约束来求解基本矩阵$$F$$。假设两幅图像上的对应点分别为$$(u, v, 1)$$和$$(u', v', 1)$$，带入对极约束得到：

$$
\begin{bmatrix}
u' & v' & 1
\end{bmatrix}

\begin{bmatrix}
f_{11} & f_{12} & f_{13} \\
f_{21} & f_{22} & f_{23} \\
f_{31} & f_{32} & f_{33} \\
\end{bmatrix}

\begin{bmatrix}
u \\ v \\ 1
\end{bmatrix}
=
0
$$

重新整理成关于$$F$$的线性方程组，得到：

$$
\begin{bmatrix}
u'u & u'v & u' & v'u & v'v & v' & u & v & 1
\end{bmatrix}

\begin{bmatrix}
f_{11} \\ f_{12} \\ f_{13} \\ f_{21} \\ f_{22} \\ f_{23} \\ f_{31} \\ f_{32} \\ f_{33}
\end{bmatrix}
=
0
$$

对于1组对应点可以建立1个线性方程。由于基本矩阵$$F$$具有8个自由度，我们至少需要8组对应点才能进行求解，这种求解方法称为**八点法(eight-point algorithm)**。

八点法的缺陷在于没有考虑基本矩阵自身的约束。前面介绍过基本矩阵的秩为2，而八点法计算出来的结果对秩是没有约束的，因此需要对八点法的计算结果进行修正。我们可以利用SVD来实现这样的约束，假设八点法的计算结果为$$F$$，对它进行奇异值分解得到：

$$
F = U D V^T
$$

在大多数情况下$$D$$是一个满秩的对角矩阵：

$$
D = 
\begin{bmatrix}
r & 0 & 0 \\
0 & s & 0 \\
0 & 0 & t
\end{bmatrix}
$$

我们可以把它的最小对角元$$t$$设置为0，然后复原基本矩阵：

$$
\hat{D} = 
\begin{bmatrix}
r & 0 & 0 \\
0 & s & 0 \\
0 & 0 & 0
\end{bmatrix}
$$

$$
\hat{F} = U \hat{D} V^T
$$

重建后的基本矩阵$$\hat{F}$$和原始基本矩阵$$F$$相比对噪声更稳定。