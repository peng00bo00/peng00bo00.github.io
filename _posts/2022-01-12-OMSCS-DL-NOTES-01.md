---
layout: post
title: OMSCS-DL课程笔记01-Linear Classifiers and Gradient Descent
date: 2022-01-12
description: 线性分类器与梯度下降法
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要回顾线性分类器以及梯度下降法在机器学习中的应用。
{: .block-preface }


## Machine Learning Overview

**机器学习(machine learning, ML)**是目前整个计算机科学中最为火热的研究领域之一。和传统的算法研究相比，机器学习的特点是算法会在和数据进行交互的过程中不断提升自身的性能。

<div align=center>
<img src="https://i.imgur.com/JQjtSas.png" width="80%">
</div>

同时，对于很多问题我们可能很难直接设计算法进行求解，有时甚至很难去精确地定义问题。在这种情况下，我们就可以利用机器学习这种数据驱动的方式来进行求解。

<div align=center>
<img src="https://i.imgur.com/4OAttGo.png" width="80%">
</div>

从研究领域来划分，机器学习属于广义**人工智能(artificial intelligence, AI)**的一个子集，而**深度学习(deep learning)**则是机器学习的一个子集。

<div align=center>
<img src="https://i.imgur.com/5sWRDGQ.png" width="80%">
</div>

目前，基于深度学习的机器学习技术已经成为整个学术界研究的主流方法。同时深度学习在计算机视觉**(computer vision, CV)**、**自然语言处理(natural language processing, NLP)**、**决策科学(decision making)**、**机器人系统(robotics)**等诸多领域中都取得了大量的应用。

<div align=center>
<img src="https://i.imgur.com/C19nbnN.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/Et3Fcll.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/KWiO8WR.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/IBQ1nB9.png" width="80%">
</div>

## Supervised Learning and Parametric Models

根据提供数据的类型，机器学习大体可以分为**监督学习(supervised learning)**、**无监督学习(unsupervised Learning)**以及**强化学习(reinforcement Learning)**三种。

<div align=center>
<img src="https://i.imgur.com/Jtjcx6p.png" width="80%">
</div>

其中监督学习是指对于给定的数据$$X$$和标签$$Y$$，算法通过不断的学习输出一个函数$$f$$将$$X$$映射为$$Y$$。基于深度学习的监督学习算法已经取得了令人瞩目的成就，因此在本课程也主要会介绍深度学习在监督学习中的应用。

<div align=center>
<img src="https://i.imgur.com/4NEaFSM.png" width="80%">
</div>

### Supervised Learning

根据模型是否具有参数，监督学习还可以细分为参数化模型和非参数化模型两类。前者的代表是**逻辑回归(logistic regression)**和**神经网络(neural networks)**，而后者的代表则包括**决策树(decision tree)**和**kNN(k nearest neighbor)**。

<div align=center>
<img src="https://i.imgur.com/teS9jEN.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/4qntRan.png" width="80%">
</div>

### Traditional Machine Learning

以图像分类为例，在深度学习诞生之前的经典分类算法是利用一个参数模型$$f(x, W)$$将输入图像映射为一个类别向量，然后去得分最高的那个维度作为图像的分类结果。

<div align=center>
<img src="https://i.imgur.com/QRV908Q.png" width="80%">
</div>

由于图像是一个高维的数组，直接带入模型进行训练的效率非常低，因此往往会利用人工设计的图像特征(如直方图)来表示整张图像。这样整个计算流程可以划分为特征工程和分类模型两部分。

<div align=center>
<img src="https://i.imgur.com/O359SH1.png" width="80%">
</div>

## Components of Parametric Learning

在介绍如何训练模型前我们首先来观察一下模型的构成，整个参数模型可以分成**输入(input)**、**模型(model)**、**损失函数(loss function)**以及**优化算法(optimization algorithm)**几部分。对于图像分类问题，参数化模型的输入就是图像的**特征(features)**。

<div align=center>
<img src="https://i.imgur.com/ZNEQkXM.png" width="80%">
</div>

对于线性模型，图像的输出类别向量是特征的线性组合，组合系数为可学习参数$$W, b$$。

<div align=center>
<img src="https://i.imgur.com/ZBsw9Af.png" width="80%">
</div>

从几何的角度上讲，每个类别的参数$$W_i, b_i$$相当于在高维空间中定义了一个**超平面(hyperplane)**来将空间划分为正反两面，其中正确类别的样本需要被分类到正面而其它类别的样本则需要分类到背面。

<div align=center>
<img src="https://i.imgur.com/3InHkYk.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/XfDi8nu.png" width="80%">
</div>

当然在有些情况下样本可能无法通过线性分类器来正确划分，这时则需要我们引入一些非线性的机制来进行处理。

<div align=center>
<img src="https://i.imgur.com/5rHsTGC.png" width="80%">
</div>

## Performance Measurement

对于任意的模型我们还需要考虑一个合适的度量函数来描述模型的性能，这样的函数称为**性能度量(performance measurement)**。同时注意到分类器输出的得分往往是无界的，我们还需要将得分转换成规范化的形式。对于分类问题最常用的方法是借助于softmax()函数，它将每个类别上的得分转换为相应的概率。

<div align=center>
<img src="https://i.imgur.com/fOSohWW.png" width="80%">
</div>

对得分进行规范化后就可以利用度量函数来描述当前模型的输出与目标输出之间的差异，然后利用优化算法来进行求解。具体地，我们希望模型能够在正确的类别上有着足够高的概率同时在其他类别上的概率尽可能低。从优化的角度上讲我们需要最小化度量函数，因此度量函数往往也被称为**目标函数(objective function)**或是**损失函数(loss function)**，记为$$L$$。

<div align=center>
<img src="https://i.imgur.com/hvrq6Fm.png" width="80%">
</div>

### Cross Entropy

对于使用softmax()函数规范化的输出，我们可以使用**交叉熵(cross entropy)**来描述输出概率与目标概率之间的差异，它相当于度量两个概率分布之间的"距离"。

<div align=center>
<img src="https://i.imgur.com/FGQAyam.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/zrV5Ee6.png" width="80%">
</div>

### Hinge Loss

另一种常用的损失函数是hinge loss，它要求正样本与决策边界需要保持足够大的间隔。实际上SVM就使用了hinge loss作为损失函数。

<div align=center>
<img src="https://i.imgur.com/NamDm4y.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/dPG8Y4G.png" width="80%">
</div>

除了cross entropy和hingeloss之外，对于回归问题还可以考虑使用L1、L2 loss或是logistic loss等不同类型的损失函数。

<div align=center>
<img src="https://i.imgur.com/rpyyvUT.png" width="80%">
</div>

### Regularization

在标准的损失函数基础上我还往往会添加**正则项(regularization)**来避免模型出现过拟合的问题。常用的正则项包括L1、L2正则，它们会约束参数的范围，使模型倾向于学习到较小的参数值。

<div align=center>
<img src="https://i.imgur.com/914vwM2.png" width="80%">
</div>

## Gradient Descent

对于给定的模型和损失函数，模型的训练过程可以看成是一个寻找最优参数的优化问题。在机器学习中求解这种优化问题可以考虑使用随机搜索、遗传算法等不同类型的算法，但目前主流的优化算法仍然是基于**梯度下降(gradient descent)**的相关方法。简单来说，在梯度下降中我们把损失函数看做是关于模型参数的函数，然后通过沿当前点的负梯度方向前进从而寻找到最优点。

<div align=center>
<img src="https://i.imgur.com/H3g7POO.png" width="80%">
</div>

梯度下降方法的总体流程如下：

<div align=center>
<img src="https://i.imgur.com/YMyZ3ag.png" width="80%">
</div>

在计算梯度值时有时还会使用数据集上的全部或是一部分样本，此时的梯度下降也称为**full batch gradient descent**和**mini-batch gradient descent**。

<div align=center>
<img src="https://i.imgur.com/oZ56R97.png" width="80%">
</div>

还需要说明的是要保证梯度下降能够收敛需要使用比较小的步长，而且梯度下降是不能保证收敛到全局最优的。不过在实践中发现即使收敛到局部最优，训练得到的模型也往往有足够好的效果。

<div align=center>
<img src="https://i.imgur.com/HNItfCk.png" width="80%">
</div>

那么梯度下降最终的问题是如何计算损失函数关于参数的导数。显然显式计算过于复杂但使用数值梯度精度又不够高，因此在深度学习框架中使用了链式法则来逐步计算梯度。对于任意可导的函数，它可以分解成一系列比较简单的可导函数的复合。这样在计算梯度时可以从后向前计算每一个可导函数的梯度，然后利用链式法则乘起来就得到了复合函数的梯度。

<div align=center>
<img src="https://i.imgur.com/79VOmCa.png" width="80%">
</div>

## How is Deep Learning Different?

本节课最后讨论了深度学习与传统机器学习的差异。目前来看，深度学习的主要特点在于**层级化(hierarchical compositionality)**、**端对端(end-to-end learning)**和**分布式表示(distributed representations)**。

<div align=center>
<img src="https://i.imgur.com/TOELRrw.png" width="80%">
</div>

### Hierarchical Compositionality

前面介绍过深度学习会将一系列简单的可导函数复合成一个复杂的函数。

<div align=center>
<img src="https://i.imgur.com/WZtuemI.png" width="80%">
</div>

这些简单的函数并不是相互独立的单元，实际上这些函数存在着层级的关系。以图像识别为例，通过可视化单元响应人们发现浅层的单元对应着浅层的图像信息，而后面更深层的单元则对应更高阶的视觉信息。

<div align=center>
<img src="https://i.imgur.com/3DpDJmF.jpg" width="80%">
</div>

### End-to-End Learning

深度学习的另一个重要特征是整个学习过程是端对端的。在传统机器学习中需要手动设计人工特征来作为模型的输入，而在深度学习中我们直接使用原始数据，即特征提取器会和模型会一起进行训练。

<div align=center>
<img src="https://i.imgur.com/6mfaFfL.png" width="80%">
</div>

### Distributed Representations

深度学习最后一个特点是模型学习到的是分布式的表示。每个单元不会学习到某个特征全部的信息，我们需要把所有的单元组合到一起才能得到特征的完整表示。

<div align=center>
<img src="https://i.imgur.com/5leFsng.png" width="80%">
</div>