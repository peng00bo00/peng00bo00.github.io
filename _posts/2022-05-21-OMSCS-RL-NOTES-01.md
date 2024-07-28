---
layout: post
title: OMSCS-RL课程笔记01-Introduction
date: 2022-05-21
description: 强化学习基础I
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节主要介绍强化学习的概念。
{: .block-preface }


## Decision Making & Reinforcement Learning

在机器学习课程中我们介绍过各种机器学习算法大致可以分为三类：**监督学习(supervised learning)**、**无监督学习(unsupervised learning)**以及**强化学习(reinforcement learning)**。其中监督学习的目标是从数据中学习到一个样本到目标的映射$$y=f(x)$$，无监督学习的目标是学习数据分布上的结构，而强化学习的目标同样是学习一个映射$$y=f(x)$$。但和监督学习不同的是，在强化学习中的目标$$f(x)$$并不是直接从数据进行学习的。在强化学习中有所谓**环境(environment)**的概念，在每一时刻**智能体(agent)**会根据它当前的**状态(state)**与环境进行互动，同时环境也会给予智能体一个反馈信号称为**奖励(reward)**。我们希望智能体能够学习到一个决策函数$$f(x)$$从而最大化环境给予的奖励。

## Markov Decision Process

**Markov决策过程(Markov decision process, MDP)**是强化学习的基本理论框架。一个标准的MDP包含以下元素：

1. **状态(state)**表示智能体所处的状态$$s$$；
2. **模型(model)**表示智能体从状态$$s$$执行动作$$a$$后转移到状态$$s'$$的概率，即$$T(s, a, s') \sim P(s' \vert s, a)$$；
3. **动作(action)**表示智能体可以执行的行为$$a$$；
4. **奖励(reward)**表示环境给予智能体的奖励信号，在不同算法中它可以记为$$R(s)$$、$$R(s, a)$$或者$$R(s, a, s')$$等；
5. **策略(policy)**表示智能体进行决策的函数，它将状态$$s$$映射为一个动作$$a$$即$$a = \pi(s)$$，其中具有最大长期奖励的策略称为**最优策略(optimal policy)**，记为$$\pi^*$$；

<div align=center>
<img src="https://i.imgur.com/24gZNVb.png" width="60%">
</div>

实际上状态、模型、动作以及奖励定义了整个待求解的问题，而强化学习的目标是求解出这个MDP的最优策略。

### Sequences of Rewards

在强化学习中一种常见的情况是考虑无限长序列的累计奖励：

$$
U(s_0, s_1, ...) = \sum_{t=0}^\infty R(s_t)
$$

在这种情况下的效用是不收敛的，我们也无法基于效用来比较不同策略的好坏。为了处理这种问题我们需要引入折扣系数$$0 \leq \gamma \lt 1$$对$$t$$时刻的奖励进行加权，此时的效用可以表示为一个几何级数：

$$
U(s_0, s_1, ...) = \sum_{t=0}^\infty \gamma^t R(s_t)
$$

这样就可以保证效用的收敛性：

$$
U(s_0, s_1, ...) \leq \sum_{t=0}^\infty \gamma^t R_{\max} = \frac{R_{\max}}{1-\gamma}
$$

### Policies

我们在上面提到过最优策略是使长期奖励最大的策略。在折扣系数的修正下我们可以更严格地来定义它：

$$
\pi^* = \arg \max_\pi \mathbb{E} \bigg[ \sum_{t=0}^\infty \gamma^t R(s_t) \bigg\vert \pi \bigg]
$$

同时，我们定义在策略$$\pi$$下的效用为：

$$
U^\pi (s) = \mathbb{E} \bigg[ \sum_{t=0}^\infty \gamma^t R(s_t) \bigg\vert \pi, s_0=s \bigg]
$$

这里需要对效用和奖励的概念进行区分：效用$$U$$需要考虑长期的累计奖励，而奖励$$R$$则只是当前状态的反馈信号。

对于最优策略$$\pi^*$$，它和它对应的效用函数还需要满足：

$$
\pi^* (s) = \arg \max_a \sum_{s'} T(s, a, s') \ U^* (s')
$$

上式说明在状态$$s$$下，最优策略$$\pi^*$$给出的动作为所有可行动作中最大化下一状态$$s'$$效用期望的那个动作。

更进一步，我们可以对$$U^* (s)$$进行展开：

$$
U^* (s) = R(s) + \gamma \cdot \max_a \sum_{s'} T(s, a, s') \ U^* (s')
$$

上式称为**Bellman方程(Bellman equation)**。Bellman方程是MDP中最核心的方程，它指出了最优策略下效用函数自身的递归关系。

### Finding Policies

显然Bellman方程是一个非线性方程一般无法直接进行求解，但是我们可以通过迭代的方法来计算最优策略对应的效用函数。具体来说，在每一步我们需要进行迭代：

$$
\hat{U}_{t+1} (s) = R(s) + \gamma \cdot \max_a \sum_{s'} T(s, a, s') \ \hat{U}_t (s')
$$

当迭代次数足够多时可以证明$$\hat{U}_t$$会收敛到最优效用函数上。这种通过不断迭代来计算最优效用函数以及最优策略的方法称为**价值迭代(value iteration)**。

除了价值迭代之外，我们还可以从策略的角度出发来寻找最优策略。具体来说，在每一步我们首先计算当前策略的效用：

$$
U_t (s) = R(s) + \gamma \cdot \sum_{s'} T(s, \pi_t(s), s') \ U_t (s')
$$

然后利用计算出的效用函数来更新策略：

$$
\pi_{t+1} (s) = \arg \max_a \sum_{s'} T(s, a, s') \ U_t (s')
$$

这种先计算当前策略效用函数再对策略进行更新的方法称为**策略迭代(policy iteration)**。

## The Bellman Equations

Bellman方程有很多等价的形式。如果我们把奖励$$R$$定义为状态和动作的函数，则可以得到价值函数的递归形式：

$$
V(s) = \max_a \bigg( R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ V (s') \bigg)
$$

对上式进行展开可以得到：

$$
V(s_1) = \max_{a_1} \bigg( R(s_1, a_1) + \gamma \cdot \sum_{s_2} T(s_1, a_1, s_2) \ \max_{a_2} \bigg( R(s_2, a_2) + \gamma \cdot \sum_{s_3} T(s_2, a_2, s_3) ... \bigg) \bigg)
$$

我们可以把最外面括号中的内容定义为一个关于状态和动作的函数$$Q(s, a)$$，这样可以得到Q函数(quantity)的递归关系：

$$
Q(s, a) = R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} Q(s', a')
$$

如果我们把上式中奖励$$R(s, a)$$后面的部分定义为$$C(s, a)$$，还可以得到C函数(continuation)的递归关系：

$$
C(s, a) = \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} \bigg( R(s', a') + C(s', a') \bigg)
$$

这样我们就推导了Bellman方程的三个等价形式，它们之间存在相互转换关系：

$$
V(s) = \max_a Q(s, a) = \max_a \bigg( R(s, a) + C(s, a) \bigg)
$$

$$
Q(s, a) = R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ V(s') = R(s, a) + C(s, a)
$$

$$
C(s, a) = \gamma \cdot \sum_{s'} T(s, a, s') \ V(s') = \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} Q(s', a')
$$