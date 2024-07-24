---
layout: post
title: OMSCS-CV课程笔记12-Generative Models
date: 2021-11-04
description: 图像识别和生成式模型
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍图像识别和生成式模型的相关内容。
{: .block-preface }


## Image Recognition

**图像识别(image recognition)**是计算机视觉中较为复杂的任务，我们希望计算机可以告诉我们图像中包含了哪些物体，它们的位置都在哪里，图像的场景是什么等问题。

<div align=center>
<img src="https://i.imgur.com/kG5Hgah.png" width="40%">
<img src="https://i.imgur.com/kG5Hgah.png" width="40%">
</div>

图像识别的难点很多，比如说现实中的图像往往容易收到光照因素的影响，同样的物体可能会有不同的姿态，图像中可能存在很多杂乱的物体对视线进行干扰等。

<div align=center>
<img src="https://i.imgur.com/5mJinzS.png" width="90%">
</div>

同时，现实中的图像或多或少会有一些遮挡的问题；同一类别的物体往往只存在细微的差别，这又提高了图像识别的难度；此外当相机的视角不同时同一个物体会有不同的呈现形式。这些都使得设计鲁棒的图像识别算法变得非常困难。

<div align=center>
<img src="https://i.imgur.com/5wGYzsT.png" width="90%">
</div>

不过近几年随着机器学习尤其是深度学习方法的流行，图像识别这一问题在很大程度上已经得到了解决。基于大数据和深度神经网络的图像识别方法在识别精度和正确率上已经接近或超越了人类水平，并在各个领域中都得到了大量的应用。

<div align=center>
<img src="https://i.imgur.com/xynzIgY.png" width="50%">
<img src="https://i.imgur.com/D3UBZqb.png" width="30%">
</div>

## Classification: Generative Models

在本课程中我们不会过多地讨论这些机器学习或深度学习方法，而是着重介绍使用机器学习解决图像识别的基本思想。以数字图像分类问题为例，给定一系列带有标签的训练图像，我们希望能从数据中学习到一个模型来识别图像中的数字：

<div align=center>
<img src="https://i.imgur.com/VuMhtYV.png" width="60%">
</div>

那么如何衡量这个模型的好坏呢？我们需要知道这个模型会犯哪些错误，以及当模型犯错时相应的损失是多少。显然我们希望模型在训练数据上的(期望)损失越小越好，这就是**监督学习(supervised learning)**的基本思想。一般来说监督学习方法有两种策略：

- 使用训练数据对不同类别的数据生成过程进行建模，分别计算不同类别的先验和条件概率，这样的方法称为**生成式模型(generative model)**；
- 直接学习不同类别数据的决策边界并对后验概率进行建模，这类方法称为**判别式模型(discriminative model)**。

这节课我们主要会关注生成式模型。回到数字图像分类的问题，我们可以定义模型$$s$$的**风险(risk)**为：

$$
R(s) = P(4 \rightarrow 9 \vert s) L(4 \rightarrow 9) + P(9 \rightarrow 4 \vert s) L(9 \rightarrow 4)
$$

其中$$P(4 \rightarrow 9 \vert s)$$和$$P(9 \rightarrow 4 \vert s)$$分别表示模型$$s$$发生错误的概率，$$L(4 \rightarrow 9)$$和$$L(9 \rightarrow 4)$$则是发生这种错误时产生的损失。使得总体风险最小的模型$$s$$称为最优分类器。

在模型的决策边界上样本$$x$$产生两种错误的损失相等，即

$$
P(9 \vert x) L(9 \rightarrow 4) = P(4 \vert x) L(4 \rightarrow 9)
$$

换句话说，我们需要去计算模型给出类别标签$$c$$的概率$$P(c \vert x)$$。利用Bayes公式可以表示为：

$$
P(c \vert x) = \frac{P(x \vert c) P(c)}{P(x)} \propto P(x \vert c) P(c)
$$

其中$$P(c)$$表示类别标签$$c$$的概率，称为**先验(prior)**；而$$P(x \vert c)$$表示从类别$$c$$中生成样本$$x$$的概率，称为**似然(likelihood)**。此时样本$$x$$对应的标签为：

$$
c^* = \arg \max_c P(c \vert x) = \arg \max_c P(x \vert c) P(c)
$$

不难发现生成式模型的核心在于计算不同类别的先验以及似然，一般情况下这可以通过对训练数据进行建模来实现。

## Principle Component Analysis

机器学习在计算机视觉中还有很多的应用，不过在讨论这些具体应用前我们先介绍**主成分分析(principle component analysis, PCA)**在计算机视觉中的应用。

我们定义数据集的**主成分(principal components)**为数据方差最大的方向。以下图为例，显然第一个主成分对应对角线方向。其余主成分方向与之前定义的主成分相互垂直，表示除去当前主方向后方差最大的方向。

<div align=center>
<img src="https://i.imgur.com/xCIVT2n.png" width="40%">
<img src="https://i.imgur.com/bDLYbvS.png" width="40%">
</div>

### Fitting a Line

假设已经知道主成分(法向)的方向$$(a, b)$$，我们可以通过最小化样本到直线的距离来计算主成分所在直线的截距：

$$
E(a, b, d) = \sum_i (a x_i + b y_i - d)^2
$$

$$
\frac{\partial E}{\partial d} = 0 \Rightarrow -2 \sum_i (a x_i + b y_i - d) = 0
$$

$$
d = a \bar{x} + b \bar{y}
$$

<div align=center>
<img src="https://i.imgur.com/aEMwkmk.png" width="30%">
</div>

把$$d$$代回$$E(a, b, d)$$中，得到：

$$
\begin{aligned}
E(a, b, d) &= \sum_i (a x_i + b y_i - d)^2 \\
&= \sum_i [a (x_i - \bar{x}) + b (y_i - \bar{y})]^2 \\
&= \Vert B n \Vert^2
\end{aligned}
$$

$$
B = 
\begin{bmatrix}
x_1 - \bar{x} & y_1 - \bar{y} \\
\vdots & \vdots \\
x_n - \bar{x} & y_n - \bar{y} \\
\end{bmatrix}
,
n = 
\begin{bmatrix}
a \\ b
\end{bmatrix}
$$

换句话说，求解主方向相当于求解约束优化问题：

$$
\begin{aligned}
\min_n & &  & n^T B^T B n \\
\text{s.t.} & & & n^T n = 1
\end{aligned}
$$

### Algebraic Interpretation

从代数的角度上讲，最小化点到直线的距离等价于最大化投影后点的距离。在此基础上我们可以得到PCA更常见的形式：

$$
E = \sum_i (x^T P_i)^2 = x^T B^T B x
$$

<div align=center>
<img src="https://i.imgur.com/0zjSOUQ.png" width="40%">
</div>

令$$M = B^T B$$，得到约束优化问题：

$$
\begin{aligned}
\max_n & &  & x^T B^T B x \\
\text{s.t.} & & & x^T x = 1
\end{aligned}
$$

通过Lagrange乘子法得到：

$$
\max L = x^T M x + \lambda (1 - x^T x)
$$

$$
\frac{\partial L}{\partial x} = 2M x - 2 \lambda x = 0
$$

$$
M x = \lambda x
$$

也就是说$$x$$实际上是$$M$$矩阵的最大特征向量。

实际上$$M$$矩阵对应了数据的协方差矩阵：

$$
M = B^T B =
\begin{bmatrix}
\sum_i x_i^2 & \sum_i x_i y_i \\
\sum_i x_i y_i & \sum_i y_i^2
\end{bmatrix}
$$

从这个角度上看，主成分也是协方差矩阵所定义的相互垂直的方向。

### Dimensionality Reduction

PCA的一个重要应用是对数据进行降维。对于高维的数据，我们可以仅采用少量几个的主方向来描述原始数据。以下图为例，我们可以用橙色点在$$v_1$$方向上的投影来对它们进行区分：

<div align=center>
<img src="https://i.imgur.com/TkwvCwZ.png" width="40%">
</div>

PCA在人脸识别上有很经典的应用。我们可以把人脸图像视为一个高维的向量，通过PCA我们可以得到人脸的平均值和特征向量：

<div align=center>
<img src="https://i.imgur.com/tFRWR5j.png" width="70%">
</div>

这样任意一张人脸图像都可以用它在特征向量上的投影来表示：

<div align=center>
<img src="https://i.imgur.com/6r5ECjd.png" width="70%">
<img src="https://i.imgur.com/UXKEBVz.png" width="90%">
</div>

换句话说我们可以把人脸视为特征空间中的点，这样就可以利用生成式模型来解决人脸识别的问题：

<div align=center>
<img src="https://i.imgur.com/fbTf6YM.png" width="70%">
</div>

当然需要说明的是PCA也有自身的局限性，在大多数情况下主方向都不一定适合分类问题。

<div align=center>
<img src="https://i.imgur.com/Zpl3Uv4.png" width="40%">
</div>
