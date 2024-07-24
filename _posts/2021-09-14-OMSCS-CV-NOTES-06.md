---
layout: post
title: OMSCS-CV课程笔记06-Camera Calibration
date: 2021-09-14
description: 相机标定
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍相机标定的相关内容。
{: .block-preface }


## Geometric Camera Calibration

在[Camera Models](/2021/09/07/OMSCS-CV-NOTES-04.html#modeling-projection)一节中我们介绍了齐次坐标和相机的投影模型，从而将三维空间中的点投影到图像平面上。本节课中我们会把这个方法推广到更一般的情形中：我们不再假定世界坐标系的原点位于相机的投影中心，而是允许将任意坐标系下的点投影到图像平面上。我们把确定空间坐标与图像坐标之间关系的过程称为**相机的几何标定(geometric camera calibration)**，简称相机标定。

相机标定包括2部分内容：

1. 世界坐标到相机坐标的变换，这部分称为相机的**外参数(extrinisic parameters)**或**位姿(pose)**。
2. 相机坐标到图像坐标的变换，这部分称为相机的**内参数(intrinisic parameters)**。

## Extrinsic Camera Calibration

世界坐标到相机坐标的变换是三维空间点的刚体变换，一共有6个外参数：3个平移+3个旋转。

<div align=center>
<img src="https://i.imgur.com/q2OohzA.png" width="70%">
</div>

### Translation

空间点$$P$$在坐标系$$A$$中的坐标可以写成基矢量和的形式：

$$
^AP = (^Ax, ^Ay, ^Az) = ^Ax \cdot i_A + ^Ay \cdot j_A + ^Az \cdot k_A
$$

<div align=center>
<img src="https://i.imgur.com/7dTllok.png" width="40%">
</div>

假设空间中还存在另一个坐标系$$B$$，它与$$A$$只相差一个平移。根据矢量加法$$P$$点在坐标系$$B$$中的坐标为：

$$
^BP = ^B(O_A) + ^AP
$$

其中$$^B(O_A)$$表示$$A$$坐标系中的原点$$O_A$$在$$B$$坐标系中的坐标。

<div align=center>
<img src="https://i.imgur.com/MkKRUPH.png" width="70%">
</div>

我们利用齐次坐标来重写上式可以得到平移变换的矩阵形式：

$$
\begin{bmatrix}
^BP \\ 1
\end{bmatrix}
=
\begin{bmatrix}
I_{3 \times 3} & ^B(O_A) \\
0^T & 1
\end{bmatrix}
\begin{bmatrix}
^AP \\ 1
\end{bmatrix}
$$

### Rotation

对于刚体旋转，我们分别取$$P$$点在两个坐标系下的表示为：

$$
P = 
\begin{bmatrix}
i_A & j_A & k_A
\end{bmatrix}
\begin{bmatrix}
^Ax \\ ^Ay \\ ^Az
\end{bmatrix}
=
\begin{bmatrix}
i_B & j_B & k_B
\end{bmatrix}
\begin{bmatrix}
^Bx \\ ^By \\ ^Bz
\end{bmatrix}
$$

<div align=center>
<img src="https://i.imgur.com/kMAzvq3.png" width="40%">
</div>

因此我们可以得到刚体旋转的坐标变换公式：

$$
^BP = ^B_AR \ ^AP
$$

$$
^B_AR
= 
\begin{bmatrix}
i_A \cdot i_B & j_A \cdot i_B & k_A \cdot i_B \\
i_A \cdot j_B & j_A \cdot j_B & k_A \cdot j_B \\
i_A \cdot k_B & j_A \cdot k_B & k_A \cdot k_B \\
\end{bmatrix}
=
\begin{bmatrix}
^Bi_A & ^Bj_A & ^Bk_A
\end{bmatrix}
=
\begin{bmatrix}
{^Ai_B}^T \\ {^Aj_B}^T \\ {^Ak_B}^T
\end{bmatrix}
$$

其中$$^Bi_A$$表示$$A$$坐标系的基矢量$$i_A$$在$$B$$坐标系中的坐标。我们称$$^B_AR$$为$$A$$坐标系到$$B$$坐标系的旋转矩阵，它的第i行j列元素为$$B$$坐标系第j个基矢量与$$A$$坐标系第i个基矢量的内积。同时需要注意的是旋转矩阵是一个**正交阵(orthogonal matrix)**即$$R^T R = I$$，而且旋转矩阵不具有交换性$$R_1 R_2 \neq R_2 R_1$$。

类似于平移变换，我们同样可以把刚体旋转用齐次坐标来表示，从而得到矩阵形式：

$$
\begin{bmatrix}
^BP \\ 1
\end{bmatrix}
=
\begin{bmatrix}
^B_AR & 0 \\
0^T & 1
\end{bmatrix}
\begin{bmatrix}
^AP \\ 1
\end{bmatrix}
$$

### Rigid Transformations

我们把平移和旋转组合到一起就得到了刚体变换公式：

$$
^BP = ^B_AR \ ^AP + ^B O_A
$$

<div align=center>
<img src="https://i.imgur.com/oEcdNCI.png" width="70%">
</div>

同样利用齐次坐标可以得到矩阵形式：

$$
\begin{bmatrix}
^BP \\ 1
\end{bmatrix}
=
\begin{bmatrix}
I_{3 \times 3} & ^BO_A \\
0^T & 1
\end{bmatrix}
\begin{bmatrix}
^B_AR & 0 \\
0^T & 1
\end{bmatrix}
\begin{bmatrix}
^AP \\ 1
\end{bmatrix}
=
\begin{bmatrix}
^B_AR & ^BO_A \\
0^T & 1
\end{bmatrix}
\begin{bmatrix}
^AP \\ 1
\end{bmatrix}
$$

我们记矩阵$$^B_AT=\begin{bmatrix}^B_AR & ^B(O_A) \\ 0^T & 1 \end{bmatrix}$$为坐标系$$A$$到坐标系$$B$$的刚体变换矩阵，它的逆阵$$^A_BT = {^B_AT}^{-1}$$为坐标系$$B$$到坐标系$$A$$的刚体变换矩阵。当我们取$$A$$为世界坐标系同时取$$B$$为相机坐标系时，$$^B_AT$$表示世界坐标到相机坐标的变换，因此也称其为相机的**外参数矩阵(extrinsic parameter matrix)**。显然外参数矩阵是一个$$4\times4$$的可逆矩阵，它具有3个旋转自由度和3个平移自由度。

## Instrinsic Camera Calibration

### Real Intrinsic Parameters

接下来考虑相机坐标系投影到图像坐标系的过程。对于理想情况下的针孔相机模型，我们只需要知道相机的焦距就可以完成投影：

$$
u = f \frac{x}{z}
$$

$$
v = f \frac{y}{z}
$$

<div align=center>
<img src="https://i.imgur.com/iOtIdbv.png" width="60%">
</div>

然而现实中相机的投影并不会这样完美。首先图像上的像素坐标没有物理意义也不代表现实中的长度，同时$$u$$、$$v$$两个方向上单位像素的长度也不一定严格相等。因此我们不能够直接使用焦距$$f$$来表示投影过程，而是需要引入2个方向上的待定参数$$\alpha$$和$$\beta$$：

$$
u = \alpha \frac{x}{z}
$$

$$
v = \beta \frac{y}{z}
$$

另一方面，按照上式进行投影后图像的原点会位于图像的中心。但在图像处理中图像的原点并不在图像中心，因此我们需要考虑将投影后相机的原点进行平移：

$$
u = \alpha \frac{x}{z} + u_0
$$

$$
v = \beta \frac{y}{z} + v_0
$$

此外，如果$$u$$和$$v$$两个方向不是严格垂直的话投影公式会变得更加复杂：

$$
u = \alpha \frac{x}{z} - \alpha \frac{y}{z} \cot (\theta) + u_0
$$

$$
v = \frac{\beta}{\sin (\theta)} \frac{y}{z} + v_0
$$

<div align=center>
<img src="https://i.imgur.com/Jlna2vV.png" width="60%">
</div>

我们将整个投影过程用齐次坐标来表达就得到了相机的**内参数矩阵(intrinsic matrix)** $$K$$：

$$
\begin{bmatrix}
z \cdot u \\ z \cdot v \\ z
\end{bmatrix}
=
\begin{bmatrix}
\alpha & -\alpha \cot (\theta) & u_0 & 0 \\
0 & \frac{\beta}{\sin (\theta)} & v_0 & 1 \\
0 & 0 & 1 & 0
\end{bmatrix}

\begin{bmatrix}
x \\ y \\ z \\ 1
\end{bmatrix}
$$

$$
p' = K \cdot C
$$

利用齐次坐标的另一个好处是我们可以去掉矩阵的最后一列，得到更常见的内参数矩阵形式：

$$
K = 
\begin{bmatrix}
f & s & c_x \\
0 & a f & c_y \\
0 & 0 & 1
\end{bmatrix}
$$

其中$$f$$为焦距；$$s$$表示坐标轴的倾斜程度，在理想情况下为0；$$c_x$$和$$c_y$$为图像坐标系原点的偏移量；$$a$$为像素的长宽比，理想条件下为1.0。显然相机的内参数矩阵$$K$$一共有5个自由度。

### Camera Parameters

我们把相机的外参数矩阵、投影矩阵以及内参数矩阵组合到一起就得到了相机矩阵$$M$$，它表示世界坐标系到图像坐标系的变换过程：

<div align=center>
<img src="https://i.imgur.com/UWsPQGv.png" width="70%">
</div>

从前面的推导不难发现相机矩阵一共有11个自由度：其中6个来自于外参数矩阵(3个平移+3个旋转)，另外5个来自于内参数矩阵。需要注意的是直接使用相机矩阵得到的坐标是图像平面上的齐次坐标，需要对最后一维进行归一化才能得到所需的像素坐标。

## Calibrating Cameras

### Direct Linear Calibration

本节最后我们来讨论如何对相机进行标定。如果不考虑相机矩阵$$M$$的结构，只把它当做是一般的$$3 \times 4$$矩阵，则可以通过求解线性方程组来进行标定。具体地，假设空间中点坐标为$$(X_i, Y_i, Z_i)$$，它在图像上的坐标为$$(u_i, v_i)$$，利用相机矩阵可以构造出变换关系：

$$
\begin{bmatrix}
u_i \\ v_i \\ 1
\end{bmatrix}
\simeq
\begin{bmatrix}
w \cdot u_i \\ w \cdot v_i \\ w
\end{bmatrix}
=
\begin{bmatrix}
m_{00} & m_{01} & m_{02} & m_{03} \\
m_{10} & m_{11} & m_{12} & m_{13} \\
m_{20} & m_{21} & m_{22} & m_{23} \\
\end{bmatrix}

\begin{bmatrix}
X_i \\ Y_i \\ Z_i \\ 1
\end{bmatrix}
$$

$$
u_i = \frac{m_{00} X_i + m_{01} Y_i + m_{02} Z_i + m_{03}}{m_{20} X_i + m_{21} Y_i + m_{22} Z_i + m_{23}}
$$

$$
v_i = \frac{m_{10} X_i + m_{11} Y_i + m_{12} Z_i + m_{13}}{m_{20} X_i + m_{21} Y_i + m_{22} Z_i + m_{23}}
$$

通过移项，我们可以把方程组改写成关于相机矩阵的方程：

$$
u_i (m_{20} X_i + m_{21} Y_i + m_{22} Z_i + m_{23}) = m_{00} X_i + m_{01} Y_i + m_{02} Z_i + m_{03}
$$

$$
v_i (m_{20} X_i + m_{21} Y_i + m_{22} Z_i + m_{23}) = m_{10} X_i + m_{11} Y_i + m_{12} Z_i + m_{13}
$$

对应的矩阵形式为：

$$
\begin{bmatrix}
X_i & Y_i & Z_i & 1 & 0 & 0 & 0 & 0 & -u_i X_i & -u_i Y_i & -u_i Z_i & -u_i \\
0 & 0 & 0 & 0 & X_i & Y_i & Z_i & 1 & -v_i X_i & -v_i Y_i & -v_i Z_i & -v_i \\
\end{bmatrix}
\begin{bmatrix}
m_{00} \\ m_{01} \\ m_{02} \\ m_{03} \\ m_{10} \\ m_{11} \\ m_{12} \\ m_{13} \\ m_{20} \\ m_{21} \\ m_{22} \\ m_{23}
\end{bmatrix}
=
\begin{bmatrix}
0 \\ 0
\end{bmatrix}
$$

即

$$
A m = 0
$$

上式说明相机标定可以通过求解一个齐次线性方程组来实现。由于相机矩阵具有12个变量，我需要至少6对对应点才能求解这个方程组。

为了避免平凡解，一般可以设置一个额外的约束条件$$\Vert m \Vert = 1$$。此时求解齐次方程等价于约束优化问题：

$$
\begin{aligned}
\min \ \ & \Vert A m \Vert \\
\text{s.t.} \ \ & \Vert m \Vert = 1
\end{aligned}
$$

利用**奇异值分解(singular value decomposition, SVD)**可以证明上述最优化问题的解为矩阵$$A^T A$$的最小特征值对应的特征向量，这里简单进行一下推导。首先对矩阵$$A$$进行奇异值分解：

$$
A = U D V^T
$$

其中$$U$$和$$V$$为正交矩阵，$$D$$为对角矩阵且对角元素均为非负数。将奇异值分解带入优化目标得到：

$$
\Vert A m \Vert = \Vert U D V^T m \Vert = \Vert D V^T m \Vert
$$

$$
\Vert m \Vert = \Vert V^T m \Vert = 1
$$

令$$y = V^T m$$，我们可以把原始的优化问题转为为一个新的优化问题：

$$
\begin{aligned}
\min \ \ & \Vert D y \Vert \\
\text{s.t.} \ \ & \Vert y \Vert = 1
\end{aligned}
$$

由于$$D$$为对角元素非负的对角矩阵，$$\Vert D y \Vert$$的最小值为$$D$$中的最小对角元素。此时$$y$$仅在该元素位置为1，其余位置均为0。我们可以将$$D$$的对角元素按从大到小顺序重新排列，这样$$y = (0, 0, ..., 0, 1)^T$$，即：

$$
V^T m = 
\begin{bmatrix}
0 \\  \vdots \\ 0 \\ 1
\end{bmatrix}
$$

上式说明$$m$$为$$V$$的最后一列，即$$A$$最小奇异值对应的右奇异向量，也就是$$A^T A$$的最小特征值对应的特征向量。

另一种求解方程$$Am=0$$的方法是将相机矩阵的最后一个元素固定为1，此时的投影过程为：

$$
\begin{bmatrix}
u_i \\ v_i \\ 1
\end{bmatrix}
\simeq
\begin{bmatrix}
w \cdot u_i \\ w \cdot v_i \\ w
\end{bmatrix}
=
\begin{bmatrix}
m_{00} & m_{01} & m_{02} & m_{03} \\
m_{10} & m_{11} & m_{12} & m_{13} \\
m_{20} & m_{21} & m_{22} & 1 \\
\end{bmatrix}

\begin{bmatrix}
X_i \\ Y_i \\ Z_i \\ 1
\end{bmatrix}
$$

$$
u_i = \frac{m_{00} X_i + m_{01} Y_i + m_{02} Z_i + m_{03}}{m_{20} X_i + m_{21} Y_i + m_{22} Z_i + 1}
$$

$$
v_i = \frac{m_{10} X_i + m_{11} Y_i + m_{12} Z_i + m_{13}}{m_{20} X_i + m_{21} Y_i + m_{22} Z_i + 1}
$$

不过需要注意的是相机矩阵本身对$$m_{23}$$没有任何约束，因此假定$$m_{23}=1$$的解法在$$m_{23}$$接近0时会有严重的数值精度问题。

直接法进行相机标定的主要缺陷在于它没有使用正确的误差函数。优化目标$$\Vert A m \Vert$$没有任何的几何意义，我们称其为**代数误差(algebraic error)**。而理想的相机矩阵需要保证$$X_i$$经过投影后与图像上的对应点$$x_i$$尽可能接近，因此我们把$$X_i$$再图像上的投影$$\hat{x}_i$$与对应点$$x_i$$之间的距离称为**几何误差(geometric error)**或**重投影误差(reprojection error)**，相应的优化问题可表示为：

$$
\min_M \sum_i d(\hat{x}_i, x_i)
$$

<div align=center>
<img src="https://i.imgur.com/bUIcq1u.png" width="30%">
</div>

由于投影过程是非线性的，我们无法像直接法那样显式地计算优化问题的解，一般情况下需要通过牛顿法(Newton method)等非线性优化算法来迭代求解。我们把直接法和非线性优化结合起来就得到了相机标定的**Gold Standard算法**：

<div align=center>
<img src="https://i.imgur.com/pH201iB.png" width="60%">
</div>

### Finding Camera Center from M

通过相机标定得到相机矩阵$$M$$后可以反推出很多有用的性质，比如说我们可以得到投影中心在世界坐标系下的坐标$$C$$。对于空间中任意点$$P$$，假设$$x$$位于$$P$$和投影中心$$C$$连成的直线上：

$$
x = \lambda P + (1 - \lambda) C
$$

将上式带入投影方程得到：

$$
M x = \lambda M P + (1 - \lambda) M C
$$

由于$$x$$在$$PC$$直线上，投影后$$x$$与$$P$$必然具有相同的图像坐标。因此$$MC = 0$$，换句话说$$C$$位于$$M$$的零空间中。我们把$$M$$写成増广矩阵的形式$$M = \begin{bmatrix} Q \ \vert \ b \end{bmatrix}$$，可以得到$$C$$的齐次坐标为：

$$
C = 
\begin{bmatrix}
-Q^{-1} b \\ 1
\end{bmatrix}
$$

### Multi-Plane Calibration

除了直接法和Gold Standard算法外，实际应用中更常见的是使用棋盘格标定板的相机标定方法。使用棋盘格进行标定时，我们只需要拍摄不同角度下棋盘格的照片即可完成相机的标定工作。和前面介绍的方法相比，棋盘格标定操作简单、无需事先知道棋盘格的位姿、而且还能对镜头畸变进行矫正，因此棋盘格标定在大多数计算机视觉库(如OpenCV、MATLAB Computer Vision Toolbox)中都有现成的实现。

<div align=center>
<img src="https://i.imgur.com/WHgRV33.png" width="90%">
</div>