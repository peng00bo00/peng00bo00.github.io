---
layout: post
title: 微分几何笔记05-曲面论基本定理
date: 2023-12-27
description: 曲面论基本定理
tags: CG Math
categories: Differential-Geometry
toc:
  sidebar: left
---


> 这个系列是北京大学陈维桓教授《微分几何(第二版)》的学习笔记，主要涉及古典微分几何中曲线曲面理论的相关知识。系统学习微分几何对于理解计算机图形学中的各种几何处理算法是十分有益的。本节介绍曲面论基本定理。
{: .block-preface }


## 自然标架的运动公式

在研究空间曲线时，我们在曲线上曲率不为零的每一个点处附加了一个确定的[Frenet标架](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的曲率和frenet标架)，那么Frenet标架沿曲线运动的状况便反映出曲线本身的弯曲情况。Frenet标架可以通过曲线的参数方程的导数和适当的代数运算显式地表示出来，从而$$\mathbb{E}^3$$中一条曲线可以转化为$$\mathbb{E}^3$$上所有的标架构成的空间中的一条曲线。曲线论的基本定理和存在定理都是通过标架空间中的这个单参数标架族进行证明的，因为只有这个单参数标架族在运动时给出的信息才完整地表达曲线的弯曲形状。对于空间中的正则参数曲面$$\boldsymbol{r} = \boldsymbol{r} (u, v)$$，我们也需要以某种确定的方式在每一点附加一个标架，这个标架就是自然标架$$\{ \boldsymbol{r}; \boldsymbol{r}_u, \boldsymbol{r}_v, \boldsymbol{n} \}$$。与曲线的情形不同的是，自然标架和曲面的参数选择是有关系的，而且一般来说自然标架不是正交标架，更不是单位正交标架。当然，我们可以在曲面上取单位正交标架场，如$$\{ \boldsymbol{r}; \boldsymbol{e}_1, \boldsymbol{e}_2, \boldsymbol{n} \}$$，其中$$\boldsymbol{e}_1$$, $$\boldsymbol{e}_2$$是曲面在该点的彼此正交的主方向单位向量。但是，主方向本身并不能够像曲线的Frenet标架那样从曲面的参数方程$$\boldsymbol{r} = \boldsymbol{r}(u, v)$$的偏导数直截了当地显式表示出来。即使我们假定在曲面上取了正交曲率线网作为曲面的参数曲线网，也只能做到$$\boldsymbol{r}_u \parallel \boldsymbol{e}_1$$，$$\boldsymbol{r}_v \parallel \boldsymbol{e}_2$$，而不是$$\boldsymbol{r}_u = \boldsymbol{e}_1$$，$$\boldsymbol{r}_v = \boldsymbol{e}_2$$。由此可见，从自然标架场出发展开我们的理论比较方便。后来Cartan发展了活动标架理论来研究微分几何学，在曲面上可以取任意的标架场，包括单位正交标架场，此时要用「微分」替代「偏导数」，因而相应地要用到外微分方法。我们将在后面介绍Cartan的活动标架和外微分法。

### Einstein求和约定

由于我们要做的是多元函数的偏导数，会出现很多求和式，因此把前面所用的由Gauss引进的记号系统改成所谓的**张量记号系统**是比较方便的。首先，曲面的参数记成带上指标的量，如$$u^1$$，$$u^2$$代替原来的$$u$$，$$v$$。要注意的是，在新的记号系统中，$$\mathrm{d} u^2$$设是参数$$u^2$$的微分，而不是微分$$\mathrm{d} u$$的平方。从上下文来看能够知道我们用的是哪一种记号系统，因此是不会混淆的。这样，曲面$$S$$的参数方程记为

$$
\boldsymbol{r} = \boldsymbol{r} (u^1, u^2)
$$

并且命

$$
\boldsymbol{r}_\alpha = \frac{\partial \boldsymbol{r} (u^1, u^2)}{\partial u^\alpha}, \ \ \ \alpha = 1, 2
$$

(需要指出的是，从现在开始如果没有特别的声明，$$\boldsymbol{r}_1$$和$$\boldsymbol{r}_2$$只表示上式所定义的两个切向量)。于是，曲面$$S$$的单位法向量是

$$
\boldsymbol{n} = \frac{\boldsymbol{r}_1 \times \boldsymbol{r}_2}{\vert \boldsymbol{r}_1 \times \boldsymbol{r}_2 \vert}
$$

这样，参数方程$$\boldsymbol{r} = \boldsymbol{r} (u^1, u^2)$$的微分是

$$
\begin{aligned}
\mathrm{d} \boldsymbol{r} = \mathrm{d} \boldsymbol{r} (u^1, u^2) &= \sum_{\alpha=1}^2 \boldsymbol{r}_\alpha (u^1, u^2) \mathrm{d} u^\alpha \\
&= \boldsymbol{r}_\alpha (u^1, u^2) \mathrm{d} u^\alpha
\end{aligned}
$$

在最后一式中，我们采用了**Einstein求和约定**，即：在一个单项式中，如果同一个指标字母$$\alpha$$出现两次，一次作为上指标，一次作为下指标，则该单项式实际上代表对于$$\alpha=1,2$$的求和式。多对重复的指标字母
表示多重的求和式，例如

$$
S_{\alpha \beta} T^{\alpha \beta \gamma} = \sum_{\alpha = 1}^2 \sum_{\beta = 1}^2 S_{\alpha \beta} T^{\alpha \beta \gamma}
$$

$$
P_\alpha^\alpha = \sum_{\alpha = 1}^2 P_\alpha^\alpha = P_1^1 + P_2^2
$$

在求和式中，求和指标的字母本身并没有实质性意义，它们是所谓的「哑」指标，也可以用别的字母来代替，例如

$$
S_{\alpha \beta} T^{\alpha \beta \gamma} = S_{\xi \eta} T^{\xi \eta \gamma}
$$

但是这里的指标$$\gamma$$**不是**哑指标，不表示求和，左、右两边应该保待一致。另外，在一个求和式中同一个指标字母出现三次以上是没有意义的，例如表达式$$S_\alpha T^{\alpha \alpha}$$的写法是不适当的，如果要表示这样的和式必须写出和号，例如

$$
\sum_{\alpha = 1}^2 S_\alpha T^{\alpha \alpha} = S_1 T^{1 1} + S_2 T^{2 2}
$$

**不能**用Einstein求和约定。在表示多重和式时，Einstein求和约定起到十分重要的简化作用。

### 张量记号

我们用$$g_{\alpha \beta}$$和$$b_{\alpha \beta}$$分别表示曲面$$S$$的第一类基本量和第二类基本量，即

$$
g_{\alpha \beta} = \boldsymbol{r}_\alpha \cdot \boldsymbol{r}_\beta
$$

$$
b_{\alpha \beta} = \boldsymbol{r}_{\alpha \beta} \cdot \boldsymbol{n} = -\boldsymbol{r}_\alpha \cdot \boldsymbol{n}_\beta = -\boldsymbol{n}_\beta \cdot \boldsymbol{r}_\alpha
$$

其中

$$
\boldsymbol{r}_{\alpha \beta} = \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{r} (u^1, u^2)}{\partial u^\alpha} \bigg)
$$

这样，曲面$$S$$的两个基本形式是

$$
\begin{cases}
\begin{aligned}
\mathrm{I} &= g_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\mathrm{II} &= b_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\end{aligned}
\end{cases}
$$

另外，记

$$
\begin{cases}
\begin{aligned}
g &= \det (g_{\alpha \beta}) = g_{11} g_{22} - g_{12}^2 \\
b &= \det (b_{\alpha \beta}) = b_{11} b_{22} - b_{12}^2 \\
\end{aligned}
\end{cases}
$$

既然矩阵$$(g_{\alpha \beta})$$是正定的，故它有逆矩阵，记为$$(g^{\alpha \beta})$$，于是

$$
g^{\alpha \beta} g_{\beta \gamma} = \delta_\gamma^\alpha = 
\begin{cases}
\begin{aligned}
1, & &\alpha = \gamma \\
0, & &\alpha \neq \gamma
\end{aligned}
\end{cases}
$$

实际上，根据2×2可逆矩阵的逆矩阵公式，显然有

$$
\begin{pmatrix}
g^{11} & g^{12} \\
g^{21} & g^{22} \\
\end{pmatrix}
=
\frac{1}{g}
\begin{pmatrix}
 g_{22} &-g_{12} \\
-g_{21} & g_{11} \\
\end{pmatrix}
$$

总体来说，Gauss的记号和张量记号对照如下表：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dQjRIxf.png" width="80%">
</div>

### Christoffel记号

采用张量记号，正则参数曲面$$S$$上的自然标架成为$$\{ \boldsymbol{r}; \boldsymbol{r}_1, \boldsymbol{r}_2, \boldsymbol{n} \}$$。下面要求自然标架场的运动公式。首先根据定义，标架原点的偏导数是

$$
\frac{\partial \boldsymbol{r}}{\partial u^\alpha} = \boldsymbol{r}_\alpha
$$

另外，既然$$\boldsymbol{r}_1$$，$$\boldsymbol{r}_2$$，$$\boldsymbol{n}$$是线性无关的，而$$\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta}$$，$$\frac{\partial \boldsymbol{n}}{\partial u^\beta}$$仍然是空间$$\mathbb{E}^3$$中的向量，因此不妨假定

$$
\begin{cases}
\begin{aligned}
\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} &= \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma + C_{\alpha \beta} \boldsymbol{n} \\
\frac{\partial \boldsymbol{n}}{\partial u^\beta} &= D_\beta^\gamma \boldsymbol{r}_\gamma + D_\beta \boldsymbol{n} 
\end{aligned}
\end{cases}
$$

其中$$\Gamma_{\alpha \beta}^\gamma$$，$$C_{\alpha \beta}$$，$$D_\beta^\gamma$$，$$D_\beta$$都是待定的系数函数。

将上面第一式的两边用法向量$$\boldsymbol{n}$$作内积，得到

$$
C_{\alpha \beta} = \frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} \cdot \boldsymbol{n} = \boldsymbol{r}_{\alpha \beta} \cdot \boldsymbol{n} = b_{\alpha \beta}
$$

即系数$$C_{\alpha \beta}$$恰好是曲面$$S$$的第二类基本量$$b_{\alpha \beta}$$。将第二式的两边用切向量$$\boldsymbol{r}_\xi$$作内积，得到

$$
\frac{\partial \boldsymbol{n}}{\partial u^\beta} \cdot \boldsymbol{r}_\xi = D_\beta^\gamma \boldsymbol{r}_\gamma \cdot \boldsymbol{r}_\xi = D_\beta^\gamma g_{\gamma \xi}
$$

因此

$$
D_\beta^\gamma g_{\gamma \xi} = -b_{\beta \xi}
$$

$$
D_\beta^\gamma = -b_{\beta \xi} g^{\xi \gamma}
$$

命

$$
b_\beta^\gamma = b_{\beta \xi} g^{\xi \gamma}
$$

把$$b_\beta^\gamma$$看成是将$$b_\beta \gamma$$的指标$$\gamma$$借助于矩阵$$(g^{\alpha \beta})$$上升的结果。这个过程是可逆的，即

$$
b_\beta^\gamma \cdot g_{\gamma \eta} = b_{\beta \xi} g^{\xi \gamma} g_{\gamma \eta} = b_{\beta \xi} \delta_{\eta}^\xi = b_{\beta \eta}
$$

这就是说$$b_{\beta \eta}$$是将$$b_\beta^\eta$$的上指标$$\eta$$借助于矩阵$$(g_{\alpha \beta})$$下降的结果。这样，$$(b_\alpha^\beta)$$这组量和第二类基本量$$(b_{\alpha \beta})$$是彼此决定的，并且系数$$D_\beta^\gamma$$是

$$
D_\beta^\gamma = -b_\beta^\gamma
$$

另外，由于$$\boldsymbol{n}$$是单位向量场，故容易得到$$D_\beta = 0$$。

综上所述，$$\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta}$$，$$\frac{\partial \boldsymbol{n}}{\partial u^\beta}$$可以重新表示为

$$
\begin{cases}
\begin{aligned}
\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} &= \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma + b_{\alpha \beta} \boldsymbol{n} \\
\frac{\partial \boldsymbol{n}}{\partial u^\beta} &= -b_\beta^\gamma \boldsymbol{r}_\gamma
\end{aligned}
\end{cases}
$$

现在我们来求系数函数$$\Gamma_{\alpha \beta}^\gamma $$。在第一式两边点乘$$\boldsymbol{r}_\xi$$，则得

$$
\boldsymbol{r}_{\alpha \beta} \cdot \boldsymbol{r}_\xi = \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma \cdot \boldsymbol{r}_\xi = \Gamma_{\alpha \beta}^\gamma g_{\gamma \xi}
$$

记

$$
\Gamma_{\alpha \beta}^\gamma g_{\gamma \xi} = \Gamma_{\xi \alpha \beta}
$$

则我们有

$$
\Gamma_{\alpha \beta}^\gamma = g^{\gamma \xi} \Gamma_{\xi \alpha \beta}
$$

这就是说，记号$$\Gamma_{\alpha \beta}^\gamma$$的上指标$$\gamma$$可以借助于第一类基本量的矩阵$$(g_{\alpha \beta})$$(以后常常称它为曲面的**度量矩阵**)及其逆矩阵$$(g^{\alpha \beta})$$下降和上升。在这里我们规定，当记号$$\Gamma_{\alpha \beta}^\gamma$$的上指标$$\gamma$$下降时落在下指标的最左边成为$$\Gamma_{\gamma \alpha \beta}$$。这样可以得到

$$
\boldsymbol{r}_{\alpha \beta} \cdot \boldsymbol{r}_\xi = \Gamma_{\xi \alpha \beta}
$$

注意到

$$
\boldsymbol{r}_{\alpha \beta} = \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{r}(u^1, u^2)}{\partial u^\alpha} \bigg) = \frac{\partial}{\partial u^\alpha} \bigg( \frac{\partial \boldsymbol{r}(u^1, u^2)}{\partial u^\beta} \bigg) = \boldsymbol{r}_{\beta \alpha}
$$

即函数$$\boldsymbol{r}(u^1, u^2)$$的两次偏导数与求导的次序无关，因此$$\boldsymbol{r}_{\alpha \beta}$$关于下指标$$\alpha$$，$$\beta$$是对称的，于是记号$$\Gamma_{\alpha \beta}^{\gamma}$$和$$\Gamma_{\xi \alpha \beta}$$关于下指标$$\alpha$$，$$\beta$$是对称的，即

$$
\Gamma_{\alpha \beta}^{\gamma} = \Gamma_{\beta \alpha}^{\gamma}
$$

$$
\Gamma_{\xi \alpha \beta} = \Gamma_{\xi \beta \alpha}
$$

对$$g_{\alpha \beta} = \boldsymbol{r}_\alpha \cdot \boldsymbol{r}_\beta$$求偏导数得到

$$
\frac{\partial g_{\alpha \beta}}{\partial u^\gamma} = \boldsymbol{r}_{\alpha \gamma} \cdot \boldsymbol{r}_\beta + \boldsymbol{r}_\alpha \cdot \boldsymbol{r}_{\beta \gamma}
$$

代入$$\Gamma_{\gamma \alpha \beta}$$的定义式$$\boldsymbol{r}_{\alpha \beta} \cdot \boldsymbol{r}_\gamma = \Gamma_{\gamma \alpha \beta}$$得到

$$
\frac{\partial g_{\alpha \beta}}{\partial u^\gamma} = \Gamma_{\beta \alpha \gamma} + \Gamma_{\alpha \beta \gamma}
$$

将下指标进行调换得到

$$
\begin{cases}
\begin{aligned}
\frac{\partial g_{\alpha \gamma}}{\partial u^\beta} &= \Gamma_{\gamma \alpha \beta} + \Gamma_{\alpha \gamma \beta} \\
\frac{\partial g_{\gamma \beta}}{\partial u^\alpha} &= \Gamma_{\beta \gamma \alpha} + \Gamma_{\gamma \beta \alpha} \\
\end{aligned}
\end{cases}
$$

将两式相加再减去$$\frac{\partial g_{\alpha \beta}}{\partial u^\gamma}$$，并且利用$$\Gamma_{\gamma \alpha \beta}$$关于后两个下指标的对称性，则得

$$
2 \Gamma_{\gamma \alpha \beta} = \frac{\partial g_{\alpha \gamma}}{\partial u^\beta} + \frac{\partial g_{\gamma \beta}}{\partial u^\alpha} - \frac{\partial g_{\alpha \beta}}{\partial u^\gamma}
$$

或者

$$
\Gamma_{\gamma \alpha \beta} = \frac{1}{2} \bigg( \frac{\partial g_{\alpha \gamma}}{\partial u^\beta} + \frac{\partial g_{\gamma \beta}}{\partial u^\alpha} - \frac{\partial g_{\alpha \beta}}{\partial u^\gamma} \bigg)
$$

因此

$$
\Gamma_{\alpha \beta}^\gamma = \frac{1}{2} g^{\gamma \xi} \bigg( \frac{\partial g_{\alpha \xi}}{\partial u^\beta} + \frac{\partial g_{\xi \beta}}{\partial u^\alpha} - \frac{\partial g_{\alpha \beta}}{\partial u^\xi} \bigg)
$$

由此可见，系数函数$$\Gamma_{\alpha \beta}^\gamma$$是由曲面$$S$$的第一类基本量及其一阶偏导数决定的。通常把上式所定义的$$\Gamma_{\alpha \beta}^\gamma$$称为由曲面$$S$$的度量矩阵$$(g_{\alpha \beta})$$决定的**Christoffel记号**。

### 自然标架场的运动公式

我们将

$$
\begin{cases}
\begin{aligned}
\frac{\partial \boldsymbol{r}}{\partial u^\alpha} &= \boldsymbol{r}_\alpha \\
\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} &= \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma + b_{\alpha \beta} \boldsymbol{n} \\
\frac{\partial \boldsymbol{n}}{\partial u^\beta} &= -b_\beta^\gamma \boldsymbol{r}_\gamma
\end{aligned}
\end{cases}
$$

称为曲面$$S$$上自然标架场$$\{ \boldsymbol{r}; \boldsymbol{r}_1, \boldsymbol{r}_2, \boldsymbol{n} \}$$的**运动公式**。通常把第二式$$\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} = \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma + b_{\alpha \beta} \boldsymbol{n}$$称为曲面的**Gauss公式**，把第三式$$\frac{\partial \boldsymbol{n}}{\partial u^\beta} = -b_\beta^\gamma \boldsymbol{r}_\gamma$$称为曲面的**Weingarten公式**。容易知道，Weingarten公式正是[Weingarten映射$$W$$的表达式](/2023/11/10/DifferentialGeometry-NOTES-04.html#gauss曲率的几何意义)：

$$
\begin{pmatrix}
\boldsymbol{n}_u \\ \boldsymbol{n}_v
\end{pmatrix}
= -
\begin{pmatrix}
W_{11} & W_{12} \\
W_{21} & W_{22} \\
\end{pmatrix}
\begin{pmatrix}
\boldsymbol{r}_u \\ \boldsymbol{r}_v
\end{pmatrix}
$$

$$
W = 
\begin{pmatrix}
L & M \\
M & N \\
\end{pmatrix}
\begin{pmatrix}
E & F \\
F & G \\
\end{pmatrix}^{-1}
$$

这就是说，$$(b_j^i)$$是Weingarten映射$$W$$在自然基底$$\{ \boldsymbol{r}_1, \boldsymbol{r}_2 \}$$下的矩阵：

$$
b_j^i = W_{i j} = b_{j k} g^{k i}
$$

从上面的公式可以看出，曲面$$S$$上的自然标架场$$\{ \boldsymbol{r}; \boldsymbol{r}_1, \boldsymbol{r}_2, \boldsymbol{n} \}$$沿参数曲线的运动公式是由曲面$$S$$的第一类基本量和第二类基本量完全确定的。要记住，$$\Gamma_{\alpha \beta}^\gamma$$的几何意义是向量$$\boldsymbol{r}_{\alpha \beta}$$用自然标架分解时在切向量$$\boldsymbol{r}_\gamma$$上的分量，而$$\Gamma_{\gamma \alpha \beta}$$的几何意义是向量$$\boldsymbol{r}_{\alpha \beta}$$在切向量$$\boldsymbol{r}_\gamma$$上的正交投影。因此，在求$$\Gamma_{\gamma \alpha \beta}$$时，我们既可以以根据曲面的度量矩阵按照定义式

$$
\Gamma_{\gamma \alpha \beta} = \frac{1}{2} \bigg( \frac{\partial g_{\alpha \gamma}}{\partial u^\beta} + \frac{\partial g_{\gamma \beta}}{\partial u^\alpha} - \frac{\partial g_{\alpha \beta}}{\partial u^\gamma} \bigg)
$$

进行计算，也可以根据曲面的参数方程依据它的几何意义进行计算。

恢复用Gauss曲面论的记法，则Christoffel记号是

$$
\begin{aligned}
\Gamma_{111} &= \frac{1}{2} \frac{\partial E}{\partial u} &\Gamma_{112} &= \Gamma_{121} = \frac{1}{2} \frac{\partial E}{\partial v} \\
\Gamma_{122} &= \frac{\partial F}{\partial u} - \frac{1}{2} \frac{\partial G}{\partial u} &\Gamma_{211} &= \frac{\partial F}{\partial u} - \frac{1}{2} \frac{\partial E}{\partial v} \\
\Gamma_{212} &= \Gamma_{221} =  \frac{1}{2} \frac{\partial G}{\partial u} &\Gamma_{222} &= \frac{1}{2} \frac{\partial G}{\partial v}
\end{aligned}
$$

以及

$$
\begin{aligned}
\Gamma_{11}^1 &= \frac{1}{EG - F^2} \bigg( \frac{G}{2} \frac{\partial E}{\partial u} + \frac{F}{2} \frac{\partial E}{\partial v} - F \frac{\partial F}{\partial u} \bigg) \\
\Gamma_{12}^1 &= \Gamma_{21}^1 = \frac{1}{EG - F^2} \bigg( \frac{G}{2} \frac{\partial E}{\partial v} - \frac{F}{2} \frac{\partial G}{\partial u} \bigg) \\
\Gamma_{22}^1 &= \frac{1}{EG - F^2} \bigg( G \frac{\partial F}{\partial u} - \frac{G}{2} \frac{\partial G}{\partial u} - \frac{F}{2} \frac{\partial G}{\partial v} \bigg) \\
\Gamma_{11}^2 &= \frac{1}{EG - F^2} \bigg( -\frac{F}{2} \frac{\partial E}{\partial u} - \frac{E}{2} \frac{\partial E}{\partial v} + E \frac{\partial F}{\partial u} \bigg) \\
\Gamma_{12}^2 &= \Gamma_{21}^2 = \frac{1}{EG - F^2} \bigg( -\frac{F}{2} \frac{\partial E}{\partial v} + \frac{E}{2} \frac{\partial G}{\partial u} \bigg) \\
\Gamma_{22}^2 &= \frac{1}{EG - F^2} \bigg( -F \frac{\partial F}{\partial v} + \frac{F}{2} \frac{\partial G}{\partial u} + \frac{E}{2} \frac{\partial G}{\partial v} \bigg) \\
\end{aligned}
$$

如果在曲面上取正交参数曲线网，则$$F \equiv 0$$，上面的公式便简化成为

$$
\begin{aligned}
\Gamma_{11}^1 &= \frac{1}{2} \frac{\partial \log E}{\partial u} &\Gamma_{12}^1 &= \Gamma_{21}^1 = \frac{1}{2} \frac{\partial \log E}{\partial v} \\
\Gamma_{22}^1 &= -\frac{1}{2E} \frac{\partial G}{\partial u} &\Gamma_{11}^2 &= -\frac{1}{2G} \frac{\partial E}{\partial v} \\
\Gamma_{12}^2 &= \Gamma_{21}^2 = \frac{1}{2} \frac{\partial \log G}{\partial u} &\Gamma_{22}^2 &= \frac{1}{2} \frac{\partial \log G}{\partial v}
\end{aligned}
$$

通常，求曲面的Christoffel记号是用曲面的第一类基本量，套用上面的公式进行计算。但是如果知道曲面的参数方程则也可以借助于Christoffel记号的几何意义直接来求。

## 曲面的唯一性定理

利用上一节所介绍的曲面上的自然标架场的运动公式，可以直接地证明，曲面在不计空间位置的情况下是由它的第一基本形式和第二基本形式唯一地确定的。

> ##### 定理5.1 
> 设$$S_1$$、$$S_2$$是定义在同一个参数区域$$D \subset \mathbb{E}^2$$上的两个正则参数曲面。若在每一点$$(u^1, u^2) \in D$$，曲面$$S_1$$、$$S_2$$都有相同的第一基本形式和第二基本形式，则曲面$$S_1$$、$$S_2$$在空间$$\mathbb{E}^3$$的一个刚体运动下是彼此重合的。
{: .block-theorem }

**证明** 因为$$S_1$$、$$S_2$$上采用了同一组参数，因此在每一点$$u = (u^1, u^2) \in D$$处曲面$$S_1$$和$$S_2$$都有相同的第一基本形式和第二基本形式的意思是，它们在每一点都有相同的第一类基本量和第二类基本量。

假定曲面$$S_i$$的自然标架场是$$\{ \boldsymbol{r}^{(i)}; \boldsymbol{r}_1^{(i)}, \boldsymbol{r}_2^{(i)}, \boldsymbol{n}^{(i)} \}$$，$$i=1,2$$，任意地取定一点$$u_0 = (u_0^1, u_0^2) \in D$$，则由假定得知

$$
\begin{aligned}
\boldsymbol{r}_{\alpha}^{(1)} (u_0) \cdot \boldsymbol{r}_{\beta}^{(1)} (u_0) &= \boldsymbol{r}_{\alpha}^{(2)} (u_0) \cdot \boldsymbol{r}_{\beta}^{(2)} (u_0) = g_{\alpha \beta} (u_0) \\
\boldsymbol{r}_{\alpha}^{(1)} (u_0) \cdot \boldsymbol{n}^{(1)} (u_0) &= \boldsymbol{r}_{\alpha}^{(2)} (u_0) \cdot \boldsymbol{n}^{(2)} (u_0) = 0 \\
\boldsymbol{n}^{(1)} (u_0) \cdot \boldsymbol{n}^{(1)} (u_0) &= \boldsymbol{n}^{(2)} (u_0) \cdot \boldsymbol{n}^{(2)} (u_0) = 1 \\
\end{aligned}
$$

并且$$\{ \boldsymbol{r}^{(i)} (u_0); \boldsymbol{r}_1^{(i)} (u_0), \boldsymbol{r}_2^{(i)} (u_0), \boldsymbol{n}^{(i)} (u_0) \}$$，$$i=1,2$$，都是右手系。这就是说，这两个标架具有相同的度量系数和定向，因而在空间$$\mathbb{E}^3$$中存在一个刚体运动$$\sigma$$，把标架$$\{ \boldsymbol{r}^{(2)} (u_0); \boldsymbol{r}_1^{(2)} (u_0), \boldsymbol{r}_2^{(2)} (u_0), \boldsymbol{n}^{(2)} (u_0) \}$$变成标架$$\{ \boldsymbol{r}^{(1)} (u_0); \boldsymbol{r}_1^{(1)} (u_0), \boldsymbol{r}_2^{(1)} (u_0), \boldsymbol{n}^{(1)} (u_0) \}$$ (参见[定理1.1](/2023/07/30/DifferentialGeometry-NOTES-01.html#正交标架))。用$$\sigma (S_2)$$表示曲面$$S_2$$在刚体运动$$\sigma$$的作用下得到的新曲面，那么这个新的曲面与曲面$$S_1$$在点$$u_0$$处有相同的自然标架，并且由于曲面的第一类基本量和第二类基本量在曲面作刚体运动时是保持不变的，所以新的曲面$$\sigma (S_2)$$与曲面$$S_1$$在所有对应于同一个参数值的点，仍旧有相同的第一类基本量和第二类基本量。由此可见，它们的自然标架场适合同一组偏微分方程

$$
\begin{cases}
\begin{aligned}
\frac{\partial \boldsymbol{r}}{\partial u^\alpha} &= \boldsymbol{r}_\alpha \\
\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} &= \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma + b_{\alpha \beta} \boldsymbol{n} \\
\frac{\partial \boldsymbol{n}}{\partial u^\beta} &= -b_\beta^\gamma \boldsymbol{r}_\gamma
\end{aligned}
\end{cases}
$$

不妨把新的曲面$$\sigma (S_2)$$仍然记为$$S_2$$，则曲面$$S_2$$和$$S_1$$在点$$u_0$$有相同的自然标架，而且处处有相同的第一类基本量和第二类基本量。我们想要证明的是，曲面$$S_2$$和$$S_1$$的自然标架场处处是重合的，进而曲面$$S_2$$和$$S_1$$是重合的。

为此，命

$$
\begin{aligned}
f_{\alpha \beta} (u) &= (\boldsymbol{r}_\alpha^{(1)} - \boldsymbol{r}_\alpha^{(2)}) \cdot (\boldsymbol{r}_\beta^{(1)} - \boldsymbol{r}_\beta^{(2)}) \\
f_\alpha (u) &= (\boldsymbol{r}_\alpha^{(1)} - \boldsymbol{r}_\alpha^{(2)}) \cdot (\boldsymbol{n}^{(1)} - \boldsymbol{n}^{(2)}) \\
f(u) &= (\boldsymbol{n}^{(1)} - \boldsymbol{n}^{(2)})^2
\end{aligned}
$$

由于曲面$$S_2$$和$$S_1$$在点$$u_0$$有相同的自然标架，所以上式所定义的函数在点$$u = u_0$$处必定满足条件

$$
f_{\alpha \beta} (u_0) = 0 , \ \ \ f_\alpha (u_0) = 0, \ \ \ f(u_0) = 0
$$

另外，根据自然标架的公式经过直接计算得到函数$$f_{\alpha \beta}$$，$$f_\alpha$$，$$f$$满足下列一阶线性齐次偏微分方程组

$$
\begin{cases}
\begin{aligned}
\frac{\partial f_{\alpha \beta}}{\partial u^\gamma} &= \Gamma_{\gamma \alpha}^\delta f_{\delta \beta} + \Gamma_{\gamma \beta}^\delta f_{\delta \alpha} + b_{\gamma \alpha} f_\beta + b_{\gamma \beta} f_\alpha \\
\frac{\partial f_{\alpha}}{\partial u^\gamma} &= -b_\gamma^\delta f_{\delta \alpha} + \Gamma_{\gamma \alpha}^\delta f_{\delta} + b_{\gamma \alpha} f \\
\frac{\partial f}{\partial u^\gamma} &= -2 b_\gamma^\delta f_\delta
\end{aligned}
\end{cases}
$$

很明显，一阶线性齐次偏微分方程组在初始条件$$f_{\alpha \beta} (u_0) = 0$$，$$f_\alpha (u_0) = 0$$，$$f(u_0) = 0$$下的一个解是零解。根据一阶偏微分方程组在已知初始条件下的唯一性得知相应的函数必定是零函数，即

$$
f_{\alpha \beta} (u) = 0 , \ \ \ f_\alpha (u) = 0, \ \ \ f(u) = 0
$$

上面的事实说明

$$
\begin{aligned}
\vert \boldsymbol{r}_\alpha^{(1)} (u) - \boldsymbol{r}_\alpha^{(2)} (u) \vert^2 &= f_{\alpha \alpha} (u) = 0 \\
\vert \boldsymbol{n}^{(1)} (u) - \boldsymbol{n}^{(2)} (u) \vert^2 &= f (u) = 0
\end{aligned}
$$

即

$$
\boldsymbol{r}^{(1)}_\alpha (u) = \boldsymbol{r}_\alpha^{(2)} (u), \ \ \ \boldsymbol{n}^{(1)} (u) = \boldsymbol{n}^{(2)} (u)
$$

再命

$$
h(u) = (\boldsymbol{r}^{(1)} (u) - \boldsymbol{r}^{(2)} (u))^2
$$

对上式求导得到

$$
\frac{\partial h}{\partial u^\gamma} = 2 (\boldsymbol{r}^{(1)} (u) - \boldsymbol{r}^{(2)} (u)) \cdot (\boldsymbol{r}_\gamma^{(1)} (u) - \boldsymbol{r}_\gamma^{(2)} (u)) = 0
$$

故

$$
h(u) = h(u_0) = 0
$$

即

$$
\boldsymbol{r}^{(1)} (u) = \boldsymbol{r}^{(2)} (u)
$$

这说明曲面$$S_1$$和$$S_2$$是重合的。证毕∎

上面的定理称为**曲面的唯一性定理**，在理论上有十分重要的意义，以后我们还要给出它的一些应用。要判断采用不同参数系的两个曲面在空间$$\mathbb{E}^3$$的一个刚体运动下是否能够重合，综合[定理3.4](/2023/10/18/DifferentialGeometry-NOTES-03.html#正则参数曲面之间的对应)和这里的曲面唯一性定理，我们有下面的结论。

> ##### 定理5.2
> 设$$S_1$$、$$S_2$$是空间$$\mathbb{E}^3$$的两个正则参数曲面，其第一基本形式和第二基本形式分别是$$\mathrm{I}_i$$和$$\mathrm{II}_i$$。如果有光滑映射$$\sigma: S_1 \rightarrow S_2$$，使得$$\sigma^* \mathrm{I}_2 = \mathrm{I}_1$$且$$\sigma^* \mathrm{II}_2 = \mathrm{II}_1$$，则曲面$$S_1$$、$$S_2$$经过$$\mathbb{E}^3$$的一个刚体运动是彼此重合的。
{: .block-theorem }

## 曲面论基本方程

现在我们着手讨论曲面的存在性问题。具体地说，如果在参数区域$$D$$上给定两个二次微分形式

$$
\begin{cases}
\begin{aligned}
\varphi &= g_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\psi &= b_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\end{aligned}
\end{cases}
$$

其中$$\alpha, \beta = 1, 2$$，$$g_{\alpha \beta} = g_{\beta \alpha}$$，$$b_{\alpha \beta} = b_{\beta \alpha}$$，并且$$\varphi$$是正定的，那么我们的问题是：在空间$$\mathbb{E}^3$$中是否存在参数曲面$$f: D \rightarrow \mathbb{E}^3$$，使得它以已知的微分形式$$\varphi$$、$$\psi$$作为它的第一基本形式和第二基本形式？这个问题比较复杂，需要作比较深入的分析。首先我们会看到，曲面上的第一基本形式和第二基本形式有一定的联系，并不是彼此独立的；它们必须适合一定的相容性条件，这就是我们在本节要讨论的**曲面论基本方程**。在[下一节](/2023/12/27/DifferentialGeometry-NOTES-05.html#曲面的存在性定理)要进一步证明，如果两个微分形式$$\varphi$$、$$\psi$$满足这些相容性条件，则上述问题的答案是肯定的。

### Riemann记号

已知曲面$$S$$的参数方程是$$\boldsymbol{r} = \boldsymbol{r} (u^1, u^2)$$，它的第一基本形式和第二基本形式是

$$
\begin{cases}
\begin{aligned}
\mathrm{I} &= g_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\mathrm{II} &= b_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\end{aligned}
\end{cases}
$$

那么曲面$$S$$上的自然标架场$$\{ \boldsymbol{r}; \boldsymbol{r}_1, \boldsymbol{r}_2, \boldsymbol{n} \}$$的运动公式是

$$
\begin{cases}
\begin{aligned}
\frac{\partial \boldsymbol{r}}{\partial u^\alpha} &= \boldsymbol{r}_\alpha \\
\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} &= \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma + b_{\alpha \beta} \boldsymbol{n} \\
\frac{\partial \boldsymbol{n}}{\partial u^\beta} &= -b_\beta^\gamma \boldsymbol{r}_\gamma
\end{aligned}
\end{cases}
$$

其中

$$
\Gamma_{\alpha \beta}^\gamma = \frac{1}{2} g^{\gamma \xi} \bigg( \frac{\partial g_{\alpha \xi}}{\partial u^\beta} + \frac{\partial g_{\xi \beta}}{\partial u^\alpha} - \frac{\partial g_{\alpha \beta}}{\partial u^\xi} \bigg)
$$

$$
b_\beta^\gamma = g^{\gamma \xi} b_{\xi \beta}
$$

由于正则参数曲面的方程具有三次以上的连续偏导数，所以$$\boldsymbol{r}_\alpha$$和$$\boldsymbol{n}$$的两次偏导数是连续的，并且它们与求导的次序无关，即

$$
\frac{\partial^2 \boldsymbol{r}_\alpha}{\partial u^\beta \partial u^\gamma} = \frac{\partial^2 \boldsymbol{r}_\alpha}{\partial u^\gamma \partial u^\beta}
$$

$$
\frac{\partial^2 \boldsymbol{n}}{\partial u^\beta \partial u^\gamma} = \frac{\partial^2 \boldsymbol{n}}{\partial u^\gamma \partial u^\beta}
$$

把自然标架场的运动公式代入上式得到

$$
\frac{\partial }{\partial u^\gamma} (\Gamma_{\alpha \beta}^\delta  \boldsymbol{r}_\delta + b_{\alpha \beta} \boldsymbol{n}) = \frac{\partial }{\partial u^\beta} (\Gamma_{\alpha \gamma}^\delta  \boldsymbol{r}_\delta + b_{\alpha \gamma} \boldsymbol{n})
$$

$$
\frac{\partial }{\partial u^\gamma} (b_\beta^\delta \boldsymbol{r}_\delta) = \frac{\partial }{\partial u^\beta} (b_\gamma^\delta \boldsymbol{r}_\delta)
$$

将上式展开，并且再次用自然标架场的运动公式代入，整理后得到

$$
\begin{aligned}
&\bigg( \frac{\partial}{\partial u^\gamma} \Gamma_{\alpha \beta}^\delta - \frac{\partial}{\partial u^\beta} \Gamma_{\alpha \gamma}^\delta + \Gamma_{\alpha \beta}^\eta \Gamma_{\eta \gamma}^\delta - \Gamma_{\alpha \gamma}^\eta \Gamma_{\eta \beta}^\delta - b_{\alpha \beta} b_\gamma^\delta + b_{\alpha \gamma} b_\beta^\delta \bigg) \boldsymbol{r}_\delta \\
+ &\bigg( \Gamma_{\alpha \beta}^\delta b_{\delta \gamma} - \Gamma_{\alpha \gamma}^\delta b_{\delta \beta} + \frac{\partial b_{\alpha \beta}}{\partial u^\gamma} - \frac{\partial b_{\alpha \gamma}}{\partial u^\beta} \bigg) \boldsymbol{n} = \boldsymbol{0}
\end{aligned}
$$

由于$$\boldsymbol{r}_1$$、$$\boldsymbol{r}_2$$、$$\boldsymbol{n}$$是处处线性无关的，所以上式的系数必须为零，即有

$$
\frac{\partial}{\partial u^\gamma} \Gamma_{\alpha \beta}^\delta - \frac{\partial}{\partial u^\beta} \Gamma_{\alpha \gamma}^\delta + \Gamma_{\alpha \beta}^\eta \Gamma_{\eta \gamma}^\delta - \Gamma_{\alpha \gamma}^\eta \Gamma_{\eta \beta}^\delta = b_{\alpha \beta} b_\gamma^\delta - b_{\alpha \gamma} b_\beta^\delta
$$

$$
\frac{\partial b_{\alpha \beta}}{\partial u^\gamma} - \frac{\partial b_{\alpha \gamma}}{\partial u^\beta} = \Gamma_{\alpha \gamma}^\delta b_{\delta \beta} - \Gamma_{\alpha \beta}^\delta b_{\delta \gamma}
$$

注意到上面第一式的左边只是由曲面$$S$$的第一类基本量$$g_{\alpha \beta}$$的不高于二阶的偏导数构成的量，可以把它记成

$$
R_{\alpha \beta \gamma}^\delta = \frac{\partial}{\partial u^\gamma} \Gamma_{\alpha \beta}^\delta - \frac{\partial}{\partial u^\beta} \Gamma_{\alpha \gamma}^\delta + \Gamma_{\alpha \beta}^\eta \Gamma_{\eta \gamma}^\delta - \Gamma_{\alpha \gamma}^\eta \Gamma_{\eta \beta}^\delta
$$

称$$R_{\alpha \beta \gamma}^\delta$$为曲面$$S$$的第一类基本量的**Riemann记号**。如同Christoffel记号$$\Gamma_{\alpha \beta}^\gamma$$一样，Riemann记号$$R_{\alpha \beta \gamma}^\delta$$的上指标可以借助于度量矩阵$$(g_{\alpha \beta})$$下降，然后该指标又能够借助于度量矩阵的逆矩阵上升，并且规定当上指标下降时落在下指标的左边第二个位置，即

$$
R_{\alpha \delta \beta \gamma} = g_{\delta \eta} R_{\alpha \beta \gamma}^\eta, \ \ \ R_{\alpha \beta \gamma}^\delta = g^{\delta \eta} R_{\alpha \eta \beta \gamma}
$$

### Gauss-Codazzi方程

利用Riemann记号，我们可以把方程组改写为

$$
R_{\alpha \beta \gamma}^\delta = b_{\alpha \beta} b_\gamma^\delta - b_{\alpha \gamma} b_\beta^\delta
\Leftrightarrow
R_{\alpha \delta \beta \gamma} = b_{\alpha \beta} b_{\delta \gamma} - b_{\alpha \gamma} b_{\delta \beta}
$$

$$
\frac{\partial b_{\alpha \beta}}{\partial u^\gamma} - \frac{\partial b_{\alpha \gamma}}{\partial u^\beta} = \Gamma_{\alpha \gamma}^\delta b_{\delta \beta} - \Gamma_{\alpha \beta}^\delta b_{\delta \gamma}
$$

其中第一式称为**Gauss方程**，第二式称为**Codazzi方程**。很明显，Gauss-Codazzi方程是曲面的第一类基本量和第二类基本量必须满足的相容条件。

Gauss-Codazzi方程看上去比较复杂，但是实质上在Gauss方程中只包含一个方程，在Codazzi方程中只包含两个方程。为了说清楚上面的事实首先需要研究Riemann记号$$R_{\alpha \beta \gamma}^\delta$$的重要的对称性质。实际上，由$$R_{\alpha \beta \gamma}^\delta$$的定义得到

$$
\begin{aligned}
R_{\alpha \delta \beta \gamma} &= g_{\delta \eta} \bigg( \frac{\partial}{\partial u^\gamma} \Gamma_{\alpha \beta}^\eta - \frac{\partial}{\partial u^\beta} \Gamma_{\alpha \gamma}^\eta + \Gamma_{\alpha \beta}^\xi \Gamma_{\xi \gamma}^\eta - \Gamma_{\alpha \gamma}^\xi \Gamma_{\xi \beta}^\eta \bigg) \\
&= \frac{\partial \Gamma_{\delta \alpha \beta}}{\partial u^\gamma} - \frac{\partial \Gamma_{\delta \alpha \gamma}}{\partial u^\beta} - \frac{\partial g_{\delta \eta}}{\partial u^\gamma} \Gamma_{\alpha \beta}^\eta + \frac{\partial g_{\delta \eta}}{\partial u^\beta} \Gamma_{\alpha \gamma}^\eta + \Gamma_{\alpha \beta}^\xi \Gamma_{\delta \xi \gamma} - \Gamma_{\alpha \gamma}^\xi \Gamma_{\delta \xi \beta} \\
&= \frac{\partial \Gamma_{\delta \alpha \beta}}{\partial u^\gamma} - \frac{\partial \Gamma_{\delta \alpha \gamma}}{\partial u^\beta} + \Gamma_{\eta \delta \beta} \Gamma_{\alpha \gamma}^\eta - \Gamma_{\eta \delta \gamma} \Gamma_{\alpha \beta}^\eta
\end{aligned}
$$

上式右端的前两项用[Christoffel记号](/2023/12/27/DifferentialGeometry-NOTES-05.html#christoffel记号)的表达式代入，经整理得到

$$
\begin{aligned}
R_{\alpha \delta \beta \gamma} &= \frac{1}{2} \bigg( \frac{\partial^2 g_{\delta \beta}}{\partial u^\alpha \partial u^\gamma} + \frac{\partial^2 g_{\alpha \gamma}}{\partial u^\delta \partial u^\beta} - \frac{\partial^2 g_{\delta \gamma}}{\partial u^\alpha \partial u^\beta} - \frac{\partial^2 g_{\alpha \beta}}{\partial u^\delta \partial u^\gamma} \bigg) \\
&+ \Gamma_{\eta \delta \beta} \Gamma_{\alpha \gamma}^\eta - \Gamma_{\eta \delta \gamma} \Gamma_{\alpha \beta}^\eta
\end{aligned}
$$

从上式容易看出Riemann记号的下列对称性：

$$
R_{\alpha \delta \beta \gamma} = R_{\beta \gamma \alpha \delta} = -R_{\delta \alpha \beta \gamma} = -R_{\alpha \delta \gamma \beta}
$$

即把$$R_{\alpha \delta \beta \gamma}$$的下指标分为前后两组：$$R_{\alpha \delta \beta \gamma}$$的这两组指标交换位置时其数值不变，当每一组中的两个指标交换位置时$$R_{\alpha \delta \beta \gamma}$$的数值只改变它的符号。由此可见

$$
R_{1 1 \beta \gamma} = R_{2 2 \beta \gamma} = 0
$$

$$
R_{\alpha \delta 1 1} = R_{\alpha \delta 2 2} = 0
$$

很明显，Gauss方程右端同样具有这样的对称性质，所以它在实质上只包含一个方程，即

$$
R_{1 2 1 2} = b_{1 1} b_{2 2} - (b_{1 2})^2
$$

或者写成

$$
b_{1 1} b_{2 2} - (b_{1 2})^2 = R_{1 2 1 2}
$$

在Codazzi方程中，如果指标$$\beta$$、$$\gamma$$取相同的值，则该式便成为平凡的恒等式，于是有意义的情况只有$$\beta = 1$$、$$\gamma = 2$$、$$\alpha = 1, 2$$，即

$$
\begin{cases}
\begin{aligned}
\frac{\partial b_{1 1}}{\partial u^2} - \frac{\partial b_{1 2}}{\partial u^1} &= -b_{2 \delta} \Gamma_{1 1}^\delta + b_{1 \delta} \Gamma_{1 2}^\delta \\
\frac{\partial b_{2 1}}{\partial u^2} - \frac{\partial b_{2 2}}{\partial u^1} &= -b_{2 \delta} \Gamma_{2 1}^\delta + b_{1 \delta} \Gamma_{2 2}^\delta \\
\end{aligned}
\end{cases}
$$

需要强调指出的是Gauss方程和Codazzi方程是$$\boldsymbol{r}_\alpha$$和$$\boldsymbol{n}$$的**两次偏导数与求导次序的无关性**的推论。反过来，如果Gauss方程和Codazzi方程成立，则$$\boldsymbol{r}_\alpha$$和$$\boldsymbol{n}$$的两次偏导数与求导的次序无关性也成立。这就是说，如果给定了两个二次微分形式$$\varphi = g_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta$$，$$\psi = b_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta$$，它们具有相应的对称性和正定性，那么我们可以利用[自然标架场的运动公式](/2023/12/27/DifferentialGeometry-NOTES-05.html#自然标架场的运动公式)构造一阶偏微分方程组，其中$$\boldsymbol{r}$$、$$\boldsymbol{r}_1$$、$$\boldsymbol{r}_2$$、$$\boldsymbol{n}$$是向量形式的未知函数(一共是12个数量未知函数)。当已知函数$$g_{\alpha \beta}$$和$$b_{\alpha \beta}$$满足Gauss-Codazzi方程时，则自然标架场的运动公式的相容性条件

$$
\begin{cases}
\begin{aligned}
\frac{\partial}{\partial u^\gamma} \bigg( \frac{\partial \boldsymbol{r}}{\partial u^\beta} \bigg) &= \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{r}}{\partial u^\gamma} \bigg) \\
\frac{\partial}{\partial u^\gamma} \bigg( \frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} \bigg) &= \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{r}_\alpha}{\partial u^\gamma} \bigg) \\
\frac{\partial}{\partial u^\gamma} \bigg( \frac{\partial \boldsymbol{n}}{\partial u^\beta} \bigg) &= \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{n}}{\partial u^\gamma} \bigg) \\
\end{aligned}
\end{cases}
$$

成立。由此可见，根据一阶偏微分方程组的解的存在性定理，自然标架场的运动公式是可积的，即在任意给定的初始条件下程组的解是存在的，并且是唯一的。

在这里可以领略到曲面论和曲线论的本质差别。在曲线论的情形，曲率和挠率可以是弧长参数的任意函数(只要求曲率函数是正的)；而在曲面论的情形，两个基本微分形式$$\mathrm{I}$$和$$\mathrm{II}$$是彼此关联的，而不是相互独立的。这就是说在$$\mathbb{E}^3$$中一张曲面**不能够**作保持第一基本形式不变的随意的弯曲变形，在曲面作保持第一基本形式不变的变形时必定会保持曲面某种内在的弯曲性质不变。这个事实将导致开创微分几何新纪元的著名的Gauss绝妙定理。

在适当的参数系下，Riemann记号$$R_{1 2 1 2}$$和Codazzi方程能够写成便于记忆和应用的形式。为此，下面恢复用Gauss曲面论的记法。先假定我们用的是正交参数曲线网$$(u, v)$$，于是$$F \equiv 0$$，并且$$\Gamma_{\alpha \beta}^\gamma$$有简单的表达式，因此

$$
\begin{aligned}
R_{1 2 1 2} &= -\frac{1}{2} \bigg( \frac{\partial^2 E}{\partial v \partial v} + \frac{\partial^2 G}{\partial u \partial u} \bigg) - \Gamma_{\eta 1 1} \Gamma_{2 2}^\eta + \Gamma_{\eta 1 2} \Gamma_{1 2}^\eta \\
&= -\frac{1}{2} \frac{\partial}{\partial v} \bigg( \frac{\partial E}{\partial v} \bigg) + \frac{1}{4E} \frac{\partial E}{\partial v} \frac{\partial E}{\partial v} + \frac{1}{4G} \frac{\partial E}{\partial v} \frac{\partial G}{\partial v} \\
& \ \ \ \ -\frac{1}{2} \frac{\partial}{\partial u} \bigg( \frac{\partial G}{\partial u} \bigg) + \frac{1}{4G} \frac{\partial G}{\partial u} \frac{\partial G}{\partial u} + \frac{1}{4E} \frac{\partial G}{\partial u} \frac{\partial E}{\partial u}
\end{aligned}
$$

也就是

$$
R_{1 2 1 2} = -\sqrt{EG} \bigg[ \bigg( \frac{\big( \sqrt{E} \big)_v}{\sqrt{G}} \bigg)_v + \bigg( \frac{\big( \sqrt{G} \big)_u}{\sqrt{E}} \bigg)_u \bigg]
$$

如果我们在曲面上采用正交的曲率线网作为参数曲线网，则$$F = M \equiv 0$$，因此Codazzi方程成为

$$
\begin{aligned}
\frac{\partial L}{\partial v} &= -N \Gamma_{1 1}^2 + L \Gamma_{1 2}^1 = \frac{N}{2G} \frac{\partial E}{\partial v} + \frac{L}{2E} \frac{\partial E}{\partial v} = H \frac{\partial E}{\partial v} \\
\frac{\partial N}{\partial u} &= \ \ \  N \Gamma_{2 1}^2 - L \Gamma_{2 2}^1 = \frac{N}{2G} \frac{\partial G}{\partial u} + \frac{L}{2E} \frac{\partial G}{\partial u} = H \frac{\partial G}{\partial u} \\
\end{aligned}
$$

即

$$
\frac{\partial L}{\partial v} = H \frac{\partial E}{\partial v}, \ \ \  \frac{\partial N}{\partial u} = H \frac{\partial G}{\partial u}
$$

其中$$H = \frac{1}{2} \bigg( \frac{L}{E} + \frac{N}{G} \bigg)$$是曲面的平均曲率。

上面介绍的Riemann记号式和Codazzi方程是比较容易记忆的，但是必须记住它们所使用的是哪一种参数系。

## 曲面的存在性定理

本节要证明，Gauss-Codazzi方程也是以给定的两个二次微分形式为它的两个基本形式的曲面存在的充分条件。

设$$D \subset \mathbb{E}^2$$是$$\mathbb{E}^2$$中的一个单连通区域，设

$$
\begin{cases}
\begin{aligned}
\varphi &= g_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\psi &= b_{\alpha \beta} \ \mathrm{d} u^\alpha \mathrm{d} u^\beta \\
\end{aligned}
\end{cases}
$$

是定义在$$D$$内的两个二次微分形式，其中$$g_{\alpha \beta}$$在$$D$$上至少是二阶连续可微的，$$b_{\alpha \beta}$$至少是一阶连续可微的，$$g_{\alpha \beta} = g_{\beta \alpha}$$，$$b_{\alpha \beta} = b_{\beta \alpha}$$，且矩阵$$(g_{\alpha \beta})$$是正定的。用$$(g^{\alpha \beta})$$表示$$(g_{\alpha \beta})$$的逆矩阵。利用$$g_{\alpha \beta}$$及其导数构造下列各个量：

$$
\Gamma_{\gamma \alpha \beta} = \frac{1}{2} \bigg( \frac{\partial g_{\gamma \beta}}{\partial u^\alpha} + \frac{\partial g_{\alpha \gamma}}{\partial u^\beta} - \frac{\partial g_{\alpha \beta}}{\partial u^\gamma} \bigg)
$$

$$
\Gamma_{\alpha \beta}^\gamma = g^{\gamma \delta} \Gamma_{\delta \alpha \beta}
$$

$$
R_{\alpha \beta \gamma}^\delta = \frac{\partial \Gamma_{\alpha \beta}^\delta}{\partial u^\gamma} - \frac{\partial \Gamma_{\alpha \gamma}^\delta}{\partial u^\beta} + \Gamma_{\alpha \beta}^\eta \Gamma_{\eta \gamma}^\delta - \Gamma_{\alpha \gamma}^\eta \Gamma_{\eta \beta}^\delta
$$

$$
R_{\alpha \delta \beta \gamma} = g_{\delta \eta} R_{\alpha \beta \gamma}^\eta
$$

> ##### 定理5.3
> 如果由给出的两个二次微分形式$$\varphi$$、$$\psi$$满足Gauss-Codazzi方程
$$
\begin{cases}
\begin{aligned}
&b_{1 1} b_{2 2} - (b_{1 2})^2 = R_{1 2 1 2} \\
&\frac{\partial b_{1 1}}{\partial u^2} - \frac{\partial b_{1 2}}{\partial u^1} = -b_{2 \gamma} \Gamma_{1 1}^\gamma + b_{1 \gamma} \Gamma_{1 2}^\gamma \\
&\frac{\partial b_{2 1}}{\partial u^2} - \frac{\partial b_{2 2}}{\partial u^1} = -b_{2 \gamma} \Gamma_{2 1}^\gamma + b_{1 \gamma} \Gamma_{2 2}^\gamma \\
\end{aligned}
\end{cases}
$$
，则在任意一点$$(u_0^1, u_0^2) \in D$$必有它的一个邻域$$U \subset D$$，以及在空间$$\mathbb{E}^3$$中定义在该邻域$$U$$上的一个正则参数曲面$$S: \boldsymbol{r} = \boldsymbol{r} (u^1, u^2)$$，其中$$(u^1, u^2) \in D$$，使得它的第一基本形式和第二基本形式分别是$$\varphi \vert_U$$和$$\psi \vert_U$$，并且在$$\mathbb{E}^3$$中任意两块满足上述条件的曲面必定能够在$$\mathbb{E}^3$$的一个刚体运动下彼此重合。
{: .block-theorem }

**证明** 此定理的唯一性部分正是[定理5.1](/2023/12/27/DifferentialGeometry-NOTES-05.html#曲面的唯一性定理)，所以在这里只要证明满足上述条件的曲面的存在性。利用$$\varphi$$、$$\psi$$的系数及其导数可以列出如下的一阶线性齐次偏微分方程组

$$
\begin{cases}
\begin{aligned}
\frac{\partial \boldsymbol{r}}{\partial u^\beta} &= \boldsymbol{r}_\beta \\
\frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} &= \Gamma_{\alpha \beta}^\gamma \boldsymbol{r}_\gamma + b_{\alpha \beta} \boldsymbol{n} \\
\frac{\partial \boldsymbol{n}}{\partial u^\beta} &= -b_\beta^\gamma \boldsymbol{r}_\gamma
\end{aligned}
\end{cases}
$$

其中$$\boldsymbol{r}$$、$$\boldsymbol{r}_1$$、$$\boldsymbol{r}_2$$、$$\boldsymbol{n}$$都是写成向量形式的未知函数，因而一共有12个未知函数，$$u^1$$和$$u^2$$是自变量。换言之，我们把求空间$$\mathbb{E}^3$$中的曲面的问题归结为求空间$$\mathbb{E}^3$$中依赖两个参数的标架族的问题。根据一阶偏微分方程组的理论，上述方程组有解的充分必要条件是它满足相容性条件

$$
\begin{cases}
\begin{aligned}
\frac{\partial}{\partial u^\gamma} \bigg( \frac{\partial \boldsymbol{r}}{\partial u^\beta} \bigg) &= \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{r}}{\partial u^\gamma} \bigg) \\
\frac{\partial}{\partial u^\gamma} \bigg( \frac{\partial \boldsymbol{r}_\alpha}{\partial u^\beta} \bigg) &= \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{r}_\alpha}{\partial u^\gamma} \bigg) \\
\frac{\partial}{\partial u^\gamma} \bigg( \frac{\partial \boldsymbol{n}}{\partial u^\beta} \bigg) &= \frac{\partial}{\partial u^\beta} \bigg( \frac{\partial \boldsymbol{n}}{\partial u^\gamma} \bigg) \\
\end{aligned}
\end{cases}
$$

然而，上述相容性条件等价于Gauss-Codazzi方程，所以在定理的假设条件下方程组是完全可积的。这就是说，对于任意给定的$$(u_0^1, u_0^2) \in D$$，以及任意给定的初始值$$\boldsymbol{r}^0$$、$$\boldsymbol{r}_1^0$$、$$\boldsymbol{r}_2^0$$、$$\boldsymbol{n}^0$$，必有它的一个邻域$$U \subset D$$和定义在$$U$$上的函数

$$
\begin{aligned}
\boldsymbol{r} &= \boldsymbol{r} (u^1, u^2) \\
\boldsymbol{r}_1 &= \boldsymbol{r}_1 (u^1, u^2) \\
\boldsymbol{r}_2 &= \boldsymbol{r}_2 (u^1, u^2) \\
\boldsymbol{n} &= \boldsymbol{n} (u^1, u^2) \\
\end{aligned}
$$

使得它们满足偏微分方程组和初始值

$$
\begin{cases}
\begin{aligned}
\boldsymbol{r} (u_0^1, u_0^2) &= \boldsymbol{r}^0 \\
\boldsymbol{r}_1 (u_0^1, u_0^2) &= \boldsymbol{r}_1^0 \\
\boldsymbol{r}_2 (u_0^1, u_0^2) &= \boldsymbol{r}_2^0 \\
\boldsymbol{n} (u_0^1, u_0^2) &= \boldsymbol{n}^0 \\
\end{aligned}
\end{cases}
$$

问题在于：这样得到的函数$$\{ \boldsymbol{r} (u^1, u^2); \boldsymbol{r}_1 (u^1, u^2), \boldsymbol{r}_2 (u^1, u^2), \boldsymbol{n} (u^1, u^2) \}$$是否构成依赖参数$$u^1$$和$$u^2$$的标架族，即向量函数$$\boldsymbol{r}_1 (u^1, u^2)$$、$$\boldsymbol{r}_2 (u^1, u^2)$$、$$\boldsymbol{n} (u^1, u^2)$$是不是处处线性无关的？由$$\boldsymbol{r} (u^1, u^2)$$给出的向量函数是不是一张正则参数曲面？它是否以$$\varphi$$和$$\psi$$为它的第一基本形式和第二基本形式？为了得到这些问题的肯定答案，初始值$$\boldsymbol{r}^0$$、$$\boldsymbol{r}_1^0$$、$$\boldsymbol{r}_2^0$$、$$\boldsymbol{n}^0$$就不能取任意的值。因为它们将构成所求曲面在点$$(u_0^1, u_0^2)$$处的自然标架$$\{ \boldsymbol{r}^0; \boldsymbol{r}_1^0, \boldsymbol{r}_2^0, \boldsymbol{n}^0 \}$$，因此必须假定它们的度量系数满足下列条件：

$$
\begin{aligned}
&\boldsymbol{r}_\alpha^0 \cdot \boldsymbol{r}_\beta^0 = g_{\alpha \beta} (u_0^1, u_0^2) \\
&\boldsymbol{r}_\alpha^0 \cdot \boldsymbol{n}^0 = 0 \\
&\boldsymbol{n}^0 \cdot \boldsymbol{n}^0 = 1 \\
&(\boldsymbol{r}_1^0, \boldsymbol{r}_2^0, \boldsymbol{n}^0) \gt 0
\end{aligned}
$$

我们要证明：初始值满足上述条件时，偏微分方程组满足初始条件的解$$\boldsymbol{r} = \boldsymbol{r} (u^1, u^2)$$给出了符合定理要求的正则参数曲面。

设$$\boldsymbol{r} (u^1, u^2)$$、$$\boldsymbol{r}_1 (u^1, u^2)$$、$$\boldsymbol{r}_2 (u^1, u^2)$$、$$\boldsymbol{n} (u^1, u^2)$$是偏微分方程组在初始条件下的解，考虑一组函数

$$
\begin{aligned}
f_{\alpha \beta} (u^1, u^2) &= \boldsymbol{r}_{\alpha} (u^1, u^2) \cdot \boldsymbol{r}_{\beta} (u^1, u^2) - g_{\alpha \beta} (u^1, u^2) \\
f_{\alpha} (u^1, u^2) &= \boldsymbol{r}_{\alpha} (u^1, u^2) \cdot \boldsymbol{n} (u^1, u^2) \\
f (u^1, u^2) &= \boldsymbol{n} (u^1, u^2) \cdot \boldsymbol{n} (u^1, u^2) - 1 \\
\end{aligned}
$$

根据点$$(u_0^1, u_0^2)$$处的初始条件，这组函数满足

$$
f_{\alpha \beta} (u_0^1, u_0^2) = f_{\alpha} (u_0^1, u_0^2) = f (u_0^1, u_0^2) = 0
$$

对函数$$f_{\alpha \beta} (u^1, u^2)$$、$$f_{\alpha} (u^1, u^2)$$、$$f (u^1, u^2)$$求偏导数，并利用函数$$\boldsymbol{r}_\alpha (u^1, u^2)$$和$$\boldsymbol{n} (u^1, u^2)$$所满足的偏微分方程，得到

$$
\begin{cases}
\begin{aligned}
\frac{\partial f_{\alpha \beta}}{\partial u^\gamma} &= \Gamma_{\gamma \alpha}^\delta f_{\delta \beta} + \Gamma_{\gamma \beta}^\delta f_{\delta \alpha} + b_{\gamma \alpha} f_{\beta} + b_{\gamma \beta} f_{\alpha} \\
\frac{\partial f_\alpha}{\partial u^\gamma} &= -b_\gamma^\delta f_{\delta \alpha} + \Gamma_{\gamma \alpha}^\delta f_\delta + b_{\gamma \alpha} f \\
\frac{\partial f}{\partial u^\gamma} &= -2b_\gamma^\delta f_\delta
\end{aligned}
\end{cases}
$$

注意到这里方程组在初始条件下的一个解是零解。根据一阶偏微分方程组在已知初始条件下的唯一性，方程组的解只能是零解，即在$$U$$上有恒等式

$$
f_{\alpha \beta} (u^1, u^2) = f_{\alpha} (u^1, u^2) = f(u^1, u^2) \equiv 0
$$

所以在$$U$$上如下的式子恒成立：

$$
\begin{aligned}
\boldsymbol{r}_{\alpha} (u^1, u^2) \cdot \boldsymbol{r}_{\beta} (u^1, u^2) &= g_{\alpha \beta} (u^1, u^2) \\
\boldsymbol{r}_{\alpha} (u^1, u^2) \cdot \boldsymbol{n} (u^1, u^2) &=0 \\
\boldsymbol{n} (u^1, u^2) \cdot \boldsymbol{n} (u^1, u^2) &=1 \\
\end{aligned}
$$

另外，由上式得到

$$
\begin{aligned}
(\boldsymbol{r}_1, \boldsymbol{r}_2, \boldsymbol{n})^2 &= \det
\begin{pmatrix}
\begin{pmatrix}
\boldsymbol{r}_1 \\ \boldsymbol{r}_2 \\ \boldsymbol{n}
\end{pmatrix}
\cdot
\begin{pmatrix}
\boldsymbol{r}_1 & \boldsymbol{r}_2 & \boldsymbol{n}
\end{pmatrix}
\end{pmatrix} \\
&= \det
\begin{pmatrix}
g_{11} & g_{12} & 0 \\
g_{21} & g_{22} & 0 \\
0 & 0 & 1 \\
\end{pmatrix}
=\det (g_{\alpha \beta}) \gt 0
\end{aligned}
$$

即向量函数$$\boldsymbol{r}_1 (u^1, u^2)$$、$$\boldsymbol{r}_2 (u^1, u^2)$$、$$\boldsymbol{n} (u^1, u^2)$$是处处线性无关的。由于解的连续性，并且$$(\boldsymbol{r}_1 (u_0^1, u_0^2), \boldsymbol{r}_2 (u_0^1, u_0^2), \boldsymbol{n} (u_0^1, u_0^2)) \gt 0$$，因此处处有

$$
(\boldsymbol{r}_1 (u^1, u^2), \boldsymbol{r}_2 (u^1, u^2), \boldsymbol{n} (u^1, u^2)) \gt 0
$$

即$$\{ \boldsymbol{r}_1 (u^1, u^2), \boldsymbol{r}_2 (u^1, u^2), \boldsymbol{n} (u^1, u^2) \}$$成右手系。因为$$\boldsymbol{r}_1 \perp \boldsymbol{n}$$、$$\boldsymbol{r}_2 \perp \boldsymbol{n}$$、$$\boldsymbol{n} \cdot \boldsymbol{n} = 1$$，所以

$$
\boldsymbol{n} = \frac{\boldsymbol{r}_1 \times \boldsymbol{r}_2}{\vert \boldsymbol{r}_1 \times \boldsymbol{r}_2 \vert}
$$

从函数$$\boldsymbol{r} (u^1, u^1)$$满足偏微分方程组的第一式可知，若把$$\boldsymbol{r} = \boldsymbol{r} (u^1, u^2)$$看作$$\mathbb{E}^3$$中的一块曲面，则$$\boldsymbol{r}_1 (u^1, u^2)$$、$$\boldsymbol{r}_2 (u^1, u^2)$$是该曲面的参数曲线的切向量。因为这两个切向量线性无关，故该曲面是正则参数曲面，并且$$\boldsymbol{n} (u^1, u^2)$$是它的单位法向量。同时，$$\varphi$$是它的第一基本形式，$$\psi$$是它的第二基本形式。证毕∎

从曲面的存在性定理的证明可以看出，把曲面看作一族标架的观念是十分重要的和基本的。把曲面放到标架空间中去看， 未知函数的偏导数不再含有新的未知函数。后面我们要更加细致地研究空间$$\mathbb{E}^3$$中的标架族的理论，而曲面的存在性定理就成为标架族存在性定理的一个特例。

## Gauss定理

### Guass绝妙定理

Gauss方程本身蕴涵着一个十分精彩的结果。[曲面论基本方程](/2023/12/27/DifferentialGeometry-NOTES-05.html#gauss-codazzi方程)已经导出

$$
b_{1 1} b_{2 2} - (b_{1 2})^2 = R_{1 2 1 2}
$$

其中Riemann记号$$R_{1 2 1 2}$$是用曲面$$S$$的第一类基本量$$g_{\alpha \beta}$$及其一阶偏导数和二阶偏导数构造的量。将上式两边分别除以$$g_{11} g_{22} - (g_{12})^2$$，则得

$$
\frac{b_{1 1} b_{2 2} - (b_{1 2})^2}{g_{11} g_{22} - (g_{12})^2} = \frac{R_{1 2 1 2}}{g_{11} g_{22} - (g_{12})^2}
$$

注意到上式的左端是曲面$$S$$的[Gauss曲率](/2023/11/10/DifferentialGeometry-NOTES-04.html#平均曲率和gauss曲率)

$$
K = \frac{b_{1 1} b_{2 2} - (b_{1 2})^2}{g_{11} g_{22} - (g_{12})^2} = \kappa_1 \kappa_2
$$

而右端$$\frac{R_{1 2 1 2}}{g_{11} g_{22} - (g_{12})^2}$$只依赖曲面$$S$$的第一类基本量及其偏导数，即

$$
K = \kappa_1 \kappa_2 = \frac{R_{1 2 1 2}}{g_{11} g_{22} - (g_{12})^2}
$$

这就是说，虽然曲面$$S$$的主曲率$$\kappa_1$$、$$\kappa_2$$是由曲面$$S$$在空间$$\mathbb{E}^3$$中的形状确定的，即它们是通过曲面$$S$$的第一基本形式和第二基本形式计算出来的，但是它们的乘积$$K = \kappa_1 \kappa_2$$却只依赖曲面$$S$$的第一基本形式，而与第二基本形式无关。换句话说，如果$$\mathbb{E}^3$$中的两个曲面$$S_1$$和$$S_2$$有相同的第一基本形式，而它们的第二基本形式却未必相同，则它们仍然有相同的Gauss曲率$$K$$。[定理3.5](/2023/10/18/DifferentialGeometry-NOTES-03.html#保长对应)断言，两个曲面能够建立保长对应的充分必要条件是，它们的第一基本形式相同。因此获得下面的**Guass绝妙定理(Egregium Theorem)**：

> ##### 定理5.4
> 曲面的Gauss 曲率是曲面在保长变换下的不变量。
{: .block-theorem }

Gauss的绝妙定理是微分几何学发展过程中的里程碑。Gauss 的这个惊人的发现开创了微分几何学的一个新的纪元。正是因为Gauss的这个发现，使我们能够研究一张抽象的具有第一基本形式的曲面，即二维平面$$\mathbb{E}^2$$上的一个区域$$D$$以及定义在$$D$$上的一个正定的对称二次微分形式(记成$$\mathrm{d} s^2$$)，而不是在空间$$\mathbb{E}^3$$中的一张具体的曲面。Gauss绝妙定理说明，曲面的度量本身蕴涵着一定的弯曲性质，这正是曲线所不具有的特性。例如，球面的Gauss曲率是正的常数，平面的Gauss曲率是零，因此球面不能够保持长度不变地摊成一张平面。反过来，平面无论如何都不可能保持长度不变地弯曲成一个球面。专门研究曲面上由它的第一基本形式决定的几何学称为曲面的**内蕴几何学**。后来，Riemann发扬了Gauss的思想提出了高维的内蕴微分几何学的观念，即在高维空间$$\mathbb{E}^n$$的一个区域$$M$$上给定一个正定的对称二次微分形式

$$
\mathrm{d} s^2 = g_{\alpha \beta} \mathrm{d} u^\alpha \mathrm{d} u^\beta , \ \ \ 1 \leq \alpha, \beta \leq n
$$

然后研究它的弯曲性质，这就是现在所称的Riemann几何学。在下一章，我们将研究曲面的更多的内蕴几何性质。

### 可展曲面

[前面](/2023/12/27/DifferentialGeometry-NOTES-05.html#gauss-codazzi方程)提到过，当曲面$$S$$上取正交参数曲线网$$(u, v)$$时，$$F \equiv 0$$，并且

$$
R_{1 2 1 2} = -\sqrt{EG} \bigg[ \bigg( \frac{\big( \sqrt{E} \big)_v}{\sqrt{G}} \bigg)_v + \bigg( \frac{\big( \sqrt{G} \big)_u}{\sqrt{E}} \bigg)_u \bigg]
$$

所以

$$
K = -\frac{1}{\sqrt{EG}} \bigg[ \bigg( \frac{\big( \sqrt{E} \big)_v}{\sqrt{G}} \bigg)_v + \bigg( \frac{\big( \sqrt{G} \big)_u}{\sqrt{E}} \bigg)_u \bigg]
$$

特别地，如果在曲面$$S$$上取[等温参数系](/2023/10/18/DifferentialGeometry-NOTES-03.html#保角对应)$$(u, v)$$，则曲面$$S$$的第一基本形式成为$$\mathrm{I} = \lambda^2 ((\mathrm{d} u)^2 + (\mathrm{d} v)^2)$$，于是它的Gauss曲率是

$$
K = -\frac{1}{\lambda^2} \bigg( \frac{\partial^2}{\partial u^2} + \frac{\partial^2}{\partial v^2} \bigg) \log \lambda
$$

根据[定理3.11](/2023/10/18/DifferentialGeometry-NOTES-03.html#可展曲面-1)，可展曲面在局部上总是可以和平面的一个区域建立保长对应。因此，根据Gauss绝妙定理得知，可展曲面的Gauss曲率$$K$$恒等于零。另外，可展曲面的直母线是曲率线，因此可展曲面沿直母线的主曲率为零，由此也能得知它的Gauss曲率为零。反过来，这个条件也是判断已知曲面是可展曲面的充分条件。

> ##### 定理5.5
> 空间$$\mathbb{E}^3$$的一块无脐点的曲面$$S$$是可展曲面的充分必要条件是，它的Gauss曲率$$K$$恒等于零。
{: .block-theorem }

**证明** 必要性已经证明过了，现在只要证明充分性成立。设曲面$$S$$的Gauss曲率$$K \equiv 0$$。现在假定点$$p \in S$$，由于处处没有脐点，故有点$$p$$在曲面$$S$$上的一个邻域$$U$$，使得在$$U$$内存在正交曲率线网作为参数曲线网$$(u, v)$$，所以$$F = M \equiv 0$$，并且

$$
K = \frac{LN}{EG} \equiv 0
$$

不妨假定$$v$$-曲线对应的主曲率$$\kappa_2 = \frac{N}{G}$$恒等于零，于是$$N \equiv 0$$，$$L \neq 0$$。由Codazzi方程得知


$$
H \frac{\partial G}{\partial u} = \frac{\partial N}{\partial u} = 0
$$

$$
2H = \frac{L}{E} + \frac{N}{G} = \frac{L}{E} \neq 0
$$

因此

$$
\frac{\partial G}{\partial u} = 0
$$

我们首先要证明曲面$$S$$是直纹面，更具体地说我们要证明每一条$$v$$-曲线是直线。为此只要证明$$v$$-曲线的切方向不变，即$$\boldsymbol{r}_{vv} \times \boldsymbol{r}_v \equiv \boldsymbol{0}$$。事实上，根据自然标架的运动公式我们有

$$
\boldsymbol{r}_{vv} = \Gamma_{22}^1 \boldsymbol{r}_u + \Gamma_{22}^2 \boldsymbol{r}_v + N \boldsymbol{n} = \Gamma_{22}^1 \boldsymbol{r}_u + \Gamma_{22}^2 \boldsymbol{r}_v
$$

$$
\boldsymbol{r}_{vv} \times \boldsymbol{r}_{v} = \Gamma_{22}^1 \boldsymbol{r}_u \times \boldsymbol{r}_{v}
$$

由$$\frac{\partial G}{\partial u} = 0$$得知

$$
\Gamma_{22}^1 = -\frac{1}{2E} \frac{\partial G}{\partial u} \equiv 0
$$

故有

$$
\boldsymbol{r}_{vv} \times \boldsymbol{r}_{v} \equiv \boldsymbol{0}
$$

得证。下面要证明曲面$$S$$的单位法向量$$\boldsymbol{n}$$沿$$v$$-曲线是不变的。实际上，根据定义和假定，我们有

$$
\boldsymbol{n}_v \cdot \boldsymbol{r}_u = -M = 0
$$

$$
\boldsymbol{n}_v \cdot \boldsymbol{r}_v = -N = 0
$$

$$
\boldsymbol{n}_v \cdot \boldsymbol{n} = 0
$$

因此$$\boldsymbol{n}_v$$只能是零向量，故$$\boldsymbol{n}$$沿$$v$$-曲线是不变的，所以$$S$$是可展曲面。证毕∎

> ##### 定理5.6
> 无脐点曲面$$S$$是可展曲面的充分必要条件是是它能够和一块平面建立保长对应。
{: .block-theorem }

**证明** [定理3.11](/2023/10/18/DifferentialGeometry-NOTES-03.html#可展曲面-1)已经证明可展曲面可以和一块平面建立保长对应。现在假定曲面$$S$$能够和一块平面建立保长对应，则根据Gauss绝妙定理，曲面$$S$$的Gauss曲率恒等于零，因而由定理5.5得知曲面$$S$$是可展曲面。证毕∎

### 法曲率

下面我们要证明一个重要的定理，它说明在一般情形下曲面的[法曲率](/2023/11/10/DifferentialGeometry-NOTES-04.html#法曲率)的确包含了曲面形状的全部信息。

> ##### 定理5.7
> 设$$\sigma: S_1 \rightarrow S_2$$是从曲面$$S_1$$到$$S_2$$的连续可微映射，其中曲面$$S_1$$没有脐点，并且它的Gauss曲率$$K$$不为零。如果曲面$$S_1$$和$$S_2$$在所有的对应点、沿所有的对应切方向的法曲率保持不变，则有空间$$\mathbb{E}^3$$中的一个刚体运动$$\tilde{\sigma}: \mathbb{E}^3 \rightarrow \mathbb{E}^3$$使得$$\sigma = \tilde{\sigma} \vert_{S_1}$$。
{: .block-theorem }

**证明** 因为在曲面$$S_1$$上没有跻点，所以在曲面$$S_1$$上可以取正交曲率线网作为参数曲线网$$(u, v)$$，于是曲面$$S_1$$的两个基本微分形式成为

$$
\begin{cases}
\begin{aligned}
\mathrm{I} &= E (\mathrm{d} u)^2 + G (\mathrm{d} v)^2 \\
\mathrm{II} &= L (\mathrm{d} u)^2 + N (\mathrm{d} v)^2 \\
\end{aligned}
\end{cases}
$$

并且

$$
L = \kappa_1 E, \ \ \
N = \kappa_2 G, \ \ \
\kappa_1 \gt \kappa_2, \ \ \
K = \kappa_1 \kappa_2 \neq 0
$$

由于假定曲面$$S_1$$和$$S_2$$在所有的对应点、沿所有的对应切方向的法曲率保持不变，因此切映射$$\sigma_*$$首先应该是处处非退化的。于是$$(u, v)$$也可以作为曲面$$S_2$$上的参数，从而$$\sigma$$是曲面$$S_1$$和曲面$$S_2$$上有相同参数值的点之间的对应。不妨设曲面$$S_2$$的两个基本微分形式是

$$
\begin{cases}
\begin{aligned}
\mathrm{\tilde{I}} &= \tilde{E} (\mathrm{d} u)^2 + 2 \tilde{F} \mathrm{d} u \mathrm{d} v + \tilde{G} (\mathrm{d} v)^2 \\
\mathrm{\tilde{II}} &= \tilde{L} (\mathrm{d} u)^2 + 2 \tilde{M} \mathrm{d} u \mathrm{d} v + \tilde{N} (\mathrm{d} v)^2 \\
\end{aligned}
\end{cases}
$$

因为在对应$$\sigma$$下，曲面$$S_1$$和$$S_2$$在所有的对应点、沿所有的对应切方向的法曲率都相等，因此曲面$$S_2$$沿$$u$$-曲线方向的法曲率应该是$$\kappa_1$$，沿$$v$$-曲线方向的法曲率应该是$$\kappa_2$$，并且它们同样是曲面$$S_2$$在每一点沿各个切方向的法曲率的最大值和最小值。这就是说，$$(u,v)$$也是曲面$$S_2$$上的正交曲率线网，故有

$$
\tilde{F} = \tilde{M} = 0, \ \ \
\tilde{L} = \kappa_1 \tilde{E}, \ \ \
\tilde{N} = \kappa_2 \tilde{G}
$$

这样，曲面$$S_1$$和$$S_2$$在对应点、沿对应切方向的法曲率相等的条件成为

$$
\frac{\kappa_1 E (\mathrm{d} u)^2 + \kappa_2 G (\mathrm{d} v)^2}{E (\mathrm{d} u)^2 + G (\mathrm{d} v)^2} = \frac{\kappa_1 \tilde{E} (\mathrm{d} u)^2 + \kappa_2 \tilde{G} (\mathrm{d} v)^2}{\tilde{E} (\mathrm{d} u)^2 + \tilde{G} (\mathrm{d} v)^2}
$$

将上式展开得到

$$
(\kappa_1 - \kappa_2) (\tilde{E} G - E \tilde{G}) (\mathrm{d} u)^2 (\mathrm{d} v)^2 = 0
$$

由于$$\kappa_1 - \kappa_2 \gt 0$$，并且上式是关于$$\mathrm{d} u$$、$$\mathrm{d} v$$的恒等式，故有

$$
\tilde{E} G - E \tilde{G} = 0
$$

设

$$
\frac{\tilde{E}}{E} = \frac{\tilde{G}}{G} = \lambda
$$

因此我们需要证明$$\lambda = 1$$。

由于采用了正交的曲率线网作为参数曲线网，曲面$$S_1$$和$$S_2$$的Codazzi方程是

$$
\frac{\partial L}{\partial v} = H \frac{\partial E}{\partial v}, \ \ \
\frac{\partial N}{\partial u} = H \frac{\partial G}{\partial u}
$$

$$
\frac{\partial \tilde{L}}{\partial v} = H \frac{\partial \tilde{E}}{\partial v}, \ \ \
\frac{\partial \tilde{N}}{\partial u} = H \frac{\partial \tilde{G}}{\partial u}
$$

其中$$H = \frac{1}{2} (\kappa_1 + \kappa_2)$$。因为$$\tilde{E} = \lambda E$$、$$\tilde{G} = \lambda G$$、$$\tilde{L} = \lambda L$$、$$\tilde{N} = \lambda N$$，将上式展开得到

$$
(\kappa_1 - \kappa_2) \frac{\partial \lambda}{\partial u} = (\kappa_1 - \kappa_2) \frac{\partial \lambda}{\partial v} = 0
$$

所以

$$
\frac{\partial \lambda}{\partial u} = \frac{\partial \lambda}{\partial v} = 0
$$

即$$\lambda$$是常数。将$$\tilde{E} = \lambda E$$、$$\tilde{G} = \lambda G$$代入$$K = -\frac{1}{\sqrt{EG}} \bigg[ \bigg( \frac{\big( \sqrt{E} \big)_v}{\sqrt{G}} \bigg)_v + \bigg( \frac{\big( \sqrt{G} \big)_u}{\sqrt{E}} \bigg)_u \bigg]$$得到

$$
\tilde{K} = \frac{1}{\lambda} K
$$

但是，在另一方面

$$
\tilde{K} = \kappa_1 \kappa_2 = K \neq 0
$$

故得

$$
\lambda = 1
$$

由此可见，曲面$$S_1$$和$$S_2$$有相同的第一基本形式和第二基本形式。根据[定理5.2](/2023/12/27/DifferentialGeometry-NOTES-05.html#曲面的唯一性定理)，在空间$$\mathbb{E}^3$$中存在一个刚体运动$$\tilde{\sigma}$$使得$$\sigma = \tilde{\sigma} \vert_{S_1}$$。证毕∎