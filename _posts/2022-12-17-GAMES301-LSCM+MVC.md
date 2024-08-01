---
layout: post
title: LSCM + Mean Value Coordinates 论文笔记
date: 2022-12-17
description: 作业3解析与LSCM参数化算法
tags: CG Geometry-Processing
categories: GAMES301
pretty_table: true
toc:
  sidebar: left
---


> GAMES301-曲面参数化([GAMES 301: Surface Parameterization](http://staff.ustc.edu.cn/~renjiec/GAMES301/index.html))作业3解析与matlab实现。
{: .block-preface }


## LSCM

**LSCM(least squares conformal map)**是经典的自由边界共形参数化算法。我们在课程中介绍过共形映射需要满足[Cauchy-Riemann方程](/blog/2022/GAMES301-NOTES-10/#plane-to-plane)：

$$
\begin{cases}
\frac{\partial f_x}{\partial x} = \frac{\partial f_y}{\partial y} \\
\frac{\partial f_x}{\partial y} =-\frac{\partial f_y}{\partial x} \\
\end{cases}
\Leftrightarrow
J = 
\begin{bmatrix}
a & -b \\
b & a
\end{bmatrix}
$$

从Jacobian矩阵来看，Cauchy-Riemann方程说明映射$$f$$是一个相似变换(旋转+缩放)因此不会改变向量的夹角。对于大多数网格我们可能很难保证映射严格满足Cauchy-Riemann方程，此时可以构造一个共形能量来描述映射$$f$$违背共形映射的程度，而LSCM的原理就是通过最小化共形能量来建立所需的共形映射。

<div align=center>
<img src="https://i.imgur.com/EHNaGWb.png" width="80%">
</div>

### Conformality in a Triangulation

LSCM使用了复平面来推导共形能量。设三角形$$T$$在其局部坐标系下的初始构型为$$\{ (x_1, y_1), (x_2, y_2), (x_3, y_3) \}$$，在复平面上二维向量可以直接转换为复数：

$$
\{ x_i + i \ y_i \vert i = 1,2,3 \}
$$

类似地，我们把参数化后uv平面上的坐标也表示为复数形式：

$$
\{ u_i + i \ v_i \vert i = 1,2,3 \}
$$

使用复数形式的好处在于我们可以把坐标转换为标量函数进行推导。假设映射$$\mathcal{X}: \mathbb{C} \mapsto \mathbb{C}$$将参数平面嵌入到三角形局部坐标系中，当$$\mathcal{X}$$为共形映射时需要满足复数形式的Cauchy-Riemann方程：

$$
\frac{\partial \mathcal{X}}{\partial u} - i \frac{\partial \mathcal{X}}{\partial v} = 0
$$

利用反函数导数定理可以得到$$\mathcal{X}$$的反函数$$\mathcal{U}$$需要满足：

$$
\frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y} = 0
$$

接下来我们定义共形能量$$C$$为等式左端的平方在整个三角形上的积分：

$$
C(T) = \int_T \bigg\vert \frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y} \bigg\vert^2 \ dA
$$

显然当$$\mathcal{U}$$为共形映射时$$C(T)$$为0，否则三角形上会产生共形能量。对网格上每个三角形进行求和就得到了网格的共形能量：

$$
C(\mathcal{T}) = \sum_i C(T_i)
$$

### Gradient in a Triangle

对于线性三角形网格，其共形能量在三角形内每一点都是相同的。因此我们可以把积分转换为能量与三角形面积相乘：

$$
C(T) = \int_T \bigg\vert \frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y} \bigg\vert^2 \ dA = \bigg\vert \frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y} \bigg\vert^2 A_T
$$

上式说明我们只需要计算三角形面积$$A_T$$以及内部的共形能量即可。对于三角形顶点上定义的标量函数$$u(x, y)$$，其梯度在三角形内部有解析形式：

$$
\begin{bmatrix}
\frac{\partial u}{\partial x} \\ \frac{\partial u}{\partial y}
\end{bmatrix}
=
\frac{1}{2 A_T}
\begin{bmatrix}
y_2-y_3 & y_3-y_1 & y_1-y_2 \\
x_3-x_2 & x_1-x_3 & x_2-x_1 \\
\end{bmatrix}
\begin{bmatrix}
u_1 \\ u_2 \\ u_3
\end{bmatrix}
$$

其中$$u_1, u_2, u_3$$分别为顶点上的函数值。对于复函数$$\mathcal{U}$$我们可以整理得到：

$$
\frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y} 
=
\frac{i}{2 A_T}
\begin{bmatrix}
W_1 & W_2 & W_3
\end{bmatrix}
\begin{bmatrix}
U_1 \\ U_2 \\ U_3
\end{bmatrix}
$$

其中

$$
\begin{cases}
W_1 = (x_3 - x_2) + i \ (y_3 - y_2) \\
W_2 = (x_1 - x_3) + i \ (y_1 - y_3) \\
W_3 = (x_2 - x_1) + i \ (y_2 - y_1) \\
\end{cases}
\ \ \ 
\begin{cases}
U_1 = u_1 + i \ v_1 \\
U_2 = u_2 + i \ v_2 \\
U_3 = u_3 + i \ v_3 \\
\end{cases}
$$

这样整个三角形上的共形能量可以表示为：

$$
\begin{aligned}
C(T) &= \bigg\vert \frac{\partial \mathcal{U}}{\partial x} + i \frac{\partial \mathcal{U}}{\partial y} \bigg\vert^2 A_T \\
&= \frac{1}{4 A_T} 
\bigg\Vert 
\begin{bmatrix}
W_1 & W_2 & W_3
\end{bmatrix}
\begin{bmatrix}
U_1 & U_2 & U_3
\end{bmatrix}^T
\bigg\Vert^2 \\
&\propto 
\frac{1}{A_T} 
\bigg\Vert 
\begin{bmatrix}
W_1 & W_2 & W_3
\end{bmatrix}
\begin{bmatrix}
U_1 & U_2 & U_3
\end{bmatrix}^T
\bigg\Vert^2 \\
\end{aligned}
$$

对所有三角形求和后就得到了网格上的共形能量，其可以表示为关于顶点复函数的二次型：

$$
C(\mathcal{T}) = \mathbf{U}^* \mathcal{C} \mathbf{U}
$$

上标$$^*$$表示共轭转置。其中二次型的系数矩阵$$\mathcal{C}$$可以分解为：

$$
\mathcal{C} = \mathcal{M}^* \mathcal{M}
$$

式中$$\mathcal{M}$$是一个$$\vert F \vert \times \vert V \vert$$维复矩阵，每个元素表示$$v_j$$顶点在$$T_i$$三角形上对网格共形能量的贡献：

$$
\mathcal{M}_{ij} = 
\begin{cases}
\frac{W_{j, T_i}}{\sqrt{A_{T_i}}}, & \text{if vertex $v_j$ belongs to triangle $T_i$} \\
0, & \text{otherwise}
\end{cases}
$$

### Least Square Optimization

对于二次型的优化问题我们可以通过求导并令导数为0的方式进行直接求解。但需要注意在LSCM中系数矩阵$$\mathcal{C}$$、$$\mathcal{M}$$和顶点uv坐标矩阵$$\mathbf{U}$$都是复数，使用复数的求导法则会比较麻烦。不过实际上我们只需要对复数矩阵进行简单的变形就可以按照通常的实数矩阵进行处理。首先对复数进行分解：

$$
\mathcal{M} = \mathbf{A} + i \ \mathbf{B}
$$

$$
\mathbf{U} = \mathbf{u} + i \ \mathbf{v}
$$

其中$$\mathbf{A}$$和$$\mathbf{u}$$表示对应矩阵的实数部分，$$\mathbf{B}$$和$$\mathbf{v}$$则表示虚数部分。在这种分解下我们可以重新构造二次型的目标函数：

$$
\begin{aligned}
\mathcal{M} \mathbf{U} 
&= (\mathbf{A} + i \ \mathbf{B}) (\mathbf{u} + i \ \mathbf{v}) \\
&= 
\big( 
\begin{bmatrix}
\mathbf{A} & -\mathbf{B}
\end{bmatrix}
+ i \ 
\begin{bmatrix}
\mathbf{B} & \mathbf{A}
\end{bmatrix}
\big)
\begin{bmatrix}
\mathbf{u} \\ \mathbf{v}
\end{bmatrix}
\end{aligned}
$$

$$
\begin{aligned}
C(\mathcal{T}) &= \mathbf{U}^* \mathcal{M}^* \mathcal{M} \mathbf{U} \\
&= (\mathcal{M} \mathbf{U})^* \mathcal{M} \mathbf{U} \\
&= 
\begin{bmatrix}
\mathbf{u}^T & \mathbf{v}^T
\end{bmatrix}
\bigg(
\begin{bmatrix}
\mathbf{A}^T \\ -\mathbf{B}^T
\end{bmatrix}
- i \ 
\begin{bmatrix}
\mathbf{B}^T \\ \mathbf{A}^T
\end{bmatrix}
\bigg)
\bigg(
\begin{bmatrix}
\mathbf{A} & -\mathbf{B}
\end{bmatrix}
+ i \ 
\begin{bmatrix}
\mathbf{B} & \mathbf{A}
\end{bmatrix}
\bigg)
\begin{bmatrix}
\mathbf{u} \\ \mathbf{v}
\end{bmatrix}\\
&= 
\begin{bmatrix}
\mathbf{u}^T & \mathbf{v}^T
\end{bmatrix}

\begin{bmatrix}
\mathbf{A}^T \mathbf{A} + \mathbf{B}^T \mathbf{B} & -\mathbf{A}^T \mathbf{B} + \mathbf{B}^T \mathbf{A} \\
-\mathbf{B}^T + \mathbf{A}^T \mathbf{B} \mathbf{A} & \mathbf{B}^T \mathbf{B} + \mathbf{A}^T \mathbf{A}
\end{bmatrix}

\begin{bmatrix}
\mathbf{u} \\ \mathbf{v}
\end{bmatrix}\\
&= 
\begin{bmatrix}
\mathbf{u}^T & \mathbf{v}^T
\end{bmatrix}

\bigg(
\begin{bmatrix}
\mathbf{A}^T & \mathbf{B}^T \\
-\mathbf{B}^T & \mathbf{A}^T
\end{bmatrix}

\begin{bmatrix}
\mathbf{A} & -\mathbf{B} \\
\mathbf{B} & \mathbf{A}
\end{bmatrix}
\bigg)

\begin{bmatrix}
\mathbf{u} \\ \mathbf{v}
\end{bmatrix} \\
&=
\bigg(
\begin{bmatrix}
\mathbf{A} & -\mathbf{B} \\
\mathbf{B} & \mathbf{A}
\end{bmatrix}
\begin{bmatrix}
\mathbf{u} \\ \mathbf{v}
\end{bmatrix}
\bigg)^T
\bigg(
\begin{bmatrix}
\mathbf{A} & -\mathbf{B} \\
\mathbf{B} & \mathbf{A}
\end{bmatrix}
\begin{bmatrix}
\mathbf{u} \\ \mathbf{v}
\end{bmatrix}
\bigg)
\end{aligned}
$$

上面的推导说明我们只需要处理实数矩阵构成的二次型优化即可。对它求导并令导数为0可以得到uv坐标需要满足的线性方程组：

$$
\begin{bmatrix}
\mathbf{A} & -\mathbf{B} \\
\mathbf{B} & \mathbf{A}
\end{bmatrix}
\begin{bmatrix}
\mathbf{u} \\ \mathbf{v}
\end{bmatrix}
=
\mathbf{0}
$$

为了避免出现平凡解，我们需要固定网格在uv平面上至少2个点的坐标。这种做法的几何意义非常直观：由于共形映射无法区分平移、旋转和缩放(共形映射关于相似变换是相互等价的)，我们至少需要2个确定的点才能获得唯一的共形映射。此时可以将原始的复系数矩阵拆分成为自由顶点和固定顶点两部分：

$$
\mathcal{M} = 
\begin{bmatrix}
\mathcal{M}_f & \mathcal{M}_p
\end{bmatrix}
$$

由于$$\mathcal{M}_p$$部分的坐标是已知的，网格的共形能量只与自由顶点有关。把固定顶点的uv坐标带入齐次方程组并展开可以得到：

$$
\begin{bmatrix}
\mathbf{A}_f & \mathbf{A}_p & -\mathbf{B}_f & -\mathbf{B}_p \\
\mathbf{B}_f & \mathbf{B}_p & \mathbf{A}_f & \mathbf{A}_p
\end{bmatrix}
\begin{bmatrix}
\mathbf{u}_f \\ \mathbf{u}_p \\ \mathbf{v}_f \\ \mathbf{v}_p
\end{bmatrix}
=
\mathbf{0}
$$

重新整理后可以得到自由顶点uv坐标需要满足的线性方程组：

$$
\begin{bmatrix}
\mathbf{A}_f & -\mathbf{B}_f \\
\mathbf{B}_f & \mathbf{A}_f
\end{bmatrix}
\begin{bmatrix}
\mathbf{u}_f \\ \mathbf{v}_f
\end{bmatrix}
=
-
\begin{bmatrix}
\mathbf{A}_p & -\mathbf{B}_p \\
\mathbf{B}_p & \mathbf{A}_p
\end{bmatrix}
\begin{bmatrix}
\mathbf{u}_p \\ \mathbf{v}_p
\end{bmatrix}
$$

求解这个线性方程组即可获得所需的共形参数化。

### Implementation

整个LSCM算法可参考下面`LSCM()`函数的实现。整个算法流程包括计算网格三角形面积、计算初始构型、构造稀疏线性方程组、固定网格上两个点、以及最后求解线性方程组得到参数坐标等步骤。其中计算初始构型的`project2Plane()`函数与作业2完全相同，可以参见之前的[代码片断](/blog/2022/GAMES301-AES/#compute-rest-pose)。

```matlab
function uv = LSCM(V, F)
%% LSCM parameterization
%% Args:
%%      V[nV, 3]: vertices in 3D
%%      F[nF, 3]: face connectivity
%% Returns:
%%      uv[nV, 2]: uv coordinates

nV = size(V, 1);
nF = size(F, 1);

%% areas
AT = doubleArea(V, F);
AT_sq = sqrt(AT);

%% rest pose
Xs = zeros(nF, 3, 2);
for i=1:nF
    Xs(i,:,:) = project2Plane(V(F(i, :), :));
end

%% set up sparse complex matrix
A = (Xs(:, [3 1 2], 1) - Xs(:, [2 3 1], 1)) ./ AT_sq;   %% real part
B = (Xs(:, [3 1 2], 2) - Xs(:, [2 3 1], 2)) ./ AT_sq;   %% imag part

MA = sparse(repmat((1:nF)', 1, 3), F, A, nF, nV);
MB = sparse(repmat((1:nF)', 1, 3), F, B, nF, nV);

%% pin 2 points on the boundary
[b1, b2] = pinBoundary(V, F);

%% split free and pinned vertices
p = [b1 b2]; f = setdiff(1:nV, p);
Af = MA(:, f); Ap = MA(:, p);  %% real part
Bf = MB(:, f); Bp = MB(:, p);  %% imag part

AM = [Af -Bf; Bf Af];
b  =-[Ap -Bp; Bp Ap] * [0; 1; 0; 0];

%% solve linear system
uv = zeros(nV, 2);
uv(f, :) = reshape(AM \ b, [nV-2 2]);

%% fix pinned points
uv(p, :) = [[0 0]; [1 0]];

end
```


`pinBoundary()`函数实现了选择网格边界的两个固定点的功能，这里我简单地选择了边界的起点和中间点。当然其它的选取方式也是可行的，但需要注意不同的选取方式会影响最终的参数化结果。

```matlab
function [b1, b2] = pinBoundary(V, F)
%% A helper function to pin 2 points on the boundary
%% Args:
%%      V[nV, 3]: vertices in 3D
%%      F[nF, 3]: face connectivity
%% Returns:
%%      b1: pinned boundary vertex
%%      b2: pinned boundary vertex

[B, ~] = findBoundary(V, F);
nB = length(B);

%% select the first and middle boundary points
b1 = B(1); b2 = B(round(nB/2));

end
```


最后，`doubleArea()`函数给出了计算网格三角形面积的一个向量化实现。和利用循环进行遍历相比，使用向量化的效率会高很多。

```matlab
function As = doubleArea(V, F)
%% Compute doubled area of triangles
%% Args:
%%      V[nV, 3]: vertices in 3D
%%      F[nF, 3]: face connectivity
%% Returns:
%%      As[nF, 1]: doubled area of triangles

nF = size(F, 1);

Es = reshape(V(F(:, 2:3), :)-V(F(:, [1 1]), :), [nF 2 3]);
As = vecnorm(cross(Es(:,1,:), Es(:, 2, :), 3), 2, 3);

end
```


### Results

在`cow.obj`网格上使用LSCM可以得到如下的参数化结果。需要说明的是LSCM可以保证参数化后网格不会出现翻转，但无法保证不会出现自交。在本例中我们可以看到网格在边界上出现了自交的情况。

<div align=center>
<img src="https://i.imgur.com/5CLahGo.png" width="40%">
<img src="https://i.imgur.com/jk97x4E.jpg" width="40%">
</div>

## Mean Value Coordinates

### Tutte's Embedding

**Mean value coordinates**是Tutte's embedding的一种变体，这里我们首先回忆一下Tutte's embedding的原理。Tutte's embedding中网格内部顶点$$v_i$$的坐标可以表示为相邻顶点的线性组合：

$$
v_i = \sum_{v_j \in N(v_i)} \lambda_j v_j
$$

其中系数$$\lambda_j$$需要满足非负和归一化条件：

$$
\forall \lambda_j \geq 0, \ \sum_{v_j \in N(v_i)} \lambda_j = 1
$$

Tutte's embedding的一个重要性质在于当边界点位于一个凸多边形上时，求解线性方程组得到的参数化是无翻转且无自交的。

### Mean Value Coordinates

Tutte's embedding的缺陷在于参数化后可能会出现比较严重的几何扭曲，因此Tutte's embedding相关算法的核心在于如何构造合适的系数来减少扭曲。Mean value coordinates使用了夹角半角的正切来构造权重：

$$
\lambda_i = \frac{w_i}{\sum_j w_j}, \ \
w_i = \frac{\tan{(\frac{\alpha_{i-1}}{2})} + \tan{(\frac{\alpha_{i}}{2})}}{\Vert v_i - v_0 \Vert}
$$

<div align=center>
<img src="https://i.imgur.com/A0SZzJ0.png" width="40%">
</div>

### Implementation

使用mean value coordinates作为权重的Tutte's embedding算法可参见`MVCTutte()`函数。这里首先使用了LSCM来固定网格参数化边界，然后对内部顶点求解线性方程组来求解uv坐标。在实现时的一个技巧是从三角形内角来考虑权重，比如说在三角形$$\triangle v_0 v_i v_{i+1}$$上内角$$\alpha_i$$参与的贡献为：

$$
w_{\alpha_i}
=
\frac{\tan{(\frac{\alpha_{i}}{2})}}{\Vert v_i - v_0 \Vert} v_i
+
\frac{\tan{(\frac{\alpha_{i}}{2})}}{\Vert v_{i+1} - v_0 \Vert} v_{i+1}
$$

因此对于三角形的每个内角我们只需要计算它的半角正切以及邻边边长，最后填到对应位置上进行求和即可。除此之外，在计算半角正切时还可以结合三角函数的变换技巧：

$$
\tan{\frac{\alpha}{2}} = \frac{\sin{\frac{\alpha}{2}}}{\cos{\frac{\alpha}{2}}} 
= \frac{2\sin{\frac{\alpha}{2}} \cos{\frac{\alpha}{2}}}{2\cos^2{\frac{\alpha}{2}}} 
= \frac{\sin{\alpha}}{1 + \cos{\alpha}}
$$

而内角$$\alpha$$的正弦和余弦可以由邻边方向向量的叉积与内积来计算，这样整个权重计算都可以进行向量化从而提升计算效率。

```matlab
function uv = MVCTutte(V, F)
%% Tutte's embedding with mean value coordinates
%% Args:
%%      V[nV, 3]: vertices in 3D
%%      F[nF, 3]: face connectivity
%% Returns:
%%      uv[nV, 2]: uv coordinates

nV = size(V, 1);

[B, ~] = findBoundary(V, F);
nB = length(B);

%% LSCM to pin the boundary
uv = LSCM(V, F);

%% edges
Es    = reshape(V(F(:, [2, 3, 1]), :) - V(F, :), [size(F), 3]);
Enorm = vecnorm(Es, 2, 3);
Edir  = Es ./ Enorm;

%% trigonometry
coss =-dot(Edir(:, :, :), Edir(:, [3, 1, 2], :), 3);
sins = vecnorm(cross(Edir(:, :, :), -Edir(:, [3, 1, 2], :), 3), 2, 3);

%% tan(theta/2) = sin(theta) / (1+cos(theta))
tanHalf = sins ./ (coss + 1);

%% set up linear system
tripI = [F F];
tripJ = [F(:, [2 3 1]) F(:, [3 1 2])];
tripV = [tanHalf./Enorm(:,:) tanHalf./Enorm(:, [3 1 2])];

A = sparse(tripI, tripJ, tripV, nV, nV);
A = A - diag(sum(A, 2));

%% pinned boundary points
A(B, :) = sparse(nB, nV);
A(B, B) = speye(nB);

b = zeros(nV, 2);
b(B,:) = uv(B, :);

%% solve linear system
uv = A \ b;

end
```


### Results

在`cow.obj`网格上使用LSCM确定边界然后使用mean value coordinates计算内部顶点uv坐标的结果如下。从结果来看mean value coordinates的结果与直接使用LSCM差异不大。

<div align=center>
<img src="https://i.imgur.com/5CLahGo.png" width="40%">
<img src="https://i.imgur.com/FkQ3IwD.jpg" width="40%">
</div>

## Reference

- [Least Squares Conformal Maps for Automatic Texture Atlas Generation](https://members.loria.fr/Bruno.Levy/papers/LSCM_SIGGRAPH_2002.pdf)
- [Mean Value Coordinates](https://cgvr.informatik.uni-bremen.de/teaching/cg_literatur/barycentric_floater.pdf)
- [Discrete One-Forms on Meshes and Applications to 3D Mesh Parameterization](http://www.cs.harvard.edu/~sjg/papers/tutte.pdf)