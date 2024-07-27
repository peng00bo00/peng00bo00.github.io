---
layout: post
title: OMSCS-DL课程笔记17-Deep Reinforcement Learning
date: 2022-03-28
description: 深度强化学习
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍深度强化学习。
{: .block-preface }


## Reinforcement Learning Introduction

到目前为止本课程主要介绍了深度学习在**监督学习(supervised learning)**中的应用，除了监督学习之外机器学习的范畴还包括**无监督学习(unsupervised learning)**以及**强化学习(reinforcement learning)**。

<div align=center>
<img src="https://i.imgur.com/Gznqv8G.png" width="80%">
</div>

强化学习的目标是求解一个序列决策问题。我们希望**智能体(agent)**能够通过与**环境(environment)**的互动和反馈来学习到合适的**策略(policy)**，从而最大化**奖励(reward)**。

<div align=center>
<img src="https://i.imgur.com/MLccZod.png" width="80%">
</div>

和监督学习相比，强化学习不存在一个明确的学习目标，智能体只能通过环境给出的反馈信号进行学习。同时反馈信号往往是滞后的，即在很多情况下只会在最后一步才能得到奖励。

<div align=center>
<img src="https://i.imgur.com/pjGwPp8.png" width="80%">
</div>

强化学习的常见难点如下：

<div align=center>
<img src="https://i.imgur.com/wZqRRE5.png" width="80%">
</div>

从更严格的角度来看，强化学习可以按照如下方式进行建模。在任意时刻$$t$$，智能体根据观测$$o_t$$执行了行为$$a_t$$，而环境则根据智能体给出的行为返回下一时刻的观测$$o_{t+1}$$以及奖励$$r_{t+1}$$。

<div align=center>
<img src="https://i.imgur.com/5RGITe3.png" width="80%">
</div>

目前强化学习在很多领域都取得了令人瞩目的研究成果。

<div align=center>
<img src="https://i.imgur.com/OieyUok.png" width="80%">
<img src="https://i.imgur.com/Eu0dWno.png" width="80%">
<img src="https://i.imgur.com/yRuLp0m.png" width="80%">
</div>

## Markov Decision Processes

### MDPs in the Context of RL

要严格介绍强化学习则需要引入**Markov决策过程(Markov decision process, MDP)**。一个MDP包括状态空间$$\mathcal{S}$$、行为空间$$\mathcal{A}$$、奖励函数$$\mathcal{R}(s, a, s')$$、转移概率$$\mathbb{T}(s, a, s')$$以及折扣系数$$\gamma$$。同时MDP假定了转移概率$$\mathbb{T}(s, a, s')$$满足一阶Markov性，即$$t$$时刻到$$t+1$$时刻的转移概率只与$$t$$时刻的状态和行为有关，与状态和行为的历史无关。

<div align=center>
<img src="https://i.imgur.com/unY51D3.png" width="80%">
</div>

在强化学习中转移概率和奖励函数对于智能体是未知的，智能体只能通过和环境的互动来学习最优策略。当然在编程时程序员是需要知道转移概率和奖励函数的，毕竟这样才能编写出运行环境。

<div align=center>
<img src="https://i.imgur.com/n9x5dKO.png" width="80%">
</div>

以二维格子世界为例，智能体从三角形位置出发到达对应的格子时会获得相应的奖励。这个MDP的状态空间包括所有格子的坐标，行为空间为上下左右四个可能的前进方向，而奖励函数则是在指定格子上得到的反馈。需要说明的是由于环境自身随机性的存在，智能体实际的行为不一定完全符合规划的预期。

<div align=center>
<img src="https://i.imgur.com/J8huydN.png" width="80%">
</div>

### Solving MDPs: Optimal Policy

求解MDP意味着寻找到当前环境下的**最优策略(optimal policy)**。智能体的策略可以是确定性的也可以是随机的，而一个好的策略不仅仅要考虑当前状态下的奖励，更要保证在将来的状态中有着比较高的奖励。

<div align=center>
<img src="https://i.imgur.com/Wzd8isN.png" width="80%">
</div>

我们为每一时刻$$t$$获得的奖励乘上一个折扣系数$$\gamma^t$$，定义策略$$\pi$$的回报为按照该策略执行后未来所有可能状态奖励的期望。这样最优策略$$\pi^*$$就是最大化这个折扣后奖励期望的策略。

<div align=center>
<img src="https://i.imgur.com/w8rXGON.png" width="80%">
</div>

### Discounting Future Rewards

之所以考虑折扣后的奖励一方面是为了保证奖励序列可以收敛，另一方面通过折扣系数来控制智能体更关注于近期的奖励。一般来说$$\gamma$$越趋向于1越关注长期的奖励，而$$\gamma$$越趋向于0则会更关注短期的奖励。

<div align=center>
<img src="https://i.imgur.com/k05gjTz.png" width="80%">
</div>

### Value Function

描述未来折扣后奖励期望的函数称为**价值函数(value function)**。在强化学习中包括两种价值函数：首先是描述状态$$s$$自身好坏的函数，称为**状态价值函数(state value function)**记为$$V(s)$$；另一种是描述在状态$$s$$下采取行为$$a$$的好坏的价值函数，称为**状态-行为价值函数(state-action value function)**记为$$Q(s, a)$$。

<div align=center>
<img src="https://i.imgur.com/YbIJ9Qe.png" width="80%">
</div>

状态价值函数定义为从$$s$$状态出发按照策略$$\pi$$选择动作后所有可能轨迹的折扣回报之和的期望。

<div align=center>
<img src="https://i.imgur.com/FMQnnsG.png" width="80%">
</div>

状态-行为价值函数与之类似，只不过除了$$s$$状态还需要考虑该时刻的行为$$t$$作为条件再计算期望。

<div align=center>
<img src="https://i.imgur.com/T5cAfEc.png" width="80%">
</div>

## Algorithms for Solving MDPs

### Optimal V & Q functions

我们把最优策略$$\pi^*$$对应的价值函数记为$$V^*(s)$$和$$Q^*(s, a)$$，显然最优策略在状态$$s$$下给出的行为等于给定$$s$$条件下$$Q^*(s, a)$$取最大值时的动作$$a$$，同时两个价值函数需要满足如下关系：

<div align=center>
<img src="https://i.imgur.com/UB5vBvP.png" width="80%">
</div>

### Bellman Optimality Equations

求解MDP的理论基础是**Bellman最优方程(Bellman optimality equation)**。我们把$$Q^*(s, a)$$进行展开，得到0时刻奖励和后续时刻最优价值函数需要满足的关系式如下：

<div align=center>
<img src="https://i.imgur.com/XSQVQ4S.png" width="80%">
</div>

再利用$$Q$$函数和$$V$$函数之间的关系，可以得到最优价值函数的递推公式：

<div align=center>
<img src="https://i.imgur.com/GnANBgM.png" width="80%">
</div>

### Value Iteration

利用$$V$$函数的递推关系可以得到**价值迭代(value iteration)**算法。在每一步迭代中需要对所有可能的状态和行为进行求和，因此价值迭代在每一步的计算复杂度为$$O(\vert \mathcal{S} \vert^2 \vert \mathcal{A} \vert)$$。

<div align=center>
<img src="https://i.imgur.com/jpLJk7P.png" width="80%">
</div>

### Q-Iteration

如果使用$$Q$$函数来代替$$V$$函数则可以另一个价值迭代**(Q-iteration)**算法。

<div align=center>
<img src="https://i.imgur.com/ls2nhRl.png" width="80%">
</div>

### Policy Iteration

类似于价值迭代，我们还可以从策略函数出发进行迭代求解。此时每个迭代中需要先估计当前策略的价值函数，然后按照估计出的价值函数来更新策略。可以证明这样的策略迭代算法同样能够收敛到最优策略，而且迭代次数一般要远小于价值迭代。

<div align=center>
<img src="https://i.imgur.com/msfFli0.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/VXErukZ.png" width="80%">
</div>

## Deep Q-Learning

在深度学习时代人们还开发出了**deep Q-learning算法**来处理更加复杂环境下的强化学习问题。它的思想在于使用一个神经网络来表示$$Q$$函数，然后通过最小化当前时刻$$Q$$函数的预测值和下一时刻$$Q$$函数的目标值之间的误差来更新价值函数。实际编程时为了提高求解的稳定性一般会考虑使用两个网络来表示$$Q$$函数。这样在最小化误差时需要先冻结$$Q_\text{old}$$的参数只更新$$Q_\text{new}$$，然后再把更新后的参数赋值给$$Q_\text{old}$$进行新一轮的迭代。

<div align=center>
<img src="https://i.imgur.com/YnKXpKq.png" width="80%">
<img src="https://i.imgur.com/2lPfipi.png" width="80%">
<img src="https://i.imgur.com/55LLWwA.png" width="80%">
</div>

这样整个算法的流程为收集一系列的状态-动作-奖励样本，然后使用两个$$Q$$函数网络估计价值函数，最后通过最小化误差来更新$$Q_\text{new}$$并把更新后的参数赋给$$Q_\text{old}$$进入下一次迭代。

<div align=center>
<img src="https://i.imgur.com/QaCnoqy.png" width="80%">
</div>

### Exploration Problem

为了使用deep Q-learning算法我们还需要一套收集数据的机制，这实际上是一个相当复杂的问题。它的难点之一在于如何去平衡智能体对环境的**探索(exploration)**以及**利用(exploitation)**当前的策略；同时我们还需要考虑数据样本之间往往不满足独立同分布假设，事实上使用同一策略进行采样时得到的状态序列必然是相互关联的。

<div align=center>
<img src="https://i.imgur.com/YrJPCko.png" width="80%">
<img src="https://i.imgur.com/RliWMOy.png" width="80%">
</div>

强化学习中进行采样的一种常用方法是**$$\boldsymbol{\varepsilon}$$-greedy**策略，此时我们通过一个超参数$$\varepsilon$$来控制随机策略和当前最优策略之间的比例。

<div align=center>
<img src="https://i.imgur.com/9YowQT5.png" width="80%">
</div>

而对于数据之间相互关联的问题则可以通过一个**replay buffer**来处理。我们把所有生成的状态样本放入一个buffer中，然后在训练网络时直接从这个buffer中进行采样。

<div align=center>
<img src="https://i.imgur.com/M7Alptz.png" width="80%">
</div>

把上面这些技巧结合起来就得到了完整的deep Q-learning算法：

<div align=center>
<img src="https://i.imgur.com/AjFZX6k.png" width="80%">
</div>

## Policy Gradients, Actor-Critic

### Policy Gradient

除了使用Bellman方程求解MDP外我们也可以考虑直接对累计奖励的期望进行优化来获取最优策略。

<div align=center>
<img src="https://i.imgur.com/aPqSYvV.png" width="80%">
</div>

利用环境的Markov性可以对状态序列进行分解，这样累计奖励的期望就是关于策略$$\pi$$以及回报$$r$$的函数。

<div align=center>
<img src="https://i.imgur.com/hBjDDnX.png" width="80%">
<img src="https://i.imgur.com/q3N1KVl.png" width="80%">
</div>

最后我们使用梯度上升来求解这个优化问题，得到的算法就称为**策略梯度(policy gradient)**。

<div align=center>
<img src="https://i.imgur.com/JRWM3iA.png" width="80%">
</div>

计算目标函数关于策略的梯度需要一定的技巧。利用期望和微分的可交换性，我们可以先对状态序列求微分然后累加起来计算期望。同时结合对数函数的性质就可以把积分转换为对数策略函数与奖励函数乘积的期望形式。

<div align=center>
<img src="https://i.imgur.com/nn66zzA.png" width="80%">
</div>

由于状态转移概率与策略函数无关，我们还可以再丢掉期望中的状态转移部分。这样整个目标函数就是对数策略的梯度与累计奖励的乘积。

<div align=center>
<img src="https://i.imgur.com/A6L087c.png" width="80%">
</div>

### The REINFORCE Algorithm

把上面介绍的技巧结合起来就得到了**REINFORCE**算法，它通过梯度上升的方式来逐步优化当前的策略。

<div align=center>
<img src="https://i.imgur.com/ZJKi2MV.png" width="80%">
</div>

实际应用REINFORCE算法时会发现它会产生非常大的方差从而导致训练过程的不稳定。要减小方差可以引入一个**基准线(baseline)** $$b(s_t)$$，这样在对对数策略加权时在累计回报的基础上减去一个基准值来降低方差。

<div align=center>
<img src="https://i.imgur.com/hq993mN.png" width="80%">
</div>

### Policy Gradient Variants

除了REINFORCE算法之外，现代策略梯度算法还包括使用$$Q$$函数代替累计回报的**actor-critc算法**以及使用advantage代替$$Q$$函数的**advantage actor-critc算法**等其它方法。

<div align=center>
<img src="https://i.imgur.com/wcDS9my.png" width="80%">
</div>