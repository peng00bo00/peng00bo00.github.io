---
layout: post
title: OMSCS-RL课程笔记05-AAA
date: 2022-06-06
description: 其它类型的强化学习算法
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节主要讨论不同强化学习算法的性质。
{: .block-preface }


## Value Iteration

**价值迭代(value iteration, VI)**是我们介绍的第一个强化学习算法。可以证明使用策略迭代算法总是可以在多项式时间内寻找到最优策略：

<div align=center>
<img src="https://i.imgur.com/7fTdf8k.png" width="80%">
</div>

## Linear Programming

另一种求解MDP的方法是使用**线性规划(linear programming)**。根据Bellman方程，价值函数的递归形式为：

$$
V(s) = \max_a \bigg( R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ V (s') \bigg)
$$

显然Bellman方程不是一个线性方程，不过我们可以利用$$\max$$运算的性质把它转换为一个线性方程。具体来说可以把上式等价地表示为一个约束优化问题：

$$
\begin{aligned}
\min &\sum_s V(s)\\
\text{s.t.} \ V(s) &\geq R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ V (s')
\end{aligned}
$$

这样求解Bellman方程就转换为了一个线性规划问题。此外我们还可以利用对偶性转而求解原始问题的对偶问题：

$$
\begin{aligned}
\max &\sum_s \sum_a q_{s,a} R(s, a)\\
\text{s.t.} \ & 1 + \gamma \sum_s \sum_a q_{s,a} T(s, a, s') = \sum_a q_{s',a}, \forall s' \\
& q_{s,a} \geq 0
\end{aligned}
$$

## Policy Iteration

**策略迭代(policy iteration, PI)**也是一种常用的求解MDP的算法。与VI不同的是，在每次迭代中PI会先更新当前的策略，然后利用新的策略再更新价值函数：

<div align=center>
<img src="https://i.imgur.com/fS82dvd.png" width="80%">
</div>

## Domination

为了比较学习到的策略，我们需要引入一些相关的数学概念：

<div align=center>
<img src="https://i.imgur.com/2zF4T7r.png" width="80%">
</div>

在此基础上则可以证明PI的收敛性：

<div align=center>
<img src="https://i.imgur.com/qquIaHn.png" width="80%">
<img src="https://i.imgur.com/2o3l9nB.png" width="80%">
<img src="https://i.imgur.com/ksTfYAC.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/apBnU7K.png" width="80%">
</div>