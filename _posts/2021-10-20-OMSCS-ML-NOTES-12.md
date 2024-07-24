---
layout: post
title: OMSCS-ML课程笔记12-Feature Selection and Transform
date: 2021-10-20
description: 特征选择
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍无监督学习中特征选择的相关内容。
{: .block-preface }


## Feature Selection

在机器学习中我们使用数据点的特征来表示实际的样本。显然不同的特征对于机器学习任务的重要性是不同的，因此我们希望能够从这些特征中挑选出特征，这样既可以提升机器学习的效率还能够避免维数灾难等问题。挑选合适特征的过程称为**特征选择(feature selection)**，一般包括两种类型：其一是对所有可能的特征组合进行搜索，称为filtering；而另一种则是由学习器来调整当前的特征集合，称为wrapping。

<div align=center>
<img src="https://i.imgur.com/l6dZdQD.png" width="70%">
</div>

### Filtering

filtering的特点是特征选择的过程与学习器无关，我们只需要找到特征的所有可能组合即可。由于不需要等待学习器，filtering要比wrapping更高效。对于包含$$k$$个特征的数据集，所有特征的可能组合为$$2^k$$。

实际上决策树的训练过程就可以看做是filtering的过程。在训练决策树时，我们每次都从当前的特征集合中挑选出一个最好的特征来对数据集进行划分。当训练完成后所有使用过的特征自然就是选择后的结果。

### Wrapping

wrapping的特点是在每次进行特征选择时需要等待学习器来告诉我们特征的好坏，因此wrapping的速度要比filtering慢一些。

如果我们把学习器给出反馈的过程看做是fitness function，那么wrapping也可以理解成一个随机优化问题：我们需要从所有可能的特征组合中找到一组特征使得学习器具有最好的反馈。在这样的情况下我们可以使用[Randomized Optimization](/2021/10/04/OMSCS-ML-NOTES-10.html)中介绍的算法来对特征进行wrapping。其它常用的wrapping方法还包括**前向搜索(forward search)**和**后向搜索(backward search)**：

- 前向搜索：从空集出发每次挑选一个得分最高的特征；
- 后向搜索：从全体特征出发每次丢掉一个最不相关的特征。

### Relevance and Usefulness

无论是使用filtering还是使用wrapping进行特征选择都需要面对一个问题，那就是如何衡量所选择的特征是否是一个"好"的特征。我们可以使用relevance的概念来描述一个特征$$x$$的好坏：

- 称特征$$x$$是strong relevance的如果移除它后贝叶斯最优分类器的性能会下降；
- 称特征$$x$$是weak relevance的如果把它加入到特征集合$$S$$后能提高贝叶斯最优分类器的性能；
- 否则称特征$$x$$是irrelevance的。

relevance实际上描述了特征带来的信息。对于特定的学习器，我们称特征$$x$$是useful的如果它能够降低学习器的误差。举个简单例子，线性分类器中我们在原始特征的基础上添加了一个常数项来形成新的特征。显然这个常数是irrelevance的，但我们可以把它作为线性分类器偏置项，因此它是useful的。

## Feature Transform

除了特征选择外，在机器学习中另一种常用的特征处理方法是**特征变换(feature transformation)**：我们利用已有的特征来构造出一组新的特征，并在这些新的特征上完成机器学习的任务。我们希望这组新的特征能够包含原始特征的全部信息，同时尽可能紧凑从而避免维数灾难的问题。

显然对特征进行变换有各种各样的方式，但这里我们只考虑对特征进行线性变换。

### PCA

**主成分分析(principal component analysis, PCA)**是线性特征变换的基本方法。在特征空间中我们把数据分布方差较大的方向称为**主成分(principal component)**，而PCA的思想就是把数据投影到不同的主方向上从而在主方向构成的空间中重新表达当前的数据集。

<div align=center>
<img src="https://i.imgur.com/qFBND88.png" width="70%">
</div>

PCA的主要流程如下：

1.  对数据集$$X$$进行中心化处理，使得$$X$$的每个特征均值为0；
2.  构造新的样本矩阵$$X' = \frac{1}{\sqrt{n-1}} X$$；
3.  对$$X'$$进行奇异值分解，得到$$X' = U \Sigma V^T$$；
4.  利用右奇异向量矩阵对$$X$$进行投影即可$$Y = V^T X$$。

主成分分析的另一个重要应用是对数据进行降维。在很多时候我们不需要全部的主成分向量，只需要最重要的几个方向即可。此时我们需要对奇异值从大到小进行排序，并选出其中最大的$$k$$个奇异值和对应的奇异向量方向构造投影矩阵。

### ICA

另一种常用的特征变换方式是**独立成分分析(independent component analysis, ICA)**。假设数据集上的特征为$$X = \{x_1, x_2, \dots \}$$，ICA的目的通过一个线性变换将特征$$X$$变换为$$Y = \{y_1, y_2, \dots \}$$使得任意$$y_i$$和$$y_j$$相互独立，同时可以证明这样的过程等价于最大化$$X$$和$$Y$$之间的互信息$$I(X; Y)$$。

### Alternatives

处理PCA和ICA之外，其它常用的降维算法还包括RCA以及LDA等。

## Reference

- [Wikipedia: Principal component analysis](https://en.wikipedia.org/wiki/Principal_component_analysis)
- [Wikipedia: Independent component analysis](https://en.wikipedia.org/wiki/Independent_component_analysis)
- [Wikipedia: Linear discriminant analysis](https://en.wikipedia.org/wiki/Linear_discriminant_analysis)