---
layout: post
title: 微分几何笔记03-曲面的第一基本形式
date: 2023-10-18
description: 曲面的第一基本形式
tags: CG Math
categories: Differential-Geometry
toc:
  sidebar: left
---


> 这个系列是北京大学陈维桓教授《微分几何(第二版)》的学习笔记，主要涉及古典微分几何中曲线曲面理论的相关知识。系统学习微分几何对于理解计算机图形学中的各种几何处理算法是十分有益的。本节介绍曲面的第一基本形式。
{: .block-preface }


## 正则参数曲面

所谓的参数曲面$$S$$是指从$$\mathbb{E}^2$$的一个区域$$D$$(即$$\mathbb{E}^2$$中的一个连通开子集)到空间$$\mathbb{E}^3$$的一个连续映射$$S: D \rightarrow \mathbb{E}^3$$。若在$$\mathbb{E}^2$$和$$\mathbb{E}^3$$中分别建立了笛卡儿直角坐标系，则参数曲面$$S$$的方程可以表示为：

$$
\begin{cases}
x = x(u, v) \\
y = y(u, v) \\
z = z(u, v) \\
\end{cases}
\ \ \ \ , (u, v) \in D
$$

或者写成向量方程的形式：

$$
\boldsymbol{r} = \boldsymbol{r} (u, v) = \big( x(u, v), y(u, v), z(u, v) \big)
$$

自变量$$u$$和$$v$$称为曲面$$S$$的参数。在曲面$$S$$上取一点$$p_0$$，$$\overrightarrow{Op_0} = \boldsymbol{r} (u_0, v_0)$$。如果让参数$$u$$固定$$u = u_0$$，而让参数$$v$$变化，则动点描出一条落在曲面$$S$$上的曲线$$\boldsymbol{r} (u_0, v)$$，这条曲线称为在曲面$$S$$上经过点$$p_0$$的$$v$$-曲线。同理，我们有曲面$$S$$上经过点$$p_0$$的$$u$$-曲线$$\boldsymbol{r} (u, v_0)$$，或记成$$v = v_0$$。这样，在参数曲面上经过每一点有一条$$u$$-曲线和一条$$v$$-曲线，它们构成曲面上的**参数曲线网**。在区域$$D$$上看，$$u$$-曲线和$$v$$-曲线分别是$$\mathbb{E}^2$$中的坐标曲线。因此在直观上，参数曲面$$S$$是把$$\mathbb{E}^2$$中的区域$$D$$映射到$$\mathbb{E}^3$$中的结果，此时$$\mathbb{E}^2$$中的坐标曲线网就变成了曲面$$S$$中的参数曲线网。这就是说，从曲面$$S$$上看，$$(u, v)$$可以作为曲面$$S$$上点的坐标，称为曲面上的**曲纹坐标**。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/149o2Eg.png" width="80%">
</div>

但是，要使$$(u, v)$$真的能够具有曲面$$S$$上点的坐标的功能，必须要求在曲面$$S$$和区域$$D$$的点之间是一一对应的，显然上面提到的方程组本身并不能满足这一点。为此，需要在方程上加一些正则性条件。曲面$$S$$的参数曲线在点$$p_0$$的两个切向量是

$$
\boldsymbol{r}_u (u_0, v_0) = \frac{\partial \boldsymbol{r}}{\partial u} \bigg|_{(u_0, v_0)}, \ \ 
\boldsymbol{r}_v (u_0, v_0) = \frac{\partial \boldsymbol{r}}{\partial v} \bigg|_{(u_0, v_0)}
$$

如果$$\boldsymbol{r}_u (u_0, v_0)$$和$$\boldsymbol{r}_v (u_0, v_0)$$是线性无关的，即$$\boldsymbol{r}_u \times \boldsymbol{r}_v \neq \mathbf{0}$$，则称曲面$$S$$在点$$p_0$$是**正则**的。今后我们所研究的曲面都是3次以上连续可微的、处处是正则点的参数曲面，称为**正则参数曲面**。

### Monge形式

设$$S: \boldsymbol{r} = \boldsymbol{r}(u, v)$$是正则参数曲面，对于任一点$$(u_0, v_0) \in D$$有

$$
\boldsymbol{r}_u \times \boldsymbol{r}_v \vert_{(u_0, v_0)} = 
\bigg( \frac{\partial (y, z)}{\partial (u, v)}, \frac{\partial (z, x)}{\partial (u, v)}, \frac{\partial (x, y)}{\partial (u, v)} \bigg) \bigg|_{(u_0, v_0)} \neq \mathbf{0}
$$

不妨设

$$
\frac{\partial (x, y)}{\partial (u, v)} \bigg|_{(u_0, v_0)} \neq 0
$$

故由反函数定理得知，存在点$$(u_0, v_0)$$在$$D$$内的邻域$$U$$，使得函数$$x=x(u, v)$$，$$y=y(u, v)$$在$$U$$上有反函数

$$
\begin{cases}
u = u(x, y) \\
v = v(x, y)
\end{cases}
$$

即它们满足恒等式

$$
\begin{cases}
x\big( u(x, y), v(x, y) \big) \equiv x \\ 
y\big( u(x, y), v(x, y) \big) \equiv y \\ 
\end{cases}
$$

这时

$$
z = z(u, v) = z(u(x, y), v(x, y))
$$

将上式右端仍旧记为函数$$z(x, y)$$，则曲面$$S$$的参数方程成为：

$$
\boldsymbol{r} = (x, y, z(x, y))
$$

于是区域$$U$$与曲面$$S\vert_U$$之间的点的一一对应是由$$(u(x, y), v(x, y)) \rightarrow (x, y, z(x, y))$$给出的。

上面的讨论还说明，正则参数曲面在任意一点的某个邻域内总是可以表示成类似于$$z = z(x, y)$$的形式。也就是说，正则参数曲面在局部上必定可以看作一个二元连续可微函数的图像。用$$z = z(x, y)$$给出曲面的方式称为**Monge形式**。对于Monge形式给出的曲面，其参数方程恰好是$$\boldsymbol{r} = (x, y, z(x, y))$$。因此有

$$
\boldsymbol{r}_x = (1, 0, z_x), \ \
\boldsymbol{r}_y = (0, 1, z_y)
$$

于是

$$
\boldsymbol{r}_x \times \boldsymbol{r}_y = \bigg( -\frac{\partial z}{\partial x}, -\frac{\partial z}{\partial y}, 1 \bigg) \neq \mathbf{0}
$$

即Monge形式给出的曲面都是正则的。

### 容许的参数变换

正则参数曲面的参数容许作一定的变换。设

$$
\begin{cases}
u = u(\tilde{u}, \tilde{v}) \\
v = v(\tilde{u}, \tilde{v}) \\
\end{cases}
$$

它们满足如下的条件：

(1) $$u(\tilde{u}, \tilde{v})$$、$$v(\tilde{u}, \tilde{v})$$都是$$\tilde{u}$$、$$\tilde{v}$$的3次以上连续可微函数；

(2) $$\frac{\partial (u, v)}{\partial (\tilde{u}, \tilde{v})} \neq 0$$。

将上面的函数代入正则参数曲面的方程可以得到

$$
\begin{aligned}
\boldsymbol{r} &= \boldsymbol{r} (u(\tilde{u}, \tilde{v}), v(\tilde{u}, \tilde{v})) \\
&= \big( 
x(u(\tilde{u}, \tilde{v}), v(\tilde{u}, \tilde{v})), 
y(u(\tilde{u}, \tilde{v}), v(\tilde{u}, \tilde{v})),
z(u(\tilde{u}, \tilde{v}), v(\tilde{u}, \tilde{v}))
\big)
\end{aligned}
$$

于是它作为$$\tilde{u}$$、$$\tilde{v}$$的函数仍然是3次以上连续可微的，并且

$$
\begin{aligned}
\boldsymbol{r}_{\tilde{u}} &= \boldsymbol{r}_u \frac{\partial u}{\partial \tilde{u}} + \boldsymbol{r}_v \frac{\partial v}{\partial \tilde{u}} \\
\boldsymbol{r}_{\tilde{v}} &= \boldsymbol{r}_u \frac{\partial u}{\partial \tilde{v}} + \boldsymbol{r}_v \frac{\partial v}{\partial \tilde{v}} \\
\boldsymbol{r}_{\tilde{u}} \times \boldsymbol{r}_{\tilde{v}} &= \frac{\partial (u, v)}{\partial (\tilde{u}, \tilde{v})} \cdot (\boldsymbol{r}_u \times \boldsymbol{r}_v) \neq \mathbf{0}
\end{aligned}
$$

由此可见，在经过如上的变量替换之后，得到的仍然是正则参数曲面。这就是说，正则参数曲面的性质在满足条件(1)、(2)的参数变换下是保持不变的。我们把满足条件(1)、(2)的参数变换称为为容许的参数变换。

我们还规定，向量$$\boldsymbol{r}_u \times \boldsymbol{r}_v$$所指的一侧为曲面的正侧。因此，参数$$u$$，$$v$$的次序决定了正则参数曲面的**定向**。当参数$$u$$，$$v$$的次序颠倒时，向量$$\boldsymbol{r}_u \times \boldsymbol{r}_v$$就改变它的指向，正则参数曲面的定向也随之颠倒。显然，容许的参数变换保持参数曲面的定向不变的充分必要条件是

$$
\frac{\partial (u, v)}{\partial (\tilde{u}, \tilde{v})} \gt 0
$$

### 正则曲面

正则参数曲面的概念在应用中是十分方便、十分广泛的，但是有的曲面却不能够用一张正则参数曲面来表示，而是要把曲面分成若干块，然后每一块用正则参数曲面来表示。因此，正则参数曲面只是表示曲面的一种手段， 正则曲面的概念本身尚需另外定义。

> #### 定义3.1
> 设$$S$$是$$\mathbb{E}^3$$的一个子集。如果对于任意一点$$p \in S$$，必存在点$$p$$在$$\mathbb{E}^3$$中的一个邻域$$V \subset \mathbb{E}^3$$，以及$$\mathbb{E}^2$$中的一个区域$$U$$，使得在$$U$$和$$V \cap S$$之间能够建立一一的、双向都是连续的对应，并且该对应$$\boldsymbol{r}: U \rightarrow V \cap S \subset \mathbb{E}^3$$本身是一个正则参数曲面，则称$$S$$是$$\mathbb{E}^3$$中的一张**正则曲面**，简称为**曲面**。
{: .block-definition }

如果曲面$$S$$有两个正则参数表示

$$
\boldsymbol{r}_i: U_i \rightarrow V_i \cap S \subset \mathbb{E}^3, \ \ i = 1,2
$$

使得$$V_1 \cap V_2 \cap S \neq \emptyset$$，那么在任意一点$$p \in V_1 \cap V_2 \cap S$$的附近便有两组曲纹坐标$$(u_1, v_1)$$和$$(u_2, v_2)$$，而且它们之间必定有容许的参数变换。事实上，设

$$
\begin{aligned}
\boldsymbol{r}_1 (u_1, v_1) &= \big( x_1(u_1, v_1), y_1(u_1, v_1), z_1(u_1, v_1) \big), \ \ \forall (u_1, v_1) \in U_1, \\
\boldsymbol{r}_2 (u_2, v_2) &= \big( x_2(u_2, v_2), y_2(u_2, v_2), z_2(u_2, v_2) \big), \ \ \forall (u_2, v_2) \in U_2,
\end{aligned}
$$

那么在点$$p$$的附近有

$$
\begin{aligned}
x &= x_1(u_1, v_1) = x_2(u_2, v_2) \\ 
y &= y_1(u_1, v_1) = y_2(u_2, v_2) \\ 
z &= z_1(u_1, v_1) = z_2(u_2, v_2) \\ 
\end{aligned}
$$

对于函数$$x = x_1(u_1, v_1)$$，$$y = y_1(u_1, v_1)$$，在正则条件

$$
\frac{\partial (x_1, y_1)}{\partial (u_1, v_1)} \neq 0
$$

下，由反函数定理得知，在点$$p$$所对应的参数$$(u_1^0, v_1^0)$$的一个邻域内存在3次以上连续可微的反函数

$$
\begin{cases}
u_1 = f(x, y) \\
v_1 = g(x, y)
\end{cases}
$$

它们满足恒等式

$$
\begin{cases}
f(x_1(u_1, v_1), y_1(u_1, v_1)) \equiv u_1 \\
g(x_1(u_1, v_1), y_1(u_1, v_1)) \equiv v_1
\end{cases}
$$

于是曲纹坐标$$(u_1, v_1)$$和$$(u_2, v_2)$$之间的变换是

$$
\begin{cases}
u_1 = f(x_2(u_2, v_2), y_2(u_2, v_2)) \\
v_1 = g(x_2(u_2, v_2), y_2(u_2, v_2)) \\
\end{cases}
$$

很明显，$$u_1$$、$$v_1$$是$$u_2$$、$$v_2$$的3次以上连续可微函数，并且

$$
\frac{\partial (u_1, v_1)}{\partial (u_2, v_2)} = \frac{\partial (f, g)}{\partial (x, y)} \cdot \frac{\partial (x_2, y_2)}{\partial (u_2, v_2)} = \frac{\partial (x_2, y_2)}{\partial (u_2, v_2)} \bigg( \frac{\partial (x_1, y_1)}{\partial (u_1, v_1)} \bigg)^{-1} \neq 0
$$

因此$$u_1$$、$$v_1$$和$$u_2$$、$$v_2$$之间的参数变换是容许的参数变换。由此可见，在正则曲面上，容许的参数变换是自然而然地出现的，定义在正则曲面上的最应该是与它的正则参数表示无关的量。如果在正则曲面的每一个正则参数表示下都定义了一个量，而它们在容许的参数变换下是保持相等的，则它们便在整个正则曲面上定义了一个完全确定的量。我们要研究的定义在曲面上的量应该具有这种不变性。

如果在正则曲面的每一点的邻域内都能够选定一个正则参数表示，使得在邻域重叠的部分其任意两组参数的变换都保持定向不变，即保持不等式$$\frac{\partial (u_1, v_1)}{\partial (u_2, v_2)} \gt 0$$成立，则称这样的正则曲面是**可定向的**。

在直观上看，每一个正则参数曲面是一个开的曲面片，它与$$\mathbb{E}^2$$中的的一个开区域是同胚的，而正则曲面是把一片片正则参数曲面粘起来的结果。我们研究的重点就是正则参数曲面以及它在容许参数变换下的不变量。

## 切平面和法线

### 切向量

假定正则参数曲面$$S$$的参数方程是$$\boldsymbol{r} = \boldsymbol{r}(u,v)$$。前面介绍过，$$(u, v)$$是曲面$$S$$上的点的曲纹坐标，因此曲面$$S$$上的任意一条连续可微曲线可以用参数方程

$$
\begin{cases}
u = u(t) \\ v = v(t)
\end{cases}
$$

表示，其中$$u(t)$$，$$v(t)$$都是$$t$$的连续可微函数。它作为空间$$\mathbb{E}^3$$中的曲线的参数方程是

$$
\boldsymbol{r} = \boldsymbol{r} (u(t), v(t))
$$

> #### 定义3.2
> 曲面$$S$$上经过点$$p$$的任意一条连续可微曲线在该点的切向量称为曲面$$S$$在点$$p$$的**切向量**。
{: .block-definition }

根据定义，曲面$$S$$上经过点$$p$$的$$u$$-曲线和$$v$$-曲线的切向量$$\boldsymbol{r}_u$$和$$\boldsymbol{r}_v$$都是曲面$$S$$在点$$p$$的切向量。假定$$p$$是曲线上对应于$$t = 0$$的点，则曲线在点$$p$$的切向量是

$$
\begin{aligned}
\frac{\mathrm{d} \boldsymbol{r} (u(t), v(t))}{\mathrm{d} t} \bigg|_{t=0} 
&= \bigg( \frac{\partial \boldsymbol{r}}{\partial u} \frac{\mathrm{d} u(t)}{\mathrm{d} t} + \frac{\partial \boldsymbol{r}}{\partial v} \frac{\mathrm{d} v(t)}{\mathrm{d} t} \bigg) \bigg|_{t=0} \\
&= \boldsymbol{r}_u \frac{\mathrm{d} u(t)}{\mathrm{d} t} \bigg|_{t=0} + \boldsymbol{r}_v \frac{\mathrm{d} v(t)}{\mathrm{d} t} \bigg|_{t=0}
\end{aligned}
$$

这意味着曲面$$S$$在点$$p$$的切向量$$\frac{\mathrm{d} \boldsymbol{r} (u(t), v(t))}{\mathrm{d} t} \bigg\vert_{t=0}$$是切向量$$\boldsymbol{r}_u$$和$$\boldsymbol{r}_v$$的线性组合，其组合系数恰好是$$\frac{\mathrm{d} u(t)}{\mathrm{d} t} \bigg\vert_{t=0}$$，$$\frac{\mathrm{d} v(t)}{\mathrm{d} t} \bigg\vert_{t=0}$$。反过来，切向量$$\boldsymbol{r}_u$$和$$\boldsymbol{r}_v$$的任意一个线性组合$$a \boldsymbol{r}_u + b \boldsymbol{r}_v$$，其中$$a$$，$$b$$是任意的实数，必定是曲面$$S$$的一个切向量。事实上，只要考虑曲面$$S$$上的连续可微曲线

$$
\begin{cases}
\begin{aligned}
u(t) &= u_0 + a t \\
v(t) &= v_0 + b t
\end{aligned}
\end{cases}
$$

其中点$$p$$对应的参数是$$(u_0, v_0)$$，则该曲线在点$$p$$的切向量是

$$
\frac{\mathrm{d} \boldsymbol{r} (u(t), v(t))}{\mathrm{d} t} \bigg|_{t=0} = a \boldsymbol{r}_u + b \boldsymbol{r}_v
$$

由此可见，曲面$$S$$在点$$p$$的切向量就是点$$p$$的切向量$$\boldsymbol{r}_u$$和$$\boldsymbol{r}_v$$的任意的线性组合。

### 切平面和法线

对于正则参数曲面，$$\boldsymbol{r}_u \times \boldsymbol{r}_v \neq \mathbf{0}$$，故切向量$$\boldsymbol{r}_u$$和$$\boldsymbol{r}_v$$是线性无关的，因此曲面$$S$$在点$$p$$(设点$$p$$对应的参数是$$(u, v)$$)的全体切向量构成一个二维向量空间，这个向量空间称为曲面$$S$$在点$$p$$的**切空间**，记为$$T_pS$$。$$\{ \boldsymbol{r}_u, \boldsymbol{r}_v \}$$是曲面$$S$$在点$$p$$的切空间$$T_pS$$的基底。在空间$$\mathbb{E}^3$$中经过点$$p$$、有切向量$$\boldsymbol{r}_u$$、$$\boldsymbol{r}_v$$张成的二维平面称为曲面$$S$$在点$$p$$的**切平面**。它的参数方程是

$$
\boldsymbol{X} (\lambda, \mu) = \boldsymbol{r} (u, v) + \lambda \boldsymbol{r}_u (u, v) + \mu \boldsymbol{r}_v (u, v)
$$

其中$$\lambda$$、$$\mu$$是切平面上动点的参数。很明显，该切平面的法向量是

$$
\boldsymbol{n} (u, v) = \frac{\boldsymbol{r}_u (u, v) \times \boldsymbol{r}_v (u, v)}{\vert \boldsymbol{r}_u (u, v) \times \boldsymbol{r}_v (u, v) \vert}
$$

在空间$$\mathbb{E}^3$$中经过点$$p$$、以法向量$$\boldsymbol{n} (u, v)$$为方向向量的直线称为曲面$$S$$在点$$p$$的**法线**，它的参数方程是

$$
\boldsymbol{X} (t) = \boldsymbol{r} (u, v) + t \boldsymbol{n} (u, v)
$$

其中$$t$$是法线上动点的参数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/okRmIbT.png" width="80%">
</div>

显然，正则参数曲面$$S$$在点$$p$$的切空间、切平面、法线等概念在曲面的容许参数变换下是不变的，因而与正则曲面的参数表示方式无关。当容许参数变换保持定向时，单位法向量$$\boldsymbol{n} (u,v)$$的指向也是不变的；当容许参数变换翻转定向时，单位法向量$$\boldsymbol{n} (u,v)$$则反转指向。由此可见，在可定向的正则曲面上存在连续可微的单位法向量场，而且只有两个互为反转的单位法向量场。确定取哪一个单位法向量场，就是确定了该曲面的一个定向。

### 自然标架

在正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r}(u, v)$$上的每一点，借助于它的参数方程定义了一个标架$$\{\boldsymbol{r}(u,v); \boldsymbol{r}_u(u,v), \boldsymbol{r}_v (u, v), \boldsymbol{n}(u, v) \}$$，称为该曲面上的**自然标架**。这样，正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r}(u, v)$$不仅是依赖参数$$(u, v)$$的点的集合，而且也是在$$\mathbb{E}^3$$中依赖参数$$u$$、$$v$$的这些自然标架的集合，即正则参数曲面不仅是在$$\mathbb{E}^3$$中依赖两个参数的点集，而被进一步看成依赖两个参数的标架族，或者是从区域$$D \subset \mathbb{E}^2$$到$$\mathbb{E}^3$$上的全体标架所构成的12维空间的一个映射。与研究正则参数曲线的情形相仿，对正则参数曲面的研究也可以归结为对自然标架场的研究，我们会在后面进行更深入的介绍。

### 隐式曲面

设$$f(x, y, z)$$是定义在$$\mathbb{E}^3$$上的一个区域$$\tilde{D}$$是的连续可微函数。当

$$
\bigg( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z} \bigg) \neq \mathbf{0}
$$

时，函数$$f$$的等值面

$$
f(x, y, z) = c
$$

是$$\mathbb{E}^3$$中的一个正则曲面。事实上，若设$$p$$是等值面上的一点，对应的参数是$$(x_0, y_0, z_0)$$，并且假定

$$
\frac{\partial f}{\partial z} \bigg\vert_p \neq 0
$$

则根据隐函数定理，在$$Oxy$$平面上点$$(x_0, y_0)$$的一个邻域$$D$$内存在连续可微函数$$z = g(x, y)$$，使得对应任一点$$(x, y) \in D$$满足

$$
f(x, y, g(x, y)) = c
$$

所以，等值面的参数方程是

$$
\boldsymbol{r} (x, y) = (x, y, g(x, y))
$$

很明显，$$\bigg( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z} \bigg)$$就是等值面的法向量。实际上，若设$$x = x(t)$$，$$y = y(t)$$，$$z = z(t)$$是等值面上任意一条连续可微曲线$$C$$的参数方程，则对于任意的$$t$$恒等地有

$$
f(x(t), y(t), z(t)) = c
$$

将上式对变量$$t$$求导得到

$$
\frac{\partial f}{\partial x} \frac{\mathrm{d} x(t)}{\mathrm{d} t} + \frac{\partial f}{\partial y} \frac{\mathrm{d} y(t)}{\mathrm{d} t}  + \frac{\partial f}{\partial z} \frac{\mathrm{d} z(t)}{\mathrm{d} t} = 0
$$

这说明向量$$\bigg( \frac{\partial f}{\partial x}, \frac{\partial f}{\partial y}, \frac{\partial f}{\partial z} \bigg)$$与曲线$$C$$的切向量$$\bigg( \frac{\mathrm{d} x(t)}{\mathrm{d} t}, \frac{\mathrm{d} y(t)}{\mathrm{d} t}, \frac{\mathrm{d} z(t)}{\mathrm{d} t} \bigg)$$是正交的，因而它与等值面上任意一条连续可微曲线都是垂直的，因此必定是等值面的一个法向量。

### 正则参数曲面的微分

现在有必要对正则参数曲面$$S$$的参数方程$$\boldsymbol{r} = \boldsymbol{r} (u, v)$$的微分$$\mathrm{d} \boldsymbol{r} (u, v)$$作一些说明。首先，$$\boldsymbol{r} = \boldsymbol{r} (u, v)$$的微分$$\mathrm{d} \boldsymbol{r} (u, v)$$是关于参数$$u$$、$$v$$的微分$$\mathrm{d} u$$、$$\mathrm{d} v$$的线性表达式：

$$
\begin{aligned}
\mathrm{d} \boldsymbol{r} (u, v) &= \boldsymbol{r}_u (u, v) \mathrm{d} u + \boldsymbol{r}_v (u, v) \mathrm{d} v \\
&=\boldsymbol{r}_u (u, v) \Delta u + \boldsymbol{r}_v (u, v) \Delta v
\end{aligned}
$$

当$$(\Delta u, \Delta v) \to (0, 0)$$时它是函数$$\boldsymbol{r} (u, v)$$的增量$$\boldsymbol{r} (u + \Delta u, v + \Delta v) - \boldsymbol{r} (u, v)$$作为无穷小量的线性的主要部分，即$$\boldsymbol{r} (u + \Delta u, v + \Delta v) - \boldsymbol{r} (u, v) - \mathrm{d} \boldsymbol{r} (u, v)$$是$$\rho = \sqrt{(\Delta u)^2 + (\Delta v)^2}$$的更高阶的无穷小量，也就是

$$
\lim_{\rho \to 0} \frac{\vert \boldsymbol{r} (u + \Delta u, v + \Delta v) - \boldsymbol{r} (u, v) - \mathrm{d} \boldsymbol{r} (u, v) \vert}{\rho} = 0
$$

另一方面，微分表达式告诉我们$$\mathrm{d} \boldsymbol{r} (u, v)$$是4个自变量$$u$$、$$v$$、$$\mathrm{d} u$$、$$\mathrm{d} v$$的函数，它关于这组新的自变量$$\mathrm{d} u$$、$$\mathrm{d} v$$是线性的。这就是说，$$\mathrm{d} \boldsymbol{r} (u, v)$$是切向量$$\boldsymbol{r}_u (u, v)$$，$$\boldsymbol{r}_v (u, v)$$的线性组合，组合系数是自变量$$\mathrm{d} u$$、$$\mathrm{d} v$$，它们可以取任意的实数值。由此可见，在自变量$$u$$、$$v$$固定的时候，也就是在曲面$$S$$上固定一点的情况下，$$\mathrm{d} \boldsymbol{r}$$代表了曲面$$S$$在点$$\boldsymbol{r}$$处的任意一个切向量，而$$\mathrm{d} u$$、$$\mathrm{d} v$$正是该切向量在自然基底$$\{ \boldsymbol{r}_u (u, v), \boldsymbol{r}_v (u, v) \}$$下的分量，因此它们是切空间$$T_p S$$上的线性函数。为说明这一点，先考察一下一般情形。

我们知道，在一个$$n$$维线性空间$$V$$中取定了一个基底$$\{ \boldsymbol{e}_1, ..., \boldsymbol{e}_n \}$$，则$$V$$中任意一个元素关于该基底的分量是线性空间$$V$$上的线性函数。例如，设

$$
\boldsymbol{w} = w^1 \boldsymbol{e}_1 + ... + w^n \boldsymbol{e}_n
$$

用$$f^i$$表示取向量$$\boldsymbol{w}$$在基底$$\{ \boldsymbol{e}_1, ..., \boldsymbol{e}_n \}$$下第$$i$$个分量的函数，即

$$
f^i (\boldsymbol{w}) = w^i
$$

则$$f^i$$是线性空间$$V$$上的线性函数。实际上，若有另一元素$$\boldsymbol{z} \in V$$，

$$
\boldsymbol{z} = z^1 \boldsymbol{e}_1 + ... + z^n \boldsymbol{e}_n
$$

则

$$
\boldsymbol{w} + \boldsymbol{z} = (w^1 + z^1) \boldsymbol{e}_1 + ... + (w^n + z^n) \boldsymbol{e}_n
$$

因此

$$
f^i (\boldsymbol{w} + \boldsymbol{z}) = w^i + z^i = f^i (\boldsymbol{w}) + f^i (\boldsymbol{z})
$$

对于任意的实数$$\lambda$$有

$$
\lambda \cdot \boldsymbol{w} = (\lambda w^1) \boldsymbol{e}_1 + ... + (\lambda w^n) \boldsymbol{e}_n
$$

因此

$$
f^i (\lambda \cdot \boldsymbol{w}) = \lambda w^i = \lambda \cdot f^i (\boldsymbol{w})
$$

这说明$$f^i$$是线性空间$$V$$上的线性函数。

由此可见，$$\mathrm{d} u$$、$$\mathrm{d} v$$是作为二维线性空间的切空间$$T_p S$$上的线性函数，它们在切向量$$\boldsymbol{X}$$上的值分别是该切向量$$\boldsymbol{X}$$在自然基底$$\{ \boldsymbol{r}_u (u, v), \boldsymbol{r}_v (u, v) \}$$下的分量。这就是说，若设$$\boldsymbol{X} = X^1 \boldsymbol{r}_u + X^2 \boldsymbol{r}_v$$，则

$$
\begin{cases}
\begin{aligned}
\mathrm{d} u (\boldsymbol{X}) &= X^1 \\
\mathrm{d} v (\boldsymbol{X}) &= X^2
\end{aligned}
\end{cases}
$$

在上述意义下，曲面$$S$$在点$$\boldsymbol{r} (u, v)$$处的切平面的方程成为

$$
\boldsymbol{X} = \boldsymbol{r} (u, v) + \boldsymbol{r}_u (u, v) \mathrm{d} u + \boldsymbol{r}_v (u, v) \mathrm{d} v
$$

而$$\mathrm{d} u$$、$$\mathrm{d} v$$成为切平面上动点的参数。因此，在已知正则参数曲面$$S$$的参数方程的前提下，对于固定的$$(u, v)$$来说，$$(\mathrm{d} u, \mathrm{d} v)$$的每一个给定的值对应于由$$\mathrm{d} \boldsymbol{r} = \boldsymbol{r}_u \mathrm{d} u + \boldsymbol{r}_v \mathrm{d} v$$给出的一个确定的切向量，$$(\mathrm{d} u, \mathrm{d} v)$$是该切向量的分量或坐标。此外，我们还经常用比值$$\mathrm{d} u : \mathrm{d} v$$表示曲面$$S$$上的一个切方向。

需要指出的是，一般说来，自然基底$$\{ \boldsymbol{r}_u (u, v), \boldsymbol{r}_v (u, v) \}$$**不是**单位正交的，因而$$(\mathrm{d} u, \mathrm{d} v)$$**不是**切向量在笛卡儿直角坐标系下的分量，两个切向量的内积**不能**写成它们的对应的分量的乘积之和。在下一节，我们将具体地研究切向量的内积的表达式。

## 第一基本形式

根据上一节的讨论我们知道，曲面$$S$$上的任意一点$$p \in S$$处的切空间$$T_p S$$是由切向量$$\boldsymbol{r}_u (u, v)$$，$$\boldsymbol{r}_v (u, v)$$张成的二维向量空间，它是$$\mathbb{R}^3$$的子空间。因此，当曲面$$S$$上的切向量作为$$\mathbb{R}^3$$中的向量时可以求它们的长度和夹角。换言之，曲面$$S$$上任意一点的任意两个切向量的内积就是它们作为$$\mathbb{R}^3$$中的向量的内积。在前面已经说过，曲面$$S$$在任意一点$$\boldsymbol{r} (u, v)$$的任意一个切向量是

$$
\mathrm{d} \boldsymbol{r} (u, v) = \boldsymbol{r}_u (u, v) \mathrm{d} u + \boldsymbol{r}_v (u, v) \mathrm{d} v 
$$

其中$$(\mathrm{d} u, \mathrm{d} v)$$是切向量$$\mathrm{d} \boldsymbol{r} (u, v)$$在自然基底$$\{ \boldsymbol{r}_u (u, v), \boldsymbol{r}_v (u, v) \}$$下的分量。一般说来，$$\{ \boldsymbol{r}_u (u, v), \boldsymbol{r}_v (u, v) \}$$**不是单位正交基底**。但是，如果知道这个基底的度量系数，则切向量$$\mathrm{d} \boldsymbol{r} (u, v)$$与其自身的内积就能够表示成它的分量$$\mathrm{d} u$$、$$\mathrm{d} v$$的二次型。命

$$
\begin{aligned}
E(u, v) &= \boldsymbol{r}_u (u, v) \cdot \boldsymbol{r}_u (u, v) \\
F(u, v) &= \boldsymbol{r}_u (u, v) \cdot \boldsymbol{r}_v (u, v) = \boldsymbol{r}_v (u, v) \cdot \boldsymbol{r}_u (u, v) \\
G(u, v) &= \boldsymbol{r}_v (u, v) \cdot \boldsymbol{r}_v (u, v) \\
\end{aligned}
$$

它们就是基底$$\{ \boldsymbol{r}_u (u, v), \boldsymbol{r}_v (u, v) \}$$的度量系数，称为曲面$$S$$的**第一类基本量**，通常把它们写成一个对称矩阵的形式

$$
\begin{pmatrix}
E & F \\ F & G
\end{pmatrix}
$$

很明显，$$E(u, v) \gt 0$$，$$G(u, v) \gt 0$$，并且

$$
\begin{aligned}
E(u, v) G(u, v) - F^2(u, v) 
&= \vert \boldsymbol{r}_u (u, v) \vert^2 \cdot \vert \boldsymbol{r}_v (u, v) \vert^2 \big( 1 - \cos^2 \angle (\boldsymbol{r}_u (u, v), \boldsymbol{r}_v (u, v)) \big) \\
&= \big\vert \boldsymbol{r}_u (u, v) \times \boldsymbol{r}_v (u, v) \big\vert^2 \\
&\gt 0
\end{aligned}
$$

(因为$$\boldsymbol{r}_u (u, v)$$、$$\boldsymbol{r}_v (u, v)$$不共线)，因此对称矩阵$$\begin{pmatrix} E & F \\ F & G \end{pmatrix}$$是一个正定矩阵。命

$$
\begin{aligned}
\mathrm{I} &= \mathrm{d} \boldsymbol{r} (u, v) \cdot \mathrm{d} \boldsymbol{r} (u, v) \\
&= \big( \boldsymbol{r}_u (u, v) \mathrm{d} u + \boldsymbol{r}_v (u, v) \mathrm{d} v  \big) \cdot \big( \boldsymbol{r}_u (u, v) \mathrm{d} u + \boldsymbol{r}_v (u, v) \mathrm{d} v  \big) \\
&= E(u, v) (\mathrm{d} u)^2 + 2F(u, v) \mathrm{d} u \mathrm{d} v + G(u, v) (\mathrm{d} v)^2 \\
&= 
\begin{pmatrix}
\mathrm{d} u, \mathrm{d} v
\end{pmatrix}

\begin{pmatrix}
E & F \\ F & G
\end{pmatrix}

\begin{pmatrix}
\mathrm{d} u \\ \mathrm{d} v
\end{pmatrix}

\end{aligned}
$$

则二次微分式$$\mathrm{I}$$与正则参数曲面$$S$$的参数选取是无关的，称其为曲面$$S$$的**第一基本形式**。

### 第一基本形式的不变性

事实上，根据一次微分的形式不变性，$$\mathrm{d} \boldsymbol{r} (u, v)$$与正则参数曲面$$S$$的参数的选取无关，因此$$\mathrm{I}$$作为$$\mathrm{d} \boldsymbol{r} (u, v)$$与其自身的内积当然也与正则参数曲面$$S$$的参数的选取无关。这个事实也能够从另一个方面进行解释。

假定正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r} (u, v)$$有一个[容许的参数变换](/2023/10/18/DifferentialGeometry-NOTES-03.html#容许的参数变换)

$$
\begin{cases}
u = u(\tilde{u}, \tilde{v}) \\
v = v(\tilde{u}, \tilde{v}) \\
\end{cases}
$$

为方便起见，把曲面$$S$$的新的参数方程仍旧记成

$$
\boldsymbol{r} = \boldsymbol{r} (\tilde{u}, \tilde{v}) = \boldsymbol{r} ( u(\tilde{u}, \tilde{v}), v(\tilde{u}, \tilde{v}))
$$

那么根据求导的链式法则和一次微分的形式不变性，我们有

$$
\begin{aligned}
\mathrm{d} \boldsymbol{r} &= \boldsymbol{r}_u (u, v) \mathrm{d} u + \boldsymbol{r}_v (u, v) \mathrm{d} v  \\
&= \boldsymbol{r}_u (u, v) \bigg( \frac{\partial u}{\partial \tilde{u}} \mathrm{d} \tilde{u} + \frac{\partial u}{\partial \tilde{v}} \mathrm{d} \tilde{v} \bigg) + \boldsymbol{r}_v (u, v) \bigg( \frac{\partial v}{\partial \tilde{u}} \mathrm{d} \tilde{u} + \frac{\partial v}{\partial \tilde{v}} \mathrm{d} \tilde{v} \bigg) \\
&= \boldsymbol{r}_{\tilde{u}} (\tilde{u}, \tilde{v}) \mathrm{d} \tilde{u} + \boldsymbol{r}_{\tilde{v}} (\tilde{u}, \tilde{v}) \mathrm{d} \tilde{v}
\end{aligned}
$$

因此

$$
\begin{aligned}
\boldsymbol{r}_{\tilde{u}} (\tilde{u}, \tilde{v}) &= \boldsymbol{r}_u (u, v) \frac{\partial u}{\partial \tilde{u}} + \boldsymbol{r}_v (u, v) \frac{\partial v}{\partial \tilde{u}} \\
\boldsymbol{r}_{\tilde{v}} (\tilde{u}, \tilde{v}) &= \boldsymbol{r}_u (u, v) \frac{\partial u}{\partial \tilde{v}} + \boldsymbol{r}_v (u, v) \frac{\partial v}{\partial \tilde{v}}
\end{aligned}
$$

并且

$$
\begin{aligned}
\mathrm{d} u &= \frac{\partial u}{\partial \tilde{u}} \mathrm{d} \tilde{u} + \frac{\partial u}{\partial \tilde{v}} \mathrm{d} \tilde{v} \\
\mathrm{d} v &= \frac{\partial v}{\partial \tilde{u}} \mathrm{d} \tilde{u} + \frac{\partial v}{\partial \tilde{v}} \mathrm{d} \tilde{v}
\end{aligned}
$$

将上面两组式子用矩阵来表示，可以得到

$$
\begin{pmatrix}
\boldsymbol{r}_{\tilde{u}} \\ \boldsymbol{r}_{\tilde{v}}
\end{pmatrix}
= J \cdot
\begin{pmatrix}
\boldsymbol{r}_u \\ \boldsymbol{r}_v
\end{pmatrix}
$$

$$
\begin{pmatrix}
\mathrm{d} u & \mathrm{d} v
\end{pmatrix}
=
\begin{pmatrix}
\mathrm{d} \tilde{u} & \mathrm{d} \tilde{v}
\end{pmatrix}
\cdot J
$$

其中

$$
J = 
\begin{pmatrix}
\frac{\partial u}{\partial \tilde{u}} & \frac{\partial v}{\partial \tilde{u}} \\
\frac{\partial u}{\partial \tilde{v}} & \frac{\partial v}{\partial \tilde{v}} \\
\end{pmatrix}
$$

根据曲面$$S$$的第一类基本量的定义

$$
\begin{pmatrix}
E & F \\ F & G
\end{pmatrix}
=
\begin{pmatrix}
\boldsymbol{r}_u \\ \boldsymbol{r}_v
\end{pmatrix}
\cdot
\begin{pmatrix}
\boldsymbol{r}_u & \boldsymbol{r}_v
\end{pmatrix}
$$

因此

$$
\begin{pmatrix}
\tilde{E} & \tilde{F} \\ \tilde{F} & \tilde{G}
\end{pmatrix}
=
\begin{pmatrix}
\boldsymbol{r}_{\tilde{u}} \\ \boldsymbol{r}_{\tilde{v}}
\end{pmatrix}
\cdot
\begin{pmatrix}
\boldsymbol{r}_{\tilde{u}} & \boldsymbol{r}_{\tilde{v}}
\end{pmatrix}
=
J \cdot
\begin{pmatrix}
E & F \\ F & G
\end{pmatrix}
\cdot J^T
$$

所以，曲面$$S$$的第一类基本量的矩阵$$\begin{pmatrix}\tilde{E} & \tilde{F} \\ \tilde{F} & \tilde{G}\end{pmatrix}$$和$$\begin{pmatrix} E & F \\ F & G \end{pmatrix}$$差一个合同变换。

将$$\begin{pmatrix}\tilde{E} & \tilde{F} \\ \tilde{F} & \tilde{G}\end{pmatrix}$$展开则得到

$$
\begin{aligned}
\tilde{E} &= E \bigg( \frac{\partial u}{\partial \tilde{u}} \bigg)^2 + 2F \frac{\partial u}{\partial \tilde{u}} \frac{\partial v}{\partial \tilde{u}} + G \bigg(\frac{\partial v}{\partial \tilde{u}} \bigg)^2 \\
\tilde{F} &= E \frac{\partial u}{\partial \tilde{u}} \frac{\partial u}{\partial \tilde{v}} + F \bigg( \frac{\partial u}{\partial \tilde{u}} \frac{\partial v}{\partial \tilde{v}} + \frac{\partial u}{\partial \tilde{v}} \frac{\partial v}{\partial \tilde{u}} \bigg) + G \frac{\partial v}{\partial \tilde{u}} \frac{\partial v}{\partial \tilde{v}} \\
\tilde{G} &= E \bigg(\frac{\partial u}{\partial \tilde{v}} \bigg)^2 + 2F \frac{\partial u}{\partial \tilde{v}} \frac{\partial v}{\partial \tilde{v}} + G \bigg(\frac{\partial v}{\partial \tilde{v}} \bigg)^2\\
\end{aligned}
$$

类似地

$$
\begin{aligned}
\tilde{E} (\mathrm{d} \tilde{u})^2 + 2 \tilde{F} \mathrm{d} \tilde{u} \mathrm{d} \tilde{v} + \tilde{G} (\mathrm{d} \tilde{v})^2
&=
\begin{pmatrix}
\mathrm{d} \tilde{u} & \mathrm{d} \tilde{v}
\end{pmatrix}
\begin{pmatrix}
\tilde{E} & \tilde{F} \\ \tilde{F} & \tilde{G}
\end{pmatrix}
\begin{pmatrix}
\mathrm{d} \tilde{u} \\ \mathrm{d} \tilde{v}
\end{pmatrix} \\
&=
\begin{pmatrix}
\mathrm{d} \tilde{u} & \mathrm{d} \tilde{v}
\end{pmatrix}
J \cdot
\begin{pmatrix}
E & F \\ F & G
\end{pmatrix}
\cdot J^T
\begin{pmatrix}
\mathrm{d} \tilde{u} \\ \mathrm{d} \tilde{v}
\end{pmatrix} \\
&=
\begin{pmatrix}
\mathrm{d} u & \mathrm{d} v
\end{pmatrix}
\begin{pmatrix}
E & F \\ F & G
\end{pmatrix}
\begin{pmatrix}
\mathrm{d} u \\ \mathrm{d} v
\end{pmatrix} \\
&= 
E (\mathrm{d} u)^2 + 2F \mathrm{d} u \mathrm{d} v + G (\mathrm{d} v)^2 \\
\end{aligned}
$$

即二次微分式$$\mathrm{I}$$与正则参数曲面$$S$$的参数的选取是无关的。

### 几何意义

第一基本形式$$\mathrm{I}$$的几何意义是切向量$$\mathrm{d} \boldsymbol{r}$$的长度平方。若在点$$(u, v)$$处有另一个切向量

$$
\delta \boldsymbol{r} (u, v) = \boldsymbol{r}_u (u, v) \delta u + \boldsymbol{r}_v (u, v) \delta v
$$

它的分量是$$\delta u$$、$$\delta v$$，则切向量$$\mathrm{d} \boldsymbol{r}$$和$$\delta \boldsymbol{r}$$的内积是

$$
\begin{aligned}
\mathrm{d} \boldsymbol{r} \cdot \delta \boldsymbol{r} &= \frac{1}{2} \big[ (\mathrm{d} \boldsymbol{r} + \delta \boldsymbol{r} )^2 - (\mathrm{d} \boldsymbol{r})^2 - (\delta \boldsymbol{r})^2 \big] \\
&= E \mathrm{d} u \delta u + F (\mathrm{d} u \delta v + \mathrm{d} v \delta u) + G \mathrm{d} v \delta v
\end{aligned}
$$

为方便起见，有时把上式右端表达式记为$$\mathrm{I} (\mathrm{d} \boldsymbol{r}, \delta \boldsymbol{r})$$。因此

$$
\vert \mathrm{d} \boldsymbol{r} \vert = \sqrt{E (\mathrm{d} u)^2 + 2F \mathrm{d} u \mathrm{d} v + G (\mathrm{d} v)^2} = \sqrt{\mathrm{I} (\mathrm{d} \boldsymbol{r}, \mathrm{d} \boldsymbol{r})}
$$

并且

$$
\begin{aligned}
\cos{\angle (\mathrm{d} \boldsymbol{r}, \delta \boldsymbol{r})} &= \frac{\mathrm{d} \boldsymbol{r} \cdot \delta \boldsymbol{r}}{\vert \mathrm{d} \boldsymbol{r} \vert \vert \delta \boldsymbol{r} \vert} = \frac{\mathrm{I} (\mathrm{d} \boldsymbol{r}, \delta \boldsymbol{r})}{\sqrt{\mathrm{I} (\mathrm{d} \boldsymbol{r}, \mathrm{d} \boldsymbol{r})} \sqrt{\mathrm{I} (\delta \boldsymbol{r}, \delta \boldsymbol{r})}} \\
&= \frac{E \mathrm{d} u \delta u + F (\mathrm{d} u \delta v + \mathrm{d} v \delta u) + G \mathrm{d} v \delta v}{\sqrt{E (\mathrm{d} u)^2 + 2F \mathrm{d} u \mathrm{d} v + G (\mathrm{d} v)^2} \sqrt{E (\delta u)^2 + 2F \delta u \delta v + G (\delta v)^2}}
\end{aligned}
$$

由此可见，切向量$$\mathrm{d} \boldsymbol{r}$$和$$\delta \boldsymbol{r}$$彼此正交的充分必要条件是：

$$
E \mathrm{d} u \delta u + F (\mathrm{d} u \delta v + \mathrm{d} v \delta u) + G \mathrm{d} v \delta v = 0
$$

> #### 定理3.1
> 在正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r} (u, v)$$上参数曲线网是正交曲线网的充分必要条件是$$F(u, v) \equiv 0$$。
{: .block-theorem }

### 曲面上曲线的弧长和区域面积

利用曲面的第一基本形式，能够计算正则参数曲面上的曲线的弧长和区域的面积。假定正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r} (u, v)$$上的一条连续可微曲线的方程是

$$
u = u(t), \ \ v = v(t), \ \ a \leq t \leq b
$$

则曲线的切向量是

$$
\frac{\mathrm{d} \boldsymbol{r} (u(t), v(t))}{\mathrm{d} t} = \boldsymbol{r}_u \frac{\mathrm{d} u(t)}{\mathrm{d} t} + \boldsymbol{r}_v \frac{\mathrm{d} v(t)}{\mathrm{d} t}
$$

结合第一基本形式$$\mathrm{I}$$可以得到

$$
\bigg\vert \frac{\mathrm{d} \boldsymbol{r} (u(t), v(t))}{\mathrm{d} t} \bigg\vert = \sqrt{E \bigg( \frac{\mathrm{d} u(t)}{\mathrm{d} t} \bigg)^2 + 2F \frac{\mathrm{d} u(t)}{\mathrm{d} t} \frac{\mathrm{d} v(t)}{\mathrm{d} t}  + G \bigg( \frac{\mathrm{d} v(t)}{\mathrm{d} t} \bigg)^2 }
$$

在根据[曲线的弧长公式](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的弧长)可以得到曲线的长度是

$$
\begin{aligned}
L &= \int_a^b \vert \boldsymbol{r}'(t) \vert \mathrm{d} t \\
&= \int_a^b \sqrt{E \bigg( \frac{\mathrm{d} u(t)}{\mathrm{d} t} \bigg)^2 + 2F \frac{\mathrm{d} u(t)}{\mathrm{d} t} \frac{\mathrm{d} v(t)}{\mathrm{d} t}  + G \bigg( \frac{\mathrm{d} v(t)}{\mathrm{d} t} \bigg)^2 } \mathrm{d} t
\end{aligned}
$$

现在假定正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r} (u, v)$$定义在区域$$D \subset \mathbb{E}^2$$上。考虑曲面上由参数曲线

$$
u = u_0, \ \ u = u_0 + \Delta u, \ \ v = v_0, \ \ v = v_0 + \Delta v
$$

所围成的一小块，其中$$\Delta u \lt 0$$，$$\Delta v \lt 0$$，它的面积近似地等于在点$$\boldsymbol{r}(u_0, v_0)$$处的切平面上由向量$$(\Delta u) \boldsymbol{r}_u$$和$$(\Delta v) \boldsymbol{r}_v$$所张成的平行四边形的面积，其大小可以表示为

$$
\begin{aligned}
\vert ((\Delta u) \boldsymbol{r}_u) \times ((\Delta v) \boldsymbol{r}_v) \vert &= \vert \boldsymbol{r}_u \times \boldsymbol{r}_v \vert \Delta u \Delta v \\
&= \vert \boldsymbol{r}_u \vert \vert \boldsymbol{r}_v \vert \sin{\angle(\boldsymbol{r}_u, \boldsymbol{r}_v)} \Delta u \Delta v \\
&= \sqrt{EG - F^2} \Delta u \Delta v
\end{aligned}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ifmiGYP.png" width="80%">
</div>

命

$$
\mathrm{d} \sigma = \sqrt{EG - F^2} \mathrm{d} u \mathrm{d} v
$$

称$$\mathrm{d} \sigma$$为曲面$$S$$的**面积元素**。那么曲面$$S$$的面积是

$$
A = \iint_D \sqrt{EG - F^2} \mathrm{d} u \mathrm{d} v
$$

根据重积分的变量替换法则以及第一类基本量的[变换规律](/2023/10/18/DifferentialGeometry-NOTES-03.html#第一基本形式的不变性)，不难知道上式右端与曲面$$S$$是容许的参数变换是无关的。

事实上，若有[容许的参数变换](/2023/10/18/DifferentialGeometry-NOTES-03.html#容许的参数变换)则曲面的参数方程成为

$$
\boldsymbol{r} = \boldsymbol{r} (\tilde{u}, \tilde{v}) \equiv \boldsymbol{r} (u(\tilde{u}, \tilde{v}), v(\tilde{u}, \tilde{v})), \ \ \forall (\tilde{u}, \tilde{v}) \in D
$$

根据重积分的变量替换法则，曲面面积公式的二次积分在变量替换下成为

$$
\iint_D \sqrt{EG - F^2} \mathrm{d} u \mathrm{d} v = \iint_{\tilde{D}} \sqrt{EG - F^2} \bigg\vert \frac{\partial u}{\partial \tilde{u}} \frac{\partial v}{\partial \tilde{v}} - \frac{\partial u}{\partial \tilde{v}} \frac{\partial v}{\partial \tilde{u}} \bigg\vert \mathrm{d} \tilde{u} \mathrm{d} \tilde{v}
$$

根据第一类基本量之间的[变换关系](/2023/10/18/DifferentialGeometry-NOTES-03.html#第一基本形式的不变性)可以得知

$$
\det {
\begin{pmatrix}
\tilde{E} & \tilde{F} \\ \tilde{F} & \tilde{G}
\end{pmatrix}
}
=
\det {J} \cdot
\det{
\begin{pmatrix}
E & F \\ F & G
\end{pmatrix}
}
\cdot \det{J^T}
$$

即

$$
\tilde{E} \tilde{G} - \tilde{F}^2 = (EG - F^2) \bigg( \frac{\partial u}{\partial \tilde{u}} \frac{\partial v}{\partial \tilde{v}} - \frac{\partial u}{\partial \tilde{v}} \frac{\partial v}{\partial \tilde{u}} \bigg)^2
$$

因此

$$
\sqrt{\tilde{E} \tilde{G} - \tilde{F}^2} = 
\sqrt{EG - F^2} \ \bigg\vert \frac{\partial u}{\partial \tilde{u}} \frac{\partial v}{\partial \tilde{v}} - \frac{\partial u}{\partial \tilde{v}} \frac{\partial v}{\partial \tilde{u}} \bigg\vert
$$

所以

$$
\iint_D \sqrt{EG - F^2} \mathrm{d} u \mathrm{d} v = \iint_{\tilde{D}} \sqrt{\tilde{E} \tilde{G} - \tilde{F}^2} \mathrm{d} \tilde{u} \mathrm{d} \tilde{v}
$$

也就是说正则曲面$$S$$的面积与的正则参数表示无关。

## 曲面上正交参数曲线网的存在性

选择适当的坐标系可以大大地简化几何图形的方程，从而降低求解问题的复杂度。对于一般的正则参数曲面，参数是它上面的曲纹坐标，因此适当的坐标系首先应该是正交参数系，此时$$F \equiv 0$$，于是曲面的第一基本形式可以化为比较简单的形式：

$$
\mathrm{I} = E (\mathrm{d} u)^2 + G (\mathrm{d} v)^2
$$

于是，我们遇到的首要问题是：在正则参数曲面片上是否存在正交参数系？答案是肯定的：在正则曲面的每一点的某个邻域内一定有正交参数曲线网，这是二维曲面所特有的性质。此外，在曲面上正交参数曲线网不是唯一的，它的确定取决于在曲面上给定两个彼此正交的切向量场。为了证明正交参数曲线网的存在性，我们首先叙述下面的引理：

**引理** 设$$f(u, v)$$，$$g(u, v)$$是定义在区域$$D \subset \mathbb{E}^2$$上的两个不同时为零的连续可微函数，则对于任意一点$$(u_0, v_0) \in D$$必有它的一个邻域$$U \subset D$$以及$$U$$上的非零连续函数$$\lambda (u, v)$$，使得$$\lambda(u, v)$$是一次微分式$$\omega = f(u, v) \mathrm{d} u + g(u, v) \mathrm{d} v$$的积分因子，即在$$U$$上存在某个连续可微函数$$k(u, v)$$使得$$\lambda(u, v) \big( f(u, v) \mathrm{d} u + g(u, v) \mathrm{d} v \big) = \mathrm{d}k(u, v)$$。
{: .block-theorem }

上述引理的证明可以参考[《全微分方程积分因子的存在性》](http://staff.ustc.edu.cn/~spliu/2017DG/Chen_Weihuan.pdf)。值得指出的是，引理的结论只对含有两个变量的一次微分式才成立，这就是本节的结论**只适用于曲面情形**的理由。

下面我们利用引理证明一个比正交参数系的存在性更一般的命题：

> #### 定理3.2 
> 假定在正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r} (u, v)$$上两个处处线性无关的连续可微的切向量场$$\boldsymbol{a} (u, v)$$，$$\boldsymbol{b} (u, v)$$，则对于每一点$$p \in S$$必有点$$p$$的邻域$$U \subset S$$，以及在$$U$$上的新的参数系$$(\tilde{u}, \tilde{v})$$，使得新参数曲线的切向量$$\boldsymbol{r}_{\tilde{u}}$$，$$\boldsymbol{r}_{\tilde{v}}$$分别与$$\boldsymbol{a}$$，$$\boldsymbol{b}$$平行，即$$\boldsymbol{r}_{\tilde{u}} \parallel \boldsymbol{a}$$，$$\boldsymbol{r}_{\tilde{u}} \parallel \boldsymbol{b}$$。
{: .block-theorem }

**证明** 假定在自然基底$$\{ \boldsymbol{r}_u, \boldsymbol{r}_v \}$$下，切向量场$$\boldsymbol{a}(u, v)$$，$$\boldsymbol{b}(u, v)$$的表达式是

$$
\begin{aligned}
\boldsymbol{a} (u, v) &= a_1 (u, v) \boldsymbol{r}_u + a_2 (u, v) \boldsymbol{r}_v \\
\boldsymbol{b} (u, v) &= b_1 (u, v) \boldsymbol{r}_u + b_2 (u, v) \boldsymbol{r}_v \\
\end{aligned}
$$

其中，$$a_i (u, v)$$，$$b_i (u, v)$$都是连续可微函数，并且

$$
A = 
\begin{vmatrix}
a_1 (u, v) & a_2 (u, v) \\
b_1 (u, v) & b_2 (u, v) \\
\end{vmatrix}
\neq 0
$$

我们先对问题做一些分析。如果有[容许的参数变换](/2023/10/18/DifferentialGeometry-NOTES-03.html#容许的参数变换)

$$
\begin{cases}
u = u(\tilde{u}, \tilde{v}) \\
v = v(\tilde{u}, \tilde{v}) \\
\end{cases}
$$

使得$$\boldsymbol{r}_{\tilde{u}} \parallel \boldsymbol{a}$$，$$\boldsymbol{r}_{\tilde{v}} \parallel \boldsymbol{b}$$，则必有函数$$\lambda(u, v)$$，$$\mu(u, v)$$使得

$$
\boldsymbol{r}_{\tilde{u}} = \lambda \boldsymbol{a}, \ \ \boldsymbol{r}_{\tilde{v}} = \mu \boldsymbol{b}
$$

展开得到

$$
\begin{aligned}
\boldsymbol{r}_{\tilde{u}} &= \lambda a_1 (u, v) \boldsymbol{r}_u + \lambda a_2 (u, v) \boldsymbol{r}_v \\
\boldsymbol{r}_{\tilde{v}} &= \mu b_1 (u, v) \boldsymbol{r}_u + \mu b_2 (u, v) \boldsymbol{r}_v \\
\end{aligned}
$$

因此

$$
J = 
\begin{pmatrix}
\frac{\partial u}{\partial \tilde{u}} & \frac{\partial v}{\partial \tilde{u}} \\
\frac{\partial u}{\partial \tilde{v}} & \frac{\partial v}{\partial \tilde{v}} \\
\end{pmatrix}
=
\begin{pmatrix}
\lambda a_1 (u, v) & \lambda a_2 (u, v) \\
\mu b_1 (u, v) & \mu b_2 (u, v) \\
\end{pmatrix}
$$

设$$u = u(\tilde{u}, \tilde{v})$$，$$v = v(\tilde{u}, \tilde{v})$$的反函数是$$\tilde{u} = \tilde{u}(u, v)$$，$$\tilde{v} = \tilde{v}(u, v)$$，即下面的恒等式成立

$$
\begin{cases}
u(\tilde{u} (u, v), \tilde{v} (u, v)) \equiv u \\
v(\tilde{u} (u, v), \tilde{v} (u, v)) \equiv v \\
\end{cases}
$$

将它们分别对$$u$$，$$v$$求导得到

$$
\begin{cases}
\begin{aligned}
\frac{\partial u}{\partial \tilde{u}} \frac{\partial \tilde{u}}{\partial u} + \frac{\partial u}{\partial \tilde{v}} \frac{\partial \tilde{v}}{\partial u} &= 1 \\
\frac{\partial u}{\partial \tilde{u}} \frac{\partial \tilde{u}}{\partial v} + \frac{\partial u}{\partial \tilde{v}} \frac{\partial \tilde{v}}{\partial v} &= 0 \\
\frac{\partial v}{\partial \tilde{u}} \frac{\partial \tilde{u}}{\partial u} + \frac{\partial v}{\partial \tilde{v}} \frac{\partial \tilde{v}}{\partial u} &= 0 \\
\frac{\partial v}{\partial \tilde{u}} \frac{\partial \tilde{u}}{\partial v} + \frac{\partial v}{\partial \tilde{v}} \frac{\partial \tilde{v}}{\partial v} &= 1 \\
\end{aligned}
\end{cases}
$$

用矩阵形式表示则是

$$
\begin{pmatrix}
\frac{\partial \tilde{u}}{\partial u} & \frac{\partial \tilde{v}}{\partial u} \\
\frac{\partial \tilde{u}}{\partial v} & \frac{\partial \tilde{v}}{\partial v} \\
\end{pmatrix}
\cdot 
\begin{pmatrix}
\frac{\partial u}{\partial \tilde{u}} & \frac{\partial v}{\partial \tilde{u}} \\
\frac{\partial u}{\partial \tilde{v}} & \frac{\partial v}{\partial \tilde{v}} \\
\end{pmatrix}
=
\begin{pmatrix}
1 & 0 \\ 0 & 1
\end{pmatrix}
$$

这就是说，反函数的Jacobi矩阵是原函数的Jacobi矩阵的逆，即

$$
J^{-1} = 
\begin{pmatrix}
\frac{\partial \tilde{u}}{\partial u} & \frac{\partial \tilde{v}}{\partial u} \\
\frac{\partial \tilde{u}}{\partial v} & \frac{\partial \tilde{v}}{\partial v} \\
\end{pmatrix}
$$

因此

$$
\begin{pmatrix}
\frac{\partial \tilde{u}}{\partial u} & \frac{\partial \tilde{v}}{\partial u} \\
\frac{\partial \tilde{u}}{\partial v} & \frac{\partial \tilde{v}}{\partial v} \\
\end{pmatrix}
=
\frac{1}{\lambda \mu A}
\begin{pmatrix}
\mu b_2 (u, v) & -\lambda a_2 (u, v) \\
-\mu b_1 (u, v) & \lambda a_1 (u, v)
\end{pmatrix}
$$

其中$$A = a_1 b_2 - a_2 b_1$$与之前的定义相同。由此可见

$$
\begin{cases}
\begin{aligned}
\mathrm{d} \tilde{u} &= \frac{\partial \tilde{u}}{\partial u} \mathrm{d} u + \frac{\partial \tilde{u}}{\partial v} \mathrm{d} v = \frac{1}{\lambda A} (b_2 \mathrm{d} u - b_1 \mathrm{d} v) \\
\mathrm{d} \tilde{v} &= \frac{\partial \tilde{v}}{\partial u} \mathrm{d} u + \frac{\partial \tilde{v}}{\partial v} \mathrm{d} v = \frac{1}{\lambda A} (-a_2 \mathrm{d} u + a_1 \mathrm{d} v) \\
\end{aligned}
\end{cases}
$$

这表明我们所假设的参数变换是存在的，则必有函数$$\lambda(u, v)$$，$$\mu(u, v)$$使得

$$
\frac{1}{\lambda A} (b_2 \mathrm{d} u - b_1 \mathrm{d} v), \ \ \frac{1}{\lambda A} (-a_2 \mathrm{d} u + a_1 \mathrm{d} v)
$$

是全微分。所以，我们要考虑的问题就是一次微分式

$$
\begin{cases}
\begin{aligned}
\alpha &= b_2 \mathrm{d} u - b_1 \mathrm{d} v \\
\beta &= -a_2 \mathrm{d} u + a_1 \mathrm{d} v
\end{aligned}
\end{cases}
$$

的积分因子的存在性问题。

根据引理，对于任意一点$$p \in S$$存在点$$p$$的邻域和定义在$$U$$上的处处非零的连续可微函数$$\xi$$，$$\eta$$，使得它们是一次微分式$$\alpha$$，$$\beta$$的积分因子，即在$$U$$上存在连续可微函数$$\tilde{u} (u, v)$$，$$\tilde{v} (u, v)$$满足条件

$$
\begin{cases}
\begin{aligned}
\mathrm{d} \tilde{u} &= \xi (b_2 \mathrm{d} u - b_1 \mathrm{d} v) \\
\mathrm{d} \tilde{v} &= \eta (-a_2 \mathrm{d} u + a_1 \mathrm{d} v) \\
\end{aligned}
\end{cases}
$$

于是

$$
\begin{pmatrix}
\frac{\partial \tilde{u}}{\partial u} & \frac{\partial \tilde{v}}{\partial u} \\
\frac{\partial \tilde{u}}{\partial v} & \frac{\partial \tilde{v}}{\partial v}
\end{pmatrix}
=
\begin{pmatrix}
\xi b_2(u, v) & -\eta a_2(u, v) \\
-\xi b_1(u, v) & \eta a_1(u, v) \\
\end{pmatrix}
$$

并且它的行列式为$$\xi \eta (a_1 b_2 - a_2 b_1) \neq 0$$。这说明$$\tilde{u}$$，$$\tilde{v}$$是曲面$$S$$在邻域$$U$$内新的参数。根据前面的分析不难知道

$$
\begin{pmatrix}
\frac{\partial u}{\partial \tilde{u}} & \frac{\partial v}{\partial \tilde{u}} \\
\frac{\partial u}{\partial \tilde{v}} & \frac{\partial v}{\partial \tilde{v}}
\end{pmatrix}
=
\frac{1}{\xi \eta (a_1 b_2 - b_1 a_2)}
\begin{pmatrix}
\eta a_1 & \eta a_2 \\
\xi b_1 & \xi b_2 \\
\end{pmatrix}
$$

所以

$$
\begin{aligned}
\boldsymbol{r}_{\tilde{u}} &= \boldsymbol{r}_u \frac{\partial u}{\partial \tilde{u}} + \boldsymbol{r}_v \frac{\partial v}{\partial \tilde{u}} = \frac{1}{\xi (a_1 b_2 - b_1 a_2)} (a_1 \boldsymbol{r}_u + a_2 \boldsymbol{r}_v) \\
&= \frac{1}{\xi (a_1 b_2 - b_1 a_2)} \boldsymbol{a} \\
\boldsymbol{r}_{\tilde{v}} &= \boldsymbol{r}_u \frac{\partial u}{\partial \tilde{v}} + \boldsymbol{r}_v \frac{\partial v}{\partial \tilde{v}} = \frac{1}{\eta (a_1 b_2 - b_1 a_2)} (b_1 \boldsymbol{r}_u + b_2 \boldsymbol{r}_v) \\
&= \frac{1}{\eta (a_1 b_2 - b_1 a_2)} \boldsymbol{b} \\
\end{aligned}
$$

证毕∎

定理3.2的意思是在曲面上存在局部适用的参数系，使得参数曲线分别与预先给定的处处线性无关的切向量场相切(即以已知的切向量场作为参数曲线的方向场)。但是，一般来说，要使已知的切向量场恰好是参数曲线的切向量场(即$$\boldsymbol{r}_{\tilde{u}} = \boldsymbol{a}$$，$$\boldsymbol{r}_{\tilde{v}} = \boldsymbol{b}$$)是做不到的。

> #### 定理3.3
> 在正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r} (u, v)$$上的每一点$$p \in S$$，必有点$$p$$的邻域$$U \subset S$$，以及在$$U$$上的新的参数系$$(\tilde{u}, \tilde{v})$$，使得新参数曲线的切向量$$\boldsymbol{r}_{\tilde{u}}$$，$$\boldsymbol{r}_{\tilde{v}}$$是彼此正交的，即$$(\tilde{u}, \tilde{v})$$是曲面$$S$$在$$U$$上的正交参数系。
{: .block-theorem }

**证明** 对正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r}(u, v)$$的自然基底$$\{ \boldsymbol{r}_u, \boldsymbol{r}_v \}$$作[Schmidt正交化](https://en.wikipedia.org/wiki/Gram%E2%80%93Schmidt_process)如下：命

$$
\boldsymbol{e}_1 = \frac{\boldsymbol{r}_u}{\vert \boldsymbol{r}_u \vert} = \frac{\boldsymbol{r}_u}{\sqrt{E}}
$$

再令

$$
\boldsymbol{b} = \boldsymbol{r}_v + \lambda \boldsymbol{e}_1
$$

要求$$\boldsymbol{b} \cdot \boldsymbol{e}_1 = 0$$。因此

$$
\lambda = -\boldsymbol{r}_v \cdot \boldsymbol{e}_1 = -\frac{F}{\sqrt{E}}
$$

故

$$
\boldsymbol{b} = \boldsymbol{r}_v - \frac{F}{\sqrt{E}} \boldsymbol{e}_1 = \boldsymbol{r}_v - \frac{F}{E} \boldsymbol{r}_u
$$

这样

$$
\vert \boldsymbol{b} \vert^2 = G - \frac{F^2}{E} = \frac{EG - F^2}{E}
$$

命

$$
\boldsymbol{e}_2 = \frac{\boldsymbol{e}_2}{\vert \boldsymbol{e}_2 \vert} = \frac{1}{\sqrt{EG - F^2}} \bigg( -\frac{F}{\sqrt{E}} \boldsymbol{r}_u  + \sqrt{E} \boldsymbol{r}_v \bigg)
$$

则$$\{ \boldsymbol{r}_u, \boldsymbol{r}_v \}$$是曲面$$S$$上的单位正交标架场。

根据定理3.2，则每一点$$p$$的一个邻域$$U$$内存在新的参数$$(\tilde{u}, \tilde{v})$$，使得$$\boldsymbol{r}_{\tilde{u}} \parallel \boldsymbol{e}_1$$，$$\boldsymbol{r}_{\tilde{v}} \parallel \boldsymbol{e}_2$$，故$$(\tilde{u}, \tilde{v})$$是正交参数系。证毕∎

当然，定理3.3只是一个存在性定理；要在已知曲面上找出正交参数曲线网相当于在曲面上找两个彼此正交的切向量场$$\boldsymbol{a}$$和$$\boldsymbol{b}$$，然后求出相应微分式$$\alpha$$和$$\beta$$的积分因子。一般来说，前一件事是容易做到的，而后一件却不是一件容易的事。尽管如此，定理3.3仍然是十分重要的，因为这个定理保证了在正则曲面上正交参数曲线网的存在性，从而使得我们在理论上处理正则曲面的问题变得比较简单了。

## 保长对应和保角对应

### 正则参数曲面之间的对应

假定有两个正则参数曲面，它们的参数方程分别是

$$
\begin{aligned}
&S_1: \boldsymbol{r} = \boldsymbol{r}_1 (u_1, v_1), &(u_1, v_1) \in D_1, \\
&S_2: \boldsymbol{r} = \boldsymbol{r}_2 (u_2, v_2), &(u_2, v_2) \in D_2, \\
\end{aligned}
$$

其中$$D_1$$，$$D_2$$是$$\mathbb{E}^2$$中的两个区域。因为$$(u_i, v_i)$$是正则曲面$$S_i$$的点的曲纹坐标，所以从曲面$$S_1$$到$$S_2$$的一个映射表现为从区域$$D_1$$到$$D_2$$的一个映射。换言之，如果有映射$$\sigma: D_1 \rightarrow D_2$$，表示为

$$
\begin{cases}
\begin{aligned}
u_2 &= f(u_1, v_1) \\
v_2 &= g(u_1, v_1)
\end{aligned}
\end{cases}
$$

则我们有从曲面$$S_1$$到$$S_2$$的映射(仍记为$$\sigma$$)，它把曲面$$S_1$$上的点$$\boldsymbol{r}_1 (u_1, v_1)$$映为曲面$$S_2$$上的点$$\boldsymbol{r}_2(f(u_1, v_1), g(u_1, v_1))$$。反过来，正则参数曲面之间的映射都可以这样来表示。这就是说，正则参数曲面之间的一个对应表现为它们的参数区域之间的一个对应。如果函数表达式$$f(u_1, v_1)$$，$$g(u_1, v_1)$$是连续可微的，则称映射$$\sigma: D_1 \rightarrow D_2$$是连续可微的。根据曲面的容许参数变换条件，正则曲面之间的映射的连续可微性与曲面的容许参数的选择是无关的。

下面假定映射$$\sigma: D_1 \rightarrow D_2$$是三次以上连续可微的。首先要指出，映射$$\sigma$$在每一点$$p \in S_1$$诱导出切空间$$T_p S_1$$到切空间$$T_{\sigma(p)} S_2$$的一个线性映射$$\sigma_{*p}: T_p S_1 \rightarrow T_{\sigma(p)} S_2$$，称此映射为由映射$$\sigma$$在$$p$$点的切空间$$T_p S_1$$上诱导的**切映射**。实际上，若有曲面$$S_1$$上一条连续可微曲线

$$
C_1: u_1 = u_1(t), \ \ v_1 = v_1(t) \ \ \ (t_0 - \epsilon \lt t \lt t_0 + \epsilon)
$$

则它在映射$$\sigma$$下映为曲面$$S_2$$上的一条连续可微曲线

$$
C_2: u_2 = u_2(t) = f(u_1(t), v_1(t)), \ \ v_2 = v_2(t) = g(u_1(t), v_1(t)) \ \ \
(t_0 - \epsilon \lt t \lt t_0 + \epsilon)
$$

那么曲线$$C_2$$的切向量是

$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d}t} \boldsymbol{r}_2 (u_2(t), v_2(t)) &= \frac{\partial \boldsymbol{r}_2}{\partial u_2} \frac{\mathrm{d} u_2(t)}{\mathrm{d} t} + \frac{\partial \boldsymbol{r}_2}{\partial v_2} \frac{\mathrm{d} v_2(t)}{\mathrm{d} t} \\
&= \frac{\partial \boldsymbol{r}_2}{\partial u_2} \bigg( \frac{\partial f(u_1, v_1)}{\partial u_1} \frac{\mathrm{d} u_1(t)}{\mathrm{d}t} + \frac{\partial f(u_1, v_1)}{\partial v_1} \frac{\mathrm{d} v_1(t)}{\mathrm{d}t} \bigg) \\
&+ \frac{\partial \boldsymbol{r}_2}{\partial v_2} \bigg( \frac{\partial g(u_1, v_1)}{\partial u_1} \frac{\mathrm{d} u_1(t)}{\mathrm{d}t} + \frac{\partial g(u_1, v_1)}{\partial v_1} \frac{\mathrm{d} v_1(t)}{\mathrm{d}t} \bigg)
\end{aligned}
$$

假定$$p = \boldsymbol{r}_1 (u_1(t_0), v_1(t_0))$$，命

$$
\sigma_{*p} \bigg( \frac{\mathrm{d}}{\mathrm{d} t} \boldsymbol{r}_1 (u_1(t), v_1(t)) \bigg\vert_{t=t_0} \bigg) = \frac{\mathrm{d}}{\mathrm{d}t} \boldsymbol{r}_2 (u_2(t), v_2(t)) \bigg\vert_{t = t_0}
$$

则$$C_2$$的切向量可以重新表示为

$$
\begin{aligned}
\sigma_{*p} \bigg( \frac{\partial \boldsymbol{r}_1}{\partial u_1} \frac{\mathrm{d} u_1(t)}{\mathrm{d} t} + \frac{\partial \boldsymbol{r}_1}{\partial v_1} \frac{\mathrm{d} v_1(t)}{\mathrm{d} t} \bigg) &=
\bigg( \frac{\partial \boldsymbol{r}_2}{\partial u_2} \frac{\partial f(u_1, v_1)}{\partial u_1} + \frac{\partial \boldsymbol{r}_2}{\partial v_2} \frac{\partial g(u_1, v_1)}{\partial u_1} \bigg) \frac{\mathrm{d} u_1(t)}{\mathrm{d}t} \\
&+ \bigg( \frac{\partial \boldsymbol{r}_2}{\partial u_2} \frac{\partial f(u_1, v_1)}{\partial v_1} + \frac{\partial \boldsymbol{r}_2}{\partial v_2} \frac{\partial g(u_1, v_1)}{\partial v_1} \bigg) \frac{\mathrm{d} v_1(t)}{\mathrm{d}t}
\end{aligned}
$$

由此可见，切映射$$\sigma_{*p}$$是线性映射并且

$$
\begin{cases}
\begin{aligned}
\sigma_{*p} \bigg( \frac{\partial \boldsymbol{r}_1}{\partial u_1} \bigg) &= \frac{\partial \boldsymbol{r}_2}{\partial u_2} \frac{\partial f(u_1, v_1)}{\partial u_1} + \frac{\partial \boldsymbol{r}_2}{\partial v_2} \frac{\partial g(u_1, v_1)}{\partial u_1} \\
\sigma_{*p} \bigg( \frac{\partial \boldsymbol{r}_1}{\partial v_1} \bigg) &= \frac{\partial \boldsymbol{r}_2}{\partial u_2} \frac{\partial f(u_1, v_1)}{\partial v_1} + \frac{\partial \boldsymbol{r}_2}{\partial v_2} \frac{\partial g(u_1, v_1)}{\partial v_1} \\
\end{aligned}
\end{cases}
$$

用矩阵表示则是

$$
\sigma_{*p}
\begin{pmatrix}
\frac{\partial \boldsymbol{r}_1}{\partial u_1} \\ \frac{\partial \boldsymbol{r}_1}{\partial v_1}
\end{pmatrix}
=
\begin{pmatrix}
\frac{\partial f(u_1, v_1)}{\partial u_1} & \frac{\partial g(u_1, v_1)}{\partial u_1} \\
\frac{\partial f(u_1, v_1)}{\partial v_1} & \frac{\partial g(u_1, v_1)}{\partial v_1} \\
\end{pmatrix}
\begin{pmatrix}
\frac{\partial \boldsymbol{r}_2}{\partial u_2} \\ \frac{\partial \boldsymbol{r}_2}{\partial v_2}
\end{pmatrix}
$$

对于$$T_p S_1$$中的任意一个切向量

$$
\boldsymbol{X} = a \frac{\partial \boldsymbol{r}_1}{\partial u_1} + b \frac{\partial \boldsymbol{r}_1}{\partial v_1}
$$

则有

$$
\begin{aligned}
\sigma_{*p} (\boldsymbol{X}) &= a \bigg( \frac{\partial \boldsymbol{r}_2}{\partial u_2} \frac{\partial f(u_1, v_1)}{\partial u_1} + \frac{\partial \boldsymbol{r}_2}{\partial v_2} \frac{\partial g(u_1, v_1)}{\partial u_1} \bigg) \\
&+ b \bigg( \frac{\partial \boldsymbol{r}_2}{\partial u_2} \frac{\partial f(u_1, v_1)}{\partial v_1} + \frac{\partial \boldsymbol{r}_2}{\partial v_2} \frac{\partial g(u_1, v_1)}{\partial v_1} \bigg) \\
&= \bigg( a \frac{\partial u_2}{\partial u_1} + b \frac{\partial u_2}{\partial v_1} \bigg) \frac{\partial \boldsymbol{r}_2}{\partial u_2} + \bigg( a \frac{\partial v_2}{\partial u_1} + b \frac{\partial v_2}{\partial v_1} \bigg) \frac{\partial \boldsymbol{r}_2}{\partial v_2} \\
&=
\begin{pmatrix}
a & b
\end{pmatrix}
\begin{pmatrix}
\frac{\partial u_2}{\partial u_1} & \frac{\partial v_2}{\partial u_1} \\
\frac{\partial u_2}{\partial v_1} & \frac{\partial v_2}{\partial v_1} \\
\end{pmatrix}

\begin{pmatrix}
\frac{\partial \boldsymbol{r}_2}{\partial u_2} \\ 
\frac{\partial \boldsymbol{r}_2}{\partial u_2}
\end{pmatrix}
\end{aligned}
$$

由此可见，切映射$$\sigma_{*p}: T_p S_1 \rightarrow T_{\sigma(p)} S_2$$是同构的，当且仅当矩阵

$$
\begin{pmatrix}
\frac{\partial u_2}{\partial u_1} & \frac{\partial v_2}{\partial u_1} \\
\frac{\partial u_2}{\partial v_1} & \frac{\partial v_2}{\partial v_1} \\
\end{pmatrix}
=
\begin{pmatrix}
\frac{\partial f(u_1, v_1)}{\partial u_1} & \frac{\partial g(u_1, v_1)}{\partial u_1} \\
\frac{\partial f(u_1, v_1)}{\partial v_1} & \frac{\partial g(u_1, v_1)}{\partial v_1} \\
\end{pmatrix}
$$

非退化，即Jacobi行列式非零

$$
\frac{\partial (u_2, v_2)}{\partial (u_1, v_1)} \bigg\vert_p =
\begin{vmatrix}
\frac{\partial f(u_1, v_1)}{\partial u_1} & \frac{\partial g(u_1, v_1)}{\partial u_1} \\
\frac{\partial f(u_1, v_1)}{\partial v_1} & \frac{\partial g(u_1, v_1)}{\partial v_1} \\
\end{vmatrix}
\neq 0
$$

> #### 定理3.4
> 设$$\sigma: S_1 \rightarrow S_2$$是从正则参数曲面$$S_1$$到正则参数曲面$$S_2$$的3次以上的连续可微映射。如果在点$$p \in S_1$$，切映射$$\sigma_{*p}: T_p S_1 \rightarrow T_{\sigma(p)} S_2$$是切空间$$T_p S_1$$和$$T_{\sigma(p)} S_2$$之间的同构，则有点$$p$$在$$S_1$$中的邻域$$U_1$$和点$$\sigma(p)$$在$$S_2$$中的邻域$$U_2$$，以及相应的参数系$$(u_1, v_1)$$和$$(u_2, v_2)$$，使得$$\sigma(U_1) \subset U_2$$，并且映射$$\sigma \vert_{U_1}$$是由$$u_2 = u_1$$，$$v_2 = v_1$$给出的。换言之，适当地选取曲面$$S_1$$和$$S_2$$上的参数系之后，映射$$\sigma\vert_{U_1}$$是从参数域$$U_1$$到$$U_2$$的、有相同参数值的点之间的对应。使映射$$\sigma$$能够由$$u_2 = u_1$$，$$v_2 = v_1$$给出的参数系$$(u_1, v_1)$$和$$(u_2, v_2)$$称为在曲面$$S_1$$和$$S_2$$上关于映射$$\sigma$$的**适用参数系**。
{: .block-theorem }

**证明** 假定映射$$\sigma$$由

$$
\begin{cases}
\begin{aligned}
u_2 &= f(u_1, v_1) \\ v_2 &= g(u_1, v_1)
\end{aligned}
\end{cases}
$$

给出。由于在点$$p$$处有$$\frac{\partial (u_2, v_2)}{\partial (u_1, v_1)} \bigg\vert_p \neq 0$$，因此上面的式子可以看作曲面$$S_1$$在点$$p$$的某个邻域$$U_1$$上的容许参数变换，使得$$(u_2, v_2)$$成为曲面$$S_1$$在点$$p$$的某个邻域$$U_1$$内的参数系。在这样的参数系下，映射$$\sigma$$恰好是参数区域上的恒等映射。证毕∎

映射$$\sigma: S_1 \rightarrow S_2$$还能够把$$S_2$$上的二次微分式拉回到$$S_1$$上，成为$$S_1$$上的二次微分式。假定$$S_2$$上的一个二次微分式$$\varphi$$的表达式是

$$
\varphi = A(u_2, v_2) (\mathrm{d} u_2)^2 + 2B(u_2, v_2) \mathrm{d} u_2 \mathrm{d} v_2 + C(u_2, v_2) (\mathrm{d} v_2)^2
$$

则在映射$$\sigma: S_1 \rightarrow S_2$$下可以得到$$S_1$$上的一个二次微分式$$\sigma^* \varphi$$如下：

$$
\begin{aligned}
\sigma^* \varphi &= A(f(u_1, v_1), g(u_1, v_1)) \bigg( \frac{\partial f}{\partial u_1} \mathrm{d} u_1 + \frac{\partial f}{\partial v_1} \mathrm{d} v_1 \bigg)^2 \\
&+2B(f(u_1, v_1), g(u_1, v_1)) \bigg( \frac{\partial f}{\partial u_1} \mathrm{d} u_1 + \frac{\partial f}{\partial v_1} \mathrm{d} v_1 \bigg) \cdot \bigg( \frac{\partial g}{\partial u_1} \mathrm{d} u_1 + \frac{\partial g}{\partial v_1} \mathrm{d} v_1 \bigg) \\
&+ C(f(u_1, v_1), g(u_1, v_1)) \bigg( \frac{\partial g}{\partial u_1} \mathrm{d} u_1 + \frac{\partial g}{\partial v_1} \mathrm{d} v_1 \bigg)^2 \\
&= \tilde{A} (u_1, v_1) (\mathrm{d} u_1)^2 + 2\tilde{B}(u_1, v_1) \mathrm{d} u_1 \mathrm{d} v_2 + \tilde{C}(u_1, v_1) (\mathrm{d} v_1)^2
\end{aligned}
$$

用矩阵表示则是

$$
\begin{pmatrix}
\tilde{A} (u_1, v_1) & \tilde{B} (u_1, v_1) \\
\tilde{B} (u_1, v_1) & \tilde{C} (u_1, v_1) \\
\end{pmatrix}
=
J
\begin{pmatrix}
A (u_2, v_2) & B (u_2, v_2) \\
B (u_2, v_2) & C (u_2, v_2) \\
\end{pmatrix}
J^T
$$

并且

$$
\begin{aligned}
\sigma^* \varphi &= 
\begin{pmatrix}
\mathrm{d} u_1 & \mathrm{d} v_1
\end{pmatrix} 
\cdot
\begin{pmatrix}
\tilde{A} & \tilde{B} \\
\tilde{B} & \tilde{C} \\
\end{pmatrix}
\cdot
\begin{pmatrix}
\mathrm{d} u_1 \\ \mathrm{d} v_1
\end{pmatrix} \\
&=
\begin{pmatrix}
\mathrm{d} u_1 & \mathrm{d} v_1
\end{pmatrix} 
\cdot J \cdot
\begin{pmatrix}
A & B \\
B & C \\
\end{pmatrix}
\cdot J^T \cdot
\begin{pmatrix}
\mathrm{d} u_1 \\ \mathrm{d} v_1
\end{pmatrix}
\end{aligned}
$$

其中

$$
J = 
\begin{pmatrix}
\frac{\partial u_2}{\partial u_1} & \frac{\partial v_2}{\partial u_1} \\
\frac{\partial u_2}{\partial v_1} & \frac{\partial v_2}{\partial v_1} \\
\end{pmatrix}
=
\begin{pmatrix}
\frac{\partial f(u_1, v_1)}{\partial u_1} & \frac{\partial g(u_1, v_1)}{\partial u_1} \\
\frac{\partial f(u_1, v_1)}{\partial v_1} & \frac{\partial g(u_1, v_1)}{\partial v_1} \\
\end{pmatrix}
$$

我们把$$\sigma^* \varphi$$称为$$S_2$$上的二次微分式$$\varphi$$经过映射$$\sigma: S_1 \rightarrow S_2$$拉回到$$S_1$$上的二次微分式。

### 保长对应

> #### 定义3.3 
> 设$$\sigma: S_1 \rightarrow S_2$$是从正则参数曲面$$S_1$$到正则参数曲面$$S_2$$的3次以上的连续可微映射。如果在每一点$$p \in S_1$$，切映射$$\sigma_{*p}: T_p S_1 \rightarrow T_{\sigma (p)} S_2$$都保持切向量的长度不变，即对于任意的$$\boldsymbol{X} \in T_p S_1$$都有$$\vert \sigma_{*p} (\boldsymbol{X}) \vert = \vert \boldsymbol{X} \vert$$，则称$$\sigma: S_1 \rightarrow S_2$$是从曲面$$S_1$$到$$S_2$$的**保长对应**。
{: .block-definition }

向量之间的内积和向量长度之间的关系是

$$
\boldsymbol{a} \cdot \boldsymbol{b} = \frac{1}{2} (\vert \boldsymbol{a} + \boldsymbol{b} \vert^2 - \vert \boldsymbol{a} \vert^2 -\vert \boldsymbol{b} \vert^2)
$$

既然保长对应$$\sigma: S_1 \rightarrow S_2$$保持切向量的长度不变，它必定保持切向量的内积不变，即对于任意的$$\boldsymbol{X}, \boldsymbol{Y} \in T_p S_1$$，都有

$$
\sigma_{*p} (\boldsymbol{X}) \cdot \sigma_{*p} (\boldsymbol{Y}) = \boldsymbol{X} \cdot \boldsymbol{Y}
$$

在[前面](/2023/10/18/DifferentialGeometry-NOTES-03.html#正则参数曲面的微分)介绍过，曲面$$S_1$$在点$$\boldsymbol{r} = \boldsymbol{r}_1 (u_1, v_1)$$处的任意一个切向量可以用它的微分

$$
\mathrm{d} \boldsymbol{r}_1 (u_1, v_1) = \mathrm{d} u_1 \frac{\partial \boldsymbol{r}_1}{\partial u_1} + \mathrm{d} v_1 \frac{\partial \boldsymbol{r}_1}{\partial v_1}
$$

来表示，这里$$(\mathrm{d} u_1, \mathrm{d} v_1)$$代表曲面$$S_1$$在点$$\boldsymbol{r} = \boldsymbol{r}_1 (u_1, v_1)$$处的任意一个切向量的分量。根据切映射的定义有

$$
\begin{aligned}
\sigma_{*p} \big( \mathrm{d} \boldsymbol{r}_1 (u_1, v_1) \big) &= 
\sigma_{*p} \Bigg( 
\begin{pmatrix}
\mathrm{d} u_1 & \mathrm{d} v_1
\end{pmatrix}
\begin{pmatrix}
\frac{\partial \boldsymbol{r}_1}{\partial u_1} \\ 
\frac{\partial \boldsymbol{r}_1}{\partial v_1}
\end{pmatrix}
\Bigg) \\
&= 
\begin{pmatrix}
\mathrm{d} u_1 & \mathrm{d} v_1
\end{pmatrix}
\sigma_{*p}
\begin{pmatrix}
\frac{\partial \boldsymbol{r}_1}{\partial u_1} \\ 
\frac{\partial \boldsymbol{r}_1}{\partial v_1}
\end{pmatrix} \\
&= 
\begin{pmatrix}
\mathrm{d} u_1 & \mathrm{d} v_1
\end{pmatrix}
J
\begin{pmatrix}
\frac{\partial \boldsymbol{r}_2}{\partial u_2} \\ 
\frac{\partial \boldsymbol{r}_2}{\partial v_2}
\end{pmatrix}
\end{aligned}
$$

其中

$$
J = \begin{pmatrix}
\frac{\partial u_2}{\partial u_1} & \frac{\partial v_2}{\partial u_1} \\
\frac{\partial u_2}{\partial v_1} & \frac{\partial v_2}{\partial v_1} \\
\end{pmatrix}
$$

因此，$$\sigma$$是保长对应的条件是

$$
\begin{aligned}
\vert \mathrm{d} \boldsymbol{r}_1 (u_1, v_1) \vert^2 &= \vert \sigma_{*p} ( \mathrm{d} \boldsymbol{r}_1 (u_1, v_1) ) \vert^2 \\
&= 
\begin{pmatrix}
\mathrm{d} u_1 & \mathrm{d} v_1
\end{pmatrix}
J
\begin{pmatrix}
\frac{\partial \boldsymbol{r}_2}{\partial u_2} \\ 
\frac{\partial \boldsymbol{r}_2}{\partial v_2}
\end{pmatrix}
\cdot
\begin{pmatrix}
\frac{\partial \boldsymbol{r}_2}{\partial u_2} & \frac{\partial \boldsymbol{r}_2}{\partial v_2}
\end{pmatrix}
J^T
\begin{pmatrix}
\mathrm{d} u_1 \\ \mathrm{d} v_1
\end{pmatrix} \\
&= \begin{pmatrix}
\mathrm{d} u_1 & \mathrm{d} v_1
\end{pmatrix}
J
\begin{pmatrix}
E_2(u_2, v_2) & F_2(u_2, v_2) \\
F_2(u_2, v_2) & G_2(u_2, v_2) \\
\end{pmatrix}
J^T
\begin{pmatrix}
\mathrm{d} u_1 \\ \mathrm{d} v_1
\end{pmatrix} \\
&= \sigma^* (\vert \mathrm{d} \boldsymbol{r}_2 (u_2, v_2) \vert^2)
\end{aligned}
$$

即

$$
\mathrm{I}_1 = \sigma^* \mathrm{I}_2
$$

这里$$E_2$$，$$F_2$$，$$G_2$$是曲面$$S_2$$的第一类基本量。由此得到下面的定理：

> #### 定理3.5
> 假定正则参数曲面$$S_1$$和$$S_2$$的第一基本形式分别是$$\mathrm{I}_1$$和$$\mathrm{I}_2$$，则$$\sigma: S_1 \rightarrow S_2$$是从曲面$$S_1$$到曲面$$S_2$$的保长对应的充分必要条件是$$\mathrm{I}_1 = \sigma^* \mathrm{I}_2$$，换言之
$$
\begin{pmatrix}
E_1 (u_1, v_1) & F_1 (u_1, v_1) \\
F_1 (u_1, v_1) & G_1 (u_1, v_1) \\
\end{pmatrix}
= J
\begin{pmatrix}
E_2 (u_2, v_2) & F_2 (u_2, v_2) \\
F_2 (u_2, v_2) & G_2 (u_2, v_2) \\
\end{pmatrix}
J^T
$$
其中
$$
J = \begin{pmatrix}
\frac{\partial u_2}{\partial u_1} & \frac{\partial v_2}{\partial u_1} \\
\frac{\partial u_2}{\partial v_1} & \frac{\partial v_2}{\partial v_1} \\
\end{pmatrix}
$$
。
{: .block-theorem }

如果将上面的式子展开，我们便得到下面的等式

$$
\begin{aligned}
E_2 (u_2, v_2) \bigg( \frac{\partial u_2}{\partial u_1} \bigg)^2 + 2F_2 (u_2, v_2) \frac{\partial u_2}{\partial u_1} \frac{\partial v_2}{\partial u_1} + G_2 (u_2, v_2) \bigg( \frac{\partial v_2}{\partial u_1} \bigg)^2 &= E_1 (u_1, v_1) \\
E_2 (u_2, v_2)  \frac{\partial u_2}{\partial u_1} \frac{\partial u_2}{\partial v_1} + 2F_2 (u_2, v_2) \bigg( \frac{\partial u_2}{\partial u_1} \frac{\partial v_2}{\partial u_1} + \frac{\partial u_2}{\partial v_1} \frac{\partial v_2}{\partial u_1} \bigg) + G_2 (u_2, v_2) \frac{\partial v_2}{\partial u_1} \frac{\partial v_2}{\partial v_1} &= F_1 (u_1, v_1) \\
E_2 (u_2, v_2) \bigg( \frac{\partial u_2}{\partial v_1} \bigg)^2 + 2F_2 (u_2, v_2) \frac{\partial u_2}{\partial v_1} \frac{\partial v_2}{\partial v_1} + G_2 (u_2, v_2) \bigg( \frac{\partial v_2}{\partial v_1} \bigg)^2 &= G_1 (u_1, v_1) \\
\end{aligned}
$$

在已知正则曲面$$S_1$$和$$S_2$$的第一基本形式的情况下，上式实际上是寻求曲面$$S_1$$和$$S_2$$之间是否存在保持对应$$u_2 = u_2(u_1, v_1)$$，$$v_2 = v_2(u_1, v_1)$$的微分方程组。但是这是一个非线性的一阶微分方程组，要判断该方程组是否有解并把解求出来是一件十分困难的事情。后面我们要进一步研究保持对应的不变量，这对我们判断两个正则曲面是否能够建立保长对应起着重要的作用。

> #### 定理3.6
> 在正则曲面$$S_1$$和$$S_2$$之间存在保长对应的充分必要条件是，能够在曲面$$S_1$$和$$S_2$$上取适当的参数系，都记成$$(u, v)$$，并且在这样的参数系下两个曲面有相同的第一基本量，即$$E_1(u, v) = E_2(u, v)$$，$$F_1(u, v) = F_2(u, v)$$，$$G_1(u, v) = G_2(u, v)$$。
{: .block-theorem }

### 保角对应

> #### 定义3.4
> 设$$\sigma: S_1 \rightarrow S_2$$是从正则参数曲面$$S_1$$到正则参数曲面$$S_2$$的意义对应，并且它和它的逆映射都是3次以上的连续可微映射。如果在每一点$$p \in S_1$$，切映射$$\sigma_{*p}: T_p S_1 \rightarrow T_{\sigma (p)} S_2$$都保持切向量的夹角不变，即对于任意的$$\boldsymbol{X}, \boldsymbol{Y} \in T_p S_1$$都有$$\angle (\sigma_{*p} (\boldsymbol{X}), \sigma_{*p} (\boldsymbol{Y})) = \angle (\boldsymbol{X}, \boldsymbol{Y})$$，则称$$\sigma: S_1 \rightarrow S_2$$是从曲面$$S_1$$到$$S_2$$的**保角对应**。
{: .block-definition }

在初等平面几何学中，所谓的相似三角形是指对应边成比例的三角形，然而相似三角形的特征是对应角相等。这就是说，判断两个三角形的内角对应相等的问题可以转化为对应的三边边长是否成比例的问题。下面的定理实际上就体现了这个原理。

> #### 定理3.7
> 假定正则参数曲面$$S_1$$和$$S_2$$的第一基本形式分别是$$\mathrm{I}_1$$和$$\mathrm{I}_2$$，则$$\sigma: S_1 \rightarrow S_2$$是从曲面$$S_1$$到曲面$$S_2$$的保角对应的充分必要条件是，在曲面$$S_1$$上存在正的连续函数$$\lambda$$，使得$$\sigma^* \mathrm{I}_2 = \lambda^2 \mathrm{I}_1$$。特别地，如果$$\sigma: S_1 \rightarrow S_2$$是从曲面$$S_1$$到曲面$$S_2$$的保角对应，则在曲面$$S_1$$和$$S_2$$上能够取适当的参数系，都记成$$(u, v)$$使得在这样的参数系下这两个曲面的第一类基本量成比例，即存在正的连续函数$$\lambda$$，使得$$E_2(u, v) = \lambda^2 (u, v) E_1(u, v)$$，$$F_2(u, v) = \lambda^2 (u, v) F_1(u, v)$$，$$G_2(u, v) = \lambda^2 (u, v) G_1(u, v)$$。
{: .block-theorem }

**证明** 充分性是明显的。如果$$\sigma^* \mathrm{I}_2 = \lambda^2 \mathrm{I}_1$$成立，则对于任意的$$p \in S_1$$，$$\boldsymbol{X}, \boldsymbol{Y} \in T_p S_1$$都有

$$
\begin{aligned}
\cos{\angle (\boldsymbol{X}, \boldsymbol{Y})} &= \frac{\boldsymbol{X} \cdot \boldsymbol{Y}}{\vert \boldsymbol{X} \vert \vert \boldsymbol{Y} \vert} = \frac{\mathrm{I}_1 (\boldsymbol{X}, \boldsymbol{Y})}{\sqrt{\mathrm{I}_1 (\boldsymbol{X}, \boldsymbol{X})} \sqrt{\mathrm{I}_1 (\boldsymbol{Y}, \boldsymbol{Y})}} \\
\cos{\angle ( \sigma_{*p} (\boldsymbol{X}), \sigma_{*p} (\boldsymbol{Y}) )} &= \frac{\mathrm{I}_2 (\sigma_{*p} (\boldsymbol{X}), \sigma_{*p} (\boldsymbol{Y}))}{\sqrt{\mathrm{I}_2 (\sigma_{*p} (\boldsymbol{X}), \sigma_{*p} (\boldsymbol{X}))} \sqrt{\mathrm{I}_2 (\sigma_{*p} (\boldsymbol{Y}), \sigma_{*p} (\boldsymbol{Y}))}} \\
&= \frac{\sigma^* \mathrm{I}_2 (\boldsymbol{X}, \boldsymbol{Y})}{\sqrt{\sigma^* \mathrm{I}_2 (\boldsymbol{X}, \boldsymbol{X})} \sqrt{\sigma^* \mathrm{I}_2 (\boldsymbol{Y}, \boldsymbol{Y})}} \\
&= \frac{\lambda^2 \mathrm{I}_1 (\boldsymbol{X}, \boldsymbol{Y})}{\sqrt{\lambda^2 \mathrm{I}_1 (\boldsymbol{X}, \boldsymbol{X})} \sqrt{\lambda^2 \mathrm{I}_1 (\boldsymbol{Y}, \boldsymbol{Y})}} \\
&= \cos{\angle (\boldsymbol{X}, \boldsymbol{Y})}
\end{aligned}
$$

反过来，假定$$\sigma: S_1 \rightarrow S_2$$是从曲面$$S_1$$到曲面$$S_2$$的保角对应，则根据定义$$\sigma$$必定是一一对应。故根据[定理3.4](/2023/10/18/DifferentialGeometry-NOTES-03.html#正则参数曲面之间的对应)在曲面$$S_1$$和$$S_2$$上能够取适用参数系，都记成$$(u, v)$$，使得在这样的参数系下$$\sigma$$是有相同参数值的点之间的对应，即

$$
\sigma(\boldsymbol{r}_1 (u, v)) = \boldsymbol{r}_2 (u, v)
$$

其中$$\boldsymbol{r}_1 (u, v)$$，$$\boldsymbol{r}_2 (u, v)$$分别是曲面$$S_1$$，$$S_2$$的参数方程。因此

$$
\sigma_* \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u} \bigg) = \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial u}, \ \ \
\sigma_* \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v} \bigg) = \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial v}
$$

并且

$$
\sigma_* \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u} + \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v} \bigg) = \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial u} + \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial v}
$$

根据切向量夹角不变的条件有

$$
\begin{aligned}
\cos{\angle \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u} , \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v} \bigg)} &= \cos{\angle \bigg( \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial u} , \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial v} \bigg)} \\
\cos{\angle \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u} + \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v}, \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u} \bigg)} &= \cos{\angle \bigg( \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial u} + \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial v}, \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u} \bigg)} \\
\cos{\angle \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u} + \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v}, \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v} \bigg)} &= \cos{\angle \bigg( \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial u} + \frac{\partial \boldsymbol{r}_2 (u, v)}{\partial v}, \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v} \bigg)}
\end{aligned}
$$

带入第一类基本量有

$$
\begin{aligned}
\frac{F_1}{\sqrt{E_1 G_1}} &= \frac{F_2}{\sqrt{E_2 G_2}} \\
\frac{E_1 + F_1}{\sqrt{E_1 + 2F_1 + G_1} \sqrt{E_1}} &= \frac{E_2 + F_2}{\sqrt{E_2 + 2F_2 + G_2} \sqrt{E_2}} \\
\frac{E_1 + F_1}{\sqrt{E_1 + 2F_1 + G_1} \sqrt{G_1}} &= \frac{E_2 + F_2}{\sqrt{E_2 + 2F_2 + G_2} \sqrt{G_2}}
\end{aligned}
$$

将上面的后两个式子相除得到

$$
\frac{\sqrt{E_1} + \frac{F_1}{\sqrt{E_1}}}{\sqrt{G_1} + \frac{F_1}{\sqrt{G_1}}} = \frac{\sqrt{E_2} + \frac{F_2}{\sqrt{E_2}}}{\sqrt{G_2} + \frac{F_2}{\sqrt{G_2}}}
$$

将上式展开并结合关系式$$\frac{F_1}{\sqrt{E_1 G_1}} = \frac{F_2}{\sqrt{E_2 G_2}}$$得到

$$
\sqrt{E_1 G_2} + \frac{F_1 F_2}{\sqrt{E_1 G_2}} = \sqrt{E_2 G_1} + \frac{F_1 F_2}{\sqrt{E_2 G_1}}
$$

$$
\begin{aligned}
(\sqrt{E_1 G_2} - \sqrt{E_2 G_1}) \bigg( 1 -\frac{F_1}{\sqrt{E_1 G_1}} \frac{F_2}{\sqrt{E_2 G_2}} \bigg) &= 
(\sqrt{E_1 G_2} - \sqrt{E_2 G_1}) \bigg( 1 -\frac{F_1^2}{E_1 G_1} \bigg) \\
&= (\sqrt{E_1 G_2} - \sqrt{E_2 G_1}) \bigg( 1 - \cos^2{\angle \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u}, \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v} \bigg)} \bigg) \\
&= 0
\end{aligned}
$$

由于$$S_1$$是正则参数曲面，切向量$$\frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u}$$，$$\frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v}$$不共线，故

$$
\cos^2{\angle \bigg( \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial u}, \frac{\partial \boldsymbol{r}_1 (u, v)}{\partial v}} \bigg) \neq 1
$$

所以

$$
E_1 G_2 = E_2 G_1
$$

带入关系式$$\frac{F_1}{\sqrt{E_1 G_1}} = \frac{F_2}{\sqrt{E_2 G_2}}$$则得到

$$
\frac{F_2}{F_1} = \frac{E_2}{E_1} = \frac{G_2}{G_1} = \lambda^2
$$

证毕∎

关于保角对应有下面的**重要定理**：

> #### 定理3.8
> 任意一个正则参数曲面$$S$$的每一点$$p$$，都有一个邻域$$U$$可以和平面上的一个开区域建立保角对应。换言之，任意两个正则参数曲面在局部上都可以建立保角对应。
{: .block-theorem }

这是一个十分深刻的定理。在曲面的参数方程是解析的情形，首先是Gauss凭借着把实解析函数看作复解析函数的技巧，利用两个变量的一次微分形式的积分因子的存在性证明了这个定理。当曲面的参数方程是光滑的情形，证明比较复杂。另外，当曲面的参数方程有2阶以上的连续可微性时，定理仍然成立。下面我们在曲面的参数方程是解析函数的假定下，给出定理的简要证明。

**证明** 假定正则参数曲面$$S$$的方程$$\boldsymbol{r} = \boldsymbol{r} (u, v)$$是$$u$$，$$v$$的解析函数，于是曲面的第一类基本量$$E$$，$$F$$，$$G$$都是$$u$$，$$v$$的解析函数。根据[定理3.3](/2023/10/18/DifferentialGeometry-NOTES-03.html#曲面上正交参数曲线网的存在性)，可以假定$$(u, v)$$给出曲面$$S$$的正交参数曲线网，即$$F(u, v) = 0$$，于是曲面$$S$$的第一基本形式成为

$$
\begin{aligned}
\mathrm{I} &= E (\mathrm{d} u)^2 + G (\mathrm{d} v)^2 \\
&= (\sqrt{E} \mathrm{d} u + \sqrt{-1} \sqrt{G} \mathrm{d} v) (\sqrt{E} \mathrm{d} u - \sqrt{-1} \sqrt{G} \mathrm{d} v)
\end{aligned}
$$

命

$$
\omega = \sqrt{E} \mathrm{d} u + \sqrt{-1} \sqrt{G} \mathrm{d} v
$$

这是系数为复数值解析函数的一次微分式。利用系数为复数值解析函数的一次微分式的积分因子的存在性，在每一点$$p \in S$$的一个充分小的邻域$$U$$内存在处处非零的复数值解析函数$$\lambda (u, v)$$，使得$$\lambda (u, v) \omega$$成为某个复数值函数$$z(u, v)$$的全微分，即

$$
\lambda (u, v) (\sqrt{E} \mathrm{d} u + \sqrt{-1} \sqrt{G} \mathrm{d} v) = \mathrm{d} z (u, v)
$$

把函数$$z(u, v)$$分解为实部和虚部，设

$$
z(u, v) = x(u, v) + \sqrt{-1} y(u, v)
$$

则

$$
\mathrm{I} = \omega \bar{\omega} = \frac{1}{\vert \lambda \vert^2} ((\mathrm{d} x)^2 + (\mathrm{d} y)^2)
$$

因此曲面$$S$$上点$$p$$的一个邻域与平面上的一个邻域是保角的，保角对应由

$$
\begin{cases}
\begin{aligned}
x &= x(u, v) \\ y &= y(u, v)
\end{aligned}
\end{cases}
$$

给出。证毕∎

在曲面$$S$$上能够使第一基本形式表示成$$\mathrm{I} = \frac{1}{\vert \lambda \vert^2} ((\mathrm{d} x)^2 + (\mathrm{d} y)^2)$$的参数系$$(x, y)$$称为**等温参数系**。在曲面$$S$$上存在等温参数系是一个十分重要的事实，它在实践中有很多应用。比如说绘制地图时常用的[Mercator投影](https://en.wikipedia.org/wiki/Mercator_projection)就是球面到圆柱面上的等温参数系，这里简单推导一下：

设$$S$$是半径为$$a$$的球面，$$\tilde{S}$$是半径为$$\tilde{a}$$的圆柱面，它们的参数方程分别为

$$
\boldsymbol{r} = (a \cos{v} \cos{u}, a \cos{v} \sin{u}, a \sin{v})
$$

$$
\tilde{\boldsymbol{r}} = (a \cos{\tilde{u}}, a \sin{\tilde{v}}, a \tilde{v})
$$

直接计算可以得到它们的第一基本形式分别为

$$
\begin{aligned}
\mathrm{I} &= a^2 \cos^2 {v} (\mathrm{d} u)^2 + a^2 (\mathrm{d} v)^2 \\
&= a^2 \cos^2 {v} \bigg( (\mathrm{d} u)^2 + \frac{1}{\cos^2 {v}} (\mathrm{d} v)^2 \bigg)
\end{aligned}
$$

$$
\begin{aligned}
\mathrm{\tilde{I}} &= a^2 (\mathrm{d} \tilde{u})^2 + a^2 (\mathrm{d} \tilde{v})^2 \\
&= a^2 ((\mathrm{d} \tilde{u})^2 + (\mathrm{d} \tilde{v})^2)
\end{aligned}
$$

命

$$
\begin{cases}
\begin{aligned}
\tilde{u} &= u \\
\tilde{v} &= \int_0^v \frac{\mathrm{d} v}{\cos{v}} = \log{\bigg\vert \tan \bigg(\frac{v}{2}+\frac{\pi}{4} \bigg) \bigg\vert}
\end{aligned}
\end{cases}
$$

则上面给出的映射$$\sigma: S \rightarrow \tilde{S}$$是保角对应，称为Mercator投影。由于圆柱面$$\tilde{S}$$和平面是保长的，因此球面可以通过Mercator投影和平面建立保角对应。

## 可展曲面

### 直纹面

直纹面是指一条直线在空间中运动所产生的曲面，或者说直纹面是单参数直线族所形成的曲面。由于确定一条直线的方式通常是指定它所经过的一个点以及它的方向向量，因此确定直纹面要有两个要素：一条曲线$$\boldsymbol{r} = \boldsymbol{a} (u)$$；这是动直线上的一个固定点在时刻$$u$$的位置向量；非零向量函数$$\boldsymbol{l}(u)$$，它是动直线在时刻$$u$$的方向向量。所以该直线在时刻$$u$$经过点$$\boldsymbol{a} (u)$$以$$\boldsymbol{l}(u)$$为方向向量，故它的参数方程是

$$
\boldsymbol{r} = \boldsymbol{r} (u, v) = \boldsymbol{a} (u) + v \boldsymbol{l} (u)
$$

上式作为$$u$$，$$v$$的二元函数，恰好是直纹面的参数方程。曲线$$\boldsymbol{r} = \boldsymbol{a} (u)$$称为直纹面的**准线**，而$$v$$-曲线称为直纹面的**直母线**。因为

$$
\begin{cases}
\boldsymbol{r}_u (u, v) &= \boldsymbol{a}' (u) + v \boldsymbol{l}' (u) \\
\boldsymbol{r}_v (u, v) &= \boldsymbol{l} (u)
\end{cases}
$$

所以参数曲面是正则参数曲面的充分必要条件是

$$
\boldsymbol{r}_u \times \boldsymbol{r}_v = (\boldsymbol{a}' (u) + v \boldsymbol{l}' (u)) \times \boldsymbol{l} (u) \neq \boldsymbol{0}
$$

即$$\boldsymbol{a}' (u) \times \boldsymbol{l} (u)$$和$$\boldsymbol{l}' (u) \times \boldsymbol{l} (u)$$不能同时为零。我们常见的柱面、锥面和一条空间曲线的切线面都是特殊的直纹面，它们的参数方程分别是

- 柱面：$$\boldsymbol{r} = \boldsymbol{a} (u) + v \boldsymbol{l}$$，其中$$\boldsymbol{l}$$是非零常向量(图(a))
- 锥面：$$\boldsymbol{r} = \boldsymbol{a} + v \boldsymbol{l} (u)$$，其中$$\boldsymbol{a}$$是常向量(图(b))
- 切线面：$$\boldsymbol{r} = \boldsymbol{a} (u) + v \boldsymbol{a}' (u)$$ (图(c))

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XIg1dkI.png" width="80%">
</div>

通过直接计算知道，这些曲面的一个共同特征是：当点沿着直母线运动时曲面的切平面是保持不变的，即这种曲面的切平面构成依赖单个参数的平面族。以切线面为例，根据其参数方程可以得到

$$
\begin{cases}
\begin{aligned}
\boldsymbol{r}_u &= \boldsymbol{a}' (u) + v \boldsymbol{a}'' (u) \\
\boldsymbol{r}_v &= \boldsymbol{a}' (u) \\
\end{aligned}
\end{cases}
$$

所以

$$
\boldsymbol{r}_u \times \boldsymbol{r}_v = -v \boldsymbol{a}' (u) \times \boldsymbol{a}'' (u)
$$

$$
\boldsymbol{n} = \pm \frac{\boldsymbol{a}' (u) \times \boldsymbol{a}'' (u)}{\vert \boldsymbol{a}' (u) \times \boldsymbol{a}'' (u) \vert}
$$

因为切线面的单位法向量场$$\boldsymbol{n}$$只依赖参数$$u$$，故切线面的法向量沿着直母线($$u=$$常数)保持不变，而且直母线又落在切线面的切平面上，因此当点沿着直母线运动时切线面的切平面是保持不变的。

### 可展曲面

> #### 定义3.5
> 设$$S$$是直纹面，如果曲面$$S$$的切平面沿每一条直母线是不变的，则称该直纹面是**可展曲面**。
{: .block-definition }

前面的讨论说明，柱面、锥面和一条空间曲线的切线面都是可展曲面。下面的命题给出了一个直纹面是可展曲面的条件。

> #### 定理3.9
> 设直纹面$$S$$的参数方程是$$\boldsymbol{r} = \boldsymbol{a} (u) + v \boldsymbol{l} (u)$$，则$$S$$是可展曲面的充分必要条件是向量函数$$\boldsymbol{a} (u)$$，$$\boldsymbol{l} (u)$$满足方程
$$
\big( \boldsymbol{a}' (u), \boldsymbol{l} (u), \boldsymbol{l}' (u) \big) = 0
$$
{: .block-theorem }

**证明** 对直纹面$$S$$的参数方程求导数得到

$$
\begin{cases}
\begin{aligned}
\boldsymbol{r}_u (u, v) &= \boldsymbol{a}' (u) + v \boldsymbol{l}' (u) \\
\boldsymbol{r}_v (u, v) &= \boldsymbol{l} (u)
\end{aligned}
\end{cases}
$$

因此曲面$$S$$的法向量是

$$
\boldsymbol{r}_u \times \boldsymbol{r}_v = (\boldsymbol{a}' (u) + v \boldsymbol{l}' (u)) \times \boldsymbol{l} (u)
$$

如果$$S$$是可展曲面，则在直母线上的任意两个不同点$$(u, v_1)$$和$$(u, v_2)$$，其中$$v_1 \neq v_2$$，曲面$$S$$的法向量应该互相平行，即

$$
(\boldsymbol{a}' (u) + v_1 \boldsymbol{l}' (u)) \times \boldsymbol{l} (u) \parallel (\boldsymbol{a}' (u) + v_2 \boldsymbol{l}' (u)) \times \boldsymbol{l} (u)
$$

根据[向量三重积公式](https://en.wikipedia.org/wiki/Triple_product#Vector_triple_product)

$$
(\boldsymbol{a} \times \boldsymbol{b}) \times \boldsymbol{c} = (\boldsymbol{a} \cdot \boldsymbol{c}) \boldsymbol{b} -  (\boldsymbol{b} \cdot \boldsymbol{c}) \boldsymbol{a}
$$

我们有

$$
\begin{aligned}
\boldsymbol{0} &= \big( (\boldsymbol{a}' (u) + v_1 \boldsymbol{l}' (u)) \times \boldsymbol{l} (u) \big) \times \big( (\boldsymbol{a}' (u) + v_2 \boldsymbol{l}' (u)) \times \boldsymbol{l} (u) \big) \\
&= \big( \boldsymbol{a}' (u) + v_1 \boldsymbol{l}' (u) \big) \cdot \big( (\boldsymbol{a}' (u) + v_2 \boldsymbol{l}' (u)) \times \boldsymbol{l} (u) \big) \boldsymbol{l} (u) \\
&= \big( \boldsymbol{a}' (u) + v_1 \boldsymbol{l}' (u), \boldsymbol{a}' (u) + v_2 \boldsymbol{l}' (u), \boldsymbol{l} (u) \big) \boldsymbol{l} (u) \\
&= (v_1 - v_2) \big( \boldsymbol{a}' (u), \boldsymbol{l} (u), \boldsymbol{l}' (u) \big) \boldsymbol{l} (u)
\end{aligned}
$$

由于$$(v_1 - v_2) \boldsymbol{l} (u) \neq \boldsymbol{0}$$，所以上式末端的混合积必为零。上面的论证过程是可逆的，因此$$\big( \boldsymbol{a}' (u), \boldsymbol{l} (u), \boldsymbol{l}' (u) \big) = 0$$也是直纹面$$S$$为可展曲面的充分条件。证毕∎

在直纹面上可能会有两个不同的连续单参数直线族，那么是否会出现这样的情况：对其中一个连续单参数直线族混合积为零，而对另一个连续单参数直线族不为零？结论是否定的，这种情况不会出现。原因是存在两个不同的连续单参数直线族的曲面只有单叶双曲面、双曲抛物面和平面这三种情况。直接验证可知，前两者都不是可展曲面。因此，要判断一个直纹面是否为可展曲面，只要就直纹面的一种参数表示进行检验就可以了。

> #### 定理3.10
> 可展曲面在局部上是柱面、锥面和一条空间曲线的切线面，或者是用这三种曲面以充分连续可微的方式沿直母线拼接的结果。
{: .block-theorem }

**证明** 设$$S$$是可展曲面，它的参数方程是$$\boldsymbol{r} = \boldsymbol{a} (u) + v \boldsymbol{l} (u)$$并且满足

$$
\big( \boldsymbol{a}' (u), \boldsymbol{l} (u), \boldsymbol{l}' (u) \big) = 0
$$

如果$$\boldsymbol{l} (u) \times \boldsymbol{l}' (u) \equiv \boldsymbol{0}$$，则根据[定理1.3](/2023/07/30/DifferentialGeometry-NOTES-01.html#向量函数)，向量$$\boldsymbol{l} (u)$$有确定的方向。因此直母线互相平行，$$S$$是一个柱面。

当$$\boldsymbol{l} (u) \times \boldsymbol{l}' (u) \not\equiv \boldsymbol{0}$$时，可以假设在$$u$$的一个小区间内$$\boldsymbol{l} (u) \times \boldsymbol{l}' (u)$$恒不为零，于是$$\boldsymbol{l} (u)$$和$$\boldsymbol{l}' (u)$$在该区间内处处线性无关。那么条件$$\big( \boldsymbol{a}' (u), \boldsymbol{l} (u), \boldsymbol{l}' (u) \big) = 0$$意味着向量$$\boldsymbol{a}' (u)$$和$$\boldsymbol{l} (u)$$，$$\boldsymbol{l}' (u)$$共面，因而是它们的线性组合。不妨设为

$$
\boldsymbol{a}' (u) = \alpha(u) \boldsymbol{l} (u) + \beta(u) \boldsymbol{l}' (u)
$$

现在让准线做变换

$$
\boldsymbol{b} (u) = \boldsymbol{a} (u) + \lambda (u) \boldsymbol{l} (u)
$$

要求$$\boldsymbol{b}' (u) \parallel \boldsymbol{l} (u)$$。对$$\boldsymbol{b} (u)$$求导并带入$$\boldsymbol{a}' (u)$$得到

$$
\begin{aligned}
\boldsymbol{b}' (u) &= \boldsymbol{a}' (u) + \lambda' (u) \boldsymbol{l} (u) + \lambda (u) \boldsymbol{l}' (u) \\
&= (\alpha (u) + \lambda' (u)) \boldsymbol{l} (u) + (\beta (u) + \lambda (u)) \boldsymbol{l}' (u)
\end{aligned}
$$

取$$\lambda (u) = -\beta (u)$$，则

$$
\begin{cases}
\begin{aligned}
\boldsymbol{b} (u) &= \boldsymbol{a} (u) - \beta (u) \boldsymbol{l} (u) \\
\boldsymbol{b}' (u) &= (\boldsymbol{a} (u) - \beta' (u)) \boldsymbol{l} (u)
\end{aligned}
\end{cases}
$$

如果$$\alpha (u) - \beta' (u) \equiv 0$$，则$$\boldsymbol{b}' (u) \equiv \boldsymbol{0}$$，于是$$\boldsymbol{b} (u) = \boldsymbol{b}_0$$是常向量。此时

$$
\boldsymbol{a} (u) - \beta (u) \boldsymbol{l} (u) = \boldsymbol{b}_0
$$

即直纹面$$S$$的直母线都通过一个定点$$\boldsymbol{b}_0$$，所以该直纹面是锥面

$$
\boldsymbol{r} = \boldsymbol{b}_0 + (v + \beta (u)) \boldsymbol{l} (u)
$$

当$$\alpha (u) - \beta' (u) \not\equiv 0$$时，不妨设在$$u$$的一个小区间内$$\alpha (u) - \beta' (u)$$恒不为零，故$$\boldsymbol{r} = \boldsymbol{b} (u)$$是一条正则曲线。此时，曲面$$S$$的参数方程成为

$$
\begin{aligned}
\boldsymbol{r} &= \boldsymbol{b} (u) + (v + \beta (u)) \boldsymbol{l} (u) \\
&= \boldsymbol{b} (u) + \frac{v + \beta (u)}{\alpha (u) - \beta' (u)} \boldsymbol{b}' (u)
\end{aligned}
$$

这说明曲面$$S$$是正则参数曲线$$\boldsymbol{r} = \boldsymbol{b}$$的切线面。

容易看出，在函数$$\boldsymbol{l} (u) \times \boldsymbol{l}' (u)$$，或$$\alpha (u) - \beta' (u)$$的例外零点，正好是柱面、锥面和曲线的切线面沿直母线的拼接之处。证毕∎

可展曲面的另一个特征是它和平面在局部上可以建立[保长对应](/2023/10/18/DifferentialGeometry-NOTES-03.html#保长对应)。在直观上，柱面和锥面都能够在不作伸缩的情况下展开成平面,正好体现了上面所述的特征。它的逆命题也成立，留待后面再来证明。

> #### 定理3.11
> 可展曲面在局部上可以和平面建立保长对应。
{: .block-theorem }

**证明** 我们只要证明柱面、锥面和曲线的切线面都可以和平面建立保长对应即可。

(1) 柱面：设柱面的参数方程为

$$
\boldsymbol{r} = \boldsymbol{a} (u) + v \boldsymbol{l}_0
$$

不妨设$$\vert \boldsymbol{l}_0 \vert = 1$$。首先做准线的变换

$$
\boldsymbol{b} (u) = \boldsymbol{a} (u) + \lambda (u) \boldsymbol{l}_0
$$

使得$$\boldsymbol{b}'(u) \perp \boldsymbol{l}_0$$。对上式求导得到

$$
\boldsymbol{b}'(u) = \boldsymbol{a}'(u) + \lambda' (u) \boldsymbol{l}_0
$$

所以

$$
0 = \boldsymbol{b}'(u) \cdot \boldsymbol{l}_0 = \boldsymbol{a}'(u) \cdot \boldsymbol{l}_0 + \lambda' (u)
$$

故只需要取

$$
\lambda (u) = - \boldsymbol{a}'(u) \cdot \boldsymbol{l}_0
$$

此时，该柱面的方程成为

$$
\boldsymbol{r} = \boldsymbol{b} (u) + (v - \lambda (u)) \boldsymbol{l}_0
$$

接着再作参数变换

$$
\begin{cases}
\begin{aligned}
\tilde{u} &= \tilde{u} (u) = \int_{u_0}^u \vert \boldsymbol{b}' (u) \vert \ \mathrm{d} u \\
\tilde{v} &= v - \lambda (u)
\end{aligned}
\end{cases}
$$

使得$$\tilde{u}$$是准线的弧长参数。此时柱面的方程成为

$$
\boldsymbol{r} = \tilde{\boldsymbol{b}} (\tilde{u}) + \tilde{v} \boldsymbol{l}_0
$$

其中

$$
\tilde{\boldsymbol{b}} (\tilde{u} (u)) = \boldsymbol{b} (u)
$$

因此

$$
\tilde{\boldsymbol{b}}' (\tilde{u}) \cdot \frac{\partial \tilde{u}}{\partial u} = \tilde{\boldsymbol{b}}' (\tilde{u}) \cdot \vert \boldsymbol{b}' (u) \vert = \boldsymbol{b}' (u)
$$

$$
\vert \tilde{\boldsymbol{b}}' (\tilde{u}) \vert = 1
$$

经计算容易得到曲面的第一基本形式是

$$
\mathrm{I} = (\mathrm{d} \tilde{u})^2 + (\mathrm{d} \tilde{v})^2
$$

这正好是平面在笛卡儿直角坐标系下的第一基本形式，所以柱面能够和平面建立保长对应。

(2) 锥面：设锥面的参数方程为

$$
\boldsymbol{r} = \boldsymbol{a}_0 + v \boldsymbol{l} (u)
$$

其中$$\boldsymbol{a}_0$$是常向量，并且假设$$\boldsymbol{l} (u)$$是单位向量，即$$\vert \boldsymbol{l} (u) \vert = 1$$。作参数变换

$$
\begin{cases}
\begin{aligned}
\tilde{u} &= \tilde{u} (u) = \int_{u_0}^u \vert \boldsymbol{l}' (u) \vert \ \mathrm{d} u \\
\tilde{v} &= v
\end{aligned}
\end{cases}
$$

则曲面的参数方程成为

$$
\boldsymbol{r} = \boldsymbol{a}_0 + \tilde{v} \tilde{\boldsymbol{l}} (\tilde{u})
$$

其中

$$
\tilde{\boldsymbol{l}} (\tilde{u} (u)) = \boldsymbol{l} (u)
$$

故

$$
\tilde{\boldsymbol{l}} (\tilde{u}) \cdot \frac{\mathrm{d} \tilde{u} (u)}{\mathrm{d} (u)} = \tilde{\boldsymbol{l}} (\tilde{u}) \cdot \vert \boldsymbol{l}' (u) \vert = \tilde{\boldsymbol{l}} (\tilde{u})
$$

于是$$\vert \tilde{\boldsymbol{l}} (\tilde{u}) \vert = 1$$，并且$$\vert \tilde{\boldsymbol{l}}' (\tilde{u}) \vert = 1$$，即$$\tilde{u}$$是是单位球面上的曲线$$\boldsymbol{r} = \tilde{\boldsymbol{l}} (\tilde{u})$$的弧长参数。这样，锥面的第一基本形式成为

$$
\mathrm{I} = \tilde{v}^2 (\mathrm{d} \tilde{u})^2 + (\mathrm{d} \tilde{v})^2
$$

命

$$
\begin{cases}
\begin{aligned}
x &= \tilde{v} \cos{\tilde{u}} \\
y &= \tilde{v} \sin{\tilde{u}}
\end{aligned}
\end{cases}
$$

则得到

$$
\mathrm{I} = (\mathrm{d} x)^2 + (\mathrm{d} y)^2
$$

因此锥面和平面是保长的。

(3) 切线面：设$$C: \boldsymbol{r} = \boldsymbol{r} (s)$$是$$\mathbb{E}^3$$中一条正则参数曲线，$$s$$是它的弧长参数，故它的切线面的参数方程是

$$
\boldsymbol{r} = \boldsymbol{r} (s) + t \boldsymbol{r}' (s) = \boldsymbol{r} (s) + t \boldsymbol{\alpha} (s)
$$

这里$$\{ \boldsymbol{r} (s); \boldsymbol{\alpha} (s), \boldsymbol{\beta} (s), \boldsymbol{\gamma} (s) \}$$是曲线$$C$$的Frenet标架。因此

$$
\begin{cases}
\begin{aligned}
\boldsymbol{r}_s &= \boldsymbol{\alpha} (s) + t \kappa (s) \boldsymbol{\beta} (s) \\
\boldsymbol{r}_t &= \boldsymbol{\alpha} (s)
\end{aligned}
\end{cases}
$$

其中$$\kappa(s)$$是曲线的曲率。所以

$$
\begin{cases}
\begin{aligned}
E &= 1 + t^2 \kappa^2 \\
F &= 1 \\
G &= 1
\end{aligned}
\end{cases}
$$

切线面的第一基本形式是

$$
\mathrm{I} = (1 + t^2 \kappa^2) (\mathrm{d} s)^2 + 2 \mathrm{d} s \mathrm{d} t + (\mathrm{d} t)^2
$$

注意到在切线面的第一基本形式中不含有曲线$$C$$的挠率，这就是说如果$$C$$，$$C_1$$是空间$$\mathbb{E}^3$$中任意两条有相同的弧长参数和相同的曲率函数的正则参数曲线，则它们的切线面必有相同的第一基本形式，因此这两个切线面必定是保长的。根据[曲线论基本定理](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的内在方程)，可以作一条平面曲线$$C_1$$，使它以$$s$$为弧长参数，以$$\kappa (s)$$为曲率函数而挠率为零，那么它的切线面是平面的一部分。由此可见，曲线$$C$$的切线面能够和平面在局部上建立保长对应。证毕∎

### 包络

可展曲面的切平面沿直母线是同一个，因此可展曲面的切平面构成依赖一个参数的平面族，而可展曲面本身可以看作与该平面族中每一个成员都相切的曲面。这个概念可以作一些推广。

> #### 定义3.6
> 设$$\{ S_\alpha \}$$是依赖参数$$\alpha$$的一族正则曲面。如果有一个正则曲面$$S$$，使得$$S$$上的每一点必定是曲面族$$\{ S_\alpha \}$$中的某个曲面$$S_\alpha$$上的一点，并且曲面$$S$$和$$S_\alpha$$在该点有相同的切平面；反过来，曲面族$$\{ S_\alpha \}$$中的每一个成员必定与曲面$$S$$在某一点有相同的切平面，则称曲面$$S$$是单参数曲面族$$\{ S_\alpha \}$$的**包络**。
{: .block-definition }

根据上述定义，可展曲面是单参数平面族的包络。