---
layout: post
title: OMSCS-ML课程笔记14-Reinforcement Learning
date: 2021-11-01
description: 强化学习
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节介绍强化学习的相关内容。
{: .block-preface }


## MDP vs. RL

在上一节中我们介绍了如何通过求解Bellman方程来解决MDP或者说是强化学习的问题。而实际上MDP与强化学习还是有一定的区别的，它们的主要区别在于智能体是否知道外界环境的状态转移$$T$$：在MDP中智能体可以利用外界环境的状态转移$$T$$来计算最优策略；而在强化学习中智能体对环境是一无所知的，只能通过观测得到的状态转移序列$$\langle s, a, r, s' \rangle$$来学习到最优策略。

### API

我们需要对一些常见的概念进行区分：

- 如果我们已知外界环境的状态转移$$T$$以及回报函数$$R$$，我们可以通过一些算法来计算出最优策略，这样的过程称为**规划(planning)**；

<div align=center>
<img src="https://i.imgur.com/8TN3RtL.png" width="40%">
</div>

- 如果我们只能通过观测得到的状态转移序列$$\langle s, a, r, s' \rangle$$，我们希望通过这些观测来学习到最优策略，这样的过程称为**强化学习(reinforcement learning)**；

<div align=center>
<img src="https://i.imgur.com/RWEKrGL.png" width="40%">
</div>

- 我们可以利用观测得到的状态转移序列$$\langle s, a, r, s' \rangle$$来估计真实的外界环境，这样的过程称为**建模(modeling)**；

<div align=center>
<img src="https://i.imgur.com/5kJR57Y.png" width="40%">
</div>

- 此外我们还可以利用已有的模型来获取状态转移序列，这样的过程称为**仿真(simulation)**。

<div align=center>
<img src="https://i.imgur.com/cjJ32VT.png" width="40%">
</div>

### Three Approaches to RL

根据计算目标的不同我们可以把强化学习分成大致三种类型：

- 直接取计算策略$$\pi$$的算法称为**策略搜索(policy search)**；
- 通过计算效用$$U$$来获得策略的算法称为**基于价值函数(value-function based)**的强化学习；
- 此外我们还可以对外界环境进行建模并在此基础上获得最优策略，这类算法称为**基于模型(model based)**的强化学习。

实际上这三种类型的算法是相互关联的：在大多数情况下直接进行策略搜索会比较困难，但我们可以先计算价值函数然后通过价值函数来获得最优策略；如果获得了外界环境的模型，我们通过求解Bellman方程来计算价值函数并获得最优策略。

<div align=center>
<img src="https://i.imgur.com/NmhFrT3.png" width="60%">
</div>

以上这三种强化学习算法在实际中都有各自的应用，不过在本课程中我们主要关注基于价值函数的强化学习算法。

## Q-Learning

### Q-function

在上节课中我们介绍过最优策略需要满足Bellman方程：

$$
U(s) = R(s) + \gamma \cdot \max_{a \in A(s)} \sum_{s'} T(s, a, s') U(s')
$$

$$
\Pi (s) = \arg \max_{a \in A(s)} \sum_{s'} T(s, a, s') U(s')
$$

这里我们把行为$$a$$加入到价值函数中从而得到一个新的价值函数，一般称为**Q函数(Q-function)**：

$$
Q(s, a) = R(s) + \gamma \cdot \sum_{s'} T(s, a, s') \max_{a'} Q(s', a')
$$

实际上Q函数表示在状态$$s$$下执行$$a$$所对应的最优价值，这样我们就可以比较不同行为之间的好坏。

不难发现我们可以利用Q函数来重新定义最优价值以及策略：

$$
U(s) = \max_a Q(s, a)
$$

$$
\Pi (s) = \arg \max_{a} Q(s, a)
$$

也就是说如果我们计算得到了Q函数也就等于计算出了最优策略，而计算Q函数的过程则称为Q-learning。

### Estimating Q-function

由于我们不知道外界环境的状态转移$$T$$，我们不能显式地去计算Q函数。假设我们观测到了状态转移序列$$\langle s, a, r, s' \rangle$$，我们可以使用下式来更新Q函数：

$$
\hat{Q}(s, a) = \hat{Q}(s, a) + \alpha_t \cdot \bigg( r + \gamma \cdot \max_{a'} \hat{Q}(s', a') - \hat{Q}(s, a) \bigg)
$$

其中$$\alpha_t$$称为学习率，一般可以取$$\alpha_t = \frac{1}{t}$$。可以证明当状态$$s$$和行为$$a$$能够无限次地重新访问时，按照上式更新的Q函数会收敛到Bellman方程的解，即$$\hat{Q}(s, a) \rightarrow Q(s, a)$$。

### Choosing Actions

在实际应用Q-learning时有很多需要考虑的内容，包括：

- 如何初始化Q函数？
- 如何设置衰减的学习率$$\alpha_t$$？
- 如何选择行为$$a$$？

其中最重要的一点在于选择行为$$a$$。如果我们在迭代时总是选择当前的最优策略，则违背了Q函数的收敛条件；而如果我们总是选择一个随机行为则可以保证Q函数的收敛，但却没有利用到Q函数的信息。显然我们需要在迭代中不断尝试各种各样的行为，同时尽可能利用学习到的信息。因此一种合适的策略是在每一步中以概率$$\varepsilon$$对$$a$$进行随机采样，否则选择当前的最优行为：

$$
\hat{\Pi}(s) = 
\left\{
\begin{aligned}
& \arg \max_{a \in A(s)} \hat{Q}(s, a), & & \text{with probability} \ 1 - \varepsilon \\
& \text{random action}, & & \text{otherwise}
\end{aligned}
\right.
$$

这样的策略称为$$\pmb{\varepsilon}$$**-greedy exploration**。当我们对$$\varepsilon$$进行衰减时，我们不仅能够保证Q函数的收敛，同时学习到的策略$$\hat{\Pi}$$也会收敛到最优策略。

从一个更高的层面上讲，我们希望策略$$\hat{\Pi}$$能够在学习过程中探索不同的可能性，同时利用到已经学习到的内容，这种情况称为**Explore-Exploit dilemma**。Explore-Exploit dilemma是强化学习的基本问题，我们需要智能体在和环境的互动中尽可能多地探索不同的可能性，同时还要利用已有的信息做出最优的决策，这就需要对explore和exploit这两个过程进行权衡。

## Reference

- [Wikipedia: Q-learning](https://en.wikipedia.org/wiki/Q-learning)