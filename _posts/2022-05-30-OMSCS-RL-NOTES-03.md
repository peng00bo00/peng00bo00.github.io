---
layout: post
title: OMSCS-RL课程笔记03-TD and Friends
date: 2022-05-30
description: TD learning算法
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节介绍TD learning的相关算法。
{: .block-preface }


在上一节中我们介绍过根据智能体是否已知环境可以大致把强化学习分为model-based RL以及model-free RL两类。model-based方法会先利用已有的数据来学习环境的dynamics，然后利用学习到的MDP来更新智能体的策略；而model-free方法则会直接更新价值函数，从而不断优化当前的策略；除了这两种方法外还可以直接对策略进行更新，这类方法称为**policy search**。这几类算法的关系可以参考下图：

<div align=center>
<img src="https://i.imgur.com/7zgKIEa.png" width="80%">
</div>

## TD Lambda

本节课介绍的**TD Lambda**算法是一种典型的model-free方法，它的目标是利用智能体和环境的互动来预测不同状态的价值函数，也即累计奖励。回忆在MDP中的价值函数$$V(s)$$需要满足递归关系：

$$
V(s) = \mathbb{E} [r + \gamma V(s')]
$$

当我们不知道环境的dynamics时可以使用采样的方法来估计这个期望：

$$
V(s) = \mathbb{E} [r + \gamma V(s')] \approx \frac{1}{n} \sum_{i=1}^n \hat{V} (s)
$$

其中$$\hat{V} (s)$$为经过采样后估计出的价值函数。除了进行采样外我们还可以使用增量更新的方式来估计价值函数，即每当我们采样出一条轨迹就更新一下当前的价值函数：

$$
V_t (s) \leftarrow V_{t-1} (s) + \alpha_t [G_t - V (s)]
$$

其中$$G_t$$为当前轨迹上计算出的累计回报，而$$\alpha_t$$为学习率。可以证明当$$\alpha_t$$满足一定条件时按上式更新的价值函数会收敛到真正的价值函数上，所需条件为：

$$
\sum_t \alpha_t = \infty
$$

$$
\sum_t \alpha_t^2 \lt \infty
$$

这种使用智能体当前轨迹来增量式更新价值函数的方法称为TD Lambda算法。

## TD(1)

TD(1)是TD Lambda算法算法中最简单的情况，它使用当前轨迹上一时刻的价值函数来进行更新：

<div align=center>
<img src="https://i.imgur.com/6PK4oo7.png" width="80%">
<img src="https://i.imgur.com/6r6pcGu.png" width="80%">
</div>

需要注意的是TD(1)收敛的条件是要有足够多的样本轨迹。当样本数比较少或是出现了小概率的轨迹时，使用TD(1)估计得到的估计可能会偏离真正的价值函数。

## TD(0)

TD(0)同样是一种增量式的价值函数更新算法，它的特点是不会对访问过的状态进行记数而是直接使用上一时刻的价值函数进行更新。可以证明这样的更新方式等价于进行极大似然估计。

<div align=center>
<img src="https://i.imgur.com/rv8w1Rz.png" width="80%">
</div>

## TD($$\lambda$$)

TD(1)和TD(0)都可以看做是TD($$\lambda$$)算法的推广，在更新记数时使用一个超参数$$\lambda$$来进行控制：

<div align=center>
<img src="https://i.imgur.com/ERGYHi7.png" width="80%">
</div>

## K-Step Estimators

要理解TD($$\lambda$$)算法首先需要了解k-step估计，其定义为使用未来k步的回报来对价值函数进行估计：

<div align=center>
<img src="https://i.imgur.com/ysJOEUs.png" width="80%">
</div>

TD($$\lambda$$)可以看做是所有k-step估计的加权和：

<div align=center>
<img src="https://i.imgur.com/JaEjJCX.png" width="80%">
</div>

一些试验表明，通过调节$$\lambda$$的值可以在有限的数据中获得真实价值函数的一个良好估计：

<div align=center>
<img src="https://i.imgur.com/FmXyUQ6.png" width="80%">
</div>