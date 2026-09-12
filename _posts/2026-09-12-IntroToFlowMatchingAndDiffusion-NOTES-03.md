---
layout: post
title: MIT 6.S184课程笔记3-Score Functions and Score Matching
date: 2026-09-12
description: 流匹配
tags: DL Diffusion
categories: MIT-6.S184
giscus_comments: false
related_posts: false
toc:
  sidebar: left
pseudocode: true
---


> 这个系列是[MIT 6.S184 - Introduction to Flow Matching and Diffusion Models](https://diffusion.csail.mit.edu/2026/index.html)的同步课程笔记。本门课程面向希望深入理解流模型与扩散模型的学生和研究者，从最基础的数学工具出发，逐步推导这些模型背后的数学原理，并介绍相应的训练与采样算法。本节课主要介绍Score Matching算法背后的数学原理。
{: .block-preface }


在上一节课中，我们从条件概率和边缘概率两个视角介绍了概率路径以及向量场的概念。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GM7g3tz.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dQ2dMTe.png" width="100%">
</div>

在此基础上，我们推导出了flow matching算法的损失函数以及训练过程。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fXlqKKh.png" width="100%">
</div>

得到(边缘)向量场后，我们就可以使用ODE进行采样实现数据的生成。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9WBCh0A.png" width="100%">
</div>

本节课中，我们会引入score function的概念，并学习如何基于SDE和score function来实现生成。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/E14VA4g.png" width="100%">
</div>

## Score Function

**score function**定义为对数似然函数$$\log q (x)$$的梯度，即$$\nabla \log q (x)$$。从优化的角度来看，score function是使对数似然函数增大最快的方向。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QFAJH5c.png" width="100%">
</div>

类似于条件概率路径和边缘概率路径，我们也可以定义**conditional score function**和**marginal score function**分别为$$\nabla \log p_t(x \vert z)$$和$$\nabla \log p_t(x)$$，它们之间的关系式如下：

$$
\begin{aligned}
\nabla \log p_t(x) &= \frac{\nabla p_t (x)}{p_t (x)} = \frac{\nabla \int p_t (x \vert z) p_{\text{data}} (z) \mathrm{d} z}{p_t (x)} \\
&= \frac{\int \nabla p_t (x \vert z) p_{\text{data}} (z) \mathrm{d} z}{p_t (x)} \\
&= \frac{\int \nabla p_t (x \vert z) p_{\text{data}} (z) \mathrm{d} z}{p_t (x)} \\
&= \int \nabla \log p_t(x \vert z) \frac{p_t (x \vert z) p_{\text{data}} (z)}{p_t (x)} \mathrm{d} z
\end{aligned}
$$

不难发现，上式中两个score function的关系类似于向量场的[边缘化技巧](/blog/2026/IntroToFlowMatchingAndDiffusion-NOTES-02/#marginal-vector-field)：marginal score function是conditional score function关于后验$$\frac{p_t (x \vert z) p_{\text{data}} (z)}{p_t (x)}$$的期望。

### Score of Gaussian Probability Path

对于Gauss probability path，我们可以显式计算它的conditional score function。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/8f8Kl2w.png" width="100%">
</div>

类似地，我们可以计算marginal score function。整理一下可以得到Gauss probability path相关的所有计算公式如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dBWQHJu.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2XKkukM.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UR03z1X.png" width="100%">
</div>

不难发现，Gauss probability path的conditional score function和conditional vector field有非常相似的形式。实际上只需要进行一些简单的代数变换，我们就可以score function使用vector field的形式来表示。因此可以认为score function和vector field是相互等价的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2361cqx.png" width="100%">
</div>

## Score Matching

类似于flow matching算法，score matching同样是使用一个神经网络来表达score function。因此可以定义score matching的损失函数为：

$$
\mathcal{L}_{\text{SM}} (\theta) = \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, x \sim p_t(x \vert z)} \big[ \| s_t^\theta (x) - \nabla \log p_t(x) \|^2 \big]
$$

而使用conditional score function作为优化目标的损失函数称为denoising score matching loss：

$$
\mathcal{L}_{\text{DSM}} (\theta) = \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, x \sim p_t(x \vert z)} \big[ \| s_t^\theta (x) - \nabla \log p_t(x \vert z) \|^2 \big]
$$

同样地，可以证明二者之间只相差一个常数$$C$$

$$
\mathcal{L}_{\text{SM}} (\theta) = \mathcal{L}_{\text{DSM}} (\theta) + C
$$

这样我们就得到了score matching算法的损失函数以及训练过程。

```pseudocode
\begin{algorithm}
\caption{Score Matching Training Procedure (General)}
\begin{algorithmic}
\REQUIRE A dataset of samples $z \sim p_{\text{data}}$, score network $s_t^\theta$
\FOR{each mini-batch of data}
    \STATE Sample a data example $z$ from the dataset
    \STATE Sample a random time $t \sim \text{Unif}_{[0,1]}$
    \STATE Sample $x \sim p_t(\cdot \vert z)$
    \STATE Compute loss $\mathcal{L}(\theta) = \| s_t^\theta (x) - \nabla \log p_t (x \vert z) \|^2$
    \STATE Update the model parameters $\theta$ via gradient descent on $\mathcal{L}(\theta)$
\ENDFOR
\end{algorithmic}
\end{algorithm}
```

### Score Matching for Gaussian Probability Paths

对于Gauss probability path，我们可以直接计算它的conditional score function。

$$
\nabla \log p_t (x \vert z) = - \frac{x - \alpha_t z}{\beta_t^2}
$$

基于此可以得到score matching的损失函数：

$$
\begin{aligned}
\mathcal{L}_{\text{DSM}} (\theta) &= \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, x \sim p_t(x \vert z)} \big[ \| s_t^\theta (x) - \nabla \log p_t(x \vert z) \|^2 \big] \\
&= \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, x \sim p_t(x \vert z)} \bigg[ \bigg\| s_t^\theta (x) + \frac{x - \alpha_t z}{\beta_t^2} \bigg\|^2 \bigg] \\
&= \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, \epsilon \sim \mathcal{N}(0, I_d)} \bigg[ \bigg\| s_t^\theta (\alpha_t z + \beta_t \epsilon) + \frac{\epsilon}{\beta_t} \bigg\|^2 \bigg]
\end{aligned}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/InwwvnU.png" width="100%">
</div>

对应的训练过程伪代码如下：

```pseudocode
\begin{algorithm}
\caption{Score Matching Training Procedure for Gaussian probability path}
\begin{algorithmic}
\REQUIRE A dataset of samples $z \sim p_{\text{data}}$, score network $s_t^\theta$ or noise predictor $\epsilon_t^\theta$
\REQUIRE Schedulers $\alpha_t$, $\beta_t$ with $\alpha_0 = \beta_1 = 0$, $\alpha_1 = \beta_0 = 1$
\FOR{each mini-batch of data}
    \STATE Sample a data example $z$ from the dataset
    \STATE Sample a random time $t \sim \text{Unif}_{[0,1]}$
    \STATE Sample noise $\epsilon \sim \mathcal{N}(0, I_d)$
    \STATE Set $x_t = \alpha_t z + \beta_t \epsilon$
    \STATE Compute loss $\mathcal{L}(\theta) = \| s_t^\theta (x_t) + \frac{\epsilon}{\beta_t} \|^2$
    \STATE Update the model parameters $\theta$ via gradient descent on $\mathcal{L}(\theta)$
\ENDFOR
\end{algorithmic}
\end{algorithm}
```

## Sampling with SDEs

### SDE Extension Trick

当我们训练好score function后，接下来的问题是如何使用它进行采样。实际上score function与边缘向量场以及SDE有着密切的联系，对于给定的边缘向量场$$u_t^{\text{target}} (X_t)$$以及扩散系数$$\sigma_t \geq 0$$，可以定义对应的**随机动力学(stochastic dynamics)**为

$$
\mathrm{d} X_t = \bigg[ u_t^{\text{target}} (X_t) + \frac{\sigma_t^2}{2} \nabla \log p_t (X_t) \bigg] \mathrm{d} t + \sigma_t \mathrm{d} W_t
$$

注意这里$$\sigma_t$$是可以任意指定的，理论上我们可以使用任意非负的$$\sigma_t$$来实现采样，它与模型的训练过程无关。上式中$$\sigma_t \mathrm{d} W_t$$表示对数据添加噪声的过程，与数据本身无关；而$$\frac{\sigma_t^2}{2} \nabla \log p_t (X_t)$$则可以理解为对噪声数据进行修正，修正量取决于当前状态的score function。

对于Gauss probability path，我们可以直接计算它的推导它的随机动力学为

$$
\mathrm{d} X_t = \bigg[ \bigg( a_t + \frac{\sigma_t^2}{2} \bigg) s_t^\theta (X_t)+ b_t X_t \bigg] \mathrm{d} t + \sigma_t \mathrm{d} W_t
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/LJRGHT0.png" width="100%">
</div>

### Fokker-Planck Equation

对于随机动力学的证明需要引入[Fokker-Planck方程](https://en.wikipedia.org/wiki/Fokker%E2%80%93Planck_equation)，它可以理解为是考虑热扩散过程的连续性方程。其物理意义可以理解为流场中密度的扩散等于流场中的扩散加上热过程产生的扩散。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YUJgtJQ.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lW3v2oK.png" width="100%">
</div>

### Why Stochastic Dynamics?

实际上我们到目前为止还没有回答这样一个问题：已经有flow matching和向量场了，为什么要使用随机动力学来进行采样？这里主要是生成模型的一些下游任务可能会需要一些随机性。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/M57Izyt.png" width="100%">
</div>

同时，我们推导的随机动力学也与Langevin动力学有一定的联系。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YPyEVsB.png" width="100%">
</div>

本节课的主要内容可以总结如下。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6f5TGyK.png" width="100%">
</div>

## Reference
- [Lecture 03A - Score Functions](https://www.youtube.com/watch?v=ngC3QnYSVNM)