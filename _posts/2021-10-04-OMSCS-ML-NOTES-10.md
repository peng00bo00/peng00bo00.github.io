---
layout: post
title: OMSCS-ML课程笔记10-Randomized Optimization
date: 2021-10-04
description: 随机优化
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍随机优化的相关内容。
{: .block-preface }


机器学习中的很多算法都可以抽象成一个优化问题。对于连续型变量的优化问题，我们可以通过梯度下降、牛顿法等算法来高效的求解。但对于离散变量的优化问题我们无法计算导数，因此不能使用这些算法。本节将介绍一些常用的随机优化算法来求解这样的离散优化问题，为便于讨论这里均假定带求解的问题是一个最大化问题。

## Hill Climbing

Hill Climbing是最简单的优化算法。假设我们当前位于$$x$$处，hill climbing算法会搜索$$x$$附近的所有点并移动到函数$$f$$取最大值的位置，如果$$x$$周围的其它点函数值都小于当前点则认为已经找到了最大值。

显然hill climbing算法不能处理局部极值的情况，因此在实际应用中往往会结合随机重启来避免局部极值点。此时算法会执行若干次标准hill climbing，每次找到极值点就会记录当前位置并重置初始位置。算法最终返回的结果为这些记录中的最大值。这样的算法也称为random hill climbing算法。

## Simulated Annealing

**模拟退火(simulated annealing)**是另一种常用的优化算法。与hill climbing算法相比，模拟退火会尝试跳出当前点的邻域从而避免陷入局部极值的问题。模拟退火的算法框架如下：

1. 初始化当前位置$$x = x_0$$；
2. 更新当前温度$$T = \alpha T$$；
3. 选择一个随机邻居$$x'$$并计算$$f(x')$$：；
   1. 如果$$f(x') \gt f(x)$$则更新当前位置$$x = x'$$；
   2. 否则按概率$$p = \exp \{ \frac{f(x') - f(x)}{T} \}$$更新当前位置$$x = x'$$；
4. 返回第2步直至满足算法终止条件。

在开始的循环中温度$$T$$比较大，此时转移概率$$p$$接近于1使得算法倾向于在状态空间中进行随机跳跃来寻找最优点。而随着循环次数的增加，转移概率$$p$$由函数差值$$f(x') - f(x)$$控制，仅在函数值有较大变化时才会进行跳转，此时算法会更接近于在当前点附近进行搜索。

模拟退火算法的结果与初始值$$x_0$$无关，而且当退火时间足够长的情况下会以概率1收敛到全局最大值。

## Genetic Algorithms

**遗传算法(genetic algorithms)**是一种经典的优化算法，它的思想来源于生物种群的演化过程。遗传算法的基本框架如下：

1. 初始化种群中的个体数量；
2. 计算每个个体的适应性；
3. 选择适应性较高的一部分个体进行交配(crossover)，得到子代个体；
4. 返回第2步直至满足算法终止条件。

从算法框架中不难发现遗传算法的核心在于选择适应性较高的个体并进行交配这一步。在实践中的常用做法是将种群中每个个体的适应性转换为概率并进行抽样作为亲代，同时对亲代的状态向量$$x$$进行随机组合来获得子代。此外对于子代的状态向量往往还会进行随机突变来提高种群中个体的多样性。

遗传算法在大多数情况下都能获得比较合理的解，但它的难点一般在于如何将问题建模成遗传算法可以求解的形式。

## MIMIC

本节课最后介绍了MIMIC算法。和前面介绍过的算法相比MIMIC算法不是直接进行优化，而是将优化问题转化为概率密度进行建模。我们可以定义如下的概率密度函数：

$$
p^\theta (x) = 
\left\{
\begin{aligned}
& \frac{1}{z_\theta}， & & f(x) \gt \theta \\
& 0 ，& & \text{otherwise}
\end{aligned}
\right.
$$

其中$$z_\theta$$为归一化因子。当$$\theta$$取函数$$f(x)$$的最大值时$$p^\theta (x)$$退化为仅在最大值点取值的均匀分布，而当$$\theta$$取$$f(x)$$的最小值时$$p^\theta (x)$$则是在所有可能状态上的均匀分布。因此我们可以通过对$$p^\theta (x)$$进行更新来求解最优化问题，MIMIC算法的整体框架如下：

1. 从概率分布$$p^{\theta_t} (x)$$中对$$x$$进行采样；
2. 保留$$f(x)$$较大的样本并利用它们估计一个新的分布$$p^{\theta_{t+1}} (x)$$；
3. 返回第1步直至满足算法终止条件。

不难发现MIMIC算法的核心在于第二步从样本中估计分布$$p^{\theta_{t+1}} (x)$$，而这一步可以通过贝叶斯网络来实现。根据链式法则高维随机变量的联合概率密度可以表示为：

$$
P(X_1, X_2, ... , X_n) = P(X_1 \vert X_2, ..., X_n) \cdot P(X_2, \vert X_3, ..., X_n) \cdots P(X_{n-1} \vert X_n) \cdot P(X_n)
$$

上式说明高维随机变量可以用一个稠密连接的贝叶斯网络来表示。我们假设这样的贝叶斯网络是一个树形结构，每个随机变量只有一个父节点，此时联合概率可以化简为：

$$
P(X_1, X_2, ... , X_n) = \prod_{i=1}^n P(X_i \vert \pi(X_i))
$$

其中$$\pi(X_i)$$表示节点$$X_i$$的父节点。这样概率密度的估计问题就转换为贝叶斯网络的结构学习问题，我们希望能从给定的样本上学习到树形结构和相应的条件概率。

记样本的真实分布为$$P$$，树形贝叶斯网络定义的分布为$$\hat{P}_\pi$$。我们可以通过最小化$$P$$和$$\hat{P}_\pi$$的KL散度来实现结构学习：

$$
\begin{aligned}
D_{KL} (P \Vert \hat{P}_\pi) &= \sum_x P(x) \log \bigg( \frac{P(x)}{\hat{P}_\pi} \bigg) =  \sum_x P(x) \big( \log P(x) - \log \hat{P}_\pi \big) \\
&= -H(P) - \sum_x P(x) \log \hat{P}_\pi \\
&= -H(P) - \sum_x P(x) \log \prod_{i=1}^n p(x_i \vert \pi(x_i)) \\
&= -H(P) - \sum_x \sum_{i=1}^n P(x) \log p(x_i \vert \pi(x_i)) \\
&= -H(P) + \sum_{i=1}^n H(X_i \vert \pi(X_i))
\end{aligned}
$$

由于第一项$$-H(P)$$与$$\pi$$无关，我们可以将它省略得到最小化目标函数：

$$
\min_\pi J_\pi = \sum_{i=1}^n H(X_i \vert \pi(X_i))
$$

为了便于推导，我们在$$J_\pi$$基础上添加一项得到新的目标函数：

$$
\min_\pi J'_\pi = \sum_{i=1}^n H(X_i \vert \pi(X_i)) - \sum_{i=1}^n H(X_i)
$$

其中新增的一项$$-\sum_{i=1}^n H(X_i)$$与$$\pi$$无关，因此不会改变最终$$\pi$$的取值。再结合条件熵的性质可以得到：

$$
\begin{aligned}
\min_\pi J'_\pi &= \sum_{i=1}^n H(X_i \vert \pi(X_i)) - \sum_{i=1}^n H(X_i) \\
&= - \sum_{i=1}^n \bigg( H(X_i) - H(X_i, \pi(X_i)) \bigg) \\
&= - \sum_{i=1}^n I(X_i ; \pi(X_i))
\end{aligned}
$$

我们最后把最小化问题取负号转换成最大化问题，得到最终的目标函数形式：

$$
\begin{aligned}
\max_\pi -J'_\pi = \max_\pi \sum_{i=1}^n I(X_i ; \pi(X_i))
\end{aligned}
$$

上式说明我们可以通过最大化节点之间的互信息来完成对概率密度的估计。具体而言我们首先需要将所有的节点连接起来形成一个全连接图，图上边的权重为两端节点的互信息；然后在图上构造一颗最大生成树就得到了所需的树形结构，一般可以使用[Prim算法](https://en.wikipedia.org/wiki/Prim%27s_algorithm)来实现；得到网络结构后再对网络的边进行参数估计就得到了完整的概率密度函数。当我们需要从学习到的概率分布进行采样时，我们只需要从根节点出发每次采样一个子节点直到所有节点都进行采样即可。

和前面介绍的算法相比，MIMIC算法比较适合具有一定结构的问题。这里的「结构」指的是不同维度的特征存在一些依赖关系，同时优化函数也更关注于这类特征的依赖关系而不是具体的取值。在这样的情况下MIMIC算法一般会得到比较好的解。同时需要说明的是由于MIMIC算法需要估计概率密度函数，一般情况下它需要更多的计算资源。但是相对的，如果计算优化函数$$f(x)$$的代价比较大，MIMIC算法则可以避免反复计算$$f(x)$$从而提升求解效率。

## Reference

- [Wikipedia: Hill climbing](https://en.wikipedia.org/wiki/Hill_climbing)
- [Wikipedia: Simulated annealing](https://en.wikipedia.org/wiki/Simulated_annealing)
- [Wikipedia: Genetic algorithm](https://en.wikipedia.org/wiki/Genetic_algorithm)
- [MLROSE.Algorithms](https://mlrose.readthedocs.io/en/stable/_modules/mlrose/algorithms.html)