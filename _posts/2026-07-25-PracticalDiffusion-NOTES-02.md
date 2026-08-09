---
layout: post
title: MIT 6.S183课程笔记2-Perspectives on Diffusion
date: 2026-07-25
description: 不同视角下的扩散模型
tags: DL Diffusion
categories: MIT-6.S183
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


> 这个系列是[MIT 6.S183 - A Practical Introduction to Diffusion](https://www.practical-diffusion.org/)的同步课程笔记。本门课程面向对扩散模型感兴趣的学生和研究者，从最底层开始逐步介绍扩散模型的数学原理以及各种应用。本节课主要介绍不同视角下的扩散模型。
{: .block-preface }

在正式开始本节的内容前首先来简要回顾一下扩散模型的相关概念。扩散模型来源于噪声扩散过程和布朗运动；在训练模型时我们实际上训练的是一个预测数据噪声的denoiser，这样就可以通过反向去噪的过程来实现从噪声中恢复原始数据，从而实现生成式学习。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/E54aLVf.png" width="100%">
</div>

本节的内容主要涵盖以DDIM为代表的确定性采样和DDPM为代表的随机采样两种数据生成策略，以及它们背后的数学原理。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FswLD29.png" width="100%">
</div>

## Continuous vs Discrete Diffusion Processes

为了方便叙述，我们首先来梳理一下相关的概念。在上一节介绍扩散模型时使用了离散的生成过程，为了数学推导上的方便这里统一使用连续视角下的扩散过程。此时扩散模型的变量都会被规范化到$$[0, 1]$$的区间上，而前向过程则可以表示为

$$
x(t) = x(0) + \sigma (t) \epsilon
$$

$$
\epsilon \sim \mathcal{N} (0, I), \ \sigma(0) = 0
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/aJRwI3e.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qFCoJEd.png" width="100%">
</div>

出于记号的便利性考虑，我们把时间参数$$t$$使用下标的形式进行表示，有下标的地方表示其是关于时间参数$$t$$的函数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/caOjYFf.png" width="100%">
</div>

## Deterministic Sampling

### Flow-ODE

我们可以把扩散过程沿着时间轴进行展开，在任意时间$$t$$数据都对应着一个概率分布$$p(x_t)$$。根据前向过程的公式，只要知道初始数据的分布就可以很容易地得到任意时间$$t$$时刻的概率分布。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/toNHHHK.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/BihVjGd.png" width="100%">
</div>

当然初始时刻数据的分布是未知的，但考虑到最后完成扩散时数据会演化为完全的噪声，我们可以考虑构造一个流将噪声分布还原为原始的数据分布。具体而言，我们可以设计一个速度场$$\dot{x}_t = v(x, t)$$，对于$$t=1$$时刻的任意噪声延速度场进行积分即可获得它在扩散前的原始数据，这样就实现了噪声分布恢复为原始数据分布的过程。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qKjEmqr.png" width="100%">
</div>

### Transport Equation

当然这个速度场不是任意的，它需要满足一些约束条件称为[Transport Equation](https://en.wikipedia.org/wiki/Continuity_equation)，它指出我们所需的速度场是如下ODE的解：

$$
\frac{\partial p_t(x)}{\partial t} + \nabla \cdot \big( v(x, t) \ p_t (x) \big)= 0
$$

实际上上式也是流体力学中的连续性方程，它表示任意时刻$$x$$处流入和流出的(概率)密度必须是相同的。

求解这个ODE并不容易，不过对于扩散过程Transport Equation存在闭式解

$$
v(x, t) = \dot{\sigma} (t) \mathbb{E} [\epsilon \vert x_t]
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/LKmXkSH.png" width="100%">
</div>

### DDIM Sampler

对于期望函数$$\mathbb{E} [\epsilon \vert x_t]$$我们可以使用训练的denoiser进行表示，这样通过Euler前向积分就可以得到初始时刻的数据分布，而这一过程的离散形式即为DDIM的采样公式。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TQBnYSA.png" width="100%">
</div>

## Stochastic Sampling

除了上面介绍的确定性采样方法外，在扩散模型刚刚问世的时候更通用的采样方法是基于随机游走的采样。在随机采样方法中，每一步采样都会加入随机噪声使得每一步采样也是随机的，这导致随机采样往往需要更多的采样次数，但也更容易获得更高的数据生成质量。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4gwg1WQ.png" width="100%">
</div>

### Stochastic Differential Equations

要更深入地讨论随机采样方法就必须要借助**随机微分方程(Stochastic Differential Equations, SDE)**这样的数学工具。当然SDE本身就是非常复杂的，这里仅做一些简单地概念性介绍。对于一维的情况，随机游走可以表示为

$$
\mathrm{d} x = f(x, t) \mathrm{d} t + \rho(x, t) \mathrm{d} B_t
$$

其中，函数$$f(x, t)$$称为漂移项(drift term)，对应均值的确定性运动；$$\rho(x, t)$$称为扩散项(diffusion term)，表示随机运动的强度；而$$\mathrm{d} B_t$$则是布朗运动(Brownian motion)，表示随机运动。

上式SDE更容易理解的离散形式可以表示为

$$
x_{t + \Delta t} \approx x_t + f(x_t, t) \Delta t + \rho(x, t) w_t \sqrt{\Delta t}
$$

其中$$w_t \sim \mathcal{N}(0, I)$$。需要注意这里扩散项的时间尺度对应的是$$\sqrt{\Delta t}$$，这使得SDE比常规的ODE要更加难以处理。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/m8691wL.png" width="100%">
</div>

同时还需要注意，在SDE中我们不能直接把$$\Delta t$$取反为$$-\Delta t$$来表示反向过程(显然根号里不能为负)。实际上反向SDE的正确表达式为

$$
x_{t - \Delta t} \approx x_t - f(x_t, t) \Delta t + \rho(x, t) w_t \sqrt{\Delta t}
$$

对应的微分表达式为

$$
\mathrm{d} x = f(x, t) \overleftarrow{\mathrm{d} t} + \rho(x, t) \mathrm{d} \overleftarrow{B_t}
$$

需要注意的是，在反向SDE中每一步仍然需要添加噪声。换句话说，在SDE中前向和反向过程是不对称的，从终点执行反向SDE**不能**保证回到起点。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/D2OBvJB.png" width="100%">
</div>

### Fokker-Planck Equation

类似于ODE中的[Transport Equation](#transport-equation)，在反向SDE中有[Fokker-Planck Equation](https://en.wikipedia.org/wiki/Fokker%E2%80%93Planck_equation)给出了反向过程存在的约束条件。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5m2lcXg.png" width="100%">
</div>

对比一下ODE版本的约束方程，在SDE中我们的目标是寻找一个合适的漂移项$$f(x, t)$$使得它能够满足Fokker-Planck Equation。假设ODE和SDE对应同一个扩散过程，此时可以定义出满足约束条件的$$f(x, t)$$如下

$$
f(x, t) = v(x, t) - \rho(t) \nabla \log p_t(x)
$$

其中，$$\nabla \log p_t(x)$$这一项也称为score function。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/p50OqZ2.png" width="100%">
</div>

前面我们已经推导过速度场的表达式$$v(x, t) = \dot{\sigma} (t) \epsilon^* (x, t)$$，剩下的只需计算$$\nabla \log p_t(x)$$即可实现SDE的反向过程。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/rRvxfX4.png" width="100%">
</div>

### Tweedie's Formula

接下来对噪声估计进行变形有

$$
\begin{aligned}
\epsilon^* (x_t, t) &= \mathbb{E} [\epsilon \vert x_t] \\
&= \frac{1}{\sigma_t} \mathbb{E} [x_t - x_0 \vert x_t] \\
&= \frac{x_t}{\sigma_t} - \frac{1}{\sigma_t} \mathbb{E} [x_0 \vert x_t]
\end{aligned}
$$

同时，[Tweedie's Formula](https://pmc.ncbi.nlm.nih.gov/articles/PMC3325056/)指出在已知$$x_t$$条件下初始状态$$x_0$$的后验期望为

$$
\mathbb{E} [x_0 \vert x_t] = x_t + \sigma_t^2 \nabla \log{p(x_t)}
$$

把上面两个公式结合起来就得到了denoiser与score function的关系式

$$
\epsilon^* (x_t, t) = -\sigma_t \nabla \log{p(x_t)}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/aVjlj77.png" width="100%">
</div>

实际上利用Tweedie's Formula可以证明denoiser、score function、flow matching等都是相互等价的，通过调整参数化可以互相转换。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/68wQcdR.png" width="100%">
</div>

### DDPM Sampler

把上面的内容整理一下就可以得到漂移项的表达式

$$
\begin{aligned}
f(x, t) &= v(x, t) - \rho(t) \nabla \log p_t(x) \\
&= \bigg( \dot{\sigma}(t) - \frac{\rho (t)}{\sigma (t)} \bigg) \epsilon^* (x, t)
\end{aligned}
$$

在离散形式下对反向SDE进行积分就可以得到DDPM的采样公式。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/piFVwjI.png" width="100%">
</div>

实际上不难发现，DDIM和DDPM采样是相互等价的。通过Fokker-Planck Equation，我们可以构建出ODE和SDE之间的桥梁，进而得到不同的采样方法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/G8JXfJ2.png" width="100%">
</div>

本节内容简单总结如下。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bOX1d8b.png" width="100%">
</div>

## Reference

- [Lecture 2: Perspectives on Diffusion](https://www.youtube.com/watch?v=-ML7zwmUjvo)
- [What are Diffusion Models?](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/)
- [Generative Modeling by Estimating Gradients of the Data Distribution](https://yang-song.net/blog/2021/score/)