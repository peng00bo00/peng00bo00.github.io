---
layout: post
title: OMSCS-ML课程笔记04-Instance Based Learning
date: 2021-09-06
description: 实例学习
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍监督学习中kNN的相关内容。
{: .block-preface }


## Overview

之前在课程中介绍的各种机器学习算法都是需要通过数据学习到一个模型(函数)来完成分类或是回归的任务，与之相对的instance based learning相关的学习算法则没有显式地学习过程。在instance based learning中我们只需要构建出数据库，然后用新的数据在已有数据库中进行查询进而完成分类和回归的任务。

和其它类型的机器学习算法相比，instance based learning相关的算法更简单、效率更高。但由于instance based learning算法极度依赖于已有的数据库，和其他类型的算法相比它的泛化性能更差，更容易收到数据库中噪声的影响，同时也更容易出现过拟合的问题。

## k-NN

instance based learning算法中最出名的是**k临近算法(k nearest neighbor), kNN**。简单来说当我们使用数据$$x$$进行查询时，从数据库中选出k个与$$x$$最接近的样本，然后利用这些样本来求解分类和回归的问题。对于分类问题可以使用这k个样本进行投票，票数最多的类别即为$$x$$的类别；而对于回归问题则可以使用k个样本的平均值来作为$$x$$的预测值。在实际应用中往往还会利用样本到$$x$$的距离进行加权来进一步提升算法的性能。

<div align=center>
<img src="https://i.imgur.com/CmEPS0p.png" width="70%">
</div>

从k-NN算法的流程不难发现如何计算样本间的距离对于算法的性能起着至关重要的作用。常用的距离函数包括欧式距离、Manhattan距离等，在实际应用中一般还会结合一些domain knowledge来设计合适的距离函数。

k值的选择同样会对kNN的结果产生重大影响。当k值比较小时模型会只考虑与输入数据$$x$$相近的样本，此时模型更容易收到数据库中误差的影响造成过拟合的问题。而当k较大时模型会考虑更多的样本，此时与$$x$$差距较大的样本也会对输出的结果产生影响从而产生误差。在k取样本数$$n$$的极端情况下无论什么样输入数据$$x$$模型都会给出相同的答复，此时的模型是没有任何意义的。在实际应用中通常会使用交叉验证的方法来选择最优的k值。

在大规模高维的数据库中对数据$$x$$进行查询会极大地影响k-NN的算法效率。最简单的查询方法是进行线性扫描，即依次计算$$x$$与每一个样本的距离并保留其中最小的k个。显然线性扫描的方法在大规模数据库中会非常低效，在实际应用中更常用的方式是使用**kd树(kd tree)**的数据结构来存储训练数据。当数据均匀分布时可以证明使用kd树进行搜索的平均复杂度是$$O(\log n)$$，远小于线性扫描的复杂度$$O(n)$$。

<div align=center>
<img src="https://i.imgur.com/5ShPChj.png" width="70%">
</div>

## Bias

k-NN的restriction bias由距离函数控制，理论上讲只要可以构造出合适的距离函数k-NN可以应用在任意类型的数据上。

而k-NN的preference bias则包括**局部性(locality)**、**光滑性(smoothness)**以及特征的**同等权重(equality)**。由于k-NN算法需要在数据库中寻找相近的数据并求平均，因此k-NN的结果是局部的而且比较光滑。同时需要指出的是k-NN没有办法区分不同的特征，即所有的特征是同等重要的。当不同维度的特征对目标类别/值的贡献不同时，直接使用k-NN算法可能不会得到理想的结果。

## Curse of Dimensionality

本节最后介绍了**维数灾难(curse of dimensionality)**的问题。维数灾难是指在机器学习中随着数据集上特征数目的增长，要保持模型具有泛化能力所需要的数据量会成指数增长。举个简单的例子，假设我们可以在直线上采样10个点来表示这条直线；而当维数升到2时，我们需要$$10 \times 10 = 100$$个数据点来描述平面；更进一步，在$$d$$维空间中我们需要$$10^d$$个数据点才能描述整个空间。

<div align=center>
<img src="https://i.imgur.com/qMGGWX6.png" width="70%">
</div>

维数灾难表明数据集的特征并不是越多越好，相反当特征的数量增加时我们需要更多的数据才能保证模型的泛化性能。由于k-NN算法不能对特征加以区分，在高维数据集上直接使用k-NN算法很容易出现过拟合的问题。

## Reference

- Chapter 8: INSTANCE-BASED LEARNING, [Machine Learning, Tom Mitchell, McGraw Hill, 1997.](http://www.cs.cmu.edu/afs/cs.cmu.edu/user/mitchell/ftp/mlbook.html)
- 第3章：k邻近法，统计学习方法（第2版）
- [Wikipedia: k-d tree](https://en.wikipedia.org/wiki/K-d_tree)
- [Wikipedia: Curse of dimensionality](https://en.wikipedia.org/wiki/Curse_of_dimensionality#Machine_Learning)