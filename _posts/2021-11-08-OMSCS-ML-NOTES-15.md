---
layout: post
title: OMSCS-ML课程笔记15-Game Theory
date: 2021-11-08
description: 博弈论
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节介绍博弈论的相关内容。
{: .block-preface }


## Games

**博弈论(game theory)**是研究个体之间如何相互合作竞争并寻找最优策略的学科。和强化学习相比，博弈论的特点是同一个系统中有多个**玩家(player)**，同时每个玩家都想要在博弈过程中最大化自身的收益。博弈论在现实生活中的应用已经超出了计算机科学的范畴，在金融市场、军事战略甚至社会科学中都能见到博弈论的身影。

### Minimax

最简单的博弈只包含2个玩家，且他们之间的信息是对等的。我们称这样的博弈为**2-player, zero-sum, finite, deterministic game with perfect information**：

- 2-player是指只有2个玩家；
- zero-sum是指博弈是零和的，两个玩家的收益之和为0；
- finite是指玩家的状态和策略都是有限的；
- deterministic是指系统是确定的，没有随机因素；
- perfect information是指两个玩家都能获得全部的信息。

我们通过一个简单的例子来进行介绍。假设博弈过程可以通过如下图所示的树来表示，其中节点表示状态边表示玩家可选的行为。由$$A$$玩家先行从状态1中选择向左或是向右，然后$$B$$选择下一个动作，再回到$$A$$进行选择。当状态到达叶节点时游戏结束，叶节点对应的数字为玩家$$A$$的收益(由于是零和博弈，玩家$$B$$的收益为它的相反数)。玩家$$A$$的目标是最大化收益，相应地玩家$$B$$的目标是最小化收益。

<div align=center>
<img src="https://i.imgur.com/zCx3SnZ.png" width="40%">
</div>

根据玩家$$A$$和玩家$$B$$所有可能的行为，我们可以利用一个矩阵来表示整个博弈过程的所有可能结果，如下图所示：

<div align=center>
<img src="https://i.imgur.com/LdhuXXd.png" width="30%">
</div>

矩阵的每一行表示玩家$$A$$采取的行为(策略)，每一列表示玩家$$B$$采取的行为(策略)；矩阵中的每个元素表示玩家$$A$$和玩家$$B$$的行为所导致的最终收益。对于玩家$$A$$来说，无论他做出什么样的选择玩家$$B$$都会试图最小化收益，因此$$A$$的最优策略是选择第2行；类似地，对玩家$$B$$而言，玩家$$A$$会试图最大化每一列的数字，因此玩家$$B$$的最优策略是选择第2列。

上文所述的过程实际上就是minimax算法。我们假设自身是玩家$$A$$，我们需要在玩家$$B$$最小化收益的策略中选择收益最大的那个策略，该策略即为最优策略。实际上Von Neumann证明了对于2-player, zero-sum, finite, deterministic game with perfect information，每个玩家都存在最优策略且他们的按照最优策略行动的话博弈过程的解是确定的，即minimax=maximin。

### Relaxation: Non-Determinism

接下来我们放松一下系统的限制，假设系统存在随机性。以下图为例，当系统状态到达方块的位置时我们可能会依据概率得到不同的收益：

<div align=center>
<img src="https://i.imgur.com/grhs70u.png" width="40%">
</div>

在这种情况下我们只需要用收益的期望来代替原本的收益即可，这样就可以得到这个博弈过程的矩阵表示：

<div align=center>
<img src="https://i.imgur.com/AkP40b6.png" width="20%">
</div>

### Relaxation: Hidden Information

我们进一步放松对系统的限制，假设现在玩家不再具有全部的信息。这里我们还是用一个例子进行说明：假设玩家$$A$$在开局会得到红色或是黑色的牌且这张牌只有他自己可见。如果$$A$$开始获得的是红色的牌则$$A$$可以选择`hold`或是`resign`，如果是`hold`收益由$$B$$的行为来决定，而如果$$A$$选择`resign`则会获得-20的收益。类似地，如果$$A$$开始获得的是黑色的牌则他必须选择`hold`，最终的收益仍然由$$B$$的行为来决定。整个博弈过程可以利用下图所示的树来表示：

<div align=center>
<img src="https://i.imgur.com/ebYkGCF.png" width="40%">
</div>

我们同样利用收益的期望来计算不同策略的收益，从而得到收益矩阵如下所示：

<div align=center>
<img src="https://i.imgur.com/FfXI2eN.png" width="30%">
</div>

此时从$$A$$的角度上看他无论选择`hold`还是`resign`都会得到-5的收益，但从$$B$$的角度上看他会选择`see`从而得到5的收益。此时$$A$$和$$B$$按照最优策略行动会得到不同的解，这说明由于隐藏信息的存在会导致minimax $$\neq$$ maximin。

实际上如果玩家$$B$$知道$$A$$的行为模式的话仍然可以保证minimax=maximin。假设当$$A$$手上的是红牌的时候他会选择`resign`而且$$B$$也知道这点，那么当$$A$$选择了`hold`时$$B$$一定会选择`resign`从而最小化收益。

### Mixed Strategies

所谓的mixed strategy是指关于关于策略的分布。我们假设$$A$$会以$$P$$的概率选择`hold`这个行为，同时假设$$B$$永远会选择`resign`，则当前策略下$$A$$的收益为：

$$
\begin{aligned}
\mathbb{E}[\text{profit}] &= (0.5 \times 10) + (0.5 \times ((P \times 10) + ((1-P) \times (-20)))) \\
&= 15 P - 5
\end{aligned}
$$

类似地，如果$$B$$永远会选择`see`则$$A$$的收益为：

$$
\begin{aligned}
\mathbb{E}[\text{profit}] &= (0.5 \times 30) + (0.5 \times ((P \times -40) + ((1-P) \times -20))) \\
&= -10 P + 5
\end{aligned}
$$

接下来我们把这两种情况下$$A$$的收益函数画在一张图上，得到它们的交点$$P=0.4$$。这个点表示当$$P$$取0.4时，无论$$B$$采取什么样的策略(pure strategy或者mixed strategy)$$A$$获得的期望收益都是相同的。但需要注意的是这一点并不意味着最优概率或是最优收益。

<div align=center>
<img src="https://i.imgur.com/qsrX1jm.png" width="40%">
</div>

### Prisoner’s Dilemma

接下来我们再去掉零和博弈的约束，然后介绍著名的**囚徒困境(prisoner’s dilemma)**：

- 假设有两个囚犯分开接受审讯；
- 每个囚徒可以选择和对方合作(cooperate)或是向警方招供(snitch)；
- 如果两名囚犯都保持和对方合作的话每个人获得1年的刑期；
- 如果两名囚犯都招供的话每个人获得6年的刑期；
- 如果一名囚犯招供而另一名囚犯选择合作，则招供的可以免罪而选择合作的获得9年刑期。

整个博弈过程的收益可以用如下所示的矩阵来表示：

<div align=center>
<img src="https://i.imgur.com/GaABC13.png" width="40%">
</div>

对于每一名囚犯来说他的最优策略都是选择招供，这是因为此时无论另一名囚犯如何选择他的刑期都是最短的。这种策略保证无论其它人如何选择自身的收益都是最大，因此这样的策略也被称为**支配策略(dominant strategy)**。但在这种情况下两个人都会获得6年的刑期，而如果两个人都选择合作的话每个人只有1年的刑期。换句话说每名囚犯的最优策略并不会导致总体的最优，要想达到总体最优每名囚犯的策略都依赖于另一名囚犯的策略。

### Nash Equilibrium

囚徒困境对于多名玩家的情况同样成立。假设有$$n$$名玩家，每名玩家可行的策略为集合$$s_i$$，我们称策略$$s_1^* \in s_1, s_2^* \in s_2, ..., s_n^* \in s_n$$达到了**Nash均衡(Nash equilibrium)**当且仅当对每名玩家有：

$$
s_i^* = \arg \max_{s_i} U_i(s_1^*, ..., s_i, ..., s_n^*)
$$

更直观来说，当系统达到Nash均衡时每个玩家都不会再试图改变自身的策略。回到囚徒困境的例子中，不难发现两名囚犯都选择招供就达到了Nash均衡，对于每个囚犯来说如果他选择了沉默都会增加自身的刑期。类似地可以证明其它的情况都不是Nash均衡。

Nash均衡有很多优秀的性质，比如说我们可以不断地删除所有可能的**被支配策略(strictly dominated strategies)**，而Nash均衡总是会保留下来。更重要的是对于有限玩家和有限策略的博弈，至少存在一个Nash均衡点。

## Repeated Games


接下来我们假设两名囚犯会进行多轮博弈，同时在每轮博弈中有$$\gamma$$的概率终止后续的过程。后面我们会看到在这样的情况下博弈过程与MDP后有非常多的相似之处。

### Tit-for-Tat

假设其中一名囚犯可以采取以牙还牙的策略(tit-for-tat)，即他会在博弈一开始选择合作，如果另一名囚犯选择告密的话就会进行同等的报复。具体来说，此时这名囚犯的行为取决于另一名囚犯：

- 如果另一名囚犯选择一直合作则他们会一直合作下去；
- 如果另一名囚犯选择一直告密则他们会一直相互告密；
- 如果另一名囚犯选择了一样的tit-for-tat策略，他们会一直合作下去；
- 如果另一名囚犯选择告密、合作、告密这样的策略，则这名囚犯会选择相反的行为。

在这样的策略下另一名囚犯的效用取决于博弈终止的概率$$\gamma$$。当他选择一直合作或是一直告密，则效用可以表示为：

$$
U(\text{always cooperate}) = -\frac{1}{1 - \gamma}
$$

$$
U(\text{always snitch}) = -\frac{6 \gamma}{1 - \gamma}
$$

上式说明当$$\gamma$$接近于0时应该选择告密，而当$$\gamma$$接近于1时应该选择合作。实际上此时这名囚犯的效用可以建模为下图表示的MDP：

<div align=center>
<img src="https://i.imgur.com/3cluhov.png" width="40%">
</div>

因此我们可以通过求解这个MDP来获得最优策略。

### Folk Theorem

不难发现当两名囚犯都选择了相互告密或是tit-for-tat策略时就达到了Nash均衡，更重要的是tit-for-tat策略会使双方达成相互合作的局面。换句话说当存在对等报复的情况时合作是可以实现的(**in repeated games, the possibility of retailiation opens the door for cooperation**)。

要严格的表述这种情形我们需要引入一些相关的概念。首先是**feasible payoff**，它是极端情况下payoff构成的凸包。然后是**minmax profile**，它是每名玩家对抗恶意攻击(malicious adversary)时所能得到的一对payoff。因此我们可以把整个博弈过程转换为一个零和博弈。

博弈论的**folk theorem**指出：

> Any feasible payoff profile that strictly dominated the minmax profile can be realized as a Nash equilibrium payoff profile, with a sufficiently large discount factor.

### Pavlov’s Strategy

接下来我们考虑另一种策略：在一开始选择合作，如果另一名囚犯选择告密的话就将行为改为告密。然后对他进行报复，每次选择和另一名囚犯相反的行为直到自身的状态回到合作。这种策略称为**Pavlov’s strategy**，可以用下图所示的MDP来表示：

<div align=center>
<img src="https://i.imgur.com/7yHcsua.png" width="40%">
</div>

Pavlov’s strategy同样达成了Nash均衡。它的另一个特点是如果两名囚犯都采取Pavlov’s strategy，那么无论他们的初始状态如何，最终都会选择相互合作。

最后我们介绍一下**computational folk theorem**：

> You can build a Pavlov-like machine for any game and construct a subgame perfect Nash equilibrium in polynomial time.

## Stochastic Games and Multiagent RL

### Generalization

接下来我们把强化学习的相关概念拓展到多智能体上。假设我们有两个智能体$$a$$和$$b$$，整个系统包括：

- 状态$$s$$，同时包括两个智能体的状态；
- 行为$$A_i$$，每个智能体都有各自的行为；
- 状态转移$$T(s, (a, b), s')$$由系统状态以及两个智能体的行为共同决定；
- 奖励函数$$R_1(s, (a, b))$$和$$R_2(s, (a, b))$$，每个智能体都有各自的奖励函数；
- 折扣系数$$\gamma$$由整个系统共享。

这样定义的系统称为**generalization of MDPs**。

### Solving Stochastic Games

对于多智能体的情况我们同样可以建立Bellman方程：

$$
Q_i^* (s, (a, b)) = R_i (s, (a, b)) + \gamma \sum_{s'} \bigg[ T(s, (a, b), s') \cdot \max_{a', b'} Q_i^* (s', (a', b')) \bigg]
$$

如果是零和博弈则可以理解利用$$\text{minimax}$$来代替$$\max$$：

$$
Q_i^* (s, (a, b)) = R_i (s, (a, b)) + \gamma \sum_{s'} \bigg[ T(s, (a, b), s') \cdot \underset{a', b'}{\text{minimax}} \ Q_i^* (s', (a', b')) \bigg]
$$

我们同样可以利用Q-learning的方式来进行更新：

$$
Q_i (s, (a, b)) = (1 - \alpha) \cdot Q_i (s, (a, b)) + \alpha \bigg[ r_i + \gamma \cdot \underset{a', b'}{\text{minimax}} \ Q_i (s', (a', b')) \bigg]
$$

上式称为**minimax-Q**。

对于更一般的非零和博弈的情况我们需要使用Nash均衡来取代$$\text{minimax}$$，对应的Bellman方程和Q-learning算法为：

$$
Q_i^* (s, (a, b)) = R_i (s, (a, b)) + \gamma \sum_{s'} \bigg[ T(s, (a, b), s') \cdot \underset{a', b'}{\text{Nash}} \ Q_i^* (s', (a', b')) \bigg]
$$

$$
Q_i (s, (a, b)) = (1 - \alpha) \cdot Q_i (s, (a, b)) + \alpha \bigg[ r_i + \gamma \cdot \underset{a', b'}{\text{Nash}} \ Q_i (s', (a', b')) \bigg]
$$

这个算法称为Nash-Q。需要注意的是由于Nash均衡的存在，Nash-Q可能不会收敛(存在多个Nash均衡)而且每个智能体的策略是相互依赖的，这些问题导致直接求解Nash-Q是非常困难的。

## Reference

- [Wikipedia: Minimax](https://en.wikipedia.org/wiki/Minimax)
- [Wikipedia: Minimax theorem](https://en.wikipedia.org/wiki/Minimax_theorem)
- [Wikipedia: Prisoner's dilemma](https://en.wikipedia.org/wiki/Prisoner%27s_dilemma)
- [Wikipedia: Nash equilibrium](https://en.wikipedia.org/wiki/Nash_equilibrium)
- [Wikipedia: Folk theorem (game theory)](https://en.wikipedia.org/wiki/Folk_theorem_(game_theory))