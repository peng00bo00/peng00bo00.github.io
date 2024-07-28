---
layout: post
title: OMSCS-RL课程笔记08-Generalization
date: 2022-06-20
description: 强化学习中的泛化问题
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节主要介绍强化学习中的泛化问题。
{: .block-preface }


## Generalization Idea

强化学习中的**泛化(generalization)**一般是指算法在状态空间中的泛化问题。当环境的状态空间过于巨大或者是连续状态空间时，智能体往往无法遍历所有可能的状态。此时我们希望强化学习算法仍然可以在没有经历过的状态上获得合理的策略。

<div align=center>
<img src="https://i.imgur.com/agP6iiR.png" width="80%">
</div>

## Basic Update Rule

以[Q-learning算法](/blog/2022/OMSCS-RL-NOTES-04/#bellman-equations-with-actions)为例，Q-learning的迭代过程实际上可以理解为对TD error使用梯度下降法进行更新：

<div align=center>
<img src="https://i.imgur.com/RVch9Np.png" width="80%">
</div>

## Linear Value Function Approximation

在上面这种观察的基础上，我们可以使用一系列基函数的线性组合来表示Q函数：

$$
Q(s, a) = \sum_i f_i(s) \cdot w_i^a
$$

这样就可以通过调节系数$$w_i^a$$来操作Q函数的行为。

<div align=center>
<img src="https://i.imgur.com/5Vktrbe.png" width="80%">
</div>

更进一步，我们还可以得到使用基函数线性组合来表示Q函数时梯度更新公式：

$$
\frac{\partial Q(s, a)}{\partial w_i^a} = f_i(s)
$$

$$
\Delta w_i^a = \alpha \bigg(r + \gamma \max_{a'} Q(s', a') - Q(s, a) \bigg) \cdot f_i(s)
$$

## Baird's Counter Example

但需要注意的是这种通过对集函数进行线性组合方法的效果在很大程度上依赖于基函数的选取。实际上对于表格型的Q函数我们同样可以把它看做是基函数的线性组合，而这种情况下的Q函数几乎没有所谓的泛化能力：

<div align=center>
<img src="https://i.imgur.com/yj92F9h.png" width="80%">
</div>

更严重的问题是Q函数时梯度更新公式也依赖于基函数的设置，当基函数设置的不好时容易导致梯度的计算出现各种各样的问题。比如说如果当前Q函数计算出的TD error为0，那么所有相关的系数$$w_i^a$$都无法得到更新。

<div align=center>
<img src="https://i.imgur.com/GkV6ELY.png" width="80%">
</div>

## Averagers

实践中更为常用的方法是使用各种插值的方式来进行近似。对于任意函数$$f(x)$$，我们可以对$$x$$附近已知点的函数值进行内插从而近似$$f(x)$$的值：

<div align=center>
<img src="https://i.imgur.com/DwiSfCs.png" width="80%">
</div>

回到价值函数的近似中，我们可以把某些状态作为anchor然后利用anchor来估计其它状态的价值：

<div align=center>
<img src="https://i.imgur.com/rs6V5A5.png" width="80%">
</div>

实际上使用这种形式的价值函数也可以理解为在anchor状态中求解Bellman方程。

<div align=center>
<img src="https://i.imgur.com/2H2P20O.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/CMJ7RTZ.png" width="80%">
</div>