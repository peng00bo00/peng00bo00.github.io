---
layout: post
title: OMSCS-RL课程笔记09-Partially Observable MDPs
date: 2022-06-27
description: 部分可观察MDP
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节主要介绍部分可观察MDP。
{: .block-preface }


## POMDPs

**部分可观察MDP(partially observable MDP, POMDP)**是对标准MDP的推广，基于POMDP的框架我们可以去讨论一些非马尔科夫的环境下智能体的决策问题。在POMDP中环境仍然是一个标准的MDP，但智能体无法直接获得系统的状态而是通过一个观测函数$$O$$来获得对状态$$s$$的观测$$z$$。在很多情况下系统状态$$s$$和观测$$z$$之间的关系是不确定的，它们的概率可记为$$P(z \vert s) = O(s, z)$$。

<div align=center>
<img src="https://i.imgur.com/Vv6zMTx.png" width="80%">
</div>

## State Estimation

显然在POMDP中智能体的核心问题在于如何估计自身的状态。记$$b(s)$$为智能体位于状态$$s$$的概率称为**信念(belief)**，通过行为$$a$$和观测$$z$$我们可以对信念进行更新：

$$
\begin{aligned}
b(s') &= P(s' \vert b, a, z) = \sum_s P(s \vert b, a, z) \cdot P(s' \vert b, a, z, s) \\
&= \sum_s b(s) P(s' \vert a, z, s) \\
&= \sum_s b(s) \cdot \frac{P(z \vert s', a, s) P(s' \vert a, s)}{P(z \vert a, s)} \\
&= \frac{\sum_s b(s) O(s', z) T(s, a, s')}{\sum_{s'} \sum_s b(s) O(s', z) T(s, a, s')}
\end{aligned}
$$

这里需要注意的是即使状态空间$$S$$是有限的，信念空间$$B$$仍然是无限的。实际上信念空间$$B$$包含任意可行的状态分布概率。

<div align=center>
<img src="https://i.imgur.com/Vua9EzY.png?1" width="80%">
<img src="https://i.imgur.com/xGPIeNt.png" width="80%">
</div>

## Value Iteration in POMDP

我们可以对[价值迭代](/blog/2022/OMSCS-RL-NOTES-01/#finding-policies)进行推广，使用估计$$b$$来代替状态$$s$$得到POMDP中的价值迭代算法。实际上这样进行更新得到的价值函数还具有**分片线性和凸性(piecewise linear & convex)**。

<div align=center>
<img src="https://i.imgur.com/OBiwNJ1.png" width="80%">
<img src="https://i.imgur.com/Ycn9GWC.png" width="80%">
<img src="https://i.imgur.com/7azLaNU.png" width="80%">
<img src="https://i.imgur.com/8our79y.png" width="80%">
<img src="https://i.imgur.com/xLGk7BP.png" width="80%">
</div>

## RL for POMDP

在价值迭代的基础上我们接下来讨论POMDP中的强化学习问题。

<div align=center>
<img src="https://i.imgur.com/n04WXnK.png" width="80%">
</div>

### Learning a POMDP

最直接的处理方法是首先学习一下环境，然后在学习得到的环境上进行训练。某种意义上讲，这样的学习方法也类似于EM算法。

<div align=center>
<img src="https://i.imgur.com/jDwTd5c.png" width="80%">
</div>

### Bayesian RL

实际上我们还可以基于POMDP来考虑RL的问题。当智能体对环境一无所知时我们可以假定一系列可能的环境，此时的最优策略可以通过求解POMDP来进行计算：

<div align=center>
<img src="https://i.imgur.com/n4UhqCT.png" width="80%">
</div>

在这种观察下我们可以认为RL实际上是POMDP中的**规划(planning)**问题。

<div align=center>
<img src="https://i.imgur.com/CvDfgPJ.png" width="80%">
</div>

### Predictive State Representation

<div align=center>
<img src="https://i.imgur.com/Cag0uN8.png" width="80%">
<img src="https://i.imgur.com/bYoA0xx.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/npqTULA.png" width="80%">
</div>