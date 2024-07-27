---
layout: post
title: OMSCS-DL课程笔记19-Generative Models
date: 2022-04-11
description: 生成式模型
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍生成式模型。
{: .block-preface }


## Introduction

**生成式模型(generative models)**是无监督学习的一种算法，它的目标是对生成数据的分布进行建模。在传统机器学习中最经典的生成式模型是**高斯混合模型(Gaussian mixture model, GMM)**，而在深度学习时代我们则可以利用深度神经网络对更加复杂的数据分布进行建模。

<div align=center>
<img src="https://i.imgur.com/HxzADa4.png" width="80%">
</div>

从数学上来看，**判别式模型(discriminative model)**的本质是计算给定样本的后验概率$P(y \vert x)$，我们熟悉的神经网络以及SVM等模型大多属于这一类型；而生成式模型则是最大化生成已有样本的概率，对于参数化的模型这等价于**极大似然估计(maximum likelihood estimation, MLE)**。此外，当我们获得了生成式模型后就可以从分布中进行采样从而生成更多的数据样本。

<div align=center>
<img src="https://i.imgur.com/P5v3Jyk.png" width="80%">
</div>

显然如何获得生成式模型是比较困难的任务，目前常用的生成式模型算法可以归纳如下：

<div align=center>
<img src="https://i.imgur.com/yWG7O0Z.png" width="80%">
</div>

## PixelRNN & PixelCNN

对于一张图片而言，我们可以把它的似然函数分解为每个像素似然函数的乘积。这样整个图片可以看做是一个大型的**贝叶斯网络(Bayesian network)**或是**语言模型(language model)**，我们只需要定义像素之间的连接关系再估计似然函数的形式即可计算概率分布。

<div align=center>
<img src="https://i.imgur.com/VoZKG81.png" width="80%">
</div>

PixelRNN就是基于上述思想实现的生成式模型。在PixelRNN中我们需要设置像素之间的连接(生成)关系，然后从某一个像素出发利用RNN来计算下一个像素的概率。显然PixelRNN的计算效率是比较低的，并且在训练时我们无法使用整张图片而是只能通过预测少量的像素来进行训练。

<div align=center>
<img src="https://i.imgur.com/QsUX6Ig.png" width="80%">
</div>

PixelCNN的思想与PixelRNN类似，不过它使用了卷积核来代替RNN生成像素。和PixelRNN相比PixelCNN的训练过程要快很多，但在进行生成时仍然效率很低。

<div align=center>
<img src="https://i.imgur.com/xB6nGGP.png" width="80%">
</div>

PixelRNN和PixelCNN有很多有趣的应用，除了直接生成图片外我们还可以遮住图片中的一部分区域然后估计这片区域可能的形式。当然PixelRNN和PixelCNN的生成过程是不符合天然图片的生成过程的，这导致了它们生成出的图片在很多细节上有不正确的结果。

<div align=center>
<img src="https://i.imgur.com/Nw0NU5s.png" width="80%">
<img src="https://i.imgur.com/osR4OwU.png" width="80%">
</div>

## Generative Adversarial Networks

**生成对抗网络(generative adversarial networks, GAN)**是最近几年比较流行的生成式模型。和传统方法相比，GAN是一种**隐式方法(implicit method)**：它不会直接去计算$P(x)$的形式而是去对$P(x)$的采样过程进行建模。

<div align=center>
<img src="https://i.imgur.com/arMFklG.png" width="80%">
</div>

GAN的本质是计算一个简单的分布(如标准正态分布$N(0, 1)$)到样本生成分布$P(x)$的映射，这样我们可以简单地从已知分布中进行采样然后利用一个神经网络将这个样本映射到所需数据分布上，就实现了从$P(x)$中进行采样的过程。

<div align=center>
<img src="https://i.imgur.com/bhaMnjB.png" width="80%">
<img src="https://i.imgur.com/5CD6EIG.png" width="80%">
</div>

当然要计算这样的一个分布到分布的映射没有那么的容易。在GAN中通过训练两个网络：**生成器(generator)**和**判别器(discriminator)**来实现完整的训练过程。其中生成器用来对样本进行映射，而判别器则用来判断生成器生成的样本是否足够接近真实的数据。

<div align=center>
<img src="https://i.imgur.com/fjWQ11x.png" width="80%">
<img src="https://i.imgur.com/XgQKqzo.png" width="80%">
</div>

### Mini-Max Two Player Game

GAN的训练过程可以视为两个网络相互博弈的过程。不过由于我们使用了神经网络来表示映射，目前尚不清楚这样的博弈过程是否能够达到**纳什均衡(Nash equilibria)**。对于生成器，它的优化目标是尽可能去让判别器相信它生成的样本是真实的数据；而判别器则是对真实样本和生成器的样本进行一个二分类，我们把真实的数据标记为正例而来自生成器的样本标记为反例。这样整个GAN的训练目标等价于求解一个极小极大问题：生成器负责极大化二分类损失，而判别器则是极小化损失。

<div align=center>
<img src="https://i.imgur.com/qjr2sku.png" width="80%">
<img src="https://i.imgur.com/G7OLA4K.png" width="80%">
<img src="https://i.imgur.com/BvjEGPY.png" width="80%">
<img src="https://i.imgur.com/CjVCTeP.png" width="80%">
</div>

以图像生成为例，GAN的训练过程如下：我们首先采样出一系列随机种子然后利用生成器产生一系列样本，然后从数据集中采样出等量的真实图片并把它们混合到一起送入判别器中进行二分类。由于整个过程都是可导的，我们可以利用标准的梯度下降方法来训练两个网络。

<div align=center>
<img src="https://i.imgur.com/uUNa394.png" width="80%">
</div>

在实践中人们还发现训练开始时生成器的样本与真实样本差距过大，导致生成器很难从判别器那里获得梯度。为了缓解这样的问题还可以稍微修正一下生成器的损失函数，这样可以提高GAN训练时的稳定性。

<div align=center>
<img src="https://i.imgur.com/7DrVZjP.png" width="80%">
</div>

完整的GAN训练流程则如下图所示：

<div align=center>
<img src="https://i.imgur.com/4UirbLW.png" width="80%">
</div>

当我们完成训练后就可以利用随机种子和生成器来生成逼真的数据样本，同时判别器中的一些特征可以服务于不同的分类任务。

<div align=center>
<img src="https://i.imgur.com/5nNN1kU.png" width="80%">
<img src="https://i.imgur.com/AP01Mgd.png" width="80%">
</div>

### Challenges

目前来看训练GAN仍然是相对困难的，因此人们提出了不同的方法来改进原始的GAN训练过程。这些方法包括使用更稳定的网络架构、为目标函数添加正则项以及使用渐进的训练方法等。

<div align=center>
<img src="https://i.imgur.com/nfgXPO8.png" width="80%">
</div>

DCGAN对GAN的网络模型进行修正，从而获得更稳定的训练过程。

<div align=center>
<img src="https://i.imgur.com/yqKdvpz.png" width="80%">
</div>

通过对GAN目标函数的理论分析，人们还提出了添加噪声作为正则项的改进方法。

<div align=center>
<img src="https://i.imgur.com/czHCHKN.png" width="80%">
</div>

基于这些改进，目前GAN相关的方法以及可以生成高分辨率的逼真图像，同时GAN也可以用来生成语音、视频等其它形式的数据。

<div align=center>
<img src="https://i.imgur.com/gmx7udH.png" width="80%">
<img src="https://i.imgur.com/xQkij75.png" width="80%">
<img src="https://i.imgur.com/1hgHWpN.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/AUthuzy.png" width="80%">
</div>

## Variational Autoencoders

**变分自编码器(variational autoencoders, VAE)**同样是这几年比较流行的生成式模型。和GAN相比，VAE是一种**显式方法(explicit method)**直接对样本的生成分布进行建模，但VAE没有去计算真实的概率分布函数而是使用多元正态分布去近似它。

### Autoencoders

从网络架构来看，VAE通过一个编码器和解码器获得与输入样本相一致的生成数据。在标准自编码器架构中，输入样本经过编码器得到一个低维空间中的嵌入，并且通过解码器来将这个嵌入还原数据，最后通过MSE来完成训练过程。

<div align=center>
<img src="https://i.imgur.com/myhu3Gb.png" width="80%">
</div>

在VAE中则对编码器的输出进行了修改。此时编码器会输出隐空间上的一个概率分布而不是隐层向量，相应地解码器则会从这个概率分布中进行采样并还原出生成样本。最后通过最大化生成样本与真实样本之间的VLB来实现训练过程。

<div align=center>
<img src="https://i.imgur.com/4vDzipa.png" width="80%">
</div>

VAE的解码器会输出样本的均值和(协)方差。对于多维的情况这里假定不同维度之间是相互独立的，这样只需要一个对角矩阵就可以描述样本分布的方差。

<div align=center>
<img src="https://i.imgur.com/MSMTbYU.png" width="80%">
</div>

而对于编码器，输入样本$X$经过编码器后会得到样本在低维空间上分布的均值和(协)方差。

<div align=center>
<img src="https://i.imgur.com/9qaYhjJ.png" width="80%">
</div>

把编码器和解码器组合到一起就得到了VAE的计算流程：输入数据$X$经过编码器得到隐空间上的分布$Z$，然后从$Z$中进行采样得到隐空间样本$z$并通过解码器来还原$X$对应的分布，最后从解码器得到的分布上进行采样就得到了新的样本。

<div align=center>
<img src="https://i.imgur.com/h7K6Z5X.png" width="80%">
</div>

### Maximizing Likelihood

VAE的难点在于如何进行训练，这里我们利用MLE来推导VAE的目标函数。首先对样本$x^{(i)}$的对数似然函数进行变形，可以得到三项：

<div align=center>
<img src="https://i.imgur.com/qGUmSYJ.png" width="80%">
<img src="https://i.imgur.com/x7TiWHI.png" width="80%">
</div>

接下来我们引入**KL散度(KL divergence)**的概念，它度量了两个概率分布之间的差异。

<div align=center>
<img src="https://i.imgur.com/z3ByKJy.png" width="80%">
</div>

利用KL散度我们可以把对数似然函数的后两项改成$q_\phi(z \vert x^{(i)})$与$p_\theta(z)$和$p_\theta(z \vert x^{(i)})$之间的KL散度。这样对数似然函数的三项都有各自的意义：第一项对应从解码器采样出$x^{(i)}$，第二项对应编码器输出的正态分布与隐空间上$z$先验分布之间的差异，而第三项则对应编码器的输出与解码器输出之间的差异。这里需要注意的是其中第三项是不可计算的，但我们知道它恒为正；而可计算的前两项也称为**ELBO(expected lower bound)**。显然ELBO是我们目标函数的一个下界，而VAE的训练过程就是去不断最大化下界的过程。

<div align=center>
<img src="https://i.imgur.com/xx2Bsrs.png" width="80%">
<img src="https://i.imgur.com/gayRHex.png" width="80%">
<img src="https://i.imgur.com/a6SYrAU.png" width="80%">
</div>

### Forward and Backward Passes

VAE的实际训练过程如下：首先对于给定样本$x^{(i)}$我们利用生成器来获得$z$的后验分布$q_\phi(z \vert x^{(i)})$，当假定$z$的先验为多元正态分布时可以直接计算ELBO中的KL散度项。

<div align=center>
<img src="https://i.imgur.com/vkURr5l.png" width="80%">
</div>

接下来我们从后验分布$q_\phi(z \vert x^{(i)})$进行采样，并且将样本送入解码器中得到重建后$X$的分布$p_\theta(z \vert x^{(i)})$。把它带入到ELBO中就得到了完整的优化目标函数。

<div align=center>
<img src="https://i.imgur.com/4zSZcXn.png" width="80%">
<img src="https://i.imgur.com/VyH8AbM.png" width="80%">
</div>

### Reparameterization Trick

上述过程的一个缺陷在于从后验分布$q_\phi(z \vert x^{(i)})$进行采样的过程是不可导的，这样我们无法使用梯度下降的方法进行训练。为了解决这个问题人们提出了**重参数化(reparameterization)**的技巧：采样时首先从标准正态分布$N(0, 1)$中进行采样，然后利用后验分布的参数对样本进行线性变换来获得后验分布的样本。根据正态分布的性质，这样的采样过程等价于直接从后验分布$q_\phi(z \vert x^{(i)})$中进行采样，但此时整个采样过程已经是可导的了。

<div align=center>
<img src="https://i.imgur.com/LMAEgjk.png" width="80%">
<img src="https://i.imgur.com/grLN3k3.png" width="80%">
</div>

使用VAE不仅可以生成逼真的样本，更可以利用后验概率的形式来控制样本生成的结果。因此VAE和其他生成模型相比往往有着更好的可解释性。

<div align=center>
<img src="https://i.imgur.com/CTBzKZJ.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/UKu1mo1.png" width="80%">
</div>