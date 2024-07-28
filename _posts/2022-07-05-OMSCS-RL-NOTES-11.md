---
layout: post
title: OMSCS-RL课程笔记11-Game Theory
date: 2022-07-05
description: 博弈论基础I
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节课介绍博弈论的基本知识。
{: .block-preface }


## What is Game Theory?

**博弈论(game theory)**是研究如何解决多体之间竞争问题的学科，在经济、军事以及社会科学等领域都有大量的应用。博弈论与强化学习也有千丝万缕的联系：在强化学习中我们一般是考虑环境中只有一个智能体时智能体如何进行决策，而当环境中有多个智能体时智能体之间如何进行互动就需要博弈论的相关知识。

<div align=center>
<img src="https://i.imgur.com/6uJ8GJZ.png" width="80%">
</div>

## Minimax

### A Simple Game

一个简单的博弈例子是考虑两个智能体之间的相互竞争行为。假设环境的状态由a和b两个智能体进行控制且它们按顺序轮流行动，在某些状态下环境会分别基于两个智能体一定的回报。此时a和b两个智能体的所有可行行为可以使用一棵树来进行表示。如果我们进一步假定两个智能体始终知道环境的全部信息且任意时刻它们的累计回报为0，这样的博弈也称为**2-player zero-sum finite game of perfect information**。

<div align=center>
<img src="https://i.imgur.com/Lv7d48Z.png" width="80%">
</div>

在博弈论中，任意智能体状态到行为的映射称为**策略(strategy)**。对于上述的博弈过程，我们可以使用一个矩阵来表示所有可能的策略组合。矩阵的每一行以及每一列表示一个智能体在不同状态下的行为，而矩阵的每一个元素则是两个智能体分别按照所选的策略行动后产生的回报。

<div align=center>
<img src="https://i.imgur.com/Ik58xay.png" width="80%">
</div>

### Minimax

实际上我们只需要这样的一个矩阵就可以描述两个智能体之间的博弈问题。由于是零和博弈，智能体a会试图最大化回报而智能体b则会试图最小化回报。此时对于智能体a来说它的最优策略是寻找经过智能体b最小化回报的策略后具有最大回报的那个策略，即在一系列最小回报中寻找其中最大的那个回报对应的策略，这样的算法称为**minimax**。

<div align=center>
<img src="https://i.imgur.com/BON3xIu.png" width="80%">
</div>

实际上对于2-player zero-sum finite game of perfect information，我们有如下定理保证minimax算法的正确性和最优性。而且无论从哪个agent开始进行决策使用minimax得到的最优策略总是相同的，即minimax等于maximin。

<div align=center>
<img src="https://i.imgur.com/TP45e7z.png" width="80%">
</div>

### Non-Deterministic Game

接下来我们考虑环境中存在随机性的情况，此时智能体的行为会依据一定的概率产生不同的回报。在这种情况下则需要利用数学期望来重新构造回报矩阵。

<div align=center>
<img src="https://i.imgur.com/ePhqyXX.png" width="80%">
</div>

得到回报矩阵后就可以使用前面介绍过的minimax算法来计算最优策略。

<div align=center>
<img src="https://i.imgur.com/WvbhWRk.png" width="80%">
</div>

### Mixed Strategy

如果我们进一步放松对环境的限制，假设智能体获得的信息是不完整的。在这种情况下我们同样可以使用树以及回报矩阵来描述博弈过程。

<div align=center>
<img src="https://i.imgur.com/a7aPDcc.png" width="80%">
<img src="https://i.imgur.com/FTV29KY.png" width="80%">
</div>

需要注意的是由于信息的不完整，两个智能体的最优策略往往是不一致的。换句话说在非完全信息的条件下minimax不等于maximin。

<div align=center>
<img src="https://i.imgur.com/voip7Ar.png" width="80%">
</div>

为了处理这种不一致的问题我们需要引入**混合策略(mixed strategy)**。所谓混合策略是指智能体会按照一定的概率来选择不同状态下的行为，此时智能体获得的回报是关于行为概率的函数。

<div align=center>
<img src="https://i.imgur.com/hizQ1np.png" width="80%">
</div>

我们进一步计算两条直线的交点可以获得最优混合策略对应的行为概率，在这个概率下无论对方选择何种行为智能体a获得的回报都是相同的同时也是最优的。此时的minimax算法相当于在每个概率上计算两条直线中较小的那个值，然后计算整个概率范围上曲线的最大值。

<div align=center>
<img src="https://i.imgur.com/EOpJb5c.png" width="80%">
</div>

## Prisoner's Dilemma

**囚徒困境(prisoner's dilemma)**是博弈论中的经典问题。我们假设两名囚犯分别单独接受审讯，他们的刑期取决于是否向警方供认。注意此时整个博弈过程就不再是零和博弈了，我们需要使用一个元组来表示两名囚犯在不同行为(策略)下自身的回报，而每名囚犯也只关注最大化自身的回报。

<div align=center>
<img src="https://i.imgur.com/J10MKQS.png" width="80%">
</div>

囚徒困境的难点在于当每名囚犯只关注自身的回报时会导致整个系统无法获得最大的整体回报。实际上对于每名囚犯而言他的最优策略都是选择向警方招供，因为此时无论对方如何选择自己招供都会获得更高的回报。但是当两名囚犯同时选择招供时他们的总体收益是远低于选择相互合作的。

<div align=center>
<img src="https://i.imgur.com/AENNuTN.png" width="80%">
</div>

### Nash Equilibrium

囚徒困境对于更多智能体的博弈问题同样成立，为了更加深入地讨论我们需要引入**Nash均衡(Nash equilibrium)**的概念。Nash均衡是指在博弈中n个智能体都选择不再改变自身策略的情况，此时整个系统就达到了某种均衡状态。

<div align=center>
<img src="https://i.imgur.com/JAIcpmZ.png" width="80%">
</div>

要计算Nash均衡实际上非常简单，我们从任意状态开始对每个智能体进行遍历不断寻找当前自身的最优策略直到所有的智能体都不再更新即可。不过需要注意的是Nash均衡往往不是唯一的，而且Nash均衡也不意味着系统的整体最优。

<div align=center>
<img src="https://i.imgur.com/CnSnKKI.png" width="80%">
</div>

关于Nash均衡还有很多实用的性质如下：

<div align=center>
<img src="https://i.imgur.com/OdUAlBH.png" width="80%">
</div>

### Repeated Games

假设两名囚犯现在会进行多轮博弈，在每一轮博弈后会得知另一名囚犯的选择，而且每一名囚犯可以选择对另一名囚犯进行威胁来影响对方的决策。可以证明此时重复博弈的解会收敛到Nash均衡上。

<div align=center>
<img src="https://i.imgur.com/QRQ9yd2.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/nkKmA9T.png" width="80%">
</div>