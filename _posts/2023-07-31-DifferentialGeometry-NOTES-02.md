---
layout: post
title: 微分几何笔记02-曲线论
date: 2023-07-31
description: 曲线论相关知识
tags: CG Math
categories: Differential-Geometry
toc:
  sidebar: left
---


> 这个系列是北京大学陈维桓教授《微分几何(第二版)》的学习笔记，主要涉及古典微分几何中曲线曲面理论的相关知识。系统学习微分几何对于理解计算机图形学中的各种几何处理算法是十分有益的。本节介绍古典微分几何中的曲线论。
{: .block-preface }

## 正则参数曲线

### 参数曲线

首先我们来介绍**曲线**的概念。在微分几何中我们认为$$\mathbb{E}^3$$中的曲线$$C$$是从区间$$[a, b]$$到$$\mathbb{E}^3$$的一个连续映射，记为

$$
p: [a, b] \rightarrow \mathbb{E}^3
$$

称为**参数曲线**。给定$$\mathbb{E}^3$$中的正交标架$$\{O; \boldsymbol{i}, \boldsymbol{j}, \boldsymbol{k} \}$$，则曲线$$C$$上的点$$p(t)$$等同于向量$$\overrightarrow{Op(t)}$$。令$$\boldsymbol{r}(t) = \overrightarrow{Op(t)}$$，则$$\boldsymbol{r}(t)$$可以表示为向量函数

$$
\boldsymbol{r}(t) = x(t) \ \boldsymbol{i} + y(t) \ \boldsymbol{j} + z(t) \ \boldsymbol{k}, \ \ t \in [a, b]
$$

记为

$$
\boldsymbol{r}(t) = (x(t), y(t), z(t)), \ \ t \in [a, b]
$$

其中$$t$$是曲线的参数，上式称为曲线$$C$$的**参数方程**。

### 曲线的切线

根据导数定义可知

$$
\boldsymbol{r}'(t) = \lim_{\Delta t \to 0} \frac{\boldsymbol{r}(t + \Delta t) - \boldsymbol{r}(t)}{\Delta t} = (x'(t), y'(t), z'(t))
$$

如果坐标函数$$x'(t)$$，$$y'(t)$$，$$z'(t)$$是连续可微的，则称曲线$$\boldsymbol{r}(t)$$是连续可微的。这个概念与笛卡尔坐标系的选取无关。

导数$$\boldsymbol{r}'(t)$$有明显的几何意义。$$\boldsymbol{r}(t + \Delta t) - \boldsymbol{r}(t)$$表示点$$\boldsymbol{r}(t)$$到点$$\boldsymbol{r}(t + \Delta t)$$的有向线段，因此$$\frac{\boldsymbol{r}(t + \Delta t) - \boldsymbol{r}(t)}{\Delta t}$$表示经过点$$\boldsymbol{r}(t)$$和点$$\boldsymbol{r}(t + \Delta t)$$的割线$$l$$方向向量。当$$\Delta t \to 0$$时，割线$$l$$的极限位置就是曲线在点$$\boldsymbol{r}(t)$$处的**切线**。如果$$\boldsymbol{r}'(t) \neq \boldsymbol{0}$$，则$$\boldsymbol{r}'(t)$$是曲线在点$$\boldsymbol{r}(t)$$处的切线的方向向量，称为曲线的**切向量**如下图所示。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zIFDBhi.png" width="80%">
</div>

此时曲线在点$$\boldsymbol{r}(t)$$处的切线是可以完全确定的，这样的点称为曲线的**正则点**。而曲线在正则点处的切线方程为

$$
\boldsymbol{X}(u) = \boldsymbol{r}(t) + u \boldsymbol{r}'(t)
$$

其中$$t$$是固定的，$$u$$是切线上点的参数，$$\boldsymbol{X}(u)$$是从原点$$O$$指向切线上参数为$$u$$的点的有向线段。

### 正则参数曲线

我们称参数曲线$$\boldsymbol{r}(t)$$为**正则参数曲线**当且仅当它满足以下两个条件：

(1) $$\boldsymbol{r}(t)$$至少是自变量$$t$$的三次以上连续可微函数 *(曲线的几何不变量涉及$$\boldsymbol{r}(t)$$的三次导数)*

(2) 处处是正则点，即对任意的$$t$$有$$\boldsymbol{r}'(t) \neq \boldsymbol{0}$$

我们把参数$$t$$增大的方向称为该参数曲线的正向，因此$$\boldsymbol{r}'(t)$$正好指向曲线的正向。

### 参数变换

显然曲线参数方程的表达式与坐标系的选取有关，另外在固定的笛卡尔直角坐标系下还可以对参数进行一些变换。为了保证正则参数曲线的两个条件在参数变换下保持不变，我们要求参数变换$$t = t(u)$$需要满足以下两个条件：

(1) $$t(u)$$是$$u$$的三次以上连续可微函数

(2) $$t'(u)$$处处不为$$0$$

这样在参数变换$$t = t(u)$$下，曲线的参数方程可以表示为$$\boldsymbol{r} (t(u))$$，简记为$$\boldsymbol{r}(u)$$。根据求导的链式法则有

$$
\boldsymbol{r}'(u) = \frac{\mathrm{d}}{\mathrm{d} u} \boldsymbol{r}(t(u)) = \boldsymbol{r}'(t(u)) \cdot t'(u)
$$

如果容许的参数变换还要求$$t'(u) \gt 0$$，则这种容许的参数变换保持曲线的定向不变。

## 曲线的弧长

设$$\mathbb{E}^3$$中的一条正则曲线$$C$$的参数方程为$$\boldsymbol{r} = \boldsymbol{r}(t)$$，$$t \in [a, b]$$，我们定义

$$
s = \int_a^b \vert \boldsymbol{r}'(t) \vert \ \mathrm{d} t
$$

则$$s$$是该曲线的一个不变量，即它与$$\mathbb{E}^3$$中笛卡尔直角坐标系的选取无关，也与该曲线的保持定向的容许参数变换无关。这里简单证明一下$$s$$对于保持定向的参数变换不变：

设参数变换为

$$
t = t(u), \ \ t'(u) \gt 0, \ \ u \in [\alpha, \beta]
$$

并且

$$
t(\alpha) = a, \ \ t(\beta) = b
$$

因此

$$
\bigg\vert \frac{\mathrm{d} \boldsymbol{r}(t(u))}{\mathrm{d} u} \bigg\vert = \bigg\vert \frac{\mathrm{d} \boldsymbol{r}}{\mathrm{d} t}(t(u)) \bigg\vert \cdot \frac{\mathrm{d} t}{\mathrm{d} u}
$$

根据积分变量替换公式有

$$
\int_a^b \bigg\vert \frac{\mathrm{d} \boldsymbol{r}(t)}{\mathrm{d} t} \bigg\vert \ \mathrm{d} t 
= \int_\alpha^\beta \bigg\vert \frac{\mathrm{d} \boldsymbol{r}}{\mathrm{d} t}(t(u)) \bigg\vert \cdot \frac{\mathrm{d} t}{\mathrm{d} u} \ \mathrm{d} u 
= \int_\alpha^\beta \bigg\vert \frac{\mathrm{d} \boldsymbol{r}(t(u))}{\mathrm{d} u} \bigg\vert \ \mathrm{d}u
$$

因此$$s$$与参数变换无关。证毕∎

不变量$$s$$的几何意义是该曲线的长度。可以证明对于对于区间$$[a, b]$$的任意一个分割$$a = t_0 \lt t_1 \lt ... \lt t_n = b$$，下面的极限成立：

$$
\lim_{\lambda \to 0} \sum_{i=1}^n \vert \boldsymbol{r}(t_i) - \boldsymbol{r}(t_{i-1}) \vert = \int_a^b \vert \boldsymbol{r}'(t) \vert \ \mathrm{d} t
$$

$$
\lambda = \max \{ \vert \Delta t_i \vert ; \ i = 1, ..., n \}
$$

不难发现等式的左端$$\sum_{i=1}^n \vert \boldsymbol{r}(t_i) - \boldsymbol{r}(t_{i-1}) \vert$$是顶点依次$$\boldsymbol{r}(t_0)$$，$$\boldsymbol{r}(t_1)$$，...，$$\boldsymbol{r}(t_n)$$为的折线长度，因此$$\int_a^b \vert \boldsymbol{r}'(t) \vert \ \mathrm{d} t$$表示将曲线进行不断细分所得到的折线段长度的极限，也就是曲线的长度，称为**弧长**。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5CSudyd.png" width="80%">
</div>

### 弧长参数

对任意$$a \leq t \leq b$$令

$$
s(t) = \int_a^t \vert \boldsymbol{r}'(t) \vert \ \mathrm{d} t
$$

则$$s(t)$$是曲线$$C$$由$$a$$到$$t$$的弧长。由于$$s(t)$$是关于$$t$$的三次以上连续可微函数，并且

$$
\frac{\mathrm{d} s(t)}{\mathrm{d} t} = \vert \boldsymbol{r}'(t) \vert \gt 0
$$

于是我们可以把弧长作为正则曲线的参数，这种参数称为曲线的**弧长参数**。弧长参数由曲线本身确定到至多相差一个常数(这反映了度量曲线长度的起点不同)，与表示曲线的笛卡尔直角坐标系的选取无关，与曲线原来的参数取法也无关。对弧长参数进行微分可以得到

$$
\mathrm{d} s = \vert \boldsymbol{r}'(t) \vert \ \mathrm{d} t
$$

上式中$$\mathrm{d} s$$也是曲线的不变量，称为曲线的**弧长元素**。

在大多数情况下我们都无法显式地写出弧长参数的表达式，因此判定已知参数$$t$$是否是弧长参数是十分重要的。我们可以使用如下定理来进行判断：

> ## 定理2.1
> 设$$\boldsymbol{r} = \boldsymbol{r}(t) \ (a \leq t \leq b)$$是$$\mathbb{E}^3$$中的一条正则参数曲线，则$$t$$是它的弧长参数的充要条件是$$\vert \boldsymbol{r}'(t) \vert \equiv 1$$。
{: .block-theorem }

这里简单证明如下：当$$t$$为弧长参数$$s$$时有$$\mathrm{d} s = \mathrm{d} t$$，所以必有$$\vert \boldsymbol{r}'(t) \vert \equiv 1$$，反之亦然。

[定理2.1](/2023/07/31/DifferentialGeometry-NOTES-02.html#弧长参数)的几何意义是曲线以弧长为参数的充要条件是它的切向量场为单位向量场。

## 曲线的曲率和Frenet标架

设曲线$$C$$的方程是$$\boldsymbol{r}(s)$$，其中$$s$$是曲线的弧长参数。令$$\boldsymbol{\alpha}(s) = \boldsymbol{r}'(s)$$，则$$\boldsymbol{\alpha}(s)$$是曲线$$C$$在$$s$$处的方向向量，其方向变化的快慢反映了曲线的弯曲程度，我们可以使用$$\big\vert \frac{\mathrm{d} \boldsymbol{\alpha}}{\mathrm{d} s} \big\vert$$来衡量。

> ## 定理2.2 
> 设$$\boldsymbol{\alpha}(s)$$是曲线$$\boldsymbol{r}(s)$$的单位切向量场，$$s$$是弧长参数，用$$\Delta \theta$$表示切向量$$\boldsymbol{\alpha}(s + \Delta s)$$和$$\boldsymbol{\alpha}(s)$$之间的夹角，则
$$
\lim_{\Delta s \to 0} \bigg \vert \frac{\Delta \theta}{\Delta s} \bigg \vert = \big\vert \frac{\mathrm{d} \boldsymbol{\alpha}}{\mathrm{d} s} \big\vert
$$
{: .block-theorem }

**证明** 把曲线$$C$$上所有的单位切向量$$\boldsymbol{\alpha}(s)$$平行移动，使它们的起点都放在原点$$O$$处，则这些切向量的端点便描出单位球面上的一条曲线，于是切向量$$\boldsymbol{\alpha}(s + \Delta s)$$和$$\boldsymbol{\alpha}(s)$$之间的夹角$$\Delta \theta$$是在单位球面上从$$\boldsymbol{\alpha}(s + \Delta s)$$到$$\boldsymbol{\alpha}(s)$$的大圆弧的弧长，而$$\boldsymbol{\alpha}(s + \Delta s) - \boldsymbol{\alpha}(s)$$正好是该角所对的弦长，所以

$$
\begin{aligned}
\bigg\vert \frac{\mathrm{d} \boldsymbol{\alpha}}{\mathrm{d} s} \bigg\vert &= \lim_{\Delta s \to 0} \frac{\vert \boldsymbol{\alpha}(s + \Delta s) - \boldsymbol{\alpha}(s) \vert}{\vert \Delta s \vert} \\
&= \lim_{\Delta s \to 0} \frac{2 \vert \sin \frac{\Delta \theta}{2} \vert}{\vert \Delta s \vert} \\
&= \lim_{\Delta s \to 0} \bigg\vert \frac{\Delta \theta}{\Delta s} \bigg\vert
\end{aligned}
$$

证毕∎

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GhhlaS3.png" width="80%">
</div>

> ## 定义2.1
> 设曲线$$C$$的方程是$$\boldsymbol{r}(s)$$，其中$$s$$是曲线的弧长参数。令$$\kappa(s) = \big\vert \frac{\mathrm{d} \boldsymbol{\alpha}}{\mathrm{d} s} \big\vert = \vert \boldsymbol{r}''(s) \vert$$，则称$$\kappa(s)$$为曲线$$\boldsymbol{r}(s)$$在$$s$$处的**曲率**，并且称$$\frac{\mathrm{d} \boldsymbol{\alpha}}{\mathrm{d} s}$$为该曲线的**曲率向量**。
{: .block-definition }

> ## 定理2.3
> 曲线$$C$$是一条直线当且仅当它的曲率$$\kappa(s) \equiv 0$$。
{: .block-theorem }

**证明** 直线$$C$$的参数方程为$$\boldsymbol{r}(s) = \boldsymbol{r}_0 + \boldsymbol{\alpha}_0 s$$，其中$$\boldsymbol{\alpha}$$是该直线的方向向量。因此有$$\boldsymbol{r}'(s) = \boldsymbol{\alpha}$$，$$\boldsymbol{r}''(s) = \boldsymbol{0}$$，故$$\kappa(s) = \vert \boldsymbol{0} \vert \equiv 0$$。上述推导是可逆的，即$$\kappa(s) \equiv 0$$蕴含着$$\boldsymbol{r}(s) = \boldsymbol{r}_0 + \boldsymbol{\alpha}_0 s$$。证毕∎

把曲线$$C$$的切向量$$\boldsymbol{\alpha}(s)$$平行移动到原点$$O$$，其端点描绘出的曲线称为曲线$$C$$的**切线像**，它的参数方程为

$$
\boldsymbol{r} = \boldsymbol{\alpha}(s)
$$

一般来说$$s$$不再是切线像的弧长，而切线像的弧长为

$$
\mathrm{d} \tilde{s} = \bigg\vert \frac{\mathrm{d} \boldsymbol{\alpha}}{\mathrm{d} s} \bigg\vert \ \mathrm{d}s = \kappa(s) \ \mathrm{d} s
$$

所以

$$
\kappa(s) = \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s}
$$

这就是说，曲线的曲率$$\kappa(s)$$是曲线切线像的弧长元素与曲线弧长元素之比。

因为$$\vert \boldsymbol{\alpha}(s) \vert = 1$$，根据[定理1.3](/2023/07/30/DifferentialGeometry-NOTES-01.html#向量函数)可知$$\boldsymbol{\alpha}'(s) \cdot \boldsymbol{\alpha}(s) = 0$$，即$$\boldsymbol{\alpha}(s) \perp \boldsymbol{\alpha}'(s)$$，所以$$\boldsymbol{\alpha}'(s)$$是曲线$$C$$的一个法向量。如果$$\kappa(s) \neq 0$$，则向量$$\boldsymbol{\alpha}'(s)$$有完全确定的方向，将这个方向的单位向量记为$$\boldsymbol{\beta}(s)$$，称其为曲线$$C$$的**主法向量**。于是，曲率向量$$\boldsymbol{\alpha}'(s)$$可以表示为

$$
\boldsymbol{\alpha}'(s) = \kappa(s) \ \boldsymbol{\beta}(s)
$$

曲线的单位切向量$$\boldsymbol{\alpha}(s)$$和主法向量$$\boldsymbol{\beta}(s)$$唯一地确定了曲线的第二个法向量

$$
\boldsymbol{\gamma} (s) = \boldsymbol{\alpha}(s) \times \boldsymbol{\beta} (s)
$$

称其为曲线的**次法向量**。这样，在正则曲线上曲率$$\kappa(s)$$不为零的点有一个完全确定的右手单位正交标架$$\{ \boldsymbol{r}(s); \boldsymbol{\alpha}(s), \boldsymbol{\beta}(s), \boldsymbol{\gamma}(s) \}$$，它与表示曲线的笛卡尔直角坐标系的选取无关，也不受曲线作保持定向的容许参数变换的影响，称为曲线在该点的**Frenet标架**。

在曲率$$\kappa(s)$$不为零的点，Frenet标架$$\{ \boldsymbol{r}(s); \boldsymbol{\alpha}(s), \boldsymbol{\beta}(s), \boldsymbol{\gamma}(s) \}$$的三根轴分别称为曲线的**切线**，**主法线**和**次法线**，三个坐标面分别称为曲线的**法平面**(以$$\boldsymbol{\alpha}$$为法向量的平面)，**从切平面**(以$$\boldsymbol{\beta}$$为法向量的平面)和**密切平面**(以$$\boldsymbol{\gamma}$$为法向量的平面)，它们的方程分别为：

(1) 法平面：$$(\boldsymbol{X} - \boldsymbol{r}(s)) \cdot \boldsymbol{\alpha}(s) = 0$$  
(2) 从切平面：$$(\boldsymbol{X} - \boldsymbol{r}(s)) \cdot \boldsymbol{\beta}(s) = 0$$  
(3) 密切平面：$$(\boldsymbol{X} - \boldsymbol{r}(s)) \cdot \boldsymbol{\gamma}(s) = 0$$

其中$$\boldsymbol{X}$$是相应平面上动点的向径。

对于弧长参数表达的曲线$$\boldsymbol{r}(s)$$，其曲率和Frenet标架可以直接根据定义计算：

$$
\boldsymbol{\alpha}(s) = \boldsymbol{r}'(s)
$$

$$
\kappa(s) = \vert \boldsymbol{\alpha}'(s) \vert = \vert \boldsymbol{r}''(s) \vert
$$

如果$$\kappa(s) \neq 0$$，则有

$$
\boldsymbol{\beta} (s) = \frac{\boldsymbol{r}''(s)}{\vert \boldsymbol{r}''(s) \vert}
$$

$$
\boldsymbol{\gamma} (s) = \boldsymbol{\alpha} (s) \times \boldsymbol{\beta} (s) = \frac{\boldsymbol{r}'(s) \times \boldsymbol{r}''(s)}{\vert \boldsymbol{r}''(s) \vert}
$$

对于一般的参数$$t$$则可以按照下式进行计算：

$$
\boldsymbol{\alpha} (t) = \frac{\boldsymbol{r}' (t)}{\vert \boldsymbol{r}' (t) \vert}, \
\boldsymbol{r}' (t) = \vert \boldsymbol{r}' (t) \vert \cdot \boldsymbol{\alpha} (t)
$$

$$
\begin{aligned}
\boldsymbol{r}'' (t) &= \frac{\mathrm{d} \vert \boldsymbol{r}' (t) \vert}{\mathrm{d} t} \cdot \boldsymbol{\alpha} (t) + \vert \boldsymbol{r}' (t) \vert \cdot \frac{\mathrm{d} \boldsymbol{\alpha} (t)}{\mathrm{d} s} \cdot \frac{\mathrm{d} s}{\mathrm{d} t} \\
&= \frac{\mathrm{d} \vert \boldsymbol{r}' (t) \vert}{\mathrm{d} t} \cdot \boldsymbol{\alpha} (t) + \vert \boldsymbol{r}' (t) \vert^2 \cdot \kappa (t) \cdot \boldsymbol{\beta}(t)
\end{aligned}
$$

所以

$$
\begin{aligned}
\boldsymbol{r}'(t) \times \boldsymbol{r}''(t) &= \vert \boldsymbol{r}'(t) \vert^3 \cdot \kappa(t) \big( \boldsymbol{\alpha}(t) \times \boldsymbol{\beta}(t) \big) \\
&= \vert \boldsymbol{r}'(t) \vert^3 \cdot \kappa(t) \ \boldsymbol{\gamma}(t)
\end{aligned}
$$

整理后可以得到

$$
\kappa(t) = \frac{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert}{\vert \boldsymbol{r}'(t) \vert^3}
$$

$$
\boldsymbol{\gamma} (t) = \frac{\boldsymbol{r}'(t) \times \boldsymbol{r}''(t)}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert}
$$

$$
\begin{aligned}
\boldsymbol{\beta} (t) &= \boldsymbol{\gamma}(t) \times \boldsymbol{\alpha} (t) \\
&= \frac{\vert \boldsymbol{r}'(t) \vert}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} \boldsymbol{r}''(t) - \frac{\boldsymbol{r}'(t) \cdot \boldsymbol{r}''(t)}{\vert \boldsymbol{r}'(t) \vert \cdot \vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} \boldsymbol{r}'(t)
\end{aligned}
$$

## 曲线的挠率和Frenet公式

[上一节](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的曲率和frenet标架)介绍了曲线在一点切线和主法线张成的平面称为曲线的密切平面，它的法向是曲线的次法向量$$\boldsymbol{\gamma}$$。如果曲线本身落在一个平面内，则该平面就是曲线的密切平面，于是它的次法向量$$\boldsymbol{\gamma}$$是常向量；如果曲线不是平面曲线，则$$\boldsymbol{\gamma}$$必定不是常向量。根据**定理2.2**，单位切向量$$\boldsymbol{\alpha}$$关于弧长参数$$s$$的导数的长度$$\vert \boldsymbol{\alpha}'(s) \vert$$反映了曲线切线方向转动的快慢；同理，次法向量$$\boldsymbol{\gamma}$$关于弧长参数$$s$$的导数的长度$$\vert \boldsymbol{\gamma}'(s) \vert$$反映了曲线的密切平面方向转动的快慢，因而它刻画了曲线偏离平面曲线程度，反映了曲线扭曲的程度，即曲线的"挠率"。

因为$$\boldsymbol{\gamma} (s)$$是单位向量场，故$$\boldsymbol{\gamma}' (s) \perp \boldsymbol{\gamma} (s)$$。此外根据Frenet标架的定义有

$$
\boldsymbol{\gamma} = \boldsymbol{\alpha} \times \boldsymbol{\beta}
$$

$$
\boldsymbol{\alpha}'(s) \parallel \boldsymbol{\beta}(s)
$$

所以

$$
\begin{aligned}
\boldsymbol{\gamma}'(s) &= \boldsymbol{\alpha}' (s) \times \boldsymbol{\beta} (s) + \boldsymbol{\alpha} (s) \times \boldsymbol{\beta}' (s) \\
&= \boldsymbol{\alpha} (s) \times \boldsymbol{\beta}' (s)
\end{aligned}
$$

这说明$$\boldsymbol{\gamma}'(s) \perp \boldsymbol{\alpha} (s)$$。于是$$\boldsymbol{\gamma}'(s)$$必定是与$$\boldsymbol{\beta} (s)$$是共线的，不妨设

$$
\boldsymbol{\gamma}'(s) = -\tau \boldsymbol{\beta} (s)
$$

因此

$$
\tau(s) = -\boldsymbol{\gamma}'(s) \cdot \boldsymbol{\beta} (s)
$$

并且

$$
\vert \tau(s) \vert = \vert \boldsymbol{\gamma}'(s) \vert
$$

> ## 定义2.2
> 设$$\boldsymbol{\beta}$$和$$\boldsymbol{\gamma}$$分别是曲线$$C$$的主法向量和次法向量，其中$$s$$是弧长参数，则$$\tau(s) = -\boldsymbol{\gamma}'(s) \cdot \boldsymbol{\beta} (s)$$称为曲线$$C$$的**挠率**。
{: .block-definition }

> ## 定理2.4
> 设曲线$$C$$不是直线，则它是平面曲线当且仅当它的挠率为零。
{: .block-theorem }

**证明** 前面已经证明过平面曲线的次法向量$$\boldsymbol{\gamma}$$是常向量，因此其挠率必为零；接下来证明挠率为零的曲线是平面曲线。设曲线$$C$$的参数方程为$$\boldsymbol{r} = \boldsymbol{r}(s)$$，$$s$$是弧长参数，并且$$\kappa(s) \neq 0$$，$$\tau(s) \equiv 0$$。此时曲线有确定的Frenet标架$$\{ \boldsymbol{r}(s); \boldsymbol{\alpha}(s), \boldsymbol{\beta}(s), \boldsymbol{\gamma}(s) \}$$，并且
$$
\boldsymbol{\gamma}'(s) = -\tau(s) \cdot \boldsymbol{\beta} (s) \equiv 0
$$

因此$$\boldsymbol{\gamma}(s) = \boldsymbol{\gamma}_0$$为常向量。由于

$$
0 = \boldsymbol{r}'(s) \cdot \boldsymbol{\gamma}(s) = \boldsymbol{r}'(s) \cdot \boldsymbol{\gamma}_0 = \frac{\mathrm{d}}{\mathrm{d} s} \big( \boldsymbol{r}(s) \cdot \boldsymbol{\gamma}_0 \big)
$$

所以

$$
\boldsymbol{r}(s) \cdot \boldsymbol{\gamma}_0 = \boldsymbol{r}(s_0) \cdot \boldsymbol{\gamma}_0
$$

$$
\big( \boldsymbol{r}(s) - \boldsymbol{r}(s_0) \big) \cdot \boldsymbol{\gamma}_0 = 0
$$

这说明曲线落在经过点$$\boldsymbol{r}(s_0)$$、以常向量$$\boldsymbol{\gamma}_0$$为法向量的平面内。证毕∎

根据曲率、挠率和Frenet标架的定义，我们可以总结出如下公式：

$$
\begin{aligned}
\boldsymbol{r}'(s) &= \boldsymbol{\alpha}(s) \\
\boldsymbol{\alpha}'(s) &= \kappa(s) \boldsymbol{\beta}(s) \\
\boldsymbol{\gamma}'(s) &= -\tau(s) \boldsymbol{\beta}(s)
\end{aligned}
$$

其中$$\boldsymbol{r}'(s)$$，$$\boldsymbol{\alpha}'(s)$$，$$\boldsymbol{\gamma}'(s)$$分别给出了Frenet标架的原点$$\boldsymbol{r}(s)$$和两个标架向量$$\boldsymbol{\alpha}(s)$$，$$\boldsymbol{\gamma}(s)$$的运动公式，要获得整个标架的运动公式只需要求出$$\boldsymbol{\beta}'(s)$$就行了。由于$$\{ \boldsymbol{r}(s); \boldsymbol{\alpha}(s), \boldsymbol{\beta}(s), \boldsymbol{\gamma}(s) \}$$是$$\mathbb{E}^3$$空间中的一个标架，所以$$\boldsymbol{\beta}'(s)$$一定可以表示为$$\boldsymbol{\alpha}(s)$$，$$\boldsymbol{\beta}(s)$$，$$\boldsymbol{\gamma}(s)$$的线性组合，不妨设

$$
\boldsymbol{\beta}'(s) = a \boldsymbol{\alpha}(s) + b \boldsymbol{\beta}(s) + c \boldsymbol{\gamma}(s)
$$

将上式分别与$$\boldsymbol{\alpha}(s)$$，$$\boldsymbol{\beta}(s)$$，$$\boldsymbol{\gamma}(s)$$进行点乘可以得到系数

$$
\begin{aligned}
a &= \boldsymbol{\beta}'(s) \cdot \boldsymbol{\alpha}(s) = -\boldsymbol{\beta}(s) \cdot \boldsymbol{\alpha}'(s) =-\kappa(s) \\
b &= \boldsymbol{\beta}'(s) \cdot \boldsymbol{\beta}(s)  = 0 \\
c &= \boldsymbol{\beta}'(s) \cdot \boldsymbol{\gamma}(s) = -\boldsymbol{\beta}(s) \cdot \boldsymbol{\gamma}'(s) = \tau(s) \\
\end{aligned}
$$

所以有

$$
\boldsymbol{\beta}'(s) = -\kappa(s) \boldsymbol{\alpha}(s) + \tau(s) \boldsymbol{\gamma}(s)
$$

总结一下可以得到Frenet标架$$\{ \boldsymbol{r}(s); \boldsymbol{\alpha}(s), \boldsymbol{\beta}(s), \boldsymbol{\gamma}(s) \}$$沿曲线$$C$$的运动公式

$$
\begin{cases}
\boldsymbol{r}'(s) &= \boldsymbol{\alpha}(s) \\
\boldsymbol{\alpha}'(s) &= \kappa(s) \boldsymbol{\beta}(s) \\
\boldsymbol{\beta}'(s) &= -\kappa(s) \boldsymbol{\alpha}(s) + \tau(s) \boldsymbol{\gamma}(s) \\
\boldsymbol{\gamma}'(s) &= -\tau(s) \boldsymbol{\beta}(s)
\end{cases}
$$

上述公式称为**Frenet公式**，是曲线论中最重要、最基本的公式。Frenet公式的后三个方程还可以写成矩阵的形式

$$
\begin{pmatrix}
\boldsymbol{\alpha}'(s) \\ \boldsymbol{\beta}'(s) \\ \boldsymbol{\gamma}'(s)
\end{pmatrix}
=
\begin{pmatrix}
0          & \kappa(s) & 0 \\
-\kappa(s) & 0         & \tau(s) \\
0          & -\tau(s)  & 0
\end{pmatrix}
\cdot
\begin{pmatrix}
\boldsymbol{\alpha}(s) \\ \boldsymbol{\beta}(s) \\ \boldsymbol{\gamma}(s)
\end{pmatrix}
$$

其中的系数矩阵为一个反对称矩阵，这不是Frenet标架的导数所特有的。实际上，沿曲线定义的任意一个单位正交标架场的导数公式的系数矩阵都是反对称的。

本节最后我们来推导挠率$$\tau(t)$$的计算公式。首先回忆次法向量$$\gamma(s)$$的计算公式

$$
\boldsymbol{\gamma} (t) = \frac{\boldsymbol{r}'(t) \times \boldsymbol{r}''(t)}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert}
$$

等式左右两边对$$t$$进行求导可以得到

$$
\begin{aligned}
\boldsymbol{\gamma}' (t) 
&= \frac{\mathrm{d} \boldsymbol{\gamma} (t)}{\mathrm{d} s} \cdot \frac{\mathrm{d} s}{\mathrm{d} t} = -\tau(t) \boldsymbol{\beta}(t) \cdot \frac{\mathrm{d} s}{\mathrm{d} t} \\
&= \frac{\boldsymbol{r}'(t) \times \boldsymbol{r}'''(t)}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} + \frac{\mathrm{d}}{\mathrm{d} t} \bigg( \frac{1}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} \bigg) \cdot \big( \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \big)
\end{aligned}
$$

即

$$
-\tau(t) \boldsymbol{\beta}(t) \cdot \frac{\mathrm{d} s}{\mathrm{d} t} 
= \frac{\boldsymbol{r}'(t) \times \boldsymbol{r}'''(t)}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} + \frac{\mathrm{d}}{\mathrm{d} t} \bigg( \frac{1}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} \bigg) \cdot \big( \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \big)
$$

再对两边同时点乘$$\boldsymbol{\beta} (t)$$的计算式

$$
\boldsymbol{\beta} (t) = \frac{\vert \boldsymbol{r}'(t) \vert}{\vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} \boldsymbol{r}''(t) - \frac{\boldsymbol{r}'(t) \cdot \boldsymbol{r}''(t)}{\vert \boldsymbol{r}'(t) \vert \cdot \vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert} \boldsymbol{r}'(t)
$$

最终得到

$$
\tau(t) = \frac{\big( \boldsymbol{r}'(t), \boldsymbol{r}''(t), \boldsymbol{r}'''(t) \big)}{ \vert \boldsymbol{r}'(t) \times \boldsymbol{r}''(t) \vert^2}
$$

如果$$t$$是弧长参数$$s$$，上式还可以简化为

$$
\tau(s) = \frac{\big( \boldsymbol{r}'(s), \boldsymbol{r}''(s), \boldsymbol{r}'''(s) \big)}{ \vert \boldsymbol{r}''(s) \vert^2}
$$

结合[定理2.4](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的挠率和frenet公式)，我们可以直接得到如下推论

> ## 定理2.5
> 曲线$$\boldsymbol{r} = \boldsymbol{r}(t)$$是一条平面曲线的充分必要条件是$$\big( \boldsymbol{r}'(s), \boldsymbol{r}''(s), \boldsymbol{r}'''(s) \big) \equiv 0$$。
{: .block-theorem }

## 曲线论基本定理

前面的讨论指出正则参数曲线的[弧长参数](/2023/07/31/DifferentialGeometry-NOTES-02.html#弧长参数)、[曲率](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的曲率和frenet标架)和[挠率](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的挠率和frenet公式)都是与曲线的保持定向的容许参数变换无关的，也与欧式空间$$\mathbb{E}^3$$中的笛卡尔直角坐标系的选取无关。当曲线在空间中经受一个刚体运动时，曲线的弧长、曲率和挠率是不变的；反过来说，如果空间$$\mathbb{E}^3$$中有两条曲线且它们的曲率和挠率表示成弧长参数的函数是分别相同的，则这两条曲线的形状是相同的。这个论断可以叙述成下面的基本定理。

> ## 定理2.6 
> 设$$\boldsymbol{r} = \boldsymbol{r}_1(s)$$和$$\boldsymbol{r} = \boldsymbol{r}_2(s)$$是$$\mathbb{E}^3$$中两条以弧长$$s$$为参数的正则参数曲线。如果它们的曲率处处不为零，并且它们的曲率和挠率分别相等，即$$\boldsymbol{\kappa}_1 (s) =\boldsymbol{\kappa}_2 (s)$$，$$\boldsymbol{\tau}_1 (s) =\boldsymbol{\tau}_2 (s)$$，则有$$\mathbb{E}^3$$中的一个刚体运动$$\sigma$$，它把曲线$$\boldsymbol{r} = \boldsymbol{r}_1(s)$$变成曲线$$\boldsymbol{r} = \boldsymbol{r}_2(s)$$。
{: .block-theorem }

对于一般的参数曲线，上述定理还可以推导出如下结论。

> ## 定理2.7
> 设$$\boldsymbol{r} = \boldsymbol{r}_1(t)$$和$$\boldsymbol{r} = \boldsymbol{r}_2(u)$$是$$\mathbb{E}^3$$中两条正则参数曲线，它们的曲率处处不为零。如果存在三次以上的连续可微函数$$u = \lambda(t)$$，$$\lambda'(t) \neq 0$$，使得这两条曲线的弧长函数、曲率函数和挠率函数之间有关系式  
$$s_1(t) = s_2(\lambda(t))$$，
$$\kappa_1(t) = \kappa_2(\lambda(t))$$，
$$\tau_1(t) = \tau_2(\lambda(t))$$  
则有$$\mathbb{E}^3$$中的一个刚体运动$$\sigma$$，它把曲线$$\boldsymbol{r} = \boldsymbol{r}_1(t)$$变成曲线$$\boldsymbol{r} = \boldsymbol{r}_2(u)$$，即曲线$$\boldsymbol{r} = \boldsymbol{r}_2(\lambda(t))$$是曲线$$\boldsymbol{r} = \boldsymbol{r}_1(t)$$在刚体运动$$\sigma$$下的像。
{: .block-theorem }

### 曲线的内在方程

除此之外我们还知道在曲率$$\kappa(s)$$处处不为零的正则曲线上有内在的、确定的[Frenet标架场](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的曲率和frenet标架)，所以$$\mathbb{E}^3$$中的曲线便变成在$$\mathbb{E}^3$$中的正交标架空间中的一条曲线。而[Frenet公式](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的挠率和frenet公式)正好是这个标架场的运动方程，其系数恰好是曲线的曲率和挠率，它们完全确定了曲线在空间中的形状。我们的问题是：给定了曲率和挠率作为弧长参数$$s$$的函数$$\kappa(s)$$，$$\tau(s)$$后，在空间$$\mathbb{E}^3$$中是否存在正则参数曲线以给定的函数$$\kappa(s)$$，$$\tau(s)$$为它的曲率和挠率？我们在$$\mathbb{E}^3$$上由全体正交标架构成的六维空间中考虑，于是[Frenet公式](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的挠率和frenet公式)成为现成的已知常微分方程组，它的解是依赖参数$$s$$的一族正交标架，其标架原点在$$\mathbb{E}^3$$中描出的轨迹正是我们所需的曲线，而这族正交标架本身应该是曲线的Frenet标架场。

> ## 定理2.8
> 设$$\kappa(s)$$，$$\tau(s)$$是在区间$$[a, b]$$上两个任意给定的连续可微函数，并且$$\kappa(s) \lt 0$$，则在空间$$\mathbb{E}^3$$中存在正则参数曲线$$\boldsymbol{r} = \boldsymbol{r}(s)$$，$$a \leq s \leq b$$，以$$s$$为弧长参数，以给定的函数$$\kappa(s)$$，$$\tau(s)$$为它的曲率和挠率，且这样的曲线在$$\mathbb{E}^3$$中时完全确定的，其差异至多为曲线在空间中的位置不同。
{: .block-theorem }

**定理2.8**说明函数$$\kappa(s) \lt 0$$与$$\tau(s)$$在空间$$\mathbb{E}^3$$中不计位置的差异位于地确定了一条曲线，因此它们可以看作是该曲线的方程，称为曲线的**内在方程**或**自然方程**。从曲线的内在方程得到参数方程的过程可以表示为求解方程组

$$
\begin{cases}
\frac{\mathrm{d} \boldsymbol{r}}{\mathrm{d} s} = \boldsymbol{e}_1 \\
\frac{\mathrm{d} \boldsymbol{e}_i}{\mathrm{d} s} = \sum_{j=1}^3 a_{ij}(s) \boldsymbol{e}_j
\end{cases}
$$

其中$$\{ \boldsymbol{r}(s); \boldsymbol{e}_1(s), \boldsymbol{e}_2(s), \boldsymbol{e}_3(s) \}$$为曲线的Frenet标架，系数$$a_{ij}(s)$$需要满足

$$
\begin{pmatrix}
a_{11}(s) & a_{12}(s) & a_{13}(s) \\
a_{21}(s) & a_{22}(s) & a_{23}(s) \\
a_{31}(s) & a_{32}(s) & a_{33}(s) \\
\end{pmatrix}
=
\begin{pmatrix}
0          & \kappa(s) & 0 \\
-\kappa(s) & 0         & \tau(s) \\
0          & -\tau(s)  & 0
\end{pmatrix}
$$

而初始条件则为

$$
\boldsymbol{r}(s_0) = \boldsymbol{r}^0 , \
\boldsymbol{e}_i(s_0) = \boldsymbol{e}_i^0
$$

基向量$$\{ \boldsymbol{e}^0_1(s), \boldsymbol{e}^0_2(s), \boldsymbol{e}^0_3(s) \}$$是一个为右手单位正交标架，即满足条件

$$
\begin{cases}
\boldsymbol{e}^0_i(s) \cdot \boldsymbol{e}^0_j(s) = \delta_{ij}, \ 1 \leq i,j \leq 3 \\
\big( \boldsymbol{e}^0_1(s), \boldsymbol{e}^0_2(s), \boldsymbol{e}^0_3(s) \big) = 1
\end{cases}
$$

可以证明求解满足初始条件的常微分方程组得到的向量函数即为所需曲线。

显然在一般情况下求解曲线内在方程和常微分方程组来获得曲线的表达式是比较困难的。对于一些常见曲线我们总结了其内在方程有如下特征：

|         |  曲率$$\kappa(s)$$   | 挠率$$\tau(s)$$  |
|  :----:   |  :----:  | :----:  |
| 直线  | $$\kappa(s) \equiv 0$$ | - |
| 圆  | $$\kappa(s) \gt 0$$为常数 | $$\tau(s) \equiv 0$$ |
| 平面曲线  | - | $$\tau(s) \equiv 0$$ |
| 圆柱螺线  | $$\kappa(s) \gt 0$$为常数 | $$\kappa(s) \neq 0$$为常数 |

## 曲线参数方程在一点的标准展开

我们知道对于解析函数$$y=f(x)$$在任意点$$x_0$$的邻域内可以展开成收敛的幂级数。如果函数$$y=f(x)$$是光滑的，即有任意阶导数，则函数$$f(x)$$可以表示成任意$$n$$次的一个多项式与一个余项之和，该多项式的系数由$$f(x)$$的直到$$n$$阶导数在$$x_0$$处的值决定，并且余项在$$x \to x_0$$时是比$$(x - x_0)^n$$更高阶的无穷小量。这样的展开式称为函数$$y=f(x)$$的[Taylor展开式](https://en.wikipedia.org/wiki/Taylor_series)，它是原来函数的近似。正则参数曲线的参数方程是由三个可微函数组成的，将Taylor展开式用到这三个函数上，便能够得到一条多项式曲线来近似原来的曲线。特别地，当曲线的曲率和挠率都不为零时，在一点的附近可以求得一条三次曲线，它与原来的曲线在该点有同样的曲率、挠率和Frenet标架，于是原曲线在该点附近的性状可以用这条近似曲线来模拟。

### 曲线的标准展开与近似曲线

设$$\boldsymbol{r} = \boldsymbol{r} (s)$$是一条以弧长$$s$$为参数的正则曲线，它在$$s = 0$$处的Taylor展开式为

$$
\boldsymbol{r} (s) = \boldsymbol{r} (0) + \frac{s}{1!} \boldsymbol{r}' (0) + \frac{s^2}{2!} \boldsymbol{r}'' (0) + + \frac{s^3}{3!} \boldsymbol{r}''' (0) + \boldsymbol{o} (s^3)
$$

其中$$\boldsymbol{o} (s^3)$$是余项，满足条件

$$
\lim_{s \to 0} \frac{\vert \boldsymbol{o} (s^3) \vert}{s^3} = 0
$$

根据[Frenet公式](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的挠率和frenet公式)，我们有

$$
\begin{aligned}
\boldsymbol{r}' (0) &= \boldsymbol{\alpha} (0) \\
\boldsymbol{r}'' (0) &= \kappa(0) \boldsymbol{\beta} (0) \\
\boldsymbol{r}''' (0) &= -\kappa^2(0) \boldsymbol{\alpha} (0) + \kappa'(0) \boldsymbol{\beta} (0) + \kappa(0) \tau (0) \boldsymbol{\gamma} (0) \\
\end{aligned}
$$

带入Taylor展开式可以得到

$$
\begin{aligned}
\boldsymbol{r} (s) &= \boldsymbol{r} (0) \\
&+ \bigg( s - \frac{\kappa_0^2}{6} s^3 \bigg) \boldsymbol{\alpha} (0) \\
&+ \bigg( \frac{\kappa_0}{2} s^2 + \frac{\kappa_0'}{6} s^3 \bigg) \boldsymbol{\beta}(0) \\
&+ \bigg( \frac{\kappa_0 \tau_0}{6} s^3 \bigg) \boldsymbol{\gamma}(0) \\
&+ \boldsymbol{o} (s^3)
\end{aligned}
$$

其中$$\kappa_0 = \kappa(0)$$，$$\kappa_0' = \kappa'(0)$$，$$\tau_0 = \tau(0)$$。如果把曲线在$$s=0$$处的Frenet标架$$\{ \boldsymbol{r}(0); \boldsymbol{\alpha}(0), \boldsymbol{\beta}(0), \boldsymbol{\gamma}(0) \}$$取作空间$$\mathbb{E}^3$$的笛卡尔直角坐标系的标架，则曲线在$$s=0$$处的参数方程成为

$$
\begin{cases}
x = s - \frac{\kappa_0^2}{6} s^3 + \boldsymbol{o} (s^3) &= s + \boldsymbol{o} (s)\\
y = \frac{\kappa_0}{2} s^2 + \frac{\kappa_0'}{6} s^3 + \boldsymbol{o} (s^3) &= \frac{\kappa_0}{2} s^2 + \boldsymbol{o} (s^2) \\
z = \frac{\kappa_0 \tau_0}{6} s^3 + \boldsymbol{o} (s^3) \\
\end{cases}
$$

上式称为曲线$$\boldsymbol{r} = \boldsymbol{r}(s)$$在$$s=0$$处的**标准展开式**。

当$$\kappa_0 \tau_0 \neq 0$$时，我们可以考虑一条新的曲线

$$
\tilde{\boldsymbol{r}} (s) = \bigg( s, \frac{\kappa_0}{2} s^2, \frac{\kappa_0 \tau_0}{6} s^3 \bigg)
$$

这是一条三次曲线，并且参数$$s$$一般不是曲线$$\tilde{\boldsymbol{r}} (s)$$的弧长参数。在$$s=0$$处有

$$
\begin{aligned}
\tilde{\boldsymbol{r}} (s) &= (0, 0, 0) \\
\tilde{\boldsymbol{r}}' (s) &= (1, 0, 0) \\
\tilde{\boldsymbol{r}}'' (s) &= (0, \kappa_0, 0) \\
\tilde{\boldsymbol{r}}''' (s) &= (0, 0, \kappa_0 \tau_0) \\
\end{aligned}
$$

不难发现，曲线$$\tilde{\boldsymbol{r}} (s)$$在$$s=0$$处的曲率是$$\kappa_0$$，挠率是$$\tau_0$$，并且Frenet标架是$$\{ \boldsymbol{r}(0); \boldsymbol{\alpha}(0), \boldsymbol{\beta}(0), \boldsymbol{\gamma}(0) \}$$，即它与原来的曲线$$\boldsymbol{r}(s)$$在$$s=0$$处有相同的曲率、挠率和Frenet标架。

曲线$$\boldsymbol{r} = \tilde{\boldsymbol{r}} (s)$$称为原曲线$$\boldsymbol{r} = \boldsymbol{r} (s)$$在$$s=0$$处的**近似曲线**，它的性状反映了原曲线的性状。它在密切平面上的投影是抛物线

$$
x = s, y = \frac{\kappa_0}{2} s^2, z = 0
$$

它在从切平面的投影是三次曲线

$$
x = s, y = 0, z = \frac{\kappa_0 \tau_0}{6} s^3
$$

它在法平面上的投影是

$$
x = 0, y = \frac{\kappa_0}{2} s^2, z = \frac{\kappa_0 \tau_0}{6} s^3
$$

这些投影曲线的图像如下图所示。从图中可以发现曲线的挠率$$\tau_0$$控制了其穿越$$s=0$$处密切平面的方向：当$$\tau_0 \gt 0$$时，曲线是从下而上地穿过密切平面的；而当$$\tau_0 \lt 0$$时，曲线是从上而下地穿过密切平面的。这就是挠率正负符号的几何意义。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qQ1wdBw.png" width="80%">
</div>

### 切触

两条相交的曲线在交点附近的接近程度是用所谓的切触阶来刻画的。设曲线$$C_1$$和$$C_2$$相交于点$$p_0$$，在$$C_1$$和$$C_2$$上各取一点$$p_1$$和$$p_2$$，使得曲线$$C_1$$在点$$p_0$$和$$p_1$$之间的弧长是$$\Delta s$$，$$C_2$$在点$$p_0$$和$$p_2$$之间的弧长也是$$\Delta s$$，若有正整数$$n$$使得

$$
\lim_{\Delta s} \frac{\vert p_1 p_2 \vert}{(\Delta s)^n} = 0, \ \
\lim_{\Delta s} \frac{\vert p_1 p_2 \vert}{(\Delta s)^{n+1}} \neq 0
$$

则称曲线$$C_1$$和$$C_2$$在交点$$p_0$$处有$$n$$阶**切触**。

> 定理2.9
> 设曲线$$\boldsymbol{r}_1 (s)$$和$$\boldsymbol{r}_2 (s)$$都以$$s$$为它们的弧长参数，且$$\boldsymbol{r}_1 (0) = \boldsymbol{r}_2 (0)$$，则它们在$$s=0$$处有$$n$$阶切触的充分必要条件是
$$\boldsymbol{r}_1^{(i)} (0) = \boldsymbol{r}_2^{(i)} (0), \ \forall 1 \leq i \leq n ;\ \ \boldsymbol{r}_1^{(n+1)} (0) \neq \boldsymbol{r}_2^{(n+1)} (0)$$ 
{: .block-theorem }

**证明** 由Taylor展开式可以得到

$$
\boldsymbol{r}_1 (s) - \boldsymbol{r}_2 (s) = \frac{s^{n+1}}{(n+1)!} \big( \boldsymbol{r}_1^{(n+1)} (0) - \boldsymbol{r}_2^{(n+1)} (0) \big) + \boldsymbol{o}(s^{n+1})
$$

因此

$$
\lim_{\Delta s} \frac{\vert \boldsymbol{r}_1 (s) - \boldsymbol{r}_2 (s) \vert}{s^n} = 0
$$

$$
\lim_{\Delta s} \frac{\vert \boldsymbol{r}_1 (s) - \boldsymbol{r}_2 (s) \vert}{s^{n+1}} = \frac{1}{(n+1)!} \vert \boldsymbol{r}_1^{(n+1)} (0) - \boldsymbol{r}_2^{(n+1)} (0) \vert \neq 0
$$

反之亦然。由此可见，一条正则曲线与由它的Taylor展开式的前$$n+1$$项之和给出的曲线在该点处至少有$$n$$阶切触。正则曲线与它的切线至少有1阶切触，与它在一点处的近似曲线在该点至少有2阶切触。证毕∎

**定理2.9**的直接推论是两条相交的正则曲线在交点$$s=0$$处有2阶以上的切触的充要条件是

$$
\begin{aligned}
\boldsymbol{r}_1 (0) &= \boldsymbol{r}_2 (0) \\
\boldsymbol{r}_1^{(1)} (0) &= \boldsymbol{r}_2^{(1)} (0) \\
\boldsymbol{r}_1^{(2)} (0) &= \boldsymbol{r}_2^{(2)} (0) \\
\end{aligned}
$$

前两式说明这两条曲线相切，第三式意味着

$$
\kappa_1 (0) \boldsymbol{\beta}_1 (0) = \kappa_2 (0) \boldsymbol{\beta}_2 (0)
$$

即具有相同的密切平面和曲率

$$
\kappa_1 (0) = \kappa_2 (0)， \ \
\boldsymbol{\beta}_1 (0) = \boldsymbol{\beta}_2 (0)
$$

### 曲率圆

对于曲线$$\boldsymbol{r} = \boldsymbol{r}(s)$$上$$\kappa(s_0) \gt 0$$的点$$s = s_0$$，还可以在该点的密切平面上构造一条特殊的平面曲线，即在$$\boldsymbol{r}(s_0)$$处的密切平面上作以$$\boldsymbol{r}(s_0) + \boldsymbol{\beta}(s_0) / \kappa(s_0)$$为中心、以$$1/\kappa(s_0)$$为半径的圆周，这个圆周与原曲线在$$s = s_0$$处相切，有相同的有向密切平面，并且曲率都是$$\kappa(s_0)$$，因此它与原曲线在点$$s = s_0$$处有2阶以上的切触。通常称这个圆周为曲线$$\boldsymbol{r} = \boldsymbol{r}(s)$$在点$$s = s_0$$处的**曲率圆**，其圆心$$\boldsymbol{r}(s_0) + \boldsymbol{\beta}(s_0) / \kappa(s_0)$$称为曲线在$$s = s_0$$处的**曲率中心**，其半径$$1/\kappa(s_0)$$称为曲线在$$s = s_0$$处的**曲率半径**。曲率圆形象地反映了曲线在一点处的弯曲程度。

类似于曲率圆，我们可以定义曲线的密切球面。

> ## 定理2.10
> 设曲线$$C: \boldsymbol{r} = \boldsymbol{r}(s)$$是曲率和挠率都不为零的正则参数曲线，$$s$$是弧长参数，则在$$s$$处与曲线$$C$$有三阶以上切触的球面$$\Sigma$$的球心是$$\boldsymbol{r}(s) + \frac{1}{\kappa(s)} \boldsymbol{\beta} (s) + \frac{1}{\tau (s)} \bigg( \frac{1}{\kappa(s)} \bigg)' \boldsymbol{\gamma}(s)$$，半径是$$\sqrt{\bigg( \frac{1}{\kappa(s)} \bigg)^2 + \bigg( \frac{1}{\tau(s)} \bigg( \frac{1}{\kappa(s)} \bigg)' \bigg)^2}$$  
该球面称为曲线$$C$$在$$s$$处的**密切球面**，其球心所在直线
$$
\boldsymbol{r} = \boldsymbol{r}(s) + \frac{1}{\kappa(s)} \boldsymbol{\beta} (s) + \lambda \boldsymbol{\gamma}(s)
$$
是通过曲线$$C$$的曲率中心、垂直于密切平面的直线，称为曲线$$C$$在$$s$$处的**曲率轴**。
{: .block-theorem }

## 存在对应关系的曲线偶

本节我们主要研究存在一定对应关系的曲线偶。假定在正则参数曲线$$C_1 : \boldsymbol{r} = \boldsymbol{r}_1 (t)$$和正则参数曲线$$C_2 : \boldsymbol{r} = \boldsymbol{r}_2 (u)$$之间存在一个对应，这个对应可以用参数$$t$$和$$u$$之间的一个对应来表示，设为$$u = u(t)$$。如果$$u'(t) \neq 0$$，则$$u = u(t)$$可以认为是曲线$$C_2$$的正则参数变换，于是曲线$$C_1$$和$$C_2$$之间的对应成为曲线$$C_1$$和$$C_2$$之间有相同参数点之间的对应。

> ## 定义2.3
> 如果在互不重合的曲线$$C_1$$和$$C_2$$之间存在一个对应，设定它们在每一对对应点有公共的主法线，则称这两条曲线为**Bertrand曲线偶**，其中一条曲线称为另一条曲线的**侣线**，或**共轭曲线**。
{: .block-definition }

每一条平面曲线都有侣线，构成Bertrand曲线偶。实际上我们可以显式地构造出这样的曲线偶：设$$\boldsymbol{r} = \boldsymbol{r}(s)$$是平面上的一条曲线，以$$s$$为它的弧长参数。于是$$\boldsymbol{r}'(s)$$是曲线的单位切向量场，因此

$$
\boldsymbol{r}''(s) = \kappa(s) \ \boldsymbol{n} (s)
$$

这里$$\boldsymbol{n}$$是沿曲线定义的法向量场。命

$$
\boldsymbol{r}_1(s) = \boldsymbol{r}(s) + \lambda \boldsymbol{n} (s) 
$$

其中$$\lambda$$是任意给定的一个非零实数，则

$$
\boldsymbol{r}_1'(s) = \boldsymbol{r}'(s) + \lambda \boldsymbol{n}' (s) 
$$

因此

$$
\boldsymbol{r}_1'(s) \cdot \boldsymbol{n} (s) = \boldsymbol{r}'(s) \cdot \boldsymbol{n} (s) + \lambda \boldsymbol{n}' (s) \cdot \boldsymbol{n} (s) = 0
$$

所以$$\boldsymbol{n} (s)$$也是曲线$$\boldsymbol{r}_1 (s)$$的法向量场。由此可见，曲线$$\boldsymbol{r}(s)$$和$$\boldsymbol{r}_1 (s)$$在对应点有相同的法向(也是主法线)。因此，寻求Bertrand曲线偶应该在空间挠曲线(即挠率不为零的曲线)中去找。

> ## 定理2.11
> 设曲线$$C_1$$和$$C_2$$是Bertrand曲线偶，则$$C_1$$和$$C_2$$的对应点之间的距离是常数，并且$$C_1$$和$$C_2$$在对应点的切线成定角。
{: .block-theorem }

**证明** 设曲线$$C_1$$和$$C_2$$的参数方程分别是$$\boldsymbol{r}_1 (s)$$和$$\boldsymbol{r}_2 (s)$$，并且曲线$$C_1$$和$$C_2$$之间的对应是有相同参数点之间的对应，而且$$s$$是曲线$$C_1$$的弧长参数。用$$\{\boldsymbol{r}_1 (s); \boldsymbol{\alpha}_1 (s), \boldsymbol{\beta}_1 (s), \boldsymbol{\gamma}_1 (s) \}$$表示曲线$$C_1$$的Frenet标架，用$$\{\boldsymbol{r}_2 (s); \boldsymbol{\alpha}_2 (s), \boldsymbol{\beta}_2 (s), \boldsymbol{\gamma}_2 (s) \}$$表示曲线$$C_2$$的Frenet标架，并且假定曲线$$C_2$$的弧长参数是$$\tilde{s}$$。因为曲线$$C_1$$和$$C_2$$在对应点有相同的主法线，$$\boldsymbol{r}_2 (s)$$一定在经过对应点$$\boldsymbol{r}_1 (s)$$且方向为主法线$$\boldsymbol{\beta}_1 (s)$$的直线上，故

$$
\boldsymbol{r}_2 (s) = \boldsymbol{r}_1 (s) + \lambda (s) \boldsymbol{\beta}_1 (s)
$$

并且$$\boldsymbol{\beta}_1 (s) = \pm \boldsymbol{\beta}_2 (s)$$。利用[Frenet公式](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的挠率和frenet公式)对上式求导得到

$$
\begin{aligned}
\boldsymbol{\alpha}_2 (s) \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} &= \boldsymbol{\alpha}_1 (s) + \lambda'(s) \boldsymbol{\beta}_1 (s) + \lambda(s) \big( -\kappa_1(s) \boldsymbol{\alpha}_1(s) + \tau_1(s)\boldsymbol{\gamma}_1(s) \big) \\
&= \big( 1-\lambda(s) \kappa_1(s) \big) \boldsymbol{\alpha}_1 (s) + \lambda'(s) \boldsymbol{\beta}_1 (s) + \lambda(s) \tau_1 (s) \boldsymbol{\gamma}_1 (s)
\end{aligned}
$$

因为$$\boldsymbol{\beta}_1 (s) = \pm \boldsymbol{\beta}_2 (s)$$，所以

$$
\boldsymbol{\alpha}_2 (s) \cdot \boldsymbol{\beta}_2 (s) \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} = \pm \lambda'(s) \boldsymbol{\beta}_1 \cdot \boldsymbol{\beta}_1 = 0
$$

即$$\lambda'(s) = 0$$，$$\lambda(s)$$为常数。故

$$
\vert \boldsymbol{r}_2(s) - \boldsymbol{r}_1 (s) \vert = \vert \lambda(s) \boldsymbol{\beta}_1 (s) \vert = \vert \lambda \vert
$$

对$$\boldsymbol{\alpha}_1 (s) \cdot \boldsymbol{\alpha}_2 (s)$$求导得到

$$
\frac{\mathrm{d}}{\mathrm{d} s} \big( \boldsymbol{\alpha}_1 (s) \cdot \boldsymbol{\alpha}_2 (s) \big) = \kappa_1 (s) \boldsymbol{\beta}_1 (s) \cdot \boldsymbol{\alpha}_2 (s) + \kappa_2(s) \boldsymbol{\alpha}_1(s) \cdot \boldsymbol{\beta}_2 (s) \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} = 0
$$

故曲线$$C_1$$和$$C_2$$在对应点的切线成定角。证毕∎

> ## 定理2.12 
> 设正则参数曲线$$C$$的曲率$$\kappa$$和挠率$$\tau$$都不是零，则存在另一条正则参数曲线$$C_1$$使得曲线$$C_1$$和$$C$$成为Bertrand曲线偶的充分必要条件是，存在常数$$\lambda \neq 0$$和$$\mu$$使得$$\lambda \kappa + \mu \tau = 1$$。
{: .block-theorem }

**证明** 设曲线$$C$$有侣线$$C_1$$，它们的参数方程分别是$$\boldsymbol{r} (s)$$和$$\boldsymbol{r}_1 (s)$$，并且曲线$$C$$和$$C_1$$的对应是由相同参数点之间的对应，而且$$s$$是曲线$$C$$的弧长参数，$$\tilde{s}$$是曲线$$C_1$$的弧长参数。用$$\{\boldsymbol{r} (s); \boldsymbol{\alpha} (s), \boldsymbol{\beta} (s), \boldsymbol{\gamma} (s) \}$$表示曲线$$C$$的Frenet标架，则根据**定理2.11**的证明有
$$
\boldsymbol{\alpha}_1 (s) \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} 
= \big( 1-\lambda \kappa(s) \big) \boldsymbol{\alpha} (s) + \lambda \tau (s) \boldsymbol{\gamma} (s)
$$

其中$$\lambda \neq 0$$是常数。因此

$$
\bigg\vert \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} \bigg\vert^2 = ( 1-\lambda \kappa(s) )^2 + ( \lambda \tau (s) )^2
$$

另一方面由于$$\boldsymbol{\alpha}(s) \cdot \boldsymbol{\alpha}_1(s)$$为常数，我们可以得到

$$
\boldsymbol{\alpha}(s) \cdot \boldsymbol{\alpha}_1 (s) \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} 
= 1-\lambda \kappa(s)
$$

所以

$$
\frac{1-\lambda \kappa(s)}{\sqrt{1-\lambda \kappa(s) )^2 + ( \lambda \tau (s)) }} = \text{常数}
$$

故

$$
\frac{1-\lambda \kappa(s)}{\tau (s)} = \mu = \text{常数}
$$

即

$$
\lambda \kappa(s) + \mu \tau(s) = 1
$$

反过来，设正则参数曲线$$C$$的参数方程是$$\boldsymbol{r}(s)$$，$$s$$是弧长参数，并且它的曲率和挠率满足关系式$$\lambda \kappa + \mu \tau = 1$$，其中$$\lambda \neq 0$$和$$\mu$$是常数。我们可以构造一条新的曲线$$C_1$$，使它的参数方程是

$$
\boldsymbol{r}_1 (s) = \boldsymbol{r}(s) + \lambda \boldsymbol{\beta}(s)
$$

则

$$
\begin{aligned}
\boldsymbol{r}_1' (s) &= \boldsymbol{\alpha}(s) + \lambda (-\kappa (s) \boldsymbol{\alpha}(s) + \tau(s) \boldsymbol{\gamma}(s)) \\
&= (1 - \lambda \kappa(s)) \boldsymbol{\alpha}(s) + \lambda \tau(s) \boldsymbol{\gamma}(s) \\
&= \mu \tau (s) \boldsymbol{\alpha}(s) + \lambda \tau(s) \boldsymbol{\gamma}(s)
\end{aligned}
$$

因此，曲线$$C_1$$的单位切向量是

$$
\boldsymbol{\alpha}_1(s) = \frac{\mu}{\sqrt{\lambda^2 + \mu^2}} \boldsymbol{\alpha}(s) + \frac{\lambda}{\sqrt{\lambda^2 + \mu^2}} \boldsymbol{\gamma}(s)
$$

若用$$\tilde{s}$$作为曲线$$C_1$$的弧长参数，则

$$
\begin{aligned}
\frac{\mathrm{d} \boldsymbol{\alpha}_1 (s)}{\mathrm{d} \tilde{s}} \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} &= \kappa_1 (s) \boldsymbol{\beta}_1 (s) \frac{\mathrm{d} \tilde{s}}{\mathrm{d} s} \\
&= \bigg( \frac{\mu}{\sqrt{\lambda^2 + \mu^2}} \kappa (s) - \frac{\lambda}{\sqrt{\lambda^2 + \mu^2}} \tau (s) \bigg) \boldsymbol{\beta} (s)
\end{aligned}
$$

所以$$\boldsymbol{\beta}_1 (s) = \pm \boldsymbol{\beta} (s)$$，$$C_1$$和$$C$$成为Bertrand曲线偶。证毕∎

### 渐伸线和渐缩线

> ## 定义2.4
> 如果曲线$$C_1$$和$$C_2$$之间存在一个对应，使得曲线$$C_1$$在任意一点的切线恰好是曲线$$C_2$$在对应点的法线，则称曲线$$C_2$$是$$C_1$$的**渐伸线**，同时称曲线$$C_1$$是曲线$$C_2$$的**渐缩线**，如下图所示。
{: .block-definition }

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uv2Yebx.png" width="80%">
</div>

> ## 定理2.13
> 设正则参数曲线$$C$$的参数方程是$$\boldsymbol{r}(s)$$，$$s$$是弧长参数，则$$C$$的渐伸线的参数方程是$$\boldsymbol{r} = \boldsymbol{r}(s) + (c-s) \boldsymbol{\alpha} (s)$$，其中$$c$$是任意的常数。
{: .block-theorem }

**证明** 设
$$
\boldsymbol{r}_1 = \boldsymbol{r}(s) + \lambda(s) \boldsymbol{\alpha} (s)
$$
是曲线$$C$$的渐伸线，因此$$\alpha (s)$$应该是曲线$$\boldsymbol{r}_1 (s)$$的法向量。对上式求导可以得到

$$
\boldsymbol{r}_1' (s) = (1 + \lambda'(s)) \boldsymbol{\alpha} (s) + \lambda (s) \kappa (s) \boldsymbol{\beta} (s)
$$

将上式左右两边与$$\boldsymbol{\alpha} (s)$$作点乘得到

$$
\boldsymbol{r}_1' (s) \cdot \boldsymbol{\alpha} (s) = 1 + \lambda'(s) = 0
$$

因此

$$
\lambda (s) = c - s
$$

即$$C$$的渐伸线的参数方程为

$$
\boldsymbol{r}_1 (s) = \boldsymbol{r}(s) + (c-s) \boldsymbol{\alpha} (s)
$$

证毕∎

曲线的渐伸线可以看作是该曲线的切线族的正交轨线，而**定理2.13**可以解释为：将一条软线沿曲线放置，把一端固定，另一端慢慢离开原曲线，并且把软线抻直，使软线抻直的部分是在保持为原曲线的切线，则这另一端描出的曲线就是原曲线渐伸线。

> ## 定理2.14
> 设正则参数曲线$$C$$的参数方程是$$\boldsymbol{r}(s)$$，$$s$$是弧长参数，则$$C$$的渐缩线的参数方程是
$$\boldsymbol{r} = \boldsymbol{r}(s) + \frac{1}{\kappa (s)} \boldsymbol{\beta} (s) - \frac{1}{\kappa (s)} \bigg( \tan{\int \tau (s) \ ds} \bigg) \boldsymbol{\gamma} (s)
$$
。
{: .block-theorem }

**证明** 设
$$
\boldsymbol{r}_1 (s) = \boldsymbol{r} (s) + \lambda(s) \boldsymbol{\beta} (s) + \mu (s) \boldsymbol{\gamma} (s)
$$
是曲线$$C$$的渐缩线，那么$$\lambda(s) \boldsymbol{\beta} (s) + \mu (s) \boldsymbol{\gamma} (s)$$应该是曲线$$\boldsymbol{r}_1 (s)$$的切向量。对上式求导得到
$$
\begin{aligned}
\boldsymbol{r}_1' (s) &= (1 - \lambda (s) \kappa (s)) \boldsymbol{\alpha} (s) \\
&+ (\lambda' (s) - \mu(s) \tau (s)) \boldsymbol{\beta} (s) \\
&+ (\mu' (s) + \lambda (s) \tau (s)) \boldsymbol{\gamma} (s)
\end{aligned}
$$
因此$$\lambda(s) \boldsymbol{\beta} (s) + \mu (s) \boldsymbol{\gamma} (s)$$与$$\boldsymbol{r}_1'$$平行，即
$$
\lambda (s) \kappa (s) = 1
$$

$$
\frac{\lambda' (s) - \mu(s) \tau (s)}{\lambda (s)} = \frac{\mu' (s) + \lambda (s) \tau (s)}{\mu (s)}
$$
因此
$$
\lambda' (s) \mu (s) - \mu' (s) \lambda (s) = (\lambda^2 (s) + \mu^2 (s)) \tau (s)
$$

$$
\frac{d}{ds} \arctan \bigg( \frac{\mu (s)}{\lambda (s)} \bigg) = -\tau (s)
$$
故
$$
\arctan \bigg( \frac{\mu (s)}{\lambda (s)} \bigg) = -\int \tau (s) \ ds
$$
整理一下可以得到
$$
\lambda (s) = \frac{1}{\kappa (s)} , \ \ \mu (s) = -\frac{1}{\kappa (s)} \bigg( \tan \int \tau (s) \ ds \bigg)
$$
即曲线$$C$$的渐缩线为

$$\boldsymbol{r}_1 (s) = \boldsymbol{r}(s) + \frac{1}{\kappa (s)} \boldsymbol{\beta} (s) - \frac{1}{\kappa (s)} \bigg( \tan{\int \tau (s) \ ds} \bigg) \boldsymbol{\gamma} (s)
$$

## 平面曲线

根据[定理2.4](/2023/07/31/DifferentialGeometry-NOTES-02.html#曲线的挠率和frenet公式)，平面曲线可以看作是挠率为零的空间曲线，因此关于空间曲线的各种结论同样适用于平面曲线的情形。不过平面曲线有它自身的特点，因此本节只限于平面本身(而不考虑外围的空间)研究其中曲线的弯曲性质。

### 平面曲线的Frenet标架

在平面$$\mathbb{E}^2$$的右手笛卡尔直角坐标系下，曲线$$\boldsymbol{r} = \boldsymbol{r}(s)$$可以表示为

$$
\boldsymbol{r}(s) = (x(s), y(s))
$$

其中$$s$$是弧长参数，因此它的单位切向量是

$$
\boldsymbol{\alpha} (s) = (x'(s), y'(s)), \ \ (x'(s))^2 + (y'(s))^2 = 1
$$

因为平面$$\mathbb{E}^2$$是有向平面，故可以把$$\boldsymbol{\alpha} (s)$$沿正向旋转90°得到唯一的一个与$$\boldsymbol{\alpha} (s)$$垂直的单位向量$$\boldsymbol{\beta}(s)$$，很明显

$$
\boldsymbol{\beta} (s) = (-y'(s), x'(s))
$$

这样，沿曲线$$\boldsymbol{r} = \boldsymbol{r}(s)$$有一个定义好的右手单位正交标架场$$\{ \boldsymbol{r}(s); \boldsymbol{\alpha} (s), \boldsymbol{\beta} (s) \}$$，它在平面曲线的理论中所担当的角色相当于空间曲线的Frenet标架，称为**平面曲线的Frenet标架**。值得指出的是，平面曲线的Frenet标架场$$\{ \boldsymbol{r}(s); \boldsymbol{\alpha}, \boldsymbol{\beta} \}$$的确定只用到曲线参数方程的一阶导数，$$\boldsymbol{\beta}(s)$$是曲线的法向量，它与曲线的主法向量可能差一个正负号。

### 相对曲率

由于$$\boldsymbol{\alpha}(s)$$是单位向量场，故有$$\boldsymbol{\alpha} (s) \perp \boldsymbol{\alpha}' (s)$$，所以$$\boldsymbol{\alpha}'(s)$$是$$\boldsymbol{\beta}(s)$$的倍数，设为

$$
\boldsymbol{\alpha}' (s) = \kappa_r (s) \boldsymbol{\beta} (s)
$$

因此

$$
\begin{aligned}
\kappa_r (s) &= \boldsymbol{\alpha}' (s) \cdot \boldsymbol{\beta}(s) \\
&= -x''(s) y'(s) + y''(s) x'(s) \\
&=
\begin{vmatrix}
x'(s) & y'(s) \\
x''(s) & y''(s)
\end{vmatrix}
\end{aligned}
$$

我们把$$\kappa_r(s)$$称为平面曲线$$\boldsymbol{r} = \boldsymbol{r}(s)$$的**相对曲率**。如果该曲线的曲率是$$\kappa(s)$$，则有

$$
\kappa_r (s) = \pm \kappa (s)
$$

其中"+"号表示曲线朝$$\boldsymbol{\beta}(s)$$所指的方向弯曲，$$\boldsymbol{\beta}(s)$$恰好是曲线的主法向量；而"-"号表示曲线朝$$\boldsymbol{\beta}(s)$$所指的相反方向弯曲，曲线的主法向量是$$-\boldsymbol{\beta}(s)$$，如下图所示。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jji2pFE.png" width="80%">
</div>

平面曲线的Frenet标架的运动公式成为

$$
\begin{cases}
\boldsymbol{r}'(s)      &= &\boldsymbol{\alpha}(s) \\
\boldsymbol{\alpha}'(s) &= & &\kappa_r \boldsymbol{\beta} (s) \\
\boldsymbol{\beta}' (s) &= -\kappa_r (s) &\boldsymbol{\alpha} (s) & &
\end{cases}
$$

平面曲线的曲率中心是$$\boldsymbol{r} (s) + \boldsymbol{\beta} (s) / \kappa_r (s)$$，这也是平面曲线渐缩线的参数方程。

用$$\theta (s)$$表示单位切向量$$\boldsymbol{\alpha}(s)$$与$$x$$轴的正向所构成的角，称为向量$$\boldsymbol{\alpha}(s)$$的方向角。方向角是一个多值函数，但是在$$s$$的一个小范围内总是可以取出函数$$\theta (s)$$的一个连续分支。此时

$$
\begin{aligned}
\boldsymbol{\alpha} (s) &= (\cos \theta (s), &\sin \theta (s)) \\
\boldsymbol{\beta} (s) &= (-\sin \theta (s), &\cos \theta (s))
\end{aligned}
$$

即

$$
\begin{aligned}
x'(s) &= \cos \theta (s) \\ 
y'(s) &= \sin \theta (s)
\end{aligned}
$$

再求导得到

$$
\begin{aligned}
x''(s) &= -\sin \theta (s) \cdot \theta'(s) \\
y''(s) &= \cos \theta (s)\cdot \theta'(s)
\end{aligned}
$$

因此

$$
\kappa_r (s) = \frac{\mathrm{d} \theta (s)}{\mathrm{d} s}
$$

上式清楚地说明了相对曲率$$\kappa_r (s)$$的几何意义。对于平面曲线来说，曲线论基本定理成为下面的显示表达式

$$
\begin{aligned}
\theta (s) &= \theta (s_0) + \int_{s_0}^s \kappa_r (s) \ ds \\
x(s) &= x(s_0) + \int_{s_0}^s \cos \theta (s) \ ds \\
y(s) &= y(s_0) + \int_{s_0}^s \sin \theta (s) \ ds \\
\end{aligned}
$$

若平面曲线$$\boldsymbol{r} = \boldsymbol{r} (t)$$的参数方程是

$$
\boldsymbol{r} (t) = (x(t), y(t))
$$

其中$$t$$未必是弧长参数。曲线的弧长元素是

$$
ds = \vert \boldsymbol{r}'(t) \vert \ \mathrm{d} t = \sqrt{(x')^2 + (y')^2} \ \mathrm{d} t
$$

因此它的单位切向量是

$$
\boldsymbol{\alpha} (t) = \frac{\boldsymbol{r}'(t)}{\vert \boldsymbol{r}'(t) \vert} = \bigg( \frac{x'}{\sqrt{(x')^2 + (y')^2}}, \frac{y'}{\sqrt{(x')^2 + (y')^2}} \bigg)
$$

法向量是

$$
\boldsymbol{\beta} (t) = \bigg( -\frac{y'}{\sqrt{(x')^2 + (y')^2}}, \frac{x'}{\sqrt{(x')^2 + (y')^2}} \bigg)
$$

因此曲线$$C$$的相对曲率是

$$
\begin{aligned}
\kappa_r (t) &= \frac{\mathrm{d} \boldsymbol{\alpha} (t)}{\mathrm{d} s} \cdot \boldsymbol{\beta}(t) = \frac{\mathrm{d} t}{\mathrm{d} s} \cdot \boldsymbol{\alpha}' (t) \cdot \boldsymbol{\beta}(t) \\
&= \frac{x' y'' - x'' y'}{\sqrt{\big( (x')^2 + (y')^2 \big)^3}}
\end{aligned}
$$

### 旋转指标

对于整条平面曲线$$\boldsymbol{r} = \boldsymbol{r}(s) \ (a \leq s \leq b)$$而言，也能取出其方向角的连续分支$$\theta (s)$$。事实上，在每一点处的单位切向量$$\boldsymbol{\alpha} (s)$$的方向角确定到差$$2 \pi$$的整数倍。这样，我们可以将区间划分得充分的小，设为

$$
a = s_0 \lt s_1 \lt \dots \lt s_n = b
$$

使得在每一小段区间$$[s_i, s_{i+1}]$$上，方向角的连续分支$$\theta (s)$$的变差不超过$$\pi$$。然后，从区间$$[s_0, s_1]$$的一个连续分支出发，依次唯一地确定了各个区间$$[s_i, s_{i+1}]$$上的连续分支$$\theta(s)$$，最终得到定义在整条曲线上的方向角连续分支$$\theta (s)$$。由此可见，方向角的任意两个连续分支$$\theta(s)$$和$$\tilde{\theta}(s)$$之间差$$2 \pi$$的一个整数倍，即有整数$$k$$使得

$$
\tilde{\theta} (s) - \theta (s) = 2k \pi
$$

由于左边是$$s$$的连续函数，因此$$k$$只能是常数。故一条平面曲线的方向角的总变差与连续分支的取法无关，即

$$
\tilde{\theta} (b) - \tilde{\theta} (a) = \theta (b) - \theta (a)
$$

根据[相对曲率](/2023/07/31/DifferentialGeometry-NOTES-02.html#相对曲率)的几何意义得知

$$
\theta (b) - \theta (a) = \int_a^b \kappa_r (s) \ \mathrm{d} s
$$

如果$$\boldsymbol{r} = \boldsymbol{r}(s) \ (a \leq s \leq b)$$是$$\mathbb{E}^2$$上的一条光滑曲线，并且

$$
\boldsymbol{r} (b) = \boldsymbol{r} (a), \ \ 
\boldsymbol{r}' (b) = \boldsymbol{r}' (a), \ \ 
\boldsymbol{r}'' (b) = \boldsymbol{r}'' (a), \ \ ...
$$

则称它为光滑闭曲线。如果$$\boldsymbol{r} = \boldsymbol{r}(s) \ (a \leq s \leq b)$$是若干段光滑曲线首尾相接而成的，并且$$\boldsymbol{r} (b) = \boldsymbol{r} (a)$$，则称它是分段光滑的闭曲线。

如果$$\boldsymbol{r} = \boldsymbol{r}(s) \ (a \leq s \leq b)$$是$$\mathbb{E}^2$$上的闭曲线，并且对于任意的$$a \leq s_1 \lt s_2 \lt b$$都有

$$
\boldsymbol{r} (s_1) \neq \boldsymbol{r} (s_2)
$$

则称该曲线是简单的。简单闭曲线就是没有自交点的闭曲线。

对于连续可微的闭曲线

$$
C: \boldsymbol{r} = \boldsymbol{r} (s), \ \ a \leq s \leq b
$$

它的单位切向量$$\boldsymbol{\alpha} (s)$$绕曲线转一圈回到起点时与原来的单位切向量重合，因此方向角的总变差$$\theta(b) - \theta(a)$$一定是$$2 \pi$$的整数倍，它与方向角连续分支$$\boldsymbol{\alpha} (s)$$的选取无关。命

$$
i(C) = \frac{1}{2 \pi} \big( \theta(b) - \theta(a) \big)
$$

称为连续可微闭曲线$$C$$的**旋转指标**。

> ## 定理2.15
> 若$$C$$是平面$$\mathbb{E}^2$$上一条连续可微的简单闭曲线，则它的旋转指标$$i(C) = \pm 1$$。
{: .block-theorem }

这是曲线的大范围微分几何性质，其直观意义是明显的，但是它的证明不是很简单。若$$C$$是分段光滑的简单闭曲线，则曲线的方向角的总变差是

$$
\theta (b) - \theta (a) = \int_a^b \kappa_r (s) \ \mathrm{d} s + \sum_i \theta_i
$$

这里$$\theta_i$$是曲线在各个角点处的外角，$$-\pi \lt \theta_i \lt \pi$$，即

$$
\theta_i = \angle (\boldsymbol{\alpha} (s_i - 0), \boldsymbol{\alpha} (s_i + 0))
$$

其中$$s_i$$是曲线的角度所对应的参数，曲线在每一段区间$$(s_i, s_{i+1})$$上是连续可微的。在上述意义下，旋转指标定理对于分段连续可微函数的简单闭曲线仍然是成立的。