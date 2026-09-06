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

## Flow Matching

从整体来看，flow matching算法涉及三个核心概念，分别是**概率路径(probability path)**、**向量场(vector field)**和**损失函数(loss)**。对于这三个概念，我们还需要从**条件概率(conditional probability)**和**边缘概率(marginal probability)**两个视角分别进行分析：条件概率视角针对单个数据样本，而边缘概率视角则针对整个数据分布。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qN8zx3G.png" width="100%">
</div>

### Probability Path

**概率路径(probability path)**描述了从初始分布$$p_{\text{init}}$$到数据分布$$p_{\text{data}}$$的转移过程。在$$t=0$$时刻它对应初始分布中的噪声，而在$$t=1$$时刻则对应真实的数据样本。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Bm2FPOy.png" width="100%">
</div>

#### Conditional Probability Path

接下来我们定义**条件概率路径(conditional probability path)** $$p_t (x \vert z)$$ 为满足如下条件的概率分布：

1. 在$$t=0$$时刻等价于初始分布$$p_0 (\cdot \vert z) = p_{\text{init}}$$，且与样本数据$$z$$无关；
2. 在$$t=1$$时刻收敛到样本数据$$z$$上，即$$p_1 (\cdot \vert z) = \delta_z$$。

我们可以参考下图来理解上述条件概率路径：初始分布$$p_{\text{init}}$$随着时间推移，最终集中到样本数据$$z$$处。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QavR3U7.png" width="100%">
</div>

#### Marginal Probability Path

在条件概率路径的基础上，我们可以定义**边缘概率路径(marginal probability path)**，它描述了从初始分布到整个数据分布的变换过程。根据联合概率密度公式，边缘概率路径$$p_t (x)$$可以表示为

$$
p_t (x) = \int p_t (x \vert z) p_{\text{data}} (z) dz
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

#### Gaussian Probability Path

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

### Vector Field

## Reference

- [Lecture 2 - Flow Matching](https://www.youtube.com/watch?v=PNkMKWW8Khw)