---
layout: post
title: OMSCS-RL课程笔记06-Messing with Rewards
date: 2022-06-07
description: 奖励函数
tags: CS7642-RL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 强化学习课程([CS 7642: Reinforcement Learning](https://omscs.gatech.edu/cs-7642-reinforcement-learning))的同步课程笔记。课程内容涉及强化学习算法的理论和相关应用，本节主要讨论奖励函数对强化学习算法的影响。
{: .block-preface }

奖励函数是强化学习中非常重要的组成部分。实际上奖励函数不仅会影响到算法的收敛速度，而且也会影响最终学习到的策略。

<div align=center>
<img src="https://i.imgur.com/D1oxTyr.png" width="80%">
</div>

## Changing the Reward Function

首先考虑修改奖励函数是否会影响到最终学习到的策略。如果不希望改变学习到的策略，我们对每一步获得的奖励乘以一个正数或者加减一个常数，实际上还有一些非线性的变形方法可以保证学习到的策略不会发生改变。

<div align=center>
<img src="https://i.imgur.com/tfm0KMJ.png" width="80%">
</div>

### Multiplying by a Scalar

最简单的变形方法是对当前的奖励函数乘以一个常数$$c \gt 0$$：

$$
R'(s, a) = c \cdot R(s, a)
$$

假设原始MDP的价值函数为$$Q(s, a)$$，

$$
Q(s, a) = R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} Q(s', a')
$$

可以证明修改奖励函数后得到的价值函数$$Q'(s, a)$$满足

$$
\begin{aligned}
c \cdot Q(s, a) &= c \cdot R(s, a) + \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} c \cdot Q(s', a') \\
&= Q'(s, a)
\end{aligned}
$$

### Adding a Scalar

同样地，很容易证明在当前奖励函数上加减一个常数相当于在价值函数进行加减：

$$
R'(s, a) = R(s, a) + c
$$

$$
\begin{aligned}
Q(s, a) + \frac{c}{1-\gamma} &= R(s, a) + c + \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} Q(s', a') \ + \frac{c}{1-\gamma} \gamma \\
&= \bigg[ R(s, a) + c \bigg] + \gamma \cdot \sum_{s'} T(s, a, s') \ \max_{a'} \bigg[ Q(s', a') + \frac{c}{1-\gamma} \bigg] \\
&= Q'(s, a)
\end{aligned}
$$

## Reward Shaping

在很多强化学习问题中智能体获得的奖励是非常稀疏的，直接进行学习可能很难学习到合理的策略。此时可以利用重构奖励函数的方法来鼓励智能体进行学习，这种方法就称为**reward shaping**。

<div align=center>
<img src="https://i.imgur.com/c6DvCMU.png" width="80%">
</div>

### Potential-Based Shaping

**potential-based shaping**的思想是把奖励函数视为关于状态的势函数：当智能体位于某些状态时会获得一定的奖励，而当它离开这些状态则会失去相应的奖励。换句话说我们可以把奖励函数定义为势函数的差值，进而避免智能体学习到次优策略：

<div align=center>
<img src="https://i.imgur.com/nVUO9RI.png" width="80%">
</div>

更严格的表述如下：我们定义关于状态的势函数$$\psi(s)$$，此时可以重新定义奖励函数为：

$$
R'(s, a, s') = R(s, a) - \psi(s) + \gamma \cdot \psi(s')
$$

可以证明potential-based shaping后的价值函数满足：

$$
\begin{aligned}
Q(s, a) - \psi(s) &= \sum_{s'} T(s, a, s') \bigg( R(s, a) - \psi(s) + \gamma \psi(s') \bigg) + \gamma \cdot \max_{a'} \bigg[ Q(s', a') - \psi(s') \bigg] \\
&= Q'(s, a)
\end{aligned}
$$

由于势函数$$\psi(s)$$与行为$$a$$无关，经过shaping后仍然会得到与原始MDP相同的策略。

### Q-Learning with Potentials

对于[Q-learning算法](/2022/05/31/OMSCS-RL-NOTES-04.html#bellman-equations-with-actions)，我们可以把势函数定义为最优状态价值函数，这样可以得到迭代步骤：

$$
Q(s, a) \leftarrow Q(s, a) + \alpha_t \bigg[ r - \psi(s) + \gamma \cdot \psi(s') + \gamma \cdot \max_{a'} Q(s', a') - Q(s, a) \bigg]
$$

$$
\psi(s) = \max_a Q^*(s, a)
$$

此时可以证明学习到价值函数满足：

$$
\begin{aligned}
Q(s, a) &= Q^*(s, a) - \psi(s) \\
&= Q^*(s, a) - \max_a Q^*(s, a) \\
&\leq 0
\end{aligned}
$$

上式仅在最优行为上取等号，因此这样的学习算法可以避免很多数值上的问题。

更进一步，我们可以将势函数定义为(随机)初始化时的最优状态价值函数：

$$
\psi(s) = \max_a Q_0(s, a)
$$

此时的Q-learning算法同样能够收敛都最优价值函数$$Q^*$$。

<div align=center>
<img src="https://i.imgur.com/hIvLgZo.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/6sBe1In.png" width="80%">
</div>