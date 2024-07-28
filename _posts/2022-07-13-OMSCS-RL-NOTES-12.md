---
layout: post
title: OMSCS-RL课程笔记12-Game Theory Reloaded
date: 2022-07-13
description: 博弈论基础II
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节课继续介绍博弈论的相关知识。
{: .block-preface }


## Iterated Prisoner's Dilemma

我们考虑需要进行**无限次重复博弈的囚徒困境问题(iterated Prisoner's Dilemma, IPD)**。在不断的博弈过程中由于Nash均衡的存在，两名囚徒实际上永远不会去选择改变自身的行为。

<div align=center>
<img src="https://i.imgur.com/IA5fIZU.png" width="80%">
</div>

然后我们引入折扣系数$$\gamma$$，它表示在每一轮的博弈后以$$\gamma$$的概率继续进行下一次博弈。利用几何级数，我们可以计算出博弈次数的期望为$$\frac{1}{1-\gamma}$$。

<div align=center>
<img src="https://i.imgur.com/BY54yLB.png" width="80%">
</div>

## Tit for Tat

对于这种依概率终止的博弈问题，一种经典的策略是**以牙还牙(tit for tat)**：此时的囚犯会选择在首次博弈中进行合作，然后接下来每一轮则采用与对手上一轮相同的行为。在这种情况下囚犯的策略可以描述为如下所示的状态机模型：

<div align=center>
<img src="https://i.imgur.com/3cgDZjN.png" width="80%">
</div>

另一名囚犯作为回应，在不同的策略下会得到不同的反馈，而且反馈值取决于系数$$\gamma$$。当$$\gamma$$值比较小时的最优策略是选择招供，而当$$\gamma$$值比较大时则会倾向于进行合作。

<div align=center>
<img src="https://i.imgur.com/Kh2g34U.png" width="80%">
</div>

## Finite State Strategy

利用状态机模型我们实际上可以把策略表示为一张图，图中黑色的部分是囚徒自己的行为，而绿色的部分则是对方的行为，而图中的数字则是双方在给定行为下囚徒得到的收益。不难发现此时囚犯的行为类似于一个MDP，只不过状态转移取决于对方的行为。

<div align=center>
<img src="https://i.imgur.com/EglcOyu.png" width="80%">
</div>

## Best Response in IPD

在IPD问题中囚犯的最优策略取决于对方的策略。比如说当对方选择一直合作时我们的最优策略是选择招供，而当对方选择招供时我们的最优策略同样是选择招供，此时两名囚犯都达到了Nash均衡。比较有趣的一点在于当双方都选择以牙还牙的策略时系统同样达到了Nash均衡，而且此时两名囚犯还会获得最大的总体收益，这表示在囚徒困境中仍然存在进行合作的可能。

<div align=center>
<img src="https://i.imgur.com/bHRFh0K.png" width="80%">
</div>

## Folk Theorem

实际上folk theorem指出在重复博弈中，**对等报复(retaliation)**的存在会使得博弈双方产生合作。

<div align=center>
<img src="https://i.imgur.com/hp6l6hx.png" width="80%">
<img src="https://i.imgur.com/deH8CUs.png" width="80%">
</div>

从几何的角度来看，我们可以把两名囚徒的收益用坐标轴上的点来表示。此时所有可能行为在平面上构成的凸包称为**可行域(feasible region)**，可以证明任意混合策略的回报一定位于可行域中。

<div align=center>
<img src="https://i.imgur.com/8KJyLb3.png" width="80%">
</div>

## Minmax Profile

**minmax profile**是指在重复博弈过程中，每名玩家面对恶性策略时所能实现的回报。这里"恶性策略"是指其它的玩家会针对当前玩家，使其可能获得的回报尽可能减少。

<div align=center>
<img src="https://i.imgur.com/wvUzGWm.png" width="80%">
</div>

## Security Level Profile

一般来说minmax profile是针对确定性策略的，而当我们需要考虑混合策略时还需要引入**security level**的概念。

<div align=center>
<img src="https://i.imgur.com/tSrPTTI.png" width="80%">
</div>

## Folksy Theorem

<div align=center>
<img src="https://i.imgur.com/CgfMmQZ.png" width="80%">
</div>

## Grim Trigger

**Grim trigger**同样是囚徒困境中的一种经典策略，它是说在一开始会无条件选择合作以获得最大的共同回报，但如果对方选择了向警察招供那么在后续的博弈中会永远选择招供作为报复。可以证明Grim trigger同样实现了Nash均衡。

<div align=center>
<img src="https://i.imgur.com/14FUUri.png" width="80%">
</div>

## Implausible Threats

Grim trigger在对方越界的之后会选择永远进行报复，但在现实的博弈中这样的行为实际上非常少见。在大多数情况下，报复的行为会更多地来自于对自身利益的最大化并且不考虑总体利益。

<div align=center>
<img src="https://i.imgur.com/GiS8w6p.png" width="80%">
</div>

## Pavlov

接下来我们再介绍一下**Pavlov**策略，它是利用对方的行为来最大化自身的利益。在一开始Pavlov策略与TfT非常相似，都是从合作出发并且当对方选择告密时自己同样会进行告密。但Pavlov策略中囚徒选择告密后的行为则与TfT策略相反：如果对方选择合作自己仍会进行告密来获取利益，而如果对方选择告密自己则会选择合作。可以证明Pavlov策略实现了Nash均衡，同时也满足subgame perfect。

<div align=center>
<img src="https://i.imgur.com/m3Y0x9G.png" width="80%">
<img src="https://i.imgur.com/WKp90DH.png" width="80%">
</div>

## Computational Folk Theorem

Pavlov策略在囚徒困境之外的其它博弈中也有大量的应用。

<div align=center>
<img src="https://i.imgur.com/k9uu2fn.png" width="80%">
</div>

## Stochastic Games and Multiagent RL

我们把MDP和重复博弈结合到一起就得到了**随机博弈(stochastic games)**，再基于随机博弈的框架我们就可以讨论**多智能体的强化学习问题(multiagent RL, MARL)**。在MARL中，智能体会在未知的环境中进行博弈同时试图最大化自身的收益。

<div align=center>
<img src="https://i.imgur.com/W5upmz9.png" width="80%">
</div>

更严格的来说，随机博弈可以按照如下方式来进行定义。

<div align=center>
<img src="https://i.imgur.com/qvchpVH.png" width="80%">
</div>

## Zero-Sum Stochastic Games

当环境中任意时刻智能体之间的回报之和为0时可以结合零和博弈的相关理论来对MARL进行处理。实际上我们可以把Bellman方程以及Q-learning的方法迁移到零和博弈中用来计算智能体的价值函数以及最优策略：

<div align=center>
<img src="https://i.imgur.com/Ppm0wf6.png" width="80%">
</div>

## General-Sum Games

对于更一般的MARL问题，我们同样可以利用Q-learning的框架进行求解。此时我们只需要将minimax操作替换为计算多智能体的Nash均衡即可，但需要注意此时的算法稳定性和收敛性往往都没有保障。

<div align=center>
<img src="https://i.imgur.com/M5WoKt1.png" width="80%">
<img src="https://i.imgur.com/twHl7sK.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/7FZwZtG.png" width="80%">
</div>
