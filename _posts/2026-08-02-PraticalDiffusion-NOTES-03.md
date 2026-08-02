---
layout: post
title: MIT 6.S183课程笔记3-Conditioning and Guidance
date: 2026-08-02
description: 条件生成模型
tags: DL Diffusion
categories: MIT-6.S183
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


> 这个系列是[MIT 6.S183 - A Practical Introduction to Diffusion](https://www.practical-diffusion.org/)的同步课程笔记。本门课程面向对扩散模型感兴趣的学生和研究者，从最底层开始逐步介绍扩散模型的数学原理以及各种应用。本节课主要介绍条件生成模型。
{: .block-preface }

在前面的课程中我们已经学习了利用扩散模型来对概率分布进行建模以及生成的技术，而本节课我们则会介绍给定某些条件或是偏好下如何进行生成。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5g8oCoh.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ubRjLvo.png" width="100%">
</div>

## Denoiser to Score Function

首先回忆一下扩散模型的目标是想要逆转数据因添加噪声而被逐渐污染为随机信号的过程，在扩散模型中一般是通过训练一个降噪器denoiser并搭配相应的采样策略来实现。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tF6ExRc.png" width="100%">
</div>

采样策略大体可以分为基于ODE的确定性采样以及基于SDE的随机采样算法两大类。尽管不同算法的实现会有差异，但整体的思路都是从一个容易采样的概率分布出发，然后通过预测前向过程的噪声$$\epsilon^* (x_t, t)$$对噪声进行采样进而实现去噪的过程。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7UwBwGT.png" width="100%">
</div>

结合[Tweedie formula](/blog/2026/PraticalDiffusion-NOTES-02/#tweedies-formula)我们可以得到噪声估计的数学表达式

$$
\epsilon^* (x_t, t) = \mathbb{E} [\epsilon \vert x_t] = -\sigma_t \nabla \log{p_t (x_t)}
$$

其中的对数项$$s^* (x_t, t) = \nabla \log{p_t (x_t)}$$称为**score function**。显然对denoiser进行训练或者采样本质上都是去计算score function，它们之间是完全相互等价的。在课程后面我们会基于score function进行分析和讨论。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/WfEfQb5.png" width="100%">
</div>


## Reference

- [Lecture 3 - Conditioning and Guidance](https://www.youtube.com/watch?v=HYIhJGUycjQ)