---
layout: post
title: OMSCS-RL课程笔记04-Convergence
date: 2022-05-31
description: TD learning算法的收敛性
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节讨论TD learning的收敛性。
{: .block-preface }


## Bellman Equations with Actions

对于不考虑智能体决策行为的情况，Bellman方程给出了价值函数的递归关系：

$$
V(s) = R(s) + \gamma \sum_{s'} T(s, s') V(s')
$$

而TD(0)则给出了价值函数的在线估计方法：

$$
V_t(s_{t-1}) = V_{t-1}(s_{t-1}) + \alpha_t \bigg[ r_t + \gamma V_{t-1} (s_t) - V_{t-1} (s_{t-1}) \bigg]
$$

我们把TD(0)的思路拓展到需要进行决策的情况，可以得到**Q-learning算法**：

$$
Q(s, a) = R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} Q(s', a')
$$

$$
Q_t(s_{t-1}, a_{t-1}) = Q_{t-1}(s_{t-1}, a_{t-1}) + \alpha_t \bigg[ r_t + \gamma \cdot \max_{a'} Q_{t-1}(s_t, a') - Q_{t-1}(s_{t-1}, a_{t-1}) \bigg]
$$

<div align=center>
<img src="https://i.imgur.com/lxnLPjy.png" width="80%">
</div>

## Bellman Operator

实际上我们可以使用算子的角度来认识Bellman方程。我们定义Bellman算子$$B$$是一个价值函数到价值函数的映射：

$$
[B \ Q] (s, a) = R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} Q(s', a')
$$

利用Bellman算子，我们可以重写Bellman方程和TD(0)更新：

$$
Q^* = B \ Q^*
$$

$$
Q_t = B \ Q_{t-1}
$$

## Contraction Mapping

Bellman算子的一个重要性质是它是一个**压缩映射(contraction mapping)**。所谓压缩映射是指对于任意函数$$F$$和$$G$$，存在$$0 \leq \gamma \lt 1$$使得对于算子$$B$$满足：

$$
\Vert B \ F - B \ G \Vert_\infty \leq \gamma \Vert F - G \Vert_\infty
$$

直观理解，压缩映射会不断减少两个函数之间的距离。压缩映射有很多有用的性质，比如说对于压缩映射$$B$$一定存在一个唯一的函数$$F^*$$满足方程：

$$
B \ F^* = F^*
$$

同时，我们可以从任意函数$$F$$开始利用$$B$$进行迭代，最终一定能收敛到$$F^*$$上：

$$
F_t = B \ F_{t-1} \Rightarrow F_t \rightarrow F^*
$$

<div align=center>
<img src="https://i.imgur.com/Y85m69p.png" width="80%">
</div>

容易验证Bellman算子满足压缩映射的定义：

$$
\begin{aligned}
\Vert B \ Q_1 - B \ Q_2 \Vert_\infty &= \max_{s, a} \bigg\vert \gamma \sum_{s'} T(s, a, s') \bigg[ \max_{a'} Q_1(s', a') - \max_{a'} Q_2(s', a') \bigg] \bigg\vert \\
&\leq \gamma \max_{s'} \vert \max_{a'} Q_1(s', a') - \max_{a'} Q_2(s', a') \vert \\
&\leq \gamma \Vert  Q_1 - Q_2 \Vert_\infty
\end{aligned}
$$

<div align=center>
<img src="https://i.imgur.com/OTjD0FM.png" width="80%">
<img src="https://i.imgur.com/LYOqJKe.png" width="80%">
</div>

## Convergence

Bellman算子作为一个压缩映射的直接推论是证明价值迭代算法的正确性。实际上存在如下定理保证了使用Bellman算子进行迭代的价值函数一定可以收敛到最优价值函数：

<div align=center>
<img src="https://i.imgur.com/CVM6Nd9.png" width="80%">
</div>

更进一步，我们可以证明TD learning算法能够收敛到最优价值函数：

<div align=center>
<img src="https://i.imgur.com/WDgiEOC.png" width="80%">
<img src="https://i.imgur.com/BQUIYSr.png" width="80%">
</div>

## Generalized MDP

除此之外，我们还可以对MDP以及Bellman方程的概念进行推广：

<div align=center>
<img src="https://i.imgur.com/kWEsF8a.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/ielFoAZ.png" width="80%">
</div>