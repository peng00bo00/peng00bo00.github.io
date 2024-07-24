---
layout: post
title: OMSCS-ML课程笔记09-Information Theory
date: 2021-10-04
description: 信息论
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍信息论的相关内容。
{: .block-preface }


## Entropy

信息量的基本度量是**信息熵(information entropty)**，对于离散随机变量$$X$$我们定义它的熵为：

$$
H(X) = \mathbb{E}_X [I(X)] = -\sum_{x \in X} p(x) \log p(x)
$$

熵度量了随机变量的随机程度：$$X$$的分布越均匀对应的熵越大，而当$$X$$的分布越集中对应的熵越小。当$$X$$退化为确定的变量时我们定义它的熵为0。

除此之外我们还可以从编码长度来理解信息熵。对于随机变量$$X$$的所有取值我们可以使用霍夫曼编码对它进行编码，概率为$$p(x)$$的取值对应的编码长度为$$-\log p(x)$$，而$$X$$的平均编码长度即为$$H(X) = -\sum_{x \in X} p(x) \log p(x)$$。因此信息熵给出了对随机变量进行无损编码的最小平均长度。

在信息熵的基础上我们可以定义两个离散随机变量的**联合熵(joint entropy)**：

$$
H(X, Y) = \mathbb{E}_{X, Y} [- \log p(x, y)] = -\sum_{x, y} p(x, y) \log p(x, y)
$$

同时已知$$Y$$条件下$$X$$的熵定义为**条件熵(conditional entropy)**：

$$
\begin{aligned}
H(X \vert Y) = \mathbb{E}_Y [H(X \vert Y)] &= -\sum_y p(y) \sum_x p(x \vert y) \log p(x \vert y) \\
&= -\sum_{x, y} p(x, y) \log p(x \vert y) \\
&= H(X, Y) - H(Y)
\end{aligned}
$$

## Mutual Information

对于随机变量$$X$$和$$Y$$我们可以定义它们之间的互信息为：

$$
I(X; Y) = \sum_{x, y} p(x, y) \log \frac{p(x, y)}{p(x) p(y)}
$$

互信息度量了随机变量$$X$$和$$Y$$的独立性质，当且仅当$$X$$和$$Y$$相互独立时它们的互信息为0。此外，互信息可以从$$X$$和$$Y$$的联合熵以及条件熵推导出来：

$$
\begin{aligned}
I(X; Y) &= I(Y; X) \\
&= H(X) - H(X \vert Y) = H(Y) - H(Y \vert X) \\
&= H(X) + H(Y) - H(X, Y)
\end{aligned}
$$

<div align=center>
<img src="https://upload.wikimedia.org/wikipedia/commons/d/d4/Entropy-mutual-information-relative-entropy-relation-diagram.svg" width="40%">
</div>

## Kullback–Leibler Divergence

本节最后介绍了**KL散度(Kullback–Leibler divergence)**，它可以用来度量概率分布$$P$$和$$Q$$之间的距离：

$$
\begin{aligned}
D_{KL} (P \Vert Q) &= \sum_x P(x) \log \bigg( \frac{P(x)}{Q(x)} \bigg) \\
&= -\sum_x P(x) \log Q(x) + \sum_x P(x) \log P(x)
\end{aligned}
$$

其中第一项称为$$P$$和$$Q$$的**交叉熵(cross entropy)**，第二项是$$P$$的信息熵。

结合互信息不难发现，互信息是联合概率与独立概率之间的KL散度。从这个角度看互信息实际上度量了$$X$$和$$Y$$的联合概率分布于它们独立分布之间的差异。

最后需要说明的是严格来说KL散度并不是一个距离度量，它不满足交换性：

$$
D_{KL} (P \Vert Q) \neq D_{KL} (Q \Vert P)
$$

## Reference

- [Wikipedia: Information theory](https://en.wikipedia.org/wiki/Information_theory#Quantities_of_information)
- [Wikipedia: Mutual information](https://en.wikipedia.org/wiki/Mutual_information)
- [Wikipedia: Kullback–Leibler divergence](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)