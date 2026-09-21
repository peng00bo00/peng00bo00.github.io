---
layout: post
title: MIT 6.S184课程笔记3-Score Functions and Score Matching
date: 2026-09-12
description: 分数匹配
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
&= \frac{\int \nabla \log p_t (x \vert z) \ p_t (x \vert z) \ p_{\text{data}} (z) \mathrm{d} z}{p_t (x)} \\
&= \int \nabla \log p_t(x \vert z) \frac{p_t (x \vert z) p_{\text{data}} (z)}{p_t (x)} \mathrm{d} z
\end{aligned}
$$

不难发现，上式中两个score function的关系类似于向量场的[边缘化技巧](/blog/2026/IntroToFlowMatchingAndDiffusion-NOTES-02/#marginal-vector-field)：marginal score function是conditional score function关于后验$$\frac{p_t (x \vert z) p_{\text{data}} (z)}{p_t (x)}$$的期望。

### Score of Gaussian Probability Path

对于高斯概率路径，我们可以显式计算它的conditional score function。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/8f8Kl2w.png" width="100%">
</div>

类似地，我们可以计算marginal score function。整理一下可以得到高斯概率路径相关的所有计算公式如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dBWQHJu.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2XKkukM.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UR03z1X.png" width="100%">
</div>

不难发现，高斯概率路径的conditional score function和conditional vector field有非常相似的形式。实际上只需要进行一些简单的代数变换，我们就可以用vector field的形式来表示score function。首先对高斯概率路径的条件向量场公式进行变形可以得到

$$
\begin{aligned}
u_t^{\text{target}} (x \vert z) &= \bigg( \dot{\alpha_t} - \frac{\dot{\beta_t}}{\beta_t} \alpha_t \bigg) z + \frac{\dot{\beta_t}}{\beta_t} x \\
&= \bigg( \beta_t^2 \frac{\dot{\alpha_t}}{\alpha_t} - \dot{\beta_t} \beta_t \bigg) \bigg( \frac{\alpha_t z - x}{\beta_t^2} \bigg) + \frac{\dot{\alpha_t}}{\alpha_t} x \\
&= \bigg( \beta_t^2 \frac{\dot{\alpha_t}}{\alpha_t} - \dot{\beta_t} \beta_t \bigg) \nabla \log{p_t (x \vert z)} + \frac{\dot{\alpha_t}}{\alpha_t} x
\end{aligned}
$$

记$$a_t = \beta_t^2 \frac{\dot{\alpha_t}}{\alpha_t} - \dot{\beta_t} \beta_t$$，$$b_t = \frac{\dot{\alpha_t}}{\alpha_t}$$，则有

$$
u_t^{\text{target}} (x \vert z) = a_t \nabla \log{p_t (x \vert z)} + b_t x
$$

对于边缘向量场，只需要按照边缘化技巧进行积分即可

$$
\begin{aligned}
u_t^\text{target} (x) &= \int u_t^\text{target} (x \vert z) \frac{p_t(x \vert z) \ p_{\text{data}} (z)}{p_t (x)} \mathrm{d} z \\
&= \int [a_t \nabla \log{p_t (x \vert z)} + b_t x] \frac{p_t(x \vert z) \ p_{\text{data}} (z)}{p_t (x)} \ \ \mathrm{d} z \\
&= a_t \nabla \log{p_t (x)} + b_t x
\end{aligned}
$$

因此可以认为score function和vector field是相互等价的。

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

对于高斯概率路径，我们可以直接计算它的conditional score function。

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

对于高斯概率路径，我们可以直接推导它的随机动力学为

$$
\mathrm{d} X_t = \bigg[ \bigg( a_t + \frac{\sigma_t^2}{2} \bigg) s_t^\theta (X_t)+ b_t X_t \bigg] \mathrm{d} t + \sigma_t \mathrm{d} W_t
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/LJRGHT0.png" width="100%">
</div>

### Fokker-Planck Equation

随机动力学的证明需要引入[Fokker-Planck方程](https://en.wikipedia.org/wiki/Fokker%E2%80%93Planck_equation)，它可以理解为考虑了扩散过程的连续性方程。Fokker-Planck方程指出随机微分方程

$$
\mathrm{d} X_t = u_t (X_t) \mathrm{d} t + \sigma_t \mathrm{d} W_t, \quad X_0 \sim p_\text{init}
$$

其概率密度函数$$p_t (x)$$满足

$$
\partial_t p_t (x) = - \nabla \cdot (p_t u_t) (x) + \frac{\sigma_t^2}{2} \Delta p_t (x)
$$

Fokker-Planck方程的物理意义在于：概率密度的时间演化由两部分构成，一部分来自向量场的输运，另一部分则来自随机噪声引起的扩散。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YUJgtJQ.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lW3v2oK.png" width="100%">
</div>

接下来我们将使用Fokker-Planck方程来证明随机动力学的正确性。根据边缘向量场的[连续性方程](/blog/2026/IntroToFlowMatchingAndDiffusion-NOTES-02/#continuity-equation)有

$$
\begin{aligned}
\partial_t p_t(x) &= -\nabla \cdot (p_t u_t^\text{target}) (x) \\
&= -\nabla \cdot (p_t u_t^\text{target}) (x) - \frac{\sigma_t^2}{2} \Delta p_t (x) + \frac{\sigma_t^2}{2} \Delta p_t (x)
\end{aligned}
$$

上式中$$\Delta$$为Laplace算子，它和散度算子$$\nabla \cdot$$的关系为

$$
\Delta w_t (x) = \sum_{i=1}^d \frac{\partial^2}{\partial x_i^2} w_t(x) = \nabla \cdot \big( \nabla w_t \big) (x)
$$

其中$$w_t (x) : \mathbb{R}^d \rightarrow \mathbb{R}$$为任意标量场。利用Laplace算子和散度算子的关系，可以得到

$$
\begin{aligned}
\partial_t p_t(x) 
&= -\nabla \cdot (p_t u_t^\text{target}) (x) - \frac{\sigma_t^2}{2} \Delta p_t (x) + \frac{\sigma_t^2}{2} \Delta p_t (x) \\
&= -\nabla \cdot (p_t u_t^\text{target}) (x) - \nabla \cdot \bigg( \frac{\sigma_t^2}{2} \nabla p_t \bigg) (x) + \frac{\sigma_t^2}{2} \Delta p_t (x) \\
&=  -\nabla \cdot (p_t u_t^\text{target}) (x) - \nabla \cdot \bigg( p_t  \frac{\sigma_t^2}{2} \nabla \log p_t \bigg) (x) + \frac{\sigma_t^2}{2} \Delta p_t (x) \\
&= -\nabla \cdot \bigg( p_t \bigg[ u_t^\text{target} + \frac{\sigma_t^2}{2} \nabla \log p_t \bigg] \bigg) (x) + \frac{\sigma_t^2}{2} \Delta p_t (x)
\end{aligned}
$$

对比Fokker-Planck方程可知，上式对应的SDE其漂移项为$$u_t^\text{target} + \frac{\sigma_t^2}{2} \nabla \log p_t$$、扩散系数为$$\sigma_t$$。换言之，只要将漂移项取为$$u_t^\text{target} + \frac{\sigma_t^2}{2} \nabla \log p_t$$，随机动力学给出的概率密度演化就与原来的连续性方程完全一致，也就证明了随机动力学的正确性。

### Why Stochastic Dynamics?

实际上我们到目前为止还没有回答这样一个问题：既然已经有了flow matching和向量场，为什么还要使用随机动力学来进行采样？这主要是因为生成模型的一些下游任务需要引入随机性。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/M57Izyt.png" width="100%">
</div>

同时，我们推导的随机动力学也与**Langevin动力学(Langevin dynamics)**有一定的联系。假设概率路径$$p_t (x)$$与时间$$t$$无关，即$$p_t(x) = p(x)$$，由[连续性方程](/blog/2026/IntroToFlowMatchingAndDiffusion-NOTES-02/#continuity-equation)有

$$
\partial_t p_t(x) = -\nabla \cdot (p_t u_t^\text{target}) (x) = 0
$$

满足该条件的最简单取法是令边缘向量场为零，即

$$
u_t^\text{target} (x) = 0
$$

在此基础上结合随机动力学公式，可以得到Langevin动力学的SDE为

$$
\mathrm{d} X_t = \frac{\sigma_t^2}{2} \nabla \log p (X_t) \mathrm{d} t + \sigma_t \mathrm{d} W_t
$$

实际上$$p(x)$$给出了Langevin动力学的**平稳分布(stationary distribution)**，而利用该SDE可以将任意初始分布$$p' \neq p$$以随机扩散的方式收敛到平稳分布$$p(x)$$上。这一性质使得Langevin动力学在分子模拟、MCMC等领域中都有着重要的应用。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YPyEVsB.png" width="100%">
</div>

本节课的主要内容可以总结如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6f5TGyK.png" width="100%">
</div>

## Reference
- [Lecture 03A - Score Functions](https://www.youtube.com/watch?v=ngC3QnYSVNM)