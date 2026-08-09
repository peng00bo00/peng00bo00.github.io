---
layout: post
title: MIT 6.S183课程笔记4-Distillation
date: 2026-08-08
description: 蒸馏技术
tags: DL Diffusion
categories: MIT-6.S183
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


> 这个系列是[MIT 6.S183 - A Practical Introduction to Diffusion](https://www.practical-diffusion.org/)的同步课程笔记。本门课程面向对扩散模型感兴趣的学生和研究者，从最底层开始逐步介绍扩散模型的数学原理以及各种应用。本节课主要介绍扩散模型相关的蒸馏技术。
{: .block-preface }

在前面的课程中我们介绍过扩散模型的噪声过程。原始数据通过不断添加噪声会逐渐趋向于噪声分布，大多情况下可以表示为一个正态分布。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fwSY3qQ.png" width="100%">
</div>

而扩散模型则需要学习噪声过程的逆过程，从而将噪声数据重新拉回到原始数据的分布上。当模型训练完成后可以使用不同的采样算法来实现去噪的过程，而且这些不同的采样算法可以共享同一个模型(denoiser)。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fhKqFHi.png" width="100%">
</div>

此外，我们还讨论了条件生成相关的技术，通过额外给定一些条件或者引导可以更好地控制模型的生成结果。目前应用最广泛的算法是classifier-free guidance，它的训练过程更简单生成结果也更好。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/518Ccxq.png" width="100%">
</div>

## What is Distillation?

本节课我们主要关注**蒸馏(distillation)**这个话题。在如今的AI领域，蒸馏本身有非常丰富的含义，而具体到扩散模型中蒸馏则可以理解为给定一个强大的模型如何利用它训练一个更加小巧的模型，以便满足某些特定任务的需求。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Y8mhUDC.png" width="100%">
</div>

## Few-Step Sampling

在扩散模型中一个非常现实的问题是数据的生成往往需要比较多的迭代。类比大模型需要不断地预测下一个token，扩散模型需要从噪声分布出发不断迭代完成去噪的过程。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FovKgaV.png" width="100%">
</div>

对于图像这种模态的数据，传统图像处理技术告诉我们图像大部分的信息都分布在较为低频的信号上。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/40udhhw.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Ps2nrvq.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5R7n2Bb.png" width="100%">
</div>

而对于高斯噪声这样的数据，其在不同频率上的能量分布是一样的。因此对图像添加高斯噪声实际上会先污染高频部分的特征。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Jv6RNGY.png" width="100%">
</div>

基于高斯噪声这样的特点不难发现，在扩散模型中一开始的几步就可以恢复图像的大致形状，而随着扩散步数的增加更多的细节会逐步添加到图像中。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4GLnkv4.png" width="100%">
</div>

### How to Sample Faster?

回到如何加速扩散模型采样过程这一问题上。实际上我们在[第一节](/blog/2026/PraticalDiffusion-NOTES-01/#sampling-from-diffusion-models)介绍过的DDIM就是一种非常实用的加速采样算法，它的一大优势在于无需额外训练只需要修改采样算法就能提升效率。而除此之外，我们也可以通过进行一些额外的训练来提升扩散模型采样的效率。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mqzJLLR.png" width="100%">
</div>

Rectified Flow的处理思路是构造更短的流动路径来实现采样过程的加速。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/y071wyl.png" width="100%">
</div>

Progressive Distillation的处理思路是在已有扩散模型的基础上单独训练一个更小的模型，并且在这个模型上使用加速后的扩散过程训练来实现对原始模型的蒸馏。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/m5YgMXT.png" width="100%">
</div>

Consistency Model的处理思路是对于训练好的模型把一些扩散过程中的中间结果固定下来作为监督学习的目标，从而减少整体扩散的步骤。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/1msp2hB.png" width="100%">
</div>

Mean Flow的处理思路对在扩散的轨迹进行优化从而降低扩散的复杂度。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/09UpK9s.png" width="100%">
</div>

## Score Distillation

除了目前常用的一些扩散模型加速技术外，本节课还介绍了Score Distillation相关的话题。Score Distillation来自于3D数据生成领域，对于2D图像扩散模型已经取得了非常显著的成绩，但是由于3D数据的稀缺性要按照图像的方式来训练扩散模型仍然是非常困难的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/doxPTSB.png" width="100%">
</div>

Score Distillation的思路是结合NeRF这样的可微渲染技术，把3D数据渲染为2D图像，同时对渲染出的图像进行diffusion来反向对NeRF进行训练，这样就间接完成了3D数据的生成。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/076nozN.png" width="100%">
</div>

Score Distillation的重要贡献在于推导出了2D图像噪声对于可微渲染器的损失函数，使得我们可以在一个训练好的2D扩散模型上生成对应的3D数据。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zzT0Ln2.png" width="100%">
</div>

实际上结合条件生成模型，SDS还可以实现文生3D这样的生成任务。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FbwbQ8h.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4lqscya.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dkeAixl.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ldCgViK.png" width="100%">
</div>

## Reference

- [Lecture 4 - Distillation](https://www.youtube.com/watch?v=WNsdbrsUB-c)
- [Rectified Flow](https://rectifiedflow.github.io/)