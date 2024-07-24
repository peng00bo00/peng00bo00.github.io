---
layout: post
title: OMSCS-ML课程笔记11-Clustering
date: 2021-10-11
description: 聚类算法
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍无监督学习中聚类算法的相关内容。
{: .block-preface }


从本节开始我们正式进入**无监督学习(unsupervised learning)**的范畴。在监督学习中每个给定的数据点$$x$$都具有对应一个标签$$y$$，我们的目标是从数据中学习到一个函数$$f$$进而推广到全体数据上进行预测；而在无监督学习中我们只具有数据点，我们的目标则是挖掘数据之间的关系。因此在某种意义上监督学习类似于函数拟合，而无监督学习则是对数据进行建模。

**聚类(clustering)**是无监督学习中的基本问题之一。在聚类问题中我们需要把给定的样本划分到不同的簇中，使得相似的样本位于同一个簇。具体来说，对于给定的数据集$$X$$和距离度量函数$$d(x, y)$$，聚类算法的目标是获得一个划分函数$$P_d(x)$$使得位于同一个簇的样本$$x$$和$$y$$具有相同的划分$$P_d(x) = P_d(y)$$。

## Single Linkage Clustering

**Single linkage clustering(SLC)**是典型的层次聚类算法，它的基本流程如下：

1. 将数据集中每个样本都视为一个聚类中心，得到$$n$$个簇；
2. 将距离最近的两个簇合并成一个新的簇，同时更新其它簇和它的距离$$D(X, Y) = \min_{x \in X, y \in Y} d(x, y)$$，即簇$$X$$和簇$$Y$$的距离为它们两个内部点距离的最小值；
3. 重复第2步直到数据集上的簇数量满足要求。

<div align=center>
<img src="https://upload.wikimedia.org/wikipedia/commons/4/43/Simple_linkage-5S.svg" width="50%">
</div>

SLC有很多有趣的性质：首先SLC是一个确定性的算法，当输入数据集确定时SLC的输出是确定的；其次，如果我们把数据看做是一张图同时把数据之间的距离看做是连接节点的边，SLC算法实际上等价于在图上构造一棵最小生成树。SLC的主要缺陷在于它的复杂度，对于包含$$n$$个样本的数据集它的时间复杂度为$$O(n^3)$$：我们需要$$O(n)$$次循环，同时每个循环需要计算$$O(n^2)$$次距离。

SLC的另一个缺陷在于它忽视了数据之间的结构。以下图为例，假设我们需要将(a)图所示的数据点划分为2类，一个直观的聚类结果是(b)图：左边的圆是一类而右边的把手部分是一类；但使用SLC算法则会得到(c)图：这是由于圆边上的距离都要小于圆的边到内部点的距离，因此在聚类时圆边上的点都会划分为一类并与右边把手相连，而剩下的圆内部点则会划分为另一类。

<div align=center>
<img src="https://i.imgur.com/G9jl8aY.png" width="90%">
</div>

## k-Means

使用k-Means算法则可以避免上述问题，它的主要流程如下：

1. 在数据集上随机选择$$k$$个点作为聚类中心；
2. 计算其它点到这些聚类中心的距离，并划分到距离最小的那个簇中；
3. 将每个聚类中心更新为簇内数据点的平均值；
4. 重复2-3步直到聚类中心不再变化。

通过合理选择初始聚类中心以及距离度量函数，使用k-Means算法可以下图所示的聚类结果。

<div align=center>
<img src="https://i.imgur.com/Yv0E87i.png" width="70%">
</div>

显然初始聚类中心的选取对于k-Means算法的结果有着重要的影响。一个好的初值可以加速算法的收敛过程，而不好的初值则可能会使算法收敛到局部最优。因此k-Means算法在实际应用中往往会搭配随机重启并选择聚类结果最好的作为最终的输出，或是在运行算法前对数据的分布进行分析并手动指定初始聚类中心。

从优化的角度来看，k-Means可以被建模为如下的优化问题：记第$$t$$次迭代样本$$x$$的划分为$$P^t(x)$$，第$$i$$个簇的中心为$$c_i^t$$，簇内样本集合为$$C_i^t = \{ x: P^t(x) = i \}$$。聚类的目标函数可以定义为：

$$
\min \sum_{i=1}^k \sum_{P(x) = i} \Vert x - c_i \Vert^2
$$

k-Means算法使用迭代更新的方式来求解这个优化问题：

$$
P^t(x) = \arg \min_i \Vert x - c_i^{t-1} \Vert^2
$$

$$
c_i^t = \frac{\sum_{x \in C_i^t} x}{\vert C_i^t \vert}
$$

在每次更新中，样本$$x$$到最近聚类中心的距离都会减小；同时在划分确定时，把聚类中心更新为样本均值会减小每个簇内的误差函数。因此在更新过程中总体误差函数不会增大，这说明k-Means算法总是可以收敛的。当然在最差的情况下我们需要枚举所有的划分算法才能收敛，此时的迭代次数为$$O(k^n)$$。

总结一下，k-Means算法具有如下特点：

1. k-Means算法在每次迭代的时间复杂度是$$O(kn)$$；
2. 在每次迭代中k-Means算法的总体误差不会增加，因此k-Means总是可以收敛；
3. 在最差情况下k-Means算法需要$$O(k^n)$$次迭代才能收敛；
4. k-Means算法可能会收敛到局部最优。

k-Means算法的缺陷在于它会倾向于构造出"圆形"的簇。以下图为例，k-Means算法会把数据集划分为上下两部分而不是内外两部分。这是由于我们使用了欧式空间中的距离作为度量而忽视了数据分布的结构。

<div align=center>
<img src="https://i.imgur.com/zRgtwbk.png" width="50%">
</div>

## Soft Clustering

前面介绍的两种聚类算法是将每个样本划分到唯一确定的簇中，但在有些情况下这样的做法是不合适的。假设我们已经有两个簇，此时再两个簇的中间再添加一个新的数据如下图所示。显然将该点划分到任何一个簇都是不合适的，因为它到两个簇的距离相等。更合理的做法是把这个点同时分配到两个簇中，此时它对两个簇的贡献分别是0.5。

<div align=center>
<img src="https://i.imgur.com/kRFSIET.png" width="50%">
</div>

从上面的例子可以发现soft clustering的基本思想是把概率引入到聚类算法中，每个数据点都包含一个概率分布对应$$k$$个不同的簇。更进一步，我们可以假定数据集是从$$k$$个正态分布中采样得到的，每个正态分布对应一个概率来表示从这个分布中进行抽样。我们的目标是从数据中学习到这$$k$$个正态分布的参数以及它们对应的先验概率。

值得注意的是我们不知道每个数据点是从哪个正态分布中抽样得到的，因此这些变量也被称为**隐变量(hidden variable)**。对于包含隐变量的问题我们可以使用**EM算法**来进行求解，它包含E步和M步两个更新过程：

$$
E(z_{ij}) = \frac{P(x_i \vert \mu=\mu_j)}{\sum_{j=1}^k P(x_i \vert \mu=\mu_j)}
$$

$$
\mu_j = \frac{\sum_i E(z_{ij}) x_i}{\sum_i E(z_{ij})}
$$

其中$$z_{ij}$$表示样本$$x_i$$来自于第$$j$$个分布的概率；$$P(x_i \vert \mu=\mu_j)$$为$$x_i$$从第$$j$$个分布进行采样的概率。因此EM算法的E步实际上是将样本$$x_i$$按照概率分配到不同的分布中，然后在M步根据每个样本的权重重新估计每个分布对应的参数。

EM算法的本质对存在隐变量的数据进行极大似然估计，在每次迭代时首先对隐变量进行估计然后再更新参数。可以证明EM算法可以保证每次迭代后数据集上的似然函数都会递增，因此EM算法能够保证收敛但无法保证会收敛到全局最优。同时EM算法不局限于正态分布的情况，它可以拓展到任意的概率分布只要它能够进行求解。

## Clustering Properties

对于聚类算法我们希望它具有如下的一些性质：

- Richness：对于任意可行的划分$$P$$总是存在一个距离函数$$d$$使聚类算法的输出满足要求；
- Scale-invariance：对距离函数进行缩放不会改变聚类结果，即$$P_d = P_{\alpha d}, \forall \alpha \gt 0$$；
- Consistency：对数据集在任意方向上进行拉伸或是压缩不会改变聚类的结果。

然而遗憾的是任何聚类算法都不可能同时具有这三条性质。实际上Kleinberg在理论上证明了不存在能够同时满足这三条性质的聚类算法，这个定理称为Impossibility Theorem。

## Reference

- 第14章：聚类方法，统计学习方法（第2版）
- [Wikipedia: Single-linkage clustering](https://en.wikipedia.org/wiki/Single-linkage_clustering)
- [Wikipedia: k-means clustering](https://en.wikipedia.org/wiki/K-means_clustering)
- 第9章：EM算法及其推广，统计学习方法（第2版）
- [Wikipedia: Expectation–maximization algorithm](https://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm)
- [An Impossibility Theorem for Clustering](https://www.cs.cornell.edu/home/kleinber/nips15.pdf)