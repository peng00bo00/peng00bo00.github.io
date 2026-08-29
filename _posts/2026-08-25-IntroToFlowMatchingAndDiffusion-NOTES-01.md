---
layout: post
title: MIT 6.S184课程笔记1-Flow and Diffusion Models
date: 2026-08-25
description: 流模型与扩散模型
tags: DL Diffusion
categories: MIT-6.S184
giscus_comments: false
related_posts: false
toc:
  sidebar: left
pseudocode: true
---


> 这个系列是[MIT 6.S184 - Introduction to Flow Matching and Diffusion Models](https://diffusion.csail.mit.edu/2026/index.html)的同步课程笔记。本门课程面向希望对流模型与扩散模型有更深入理解的学生和研究者，从最基础的数学工具出发逐步推导这些模型背后的数学原理，并介绍相应的训练与采样算法。本节课主要介绍生成模型的基本概念，以及流模型与扩散模型的整体框架。
{: .block-preface }


生成式模型是如今AI社区最为火热的话题。从图像生成到视频生成，再到如今已经取得广泛应用的大语言模型，这些应用的背后都是生成式模型在发挥作用。尽管这些场景中的数据类型各不相同，但它们大多都是使用了同一套算法，即本门课程将要着重讨论的流模型与扩散模型。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jITR263.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sRwib3d.png" width="100%">
</div>

和其它同类课程相比，本门课程会更关注流模型与扩散模型背后的数学原理。具体来说，我们会借助ODE以及SDE这样的数学工具来理解模型的数学本质，同时也会通过一些lab动手实现相关的模型和算法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/hutjaaw.png" width="100%">
</div>

## From Generation to Sampling

为了方便计算机表示，我们一般会把不同类型的数据都结构化为多维数组。以RGB图像为例，在计算机中图像都是以`[H, W, 3]`尺寸的3维数组进行表示。类似地，视频可以表示为`[T, H, W, 3]`尺寸的4维数组，而分子结构则可以类似表示为`[N, 3]`尺寸的2维数组。显然，这些结构化的数据本质上都是使用向量来进行表示。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bck4xwK.png" width="100%">
</div>

对于结构化数据的生成问题，我们可以将其形式化为高维向量的概率分布问题。我们为高维空间定义一个概率密度函数，每个数据点上的函数值都对应标签的概率(密度)。这样生成结果的好坏就可以使用进行概率密度的大小进行描述。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fvT0Tql.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RAYOaS3.png" width="100%">
</div>

在这样的视角下，数据的生成过程实际上就是从数据的概率密度函数中进行采样的过程。类似地，我们的训练数据实际上也是从真实数据的概率分布中获取的一次抽样。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Sdfuu3P.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/10xZxtQ.png" width="100%">
</div>

值得额外说明的是，实践中应用更为广泛的模型是条件生成模型，这种模型的好处是可以通过提示词来更好地控制生成数据的结果。从数学的角度出发，迁移到条件生成模型只需要把普通的概率换成条件概率即可。在后面的课程里我们还会专门介绍相关的技术，现阶段先专注于通常的无条件概率即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/X0EEhgI.png" width="100%">
</div>

而数据生成则需要从高维数据的概率分布中进行采样。一般来说直接对高维分布进行采样是比较困难的，目前通用的做法是从一个相对好采样的初始概率分布(如正态分布)中进行采样，然后通过模型变换到目标概率分布上。从这个角度看，生成模型的实质是从初始分布到目标分布的变换。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PgUCgEA.png" width="100%">
</div>

## Flow and Diffusion Models

### Flow Models

接下来我们会开始介绍流模型与扩散模型，在进入具体的代码之前我们需要先了解回顾一些基本的数学知识。首先我们定义**轨迹(Trajectory)**为$$[0, 1]$$区间到$$\mathbb{R}^{d}$$空间的映射

$$
X : [0, 1] \rightarrow \mathbb{R}^{d}, \ \ \ t \mapsto X_t
$$

**向量场(Vector Field)**则是一个从$$\mathbb{R}^{d}$$空间到$$\mathbb{R}^{d}$$空间的映射

$$
u : \mathbb{R}^{d} \times [0, 1]\rightarrow \mathbb{R}^{d}, \ \ \ (x, t) \mapsto u_t(x)
$$

二者通过**常微分方程(oridinary differential equation, ODE)**联系在一起

$$
\frac{\mathrm{d}}{\mathrm{d} t} X_t = u_t(X_t), \ \ \ X_0 = x_0
$$

换句话说，ODE和向量场定义了粒子的行为，而粒子的轨迹则是ODE的解。以下图为例，背景中的箭头描述了不同时刻空间中的向量场，而白色的曲线则是粒子在向量场驱动下的运动轨迹。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6YXBhP8.png" width="100%">
</div>

基于ODE的概念，我们定义**flow**$$\psi$$为初始点$$x_0$$与时间$$t$$到时刻$$t$$所在位置的映射，其在时间上的演化则由向量场给出称为**flow ODE**。其严格数学定义如下

$$\
\begin{aligned}
\psi : \mathbb{R}^d \times [0, 1] \rightarrow \mathbb{R}^d, &\quad (x_0, t) \mapsto \psi_t(x_0) \\
\frac{\mathrm{d}}{\mathrm{d} t} \psi_t(x_0) &= u_t(\psi_t(x_0)) \\
\psi_0(x_0) &= x_0
\end{aligned}
$$

因此从直觉上来讲向量场、ODE和flow都在描述同样的事情，即粒子在向量场驱动下的运动轨迹：向量场定义了ODE，而ODE的解即为flow。

接下来的问题是ODE是否有解以及如果有解的情况下是否存在唯一解。在数学上这个问题已经被回答了：当向量场是Lipschitz连续时ODE有唯一解，称为[Picard–Lindelöf定理](https://en.wikipedia.org/wiki/Picard%E2%80%93Lindel%C3%B6f_theorem)。不过在机器学习的范畴里我们一般可以直接假定上述条件成立，ODE以及对应的flow一定存在且唯一。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QtSRsBM.png" width="100%">
</div>

对于简单形式的ODE是可以通过直接求解来获得解析解的。以下图为例，对于向量场$$u_t(x) = -\theta x$$，其解析解为

$$
\psi_t(x_0) = \exp(-\theta t) x_0
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/n3gLAAk.png" width="100%">
</div>

不过大多数情况下ODE都是无法解析求解的，此时则需要通过数值方法来近似求解。目前最简单也是最常用的方法是Euler方法，通过前向积分来近似求解ODE。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/0W831Do.png" width="100%">
</div>

对于Euler方法来说，积分的时间步长$$h$$对于最终的结果有很大的影响。过大的步长会影响精度，而过小的步长则会影响效率。实际应用中需要对二者进行权衡。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eHtCKMT.png" width="100%">
</div>

回到机器学习和生成模型的话题，前面已经介绍过生成的过程可以理解为从初始分布到目标分布的变换，因此我们可以利用flow的方式将初始分布$$p_\text{init}$$变换到目标分布$$p_\text{data}$$上。具体来说，对于给定学习好的神经网络向量场$$u_t^\theta$$，可以通过Euler方法从初始分布中采样并逐步积分到目标分布，这样就得到了如下所示的流模型数据生成算法。

```pseudocode
\begin{algorithm}
\caption{Sampling from a Flow Model with Euler method}
\begin{algorithmic}
\REQUIRE Neural network vector field $$u_t^\theta$$, number of steps $$n$$
\STATE Set $$t = 0$$
\STATE Set step size $$h = \frac{1}{n}$$
\STATE Draw a sample $$X_0 \sim p_\text{init}$$
\FOR{$$i = 1, \ldots, n$$}
    \STATE $$X_{t+h} = X_t + h u_t^\theta(X_t)$$
    \STATE Update $$t \leftarrow t + h$$
\ENDFOR
\RETURN $$X_1$$
\end{algorithmic}
\end{algorithm}
```

### Diffusion Models

扩散模型和流模型整体上是类似的，不过在扩散模型中我们需要引入随机性来描述轨迹。为此我们需要引入**随机微分方程(stochastic differential equation, SDE)**来描述扩散模型。回顾一下在ODE中对轨迹的微分可以表示为

$$
\mathrm{d} X_t = u_t(X_t) \  \mathrm{d} t
$$

而在SDE中，我们还需要加入随机微分项来表示随机性

$$
\mathrm{d} X_t = u_t(X_t) \  \mathrm{d} t + \sigma_t \ \mathrm{d} W_t
$$

其中$$\sigma_t$$称为**扩散系数(diffusion coefficient)**，$$W_t$$则是**布朗运动(Brownian motion)**。

布朗运动也称为**Wiener过程(Wiener process)**，其本身是一类非常重要的随机过程。它的初值为$$W_0 = 0$$，且满足以下两条重要性质：

1. **正态增量(Normal increments)**: 任意两个时刻$$0 \leq s \lt t$$的布朗运动的增量服从正态分布且方差随时间差线性增长，即$$W_t - W_s \sim \mathcal{N} (0, (t-s) I_d)$$
2. **独立增量(Independent increments)**: 任意两个时刻的布朗运动的增量相互独立

因此我们可以利用布朗运动的增量性质来构造出布朗运动

$$
W_{t+h} = W_t + \sqrt{h} \epsilon, \quad \epsilon \sim \mathcal{N}(0, I_d)
$$

需要额外说明的是这里我们对微分符号$$\mathrm{d}$$存在一些滥用，实际上由于随机性的存在是无法直接进行微分运算的。这里的微分符号可以理解为差分，以ODE为例其前向过程可以表示为

$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d} t} X_t = u_t(X_t) &\Leftrightarrow \frac{1}{h} (X_{t+h} - X_t) = u_t(X_t) \\
&\Leftrightarrow X_{t+h} = X_t + h u_t(X_t) + h R_t(h)
\end{aligned}
$$

其中$$R_t(h)$$是积分误差，满足

$$\lim_{h \to 0} R_t(h) = 0$$

对于SDE的情况则可以表示为

$$
X_{t+h} = X_t + \underbrace{h u_t(X_t)}_{\text{deterministic}} + \underbrace{\sigma_t (W_{t+h} - W_t)}_{\text{stochastic}} + \underbrace{h R_t(h)}_{\text{error term}}
$$

其中的随机误差项$$R_t(h)$$的标准差满足

$$
\lim_{h \to 0} \mathbb{E}[R_t(h)^2]^{1/2} = 0
$$

类似于ODE的情况，SDE是否有解以及解是否唯一同样由向量场$$u_t(x)$$决定。不过在实际应用中我们一般直接假定SDE有解且唯一。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/72Fh2fI.png" width="100%">
</div>

同样我们也可以通过Euler方法来近似求解SDE，称为Euler-Maruyama方法。基于Euler-Maruyama方法的扩散模型生成算法流程如下：

```pseudocode
\begin{algorithm}
\caption{Sampling from a Diffusion Model (Euler-Maruyama method)}
\begin{algorithmic}
\REQUIRE Neural network $$u_t^\theta$$, number of steps $$n$$, diffusion coefficient $$\sigma_t$$
\STATE Set $$t = 0$$
\STATE Set step size $$h = \frac{1}{n}$$
\STATE Draw a sample $$X_0 \sim p_\text{init}$$
\FOR{$$i = 1, \ldots, n$$}
    \STATE Draw a sample $$\epsilon \sim \mathcal{N}(0, I_d)$$
    \STATE $$X_{t+h} = X_t + h u_t^\theta(X_t) + \sigma_t \sqrt{h} \epsilon$$
    \STATE Update $$t \leftarrow t + h$$
\ENDFOR
\RETURN $$X_1$$
\end{algorithmic}
\end{algorithm}
```

## Reference

- [Lecture 1 - Flow and Diffusion Models](https://www.youtube.com/watch?v=9eJQQVrUUoI)
- [Introduction to Flow Matching and Diffusion Models](https://arxiv.org/abs/2506.02070)