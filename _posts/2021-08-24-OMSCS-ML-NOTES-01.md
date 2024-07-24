---
layout: post
title: OMSCS-ML课程笔记01-Decision Trees
date: 2021-08-24
description: 决策树
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍监督学习中的决策树相关内容。
{: .block-preface }


## Representation

**决策树(Decision Tree)**是应用最为广泛的机器学习方法之一，通常用于离散变量的分类问题。日常生活中的很多分类问题都可以通过一系列的判断来解决，此时我们的决策过程就可以利用决策树来表示。比如说我们通过天气的状况来判断是否要出去，此时的决策过程就对应着一个决策树：

<div align=center>
<img src="https://i.imgur.com/Lk80vLQ.png" width="50%">
</div>

除此之外决策树可以看做是一系列的if-then规则：从根节点出发的每一条路径都对应着一个决策规则，而叶节点对应规则的结论。对于分类问题我们从根节点出发每次选择一个属性进行判断，然后不断向下移动直到返回所需的结果。

## Decision Tree Learning

决策树算法的核心在于如何从数据中构建出一个符合要求的决策树。通常情况下我们会使用自上而下的构造方法：选择当前数据集上的最优特征作为决策树的根节点，然后利用根节点的取值将数据集划分为不同子集，最后在每个子集上重新构建新的决策树作为根节点的子节点。不难发现如何取选择最优特征对决策树的构建起着重要的作用，而不同类型的决策树算法主要的差异在就于对特征的选取。

### Information Gain

在ID3算法中使用**信息增益(information gain)**来进行特征选择。要介绍信息增益首先需要引入**信息熵(entropy)**的概念，对于离散型随机变量$$X$$定义它具有的熵$$H(X)$$为：

$$
H(X) = -\sum_{i=1}^m p_i \log p_i
$$

其中$$p_i$$表示$$X$$不同取值对应的概率。信息熵描述了随机变量的混乱程度，可以证明当随机变量为均匀分布时信息熵最大。

接着我们定义随机变量$$Y$$在已知$$X$$的情况下对应的**条件熵(conditional entropy)**$$H(Y \vert X)$$为：

$$
\begin{aligned}
H(Y|X) &= \sum_{i=1}^m P(x_i) H(Y| X = x_i) \\
&= -\sum_{i=1}^m P(x_i) \sum_{j=1}^n P(y_j | x_i) \log P(y_j | x_i)
\end{aligned}
$$

最后我们定义信息增益$$g(Y, X)$$为熵与条件熵之差：

$$
g(Y, X) = H(Y) - H(Y|X)
$$

从信息论的角度出发，信息增益实际上是随机变量$$X$$和$$Y$$的**互信息(mutual information)**。我们定义互信息为$$X$$和$$Y$$的联合概率与它们边缘概率的KL散度：

$$
\begin{aligned}
I(X; Y) &= D_{KL}(P(X, Y) \Vert P(X)P(Y)) \\
&= \sum_{i=1}^m \sum_{j=1}^n P(x_i, y_j) \log \Big( \frac{P(x_i, y_j)}{P(x_i) P(y_j)} \Big) \\
&= \sum_{i=1}^m \sum_{j=1}^n P(x_i, y_j) \log \Big( \frac{P(x_i, y_j)}{P(x_i)} \Big) - \sum_{i=1}^m \sum_{j=1}^n P(x_i, y_j) \log{P(y_j)} \\
&= \sum_{i=1}^m P(x_i) \sum_{j=1}^n P(y_j | x_i) \log P(y_j | x_i) - \sum_{j=1}^n P(y_j) \log{P(y_j)} \\
&= H(Y) - H(Y|X)
\end{aligned}
$$

利用Venn图可将互信息与熵的关系表示如下：

$$
\begin{aligned}
I(X; Y) = I(Y; X) &= H(Y) - H(Y|X) \\
&= H(X) - H(X|Y) \\
&= H(X) + H(Y) - H(X, Y) \\
&= H(X, Y) - H(X|Y)- H(Y|X)
\end{aligned}
$$

<div align=center>
<img src="https://upload.wikimedia.org/wikipedia/commons/d/d4/Entropy-mutual-information-relative-entropy-relation-diagram.svg" width="40%">
</div>

互信息度量了随机变量$$X$$和$$Y$$的相关性，互信息越大表示$$X$$和$$Y$$的相关性越强，且互信息为0等价于$$X$$和$$Y$$相互独立。因此ID3算法使用信息增益进行特征选择实质上是每次选择与类别信息最相关的一个特征对数据进行划分。

### Information Gain Ratio

在C4.5算法使用了**信息增益比(information gain ratio)**来修正信息增益，其定义为信息增益与特征自身信息熵的比值：

$$
g_R(Y, X) = \frac{g(Y, X)}{H(X)}
$$

信息增益比相当于对特征进行了一定的约束，从而防止模型出现过拟合的问题。

### Gini Impurity

另一种常用的特征选择方法是使用**基尼指数(Gini impurity)**，定义随机变量$$X$$的基尼指数为：

$$
\text{Gini}(X) = \sum_{i=1}^m p_i (1-p_i) = 1 - \sum_{i=1}^m p_i^2
$$

类似于信息熵，基尼指数度量了随机变量的「纯净」程度：随机变量的概率分布越均匀基尼指数越大，概率分布越集中基尼指数越小。在进行特征选择时我们定义节点的基尼指数为：

$$
\text{Gini}(X) = \sum_{i=1}^m \frac{\vert X_i \vert}{\vert X \vert} \text{Gini}(Y | X_i)
$$

其中$$\text{Gini}(Y \vert X_i)$$表示节点属性为$$X_i$$的子集上类别$$Y$$对应的基尼指数。与信息增益相反，节点的基尼指数越大表示样本集合的复杂度越大，因此在特征选择时需要选择基尼指数最小的特征。

## Bias

对于给定的数据集可能存在多个不同的模型都能够满足我们的需求，而此时我们的学习算法只能给出一个特定的模型。换句话说我们的学习算法在模型构成的集合中进行了选择，即算法对模型有一定的**偏好(bias)**。

偏好分为2种：restriction bias以及preference bias。其中restriction bias对应模型的**假设空间(hypothesis space)** $$H$$，它代表模型所能表示的范围。以决策树为例，决策树的hypothesis space为给定数据集上所有决策树构成的集合，此时我们并不会考虑其他类型的模型。而preference bias则对应学习算法在假设空间中的偏好：我们可以把学习的过程抽象为在假设空间里进行搜索，此时学习算法会根据自身的preference bias从假设空间中寻找的合适的模型(假设)。

对于决策树的ID3算法，它的restriction bias为节点特征构成的所有决策树，而preference bias则包括：

- 优先选择较短的树(short trees over longer trees)；
- 优先选择靠近根节点且具有最大信息增益的树(trees with highest information gain closest to the root)。

## Other Considerations

### Continuous Attributes

最后我们来讨论一下使用决策树时一些其他的常见问题，首先是对连续型特征的处理。在前面的讨论中我们都假定特征是离散型随机变量，但在实际应用中有大量的连续型特征(年龄、身高、体重等)。决策树算法是不能直接应用在连续型随机变量上的，因此在使用决策树算法前首先需要对连续型随机变量进行离散化。常见的离散化方法是使用一些阈值对变量进行划分，从而将连续型随机变量转换为离散型随机变量；此外也可以结合任务相关的知识(domain knowledge)来进行离散化。

### Stopping Criteria

决策树的构建过程中使用了自上而下递归式的构造方法，因此在构建决策树时需要提供停止递归的条件。最简单的条件是当决策树对当前样本分类全部正确时停止递归，但由于数据集上噪声的存在一般情况下不太可能会通过这个条件来停止递归。另一个终止条件是当我们用尽所有的特征时停止递归，显然此时已经不能继续构建决策树了算法自然会停止。为了避免出现过拟合的问题还可以利用验证集来提前终止递归：在构建决策树的同时在验证集上进行交叉验证，如果模型的性能下降则说明出现了过拟合，此时需要终止程序。在决策树算法中更常用的方法是**剪枝(pruning)**，即利用交叉验证来减去决策树的一些叶节点和子树从而避免模型出现过拟合等问题。

### Regression

决策树也可以用来求解回归问题。我们把决策树看做是对样本空间进行划分，对于回归问题在每个子空间(叶节点)内可以利用样本的均值来作为模型的输出，这个过程相当于在子空间内最小化平方误差。因此使用决策树求解回归问题的核心是如何划分样本空间，也就是如何选择和划分样本的特征。在CART算法中使用了启发式的算法来进行处理：每次构建决策树时对当前数据集中每个特征以及每个取值进行遍历，构造出两个相应的子空间分别计算数据集在子空间内的平方误差，然后选择平方误差最小的特征和取值作为当前数据集的切分点继续向下递归。

## Reference
- Chapter 3: Decision Tree Learning, [Machine Learning, Tom Mitchell, McGraw Hill, 1997.](http://www.cs.cmu.edu/afs/cs.cmu.edu/user/mitchell/ftp/mlbook.html)
- 第5章：决策树，统计学习方法（第2版）
- [Wikipedia: Mutual information](https://en.wikipedia.org/wiki/Mutual_information)