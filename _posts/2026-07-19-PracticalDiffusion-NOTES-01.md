---
layout: post
title: MIT 6.S183课程笔记1-Introduction to Diffusion
date: 2026-07-19
description: 扩散模型简介
tags: DL Diffusion
categories: MIT-6.S183
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


> 这个系列是[MIT 6.S183 - A Practical Introduction to Diffusion](https://www.practical-diffusion.org/)的同步课程笔记。本门课程面向对扩散模型感兴趣的学生和研究者，从最底层开始逐步介绍扩散模型的数学原理以及各种应用。本节课主要介绍生成模型的发展历史以及扩散模型的基本工作流程。
{: .block-preface }


## Why Use Generative Models?

在具体介绍扩散模型前我们先来简单回顾一下机器学习中的一些基础概念。首先，**监督学习(Supervised Learning)**是指给定一些输入输出数据，在训练数据上学习到模型，从而使得我们可以在全新的输入数据上能够预测对应的输出。需要注意的是监督学习并不关心输入数据的分布信息，某种意义上讲监督学习的目标实际上是给定输入数据的情况下预测输出数据的"均值"。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XO5xh7r.png" width="100%">
</div>

与监督学习相对的另一种任务是**生成式学习(Generative Learning)**。在生成式学习中，我们的学习目标是输入输出数据的联合概率分布。当这个联合概率分布被正确学习到时，我们不仅可以对输入数据对应的输出进行预测，还可以给出此时输出数据的分布，甚至可以对输入输出数据进行采样。因此，生成式学习是远比监督学习更为灵活也更为复杂的任务。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ReI7PTc.png" width="100%">
</div>

生成式学习的一个典型应用场景是图像生成。随着技术的演进，传统基于回归的生成模型已经被基于扩散的生成模型所取代。现在网络上流行的Text-to-Image、图像编辑以及超分辨率等技术都是基于这样的生成式模型来实现的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jilZGRp.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fcH4eYN.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/OMB3zmT.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yOiVNYa.png" width="100%">
</div>

## A Brief History of Generative Models

### VAEs

我们来形式化一下生成式学习的过程:对于给定标签的数据集$$\mathcal{D}=\{(x, l)\}$$，生成式学习的目标是学习到一个(参数)模型$$p_{\theta}(x \vert l)$$，使得我们在这个模型上进行采样时，原本的数据$$(x, l)$$有比较大的概率(似然)。因此一个直白的想法是直接去最大化似然函数$$\log{p_{\theta}(x \vert l)}$$，这种思路推动了VAE的产生。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ulFb5Ew.png" width="100%">
</div>

VAE的一个基本假设是存在一个隐变量$$z$$来控制数据$$x$$的生成。因此我们可以设计一个编码器$$g(x, l)$$将输入数据$$(x, l)$$映射为隐变量$$z$$，再利用一个解码器$$h(z, l)$$来恢复原始数据$$x$$。VAE的具体推导过程比较复杂，这里直接给出结论:我们不再直接优化原始的似然函数，而是去优化它的下界[ELBO](https://en.wikipedia.org/wiki/Evidence_lower_bound)。一般地，我们可以假设隐变量$$z$$关联的分布都是正态分布，这样VAE的损失函数可以表示为一个标准自编码器的损失以及一个正则项。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/s5rhItK.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/WpzllD4.png" width="100%">
</div>

在VAE刚刚发布的年代，由于其理论的优雅性以及实现起来比较简单，VAE得到了学术界的广泛推崇。但需要指出的是VAE的缺陷也十分明显：首先VAE生成的图像大多比较模糊，人们推测可能是解码器不够强导致数据被压缩了；更严重的问题在于VAE可能会产生**后验坍缩(posterior collapse)**的现象，当解码器比较强时可能无法学习到隐变量$$z$$相关的信息，换句话说我们无法使用隐变量$$z$$来控制生成的结果。这样的缺陷导致VAE在2010年代后期逐渐被其它生成式模型取代。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4BJJyOH.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/A47lSKE.png" width="100%">
</div>

### GANs

人们从VAE中学到的重要一课在于不应该使用似然函数来进行优化，而是应该直接去优化概率分布本身。后面出现的GAN实际上就是对概率分布的TV loss进行优化。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cxbazQC.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/gCQTOHR.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/HCP81vz.png" width="100%">
</div>

而GAN的一个重要缺陷在于它本身非常难以进行训练，在实际工程中往往需要结合大量的trick才能成功训练出模型。尽管如此，GAN在今天仍然是一些生成任务的SOTA模型。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nlC9NND.png" width="100%">
</div>

### Enter: Diffusion

在后GAN时代，扩散模型的出现极大地降低了生成式模型的训练难度。扩散模型的主要贡献在于使用一类特定的函数来实现噪声到真实数据的映射，这一过程类似于对信号进行降噪。课程后面会更详细地介绍扩散模型背后的数学原理。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4apk3VE.png" width="100%">
</div>

## Diffusion Models

从直觉上来讲，扩散模型来源于物理上的扩散过程以及布朗运动。可以想象对于一张给定的照片，我们通过不断地添加少量的噪声最终会使得整个照片最终退化成一张完全由噪声组成的图像。而如果我们可以逆转每一步添加噪声的过程，那么就能够实现从噪声图像恢复出原始的图像。这就是扩散模型的核心思想：通过学习添加噪声的逆过程来实现从噪声中恢复原始数据。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zoqBvba.png" width="100%">
</div>

### Training Diffusion Models

扩散模型的训练过程大体是类似的。我们的目标是训练一个**降噪器(denoiser)**$$\epsilon_\theta$$来预测给定噪声水平$$\sigma$$下训练数据的噪声$$\epsilon$$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jqH5cDp.png" width="100%">
</div>

在训练扩散模型时如何设置噪声水平$$\sigma$$至关重要。目前通用的做法是使用一些scheduler来控制不同扩散步数下噪声的强度。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jbe9DIa.png" width="100%">
</div>

denoiser的实现是相对简单的。以二维数据为例，一个简单的全连接网络就足以满足降噪的需求。当然对于更复杂的生成任务也需要更加复杂的网络才能实现比较好的生成结果。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/h1leTHq.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/8RKg6UU.png" width="100%">
</div>

完成训练后就可以利用denoiser来预测给定噪声数据以及强度下的噪声，进而实现对噪声数据的去噪。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/0zAFEAC.png" width="100%">
</div>

实际上denoiser的训练过程还可以理解为将噪声数据投影到原始数据的分布上。在这样的观察下，denoiser的损失函数描述了噪声数据到原始数据分布上的"距离"。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/43DYlAo.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/69JsF7f.png" width="100%">
</div>

### Sampling from Diffusion Models

完成denoiser的训练后就可以通过预测的噪声一步一步地将噪声数据移动到原始数据上。当然不同的算法在设计移动步长上有不同的取舍，比如说DDIM使用的是确定性的移动步长，而DDPM则是有随机性的步长。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GyKVWtW.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Mzjq9W1.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bEh7XJk.png" width="100%">
</div>

## Reference

- [Lecture 1: Introduction to Diffusion](https://www.youtube.com/watch?v=1RbYA_eAt-A)
- [Diffusion models from scratch](https://chenyang.co/diffusion.html)