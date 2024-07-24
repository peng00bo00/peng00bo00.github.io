---
layout: post
title: OMSCS-ML课程笔记07-Computational Learning Theory
date: 2021-09-20
description: 计算学习理论
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍监督学习中计算学习理论的相关内容。
{: .block-preface }


## Learning to Learn

到目前为止我们已经介绍了很多类型的机器学习算法，而本节介绍的**计算学习理论(computational learning theory)**不涉及任何具体地算法，而是尝试去解决为什么机器学习是可行的以及我们需要多少样本才能保证学习算法是可靠的这样的问题。

如果将机器学习比作现实生活中的学习过程，那么机器学习算法实际上是使用**自然学习(natural learning)**的模式。在这样的模式下我们只会给学习器提供一系列样本，然后由模型自己来进行学习。我们关心的是需要多少样本才能保证学习器能够学习到正确的假设，称为**样本复杂度(sample complexity)**。

## PAC Learning

### PAC Learnable

计算学习理论的主要成果是**概率近似正确理论(probably approximately correct, PAC)**，基于PAC学习理论我们可以去计算学习算法的样本复杂度。在介绍具体的理论前首先要引入一些相关的概念：假设样本$$X$$都是从某个分别$$D$$中采样得到的；$$D$$可以是任意的分布同时学习器$$L$$无法得知$$D$$的信息；学习器$$L$$的目标是从假设空间$$H$$中学习到正确的概念$$c$$，同时$$c$$存在于假设空间$$H$$中。设学习器学习到的假设为$$h$$，我们定义$$h$$在分布$$D$$上的误差为：

$$
error_D(h) = \underset{x \in D}{Pr} [c(x) \neq h(x)]
$$

需要说明的是学习器$$L$$是从样本$$X$$中进行学习的，因此无法直接计算在分布$$D$$上误差$$error_D(h)$$而只能计算$$h$$在训练数据上的误差。我们把在训练样本$$X$$上与正确的概念$$c$$表现一致的假设$$h$$称为是**一致的(consistent)**，一致假设构成的集合称为**版本空间(version space)**：

$$
VS_{H, X} = \{h \in H \vert h(x) = c(x) , \forall \langle x, c(x) \rangle \in X \}
$$

显然在给定样本$$X$$的条件下可能存在多个一致的假设，它们在训练样本$$X$$上的误差为0。同时由于$$X$$都是从$$D$$中采样得到的，$$X$$自身可能会对学习器产生一些误导使得学习器学到一些不存在于分布$$D$$中的信息。

因此直接学习到正确的假设$$c$$是比较困难的，我们放宽要求只要学习到误差足够小的假设即可。此外我们不要求学习器对于任意采样出来的样本$$X$$都能够进行学习，只要学习失败的概率很小就可以。换句话说，学习器只要能够以一定的概率学习到近似正确的假设即可，这也是PAC学习理论得名的原因。PAC学习严格的定义如下：

> 设正确概念的集合$$C$$，大小为$$n$$的样本集合为$$X$$，学习器为$$L$$，假设空间为$$H$$。称$$C$$是**PAC可学习(PAC-learnable)**的当且仅当$$C$$中的任意正确概念$$c \in C$$能够被学习器$$L$$以概率$$(1-\delta)$$从$$X$$中进行学习，学习到的假设$$h$$在分布$$D$$上的误差满足$$error_D(h) \leq \varepsilon$$，且学习所需的时间为$$\frac{1}{\varepsilon}$$、$$\frac{1}{\delta}$$、$$n$$以及$$\vert c \vert$$的多项式函数。

PAC学习对于学习器$$L$$提出了两个要求：首先学习器要有足够高的成功概率$$(1-\delta)$$以及足够低的误差$$error_D(h) \leq \varepsilon$$；同时学习算法要保证足够的效率，必须是$$\frac{1}{\varepsilon}$$和$$\frac{1}{\delta}$$的多项式函数。

### Sample Complexity

假设学习器处理每个样本所需的时间为常数，那么PAC可学习实际上暗示了学习器的样本复杂度同样是多项式函数，这样我们就把学习算法的时间复杂度转化为样本复杂度。在推导PAC可学习的样本复杂度前我们还需要补充一些相关的知识。对于版本空间$$VS_{H, X}$$，我们称它是$$\varepsilon$$-exhausted如果其中的任意假设$$h$$在分布$$D$$上的误差小于$$\varepsilon$$：

$$
error_D(h) \leq \varepsilon, \ \forall h \in VS_{H, X}
$$

如果假设空间中存在$$k$$个假设，它们的泛化误差都大于$$\varepsilon$$。那么它们中如果有1个位于版本空间$$VS_{H, X}$$中的话，版本空间就不再是$$\varepsilon$$-exhausted。我们设这个泛化误差大于$$\varepsilon$$且位于版本空间的假设为$$h$$，在这样的情况下每个样本从分布$$D$$中抽样出的概率最大为$$(1 - \varepsilon)$$，否则$$h$$就不再是一致假设了。由于训练数据是独立同分布的，抽样出样本集$$X$$的概率最大为$$(1 - \varepsilon)^n$$，也就是说$$h$$在样本集$$X$$上是一致假设的概率最大为$$(1 - \varepsilon)^n$$。由于我们一共有$$k$$个这样假设，它们中至少有一个位于版本空间的概率小于等于$$k(1 - \varepsilon)^n$$。在大多数情况下我们无法得知$$k$$的具体值，因此进一步对概率进行缩放得到版本空间不满足$$\varepsilon$$-exhausted条件的概率上界：

$$
k(1 - \varepsilon)^n \leq \vert H \vert (1 - \varepsilon)^n \leq \vert H \vert e^{- \varepsilon n}
$$

其中$$\vert H \vert$$表示假设空间的大小。上式说明学习器$$L$$学习失败的概率一定小于等于$$\vert H \vert e^{- \varepsilon n}$$，我们再令这个上界小于等于某个小量$$\delta$$就能得到PAC学习的样本复杂度：

$$
\vert H \vert e^{- \varepsilon n} \leq \delta
$$

$$
n \geq \frac{1}{\varepsilon} (\ln \vert H \vert + \ln \frac{1}{\delta})
$$

上式也被称为**Haussler定理(Haussler theorem)**，它说明我们至少需要$$n = \frac{1}{\varepsilon} (\ln \vert H \vert + \ln \frac{1}{\delta})$$数量的样本才能保证学习到的一致假设$$h$$满足泛化误差$$error_D(h) \leq \varepsilon$$。

## VC Dimension

Haussler定理的一个主要缺陷在于它过于高估了样本复杂度。当假设空间$$H$$存在无穷多假设时，按照Haussler定理我们需要无穷多的样本才能保证PAC可学习，这与我们的认知是相违背的。实际上对于包含无穷多假设的假设空间，其中很多的假设是相互等价的，显然将它们视作不同的假设会极大地增加样本复杂度。因此我们需要缩小$$\vert H \vert$$来获得样本复杂度的一个更紧的界。

对于二分问题，假设空间$$H$$中的假设对样本赋予标记的每种可能结果称为对样本的一种**对分(dichotomy)**。显然对于包含$$n$$个样本的数据集所有可能的标记总共有$$2^n$$种可能性。如果假设空间$$H$$能够实现样本上的所有对分，则称样本能被假设空间$$H$$**打散(shattering)**。在此基础上我们给出**VC维(Vapnik and Chervonenkis dimension)**的定义：

> 假设空间$$H$$的VC维是能被$$H$$打散的最大样本集的大小，即$$VC(H) = \max \{ m: \Pi_H (m) = 2^m \}$$

$$VC(H) = d$$表示存在大小为$$d$$的样本集能被假设空间$$H$$打散，但这并不意味着任意大小为$$d$$的样本集都能被$$H$$打散。同时VC维与样本分布$$D$$无关，仅与假设空间$$H$$有关。因此在数据未知的情况下仍然能够计算出VC维。

以平面上的二分类问题为例，对于线性分类器当我们有3个样本时无论样本如何布置都可以实现对分，但对于4个样本的情况则会出现无法对分的情况。因此二分问题中线性分类器的VC维为3，同时可以证明$$n$$维情况下的VC维为$$n+1$$。

<div align=center>
<img src="https://i.imgur.com/LaiOIaX.png" width="70%">
</div>

使用VC维来代替假设空间的模可以得到更准确的样本复杂度：

$$
n \geq \frac{1}{\varepsilon} (4 \log_2 \big( \frac{2}{\delta} \big) + 8 VC(H) \log_2 \big( \frac{13}{\varepsilon} \big) )
$$

利用VC维还可以证明假设空间$$H$$是可学习的当且仅当它的VC维有限。

## Reference

- Chapter 7: COMPUTATIONAL LEARNING THEORY, [Machine Learning, Tom Mitchell, McGraw Hill, 1997.](http://www.cs.cmu.edu/afs/cs.cmu.edu/user/mitchell/ftp/mlbook.html)