---
layout: post
title: MIT 6.S183课程笔记6-Generalization in Diffusion Models
date: 2026-08-18
description: 扩散模型的泛化
tags: DL Diffusion
categories: MIT-6.S183
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


> 这个系列是[MIT 6.S183 - A Practical Introduction to Diffusion](https://www.practical-diffusion.org/)的同步课程笔记。本门课程面向对扩散模型感兴趣的学生和研究者，从最底层开始逐步介绍扩散模型的数学原理以及各种应用。本节课主要介绍扩散模型的泛化能力。
{: .block-preface }

到目前为止，我们已经介绍过扩散模型的数学原理以及各种不同领域上的应用，而今天我们则要讨论扩散模型的泛化能力。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/I61K6oW.png" width="100%">
</div>

## Defining Generalization

首先让我们先来定义一下什么是泛化能力。不严谨的说，模型的泛化能力是指其能否生成全新而且真实的样本数据。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nCZMHbi.png" width="100%">
</div>

但需要说明的是，"真实"本身是一个相当主观的说法。尽管也有一些数学工具来帮助我们描述这一问题，但整体而言图片是否"真实"仍然依赖于人们的主观认知。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/j254rol.png" width="100%">
</div>

从更严谨的角度来讲，模型的泛化能力是指其能否表达训练数据所在的目标数据分布。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Ua2p8UK.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uuxH6Fv.png" width="100%">
</div>

因此，如果使用似然函数来描述泛化能力的话，那生成模型给出的似然函数应该要尽可能大于训练数据集上经验分布的似然函数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9BA0wnz.png" width="100%">
</div>

以下图为例，我们从数据集上采样出红色的数据点。一个具有良好泛化能力的模型显然不能只在采样数据附近有概率密度，相反它应该能够近似表达采样数据可能来自的概率密度函数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yhnUJqU.png" width="100%">
</div>

当然，使用似然函数也不一定是一个非常好的选择。一些研究指出，生成模型的泛化能力和似然函数之间可能也没有那么紧密的联系。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/KGacmKQ.png" width="100%">
</div>

## Do Diffusion Models Generalize?

回到扩散模型上，从实践来看扩散模型已经能够生成非常逼真的图像数据了。但一些近期的研究显示，扩散模型的生成结果中至少有1.88%的数据来自于对训练数据集的拷贝，这对很多涉及敏感信息以及版权问题的场景非常不利。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7j2zG37.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/1EA32FW.png" width="100%">
</div>

不过整体而言，扩散模型还是能给出大量高质量的数据样本的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7GwxZSc.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/U4kjYd3.png" width="100%">
</div>

如果使用似然函数来描述模型泛化能力的话，一些更偏理论的研究指出扩散模型确实已经在一些经典的数据集上表现出非常强的泛化能力。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vUR2mRS.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JSvUTke.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vAT7IMx.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/x2U2uFk.png" width="100%">
</div>

## Should They Generalize?

从优化的角度来看，扩散模型的训练目标是训练一个denoiser。对于有限数据集，denoiser存在最优估计$$\epsilon^*$$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/urJvuXt.png" width="100%">
</div>

那么为什么不直接使用最优denoiser呢而是需要大量的训练过程呢？实际上这样的最优denoiser没有任何泛化能力，它只能从已有数据上复制已有数据完全不能生成新数据。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/c03t68D.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/rY74eLv.png" width="100%">
</div>

我们可以借助**过拟合(overfitting)**的概念来理解这一现象。在监督学习中，当模型的能力过强时很容易能够精确地拟合训练集上的所有数据，而在这些数据外反而出现比较大的偏差。因此，监督学习的目标是找到真实的拟合函数而不是最小化模型在训练数据上的误差。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/wt4rhdd.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/rBUTp81.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ia1Sxes.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JfzpwI3.png" width="100%">
</div>

生成式模型的需求是类似的：我们不需要最小化训练数据误差的模型，这个模型实际上可以显示构造出来不需要任何训练过程。如果模型在训练数据上有非常好的表现，这反而需要引起我们的注意。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/kkQDQUS.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/hpsZudo.png" width="100%">
</div>

## Why Do They Generalize?

接下来的一个自然而然的问题是为什么扩散模型能够具有泛化能力呢？整体而言，扩散模型并不是去训练一个在数据集上表现最好的模型，相反我们一般会在训练收敛之前就停下来。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yQZkTyk.png" width="100%">
</div>

目前学术界对于扩散模型泛化能力的来源还没有形成定论，这里简要从三个方面来进行讨论。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DNZgFn3.png" width="100%">
</div>

### Inductive Biases

对于包含ReLU函数的denoiser，其输出可以表示为一系列基向量的线性组合，而且组合系数是非常稀疏的。这表明噪声图像可以使用少量的基向量来进行表示。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XiWpjMe.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9RjetRP.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RVhkfip.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vfAXhDF.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/o9gSyJP.png" width="100%">
</div>

更进一步，噪声图像还取决于denoiser的输入。这使得denoiser不会完全丢掉数据本身的信息，而是能够学习到数据分布上的一些特征。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9kKaVaG.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/w9WC0Z4.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sYDoHZ2.png" width="100%">
</div>

除了稀疏性外，也有观点认为扩散模型的泛化能力来自于其和监督学习相比更加光滑的性质。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XVkeafm.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/f0fXtrc.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/VwlRBYs.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ICIDFGi.png" width="100%">
</div>

一些近期的文章认为扩散模型的泛化能力来自于模型的局部性和等变性，这两种能力主要来自于模型中的卷积运算以及训练数据本身的性质。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mXnwV0R.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2SmIX9R.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Sq47VZG.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/M9krQ37.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/1xWUAeX.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/03q2K4j.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jDIQhqn.png" width="100%">
</div>

### Training Procedure

除了模型自身外，模型的训练过程也会对其泛化能力产生影响。训练过于充分甚至产生过拟合的模型显然是不具有泛化能力的，近期的研究发现扩散模型的生成结果会和训练轮数有一定的关联。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sF0gN0Y.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ygYR0Gt.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/U7EGZ7o.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XAyjUvC.png" width="100%">
</div>

基于SGD的模型训练方式可能也是扩散模型泛化能力的来源之一。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4zyuxGK.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/VaR1RxU.png" width="100%">
</div>

总之，关于扩散模型泛化能力的研究虽然目前仍是以探索为主，但它对我们理解扩散模型以及指导设计更优质更高效模型的过程都是大有裨益的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qrOP79D.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yeiuQlC.png" width="100%">
</div>

## Reference

- [Lecture 6 - Generalization in Diffusion Models](https://www.youtube.com/watch?v=50Ksf6NQE5c)