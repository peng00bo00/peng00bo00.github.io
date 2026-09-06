---
layout: post
title: MIT 6.S184课程笔记2-Flow Matching
date: 2026-09-06
description: 流匹配
tags: DL Diffusion
categories: MIT-6.S184
giscus_comments: false
related_posts: false
toc:
  sidebar: left
pseudocode: true
---


> 这个系列是[MIT 6.S184 - Introduction to Flow Matching and Diffusion Models](https://diffusion.csail.mit.edu/2026/index.html)的同步课程笔记。本门课程面向希望深入理解流模型与扩散模型的学生和研究者，从最基础的数学工具出发，逐步推导这些模型背后的数学原理，并介绍相应的训练与采样算法。本节课主要介绍Flow Matching算法背后的数学原理。
{: .block-preface }


在上一节课中，我们基于ODE和SDE介绍了流模型与扩散模型的基本框架。从生成数据的流程上看，流模型和扩散模型都需要从一个给定的初始分布$$p_{\text{init}}$$出发，沿着由神经网络表示的向量场$$u_t^\theta (X_t)$$对轨迹进行积分，从而实现数据生成。二者的差异在于积分过程中是否需要考虑扩散系数$$\sigma_t$$，因此流模型可以看作是扩散模型的一个特例。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UbAyCJQ.png" width="100%">
</div>

本节课我们将介绍流模型中向量场$$u_t^\theta (X_t)$$的训练方法，并从数学角度推导flow matching算法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/HQWeTDi.png" width="100%">
</div>

从整体来看，flow matching算法涉及三个核心概念，分别是**概率路径(probability path)**、**向量场(vector field)**和**损失函数(loss)**。对于这三个概念，我们还需要从**条件概率(conditional probability)**和**边缘概率(marginal probability)**两个视角分别进行分析：条件概率视角针对单个数据样本，而边缘概率视角则针对整个数据分布。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qN8zx3G.png" width="100%">
</div>

## Probability Path

**概率路径(probability path)**描述了从初始分布$$p_{\text{init}}$$到数据分布$$p_{\text{data}}$$的转移过程。在$$t=0$$时刻它对应初始分布中的噪声，而在$$t=1$$时刻则对应真实的数据样本。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Bm2FPOy.png" width="100%">
</div>

### Conditional Probability Path

接下来我们定义**条件概率路径(conditional probability path)** $$p_t (x \vert z)$$ 为满足如下条件的概率分布：

1. 在$$t=0$$时刻等价于初始分布$$p_0 (\cdot \vert z) = p_{\text{init}}$$，且与样本数据$$z$$无关
2. 在$$t=1$$时刻收敛到样本数据$$z$$上，即$$p_1 (\cdot \vert z) = \delta_z$$

我们可以参考下图来理解上述条件概率路径：初始分布$$p_{\text{init}}$$随着时间推移，最终集中到样本数据$$z$$处。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QavR3U7.png" width="100%">
</div>

### Marginal Probability Path

在条件概率路径的基础上，我们可以定义**边缘概率路径(marginal probability path)**，它描述了从初始分布到整个数据分布的变换过程。根据联合概率密度公式，边缘概率路径$$p_t (x)$$可以表示为

$$
p_t (x) = \int p_t (x \vert z) \ p_{\text{data}} (z) \ \mathrm{d} z
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/d4HiHNR.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ceRCn2r.png" width="100%">
</div>

条件概率路径和边缘概率路径之间的关系可以总结如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fadc9jm.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mtWHCP1.png" width="100%">
</div>

### Gaussian Probability Path

在生成模型中，有一类重要的条件概率路径称为**高斯概率路径(Gaussian probability path)**，它描述了从标准正态分布$$\mathcal{N} (0, I_d)$$到样本数据$$p_{\text{data}}$$的转移过程。我们首先定义**高斯条件概率路径(Gaussian conditional probability path)**为：

$$
p_t (x \vert z) \sim \mathcal{N} (\alpha_t z, \beta_t^2 I_d)
$$

上式的两个参数$$\alpha_t$$和$$\beta_t$$称为**噪声调度(noise scheduler)**，需要满足以下条件

$$
\alpha_0 = \beta_1 = 0, \quad \alpha_1 = \beta_0 = 1
$$

容易验证，高斯条件概率路径满足条件概率路径的定义：

$$
p_0 (\cdot \vert z) \sim \mathcal{N} (0, I_d), \quad p_1 (\cdot \vert z) \sim \delta_z
$$

高斯概率路径的一个重要性质在于，我们可以利用正态分布的性质方便地从边缘概率路径中采样。具体来说，对于样本数据$$z \sim p_{\text{data}}$$，从边缘分布中采样可以表示为：

$$
x_t = \alpha_t z + \beta_t \epsilon_t \sim p_t
$$

其中$$\epsilon_t \sim \mathcal{N} (0, I_d)$$。上式表明，高斯概率路径可以看作是对样本数据$$\alpha_t z$$逐步添加高斯噪声$$\beta_t \epsilon_t$$的过程。

## Vector Field

接下来的问题是如何设计向量场，使得样本轨迹能够沿着我们期望的概率路径变换到数据分布上。

### Conditional Vector Field

记**条件向量场(conditional vector field)**为$$u_t^\text{target} (\cdot \vert z)$$，它对应条件概率路径$$p_t (x \vert z)$$，即满足如下ODE

$$
\frac{\mathrm{d}}{\mathrm{d} t} X_t = u_t^\text{target} (X_t \vert z), \quad X_0 \sim p_{\text{init}}
$$

对于高斯概率路径，可以证明其条件向量场具有如下解析形式：

$$
u_t^\text{target} (x \vert z) = \bigg( \dot{\alpha_t} - \frac{\dot{\beta_t}}{\beta_t} \alpha_t \bigg) z + \frac{\dot{\beta_t}}{\beta_t} x
$$

下面给出上述公式的证明。首先构造**条件流模型(conditional flow model)**

$$
\psi_t^{\text{target}} (x \vert z) = \alpha_t z + \beta_t x
$$

设$$X_t$$为上述条件流模型对应的轨迹，由高斯概率路径的定义有

$$
X_t = \psi_t^{\text{target}} (X_0 \vert z) = \alpha_t z + \beta_t X_0 \sim \mathcal{N} (\alpha_t z, \beta_t^2 I_d) = p_t (\cdot \vert z)
$$

上式说明，我们所构造的条件流模型$$\psi_t^{\text{target}} (x \vert z)$$恰好对应高斯条件概率路径$$p_t (\cdot \vert z)$$。

接下来开始求条件向量场的具体形式。根据流模型的ODE定义$$\partial_t \psi_t = u_t (\psi_t)$$，可以得到

$$
\begin{aligned}
\frac{\mathrm{d}}{\mathrm{d} t} \psi_t^{\text{target}} (x \vert z) &= u_t^{\text{target}} (\psi_t^{\text{target}} (x \vert z) \vert z) \\
\dot{\alpha_t} z + \dot{\beta_t} x &= u_t^{\text{target}} (\alpha_t z + \beta_t x \vert z)
\end{aligned}
$$

记$$x' = \alpha_t z + \beta_t x$$，即$$x = (x' - \alpha_t z) / \beta_t$$，将其代入上式则有

$$
\begin{aligned}
\dot{\alpha_t} z + \dot{\beta_t} \bigg( \frac{x' - \alpha_t z}{\beta_t} \bigg) &= u_t^{\text{target}} (x' \vert z) \\
\bigg( \dot{\alpha_t} - \frac{\dot{\beta_t}}{\beta_t} \alpha_t \bigg) z + \frac{\dot{\beta_t}}{\beta_t} x' &= u_t^{\text{target}} (x' \vert z)
\end{aligned}
$$

最后将$$x'$$重新记作$$x$$，就得到了高斯概率路径的条件向量场公式。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/A1wPuI8.png" width="100%">
</div>

实际上，只需按照上式中的向量场进行积分，就可以将初始正态分布的噪声变换到给定的样本数据$$z$$上。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/E5klWn5.png" width="100%">
</div>

### Marginal Vector Field

类似于边缘概率路径，**边缘向量场(marginal vector field)** $$u_t^\text{target} (x)$$描述了从初始分布$$p_{\text{init}}$$到整个数据分布$$p_{\text{data}}$$的变换过程。基于条件向量场计算边缘向量场的过程称为**边缘化技巧(marginalization trick)**，其公式可以表达为：

$$
u_t^\text{target} (x) = \int \underbrace{u_t^\text{target} (x \vert z) \vphantom{\frac{p_t(x \vert z) \ p_{\text{data}} (z)}{p_t (x)}}}_{\text{conditional vector field}} \ \underbrace{\frac{p_t(x \vert z) \ p_{\text{data}} (z)}{p_t (x)}}_{\text{posterior}}  \mathrm{d} z
$$

其中第一项$$u_t^\text{target} (x \vert z)$$是条件向量场，而第二项$$\frac{p_t(x \vert z) \ p_{\text{data}} (z)}{p_t (x)} = p_t(z \vert x)$$则是后验分布，它表示在$$t$$时刻给定样本$$x$$时，该样本来自真实数据$$z$$的条件概率。

边缘化技巧的几何意义在于，$$t$$时刻的边缘向量场实际上是条件向量场的加权平均(条件期望)，其权重为当前样本$$x$$来自不同数据$$z$$的后验分布。

利用边缘向量场，我们就可以将初始分布$$p_{\text{init}}$$变换到数据分布$$p_{\text{data}}$$上，对应的ODE为

$$
\frac{\mathrm{d}}{\mathrm{d} t} X_t = u_t^\text{target} (X_t), \quad X_0 \sim p_{\text{init}}
$$

条件向量场和边缘向量场的关系可以参考下图：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/kXz3q0T.png" width="100%">
</div>

#### Continuity Equation

边缘化技巧的数学证明依赖于概率密度的**连续性方程(continuity equation)**，它描述了概率密度函数在连续时间上的守恒关系。

$$
\partial_t p_t(x) = -\nabla \cdot (p_t u_t^\text{target}) (x)
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/AA9uvB3.png" width="100%">
</div>

不难发现，概率密度的连续性方程实际上也是流体力学中的质量守恒方程，其物理意义在于概率质量在流动过程中既不会凭空产生也不会凭空消失：某一点上概率密度的变化率，恰好等于流入与流出该点的概率流之差。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/s10Dn2O.png" width="100%">
</div>

总结一下，概率路径和向量场的关系如下图所示：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ft6sjGo.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IX0cNxU.png" width="100%">
</div>

## Learning the Marginal Vector Field

最后我们来介绍如何学习边缘向量场，这实际上也是整个flow matching算法的核心所在。从直觉上看，向量场的学习过程等价于一个回归问题：对于神经网络表示的向量场$$u_t^\theta$$，我们可以通过最小化它与目标向量场之间的平方误差来进行学习。因此可以构造出如下损失函数：

$$
\begin{aligned}
\mathcal{L}_{\text{FM}} (\theta) &= \mathbb{E}_{t \sim \text{Unif}, x \sim p_t} \big[\| u_t^\theta (x) - u_t^\text{target} (x) \|^2 \big] \\
&= \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, x \sim p_t (\cdot \vert z)} \big[\| u_t^\theta (x) - u_t^\text{target} (x) \|^2 \big]
\end{aligned}
$$

其中$$\text{Unif}$$表示$$[0, 1]$$区间上的均匀分布。

需要注意的是，上述损失函数的拟合目标是边缘向量场$$u_t^\text{target} (x)$$，而计算边缘向量场的过程比较困难，它需要对所有真实数据进行积分。然而条件向量场$$u_t^\text{target} (x \vert z)$$是容易计算的，用它替换掉边缘向量场可以得到一个更容易优化的损失函数：

$$
\mathcal{L}_{\text{CFM}} (\theta) = \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, x \sim p_t (\cdot \vert z)} \big[\| u_t^\theta (x) - u_t^\text{target} (x \vert z) \|^2 \big]
$$

更进一步，flow matching算法的一大贡献在于，它从理论上证明了上述两个损失函数之间只相差一个常数

$$
\mathcal{L}_{\text{FM}} = \mathcal{L}_{\text{CFM}} + C
$$

因此，无论使用哪个损失函数，它们的梯度都是相同的，也就是说我们可以通过最小化条件向量场的平方误差来学习边缘向量场。这样我们就得到了flow matching的整体算法框架：

```pseudocode
\begin{algorithm}
\caption{Flow Matching Training Procedure (General)}
\begin{algorithmic}
\REQUIRE A dataset of samples $z \sim p_{\text{data}}$, neural network $u_t^\theta$
\FOR{each mini-batch of data}
    \STATE Sample a data example $z$ from the dataset
    \STATE Sample a random time $t \sim \text{Unif}_{[0,1]}$
    \STATE Sample $x \sim p_t(\cdot \vert z)$
    \STATE Compute loss $\mathcal{L}(\theta) = \| u_t^\theta (x) - u_t^\text{target} (x \vert z) \|^2$
    \STATE Update the model parameters $\theta$ via gradient descent on $\mathcal{L}(\theta)$
\ENDFOR
\end{algorithmic}
\end{algorithm}
```

### Flow Matching for Gaussian Conditional Probability Paths

对于高斯概率路径，我们可以进一步简化上述算法框架。首先，高斯概率路径的条件向量场表达式为

$$
u_t^\text{target} (x \vert z) = \bigg( \dot{\alpha_t} - \frac{\dot{\beta_t}}{\beta_t} \alpha_t \bigg) z + \frac{\dot{\beta_t}}{\beta_t} x
$$

而在$$t$$时刻采样的过程可以表示为

$$
x = \alpha_t z + \beta_t \epsilon, \quad \epsilon \sim \mathcal{N} (0, I_d)
$$

因此，$$t$$时刻的条件向量场可以表示为

$$
u_t^\text{target} (x \vert z) = \dot{\alpha_t} z + \dot{\beta_t} \epsilon
$$

将上述两式代入条件向量场的损失函数中，可以得到

$$
\begin{aligned}
\mathcal{L}_{\text{CFM}} (\theta) &= \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, \epsilon \sim \mathcal{N} (0, I_d)} \big[\| u_t^\theta (x) - u_t^\text{target} (x \vert z) \|^2 \big] \\
&= \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, \epsilon \sim \mathcal{N} (0, I_d)} \big[\| u_t^\theta (\alpha_t z + \beta_t \epsilon) - (\dot{\alpha_t} z + \dot{\beta_t} \epsilon) \|^2 \big]
\end{aligned}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uIYYjMg.png" width="100%">
</div>

若将两个噪声调度设置为关于时间$$t$$的线性插值，则有

$$
\alpha_t = t, \quad \beta_t = 1 - t
$$

将其代入损失函数，可以得到

$$
\mathcal{L}_{\text{CFM}} (\theta) = \mathbb{E}_{t \sim \text{Unif}, z \sim p_{\text{data}}, \epsilon \sim \mathcal{N} (0, I_d)} \big[\| u_t^\theta (t z + (1 - t) \epsilon) - (z - \epsilon) \|^2 \big]
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/V6ToPfY.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/15YxBye.png" width="100%">
</div>

整理后可以得到高斯概率路径下的flow matching：

```pseudocode
\begin{algorithm}
\caption{Flow Matching Training for CondOT path}
\begin{algorithmic}
\REQUIRE A dataset of samples $z \sim p_{\text{data}}$, neural network $u_t^\theta$
\FOR{each mini-batch of data}
    \STATE Sample a data example $z$ from the dataset
    \STATE Sample a random time $t \sim \text{Unif}_{[0,1]}$
    \STATE Sample noise $\epsilon \sim \mathcal{N}(0, I_d)$
    \STATE Set $x = tz + (1-t)\epsilon$
    \STATE Compute loss $\mathcal{L}(\theta) = \| u_t^\theta (x) - (z - \epsilon) \|^2$
    \STATE Update the model parameters $\theta$ via gradient descent on $\mathcal{L}(\theta)$
\ENDFOR
\end{algorithmic}
\end{algorithm}
```

实际上，目前最先进的生成模型大多都基于上述高斯路径的flow matching算法进行训练。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RfwZ9ZH.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/D2QeZq2.png" width="100%">
</div>

本节课的主要内容可以总结如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bKEjxW4.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/EWG8pWG.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mxkO7hM.png" width="100%">
</div>

## Reference
- [Lecture 2 - Flow Matching](https://www.youtube.com/watch?v=PNkMKWW8Khw)