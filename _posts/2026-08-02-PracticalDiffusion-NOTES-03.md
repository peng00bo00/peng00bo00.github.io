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

## Training (Unconditional) Score Function

接下来考虑如何去训练score function。一个直观的想法是使用L2损失去度量真实数据和模型输出的差异，但这种思路的问题在于我们无法计算真实数据的概率分布，也无法计算对应的score function。不过对于扩散过程，我们实际上是知道给定0时刻数据分布，在$$t$$时刻的概率分布满足$$p_{t \vert 0} (x_t \vert x_0) \sim \mathcal{N} (x_0, \sigma_t^2 I)$$，其对应的score function有解析形式

$$
\nabla \log{p_{t \vert 0} (x_t \vert x_0)} = - \frac{x_t - x_0}{\sigma_t^2}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PhRC9BL.png" width="100%">
</div>

实际上通过一些数学变形可以把$$t$$时刻的无条件概率分布写成条件概率分布的期望

$$
\begin{aligned}
\nabla \log{p_t (x_t)} &= \int \nabla_x \log{p_{t \vert 0} (x_t \vert x_0)} \ p_0 (x_0 \vert x_t) \ \mathrm{d} x_0 \\
&= \mathbb{E}_{x_0 \sim X_0 \vert X_t = x_t} [\nabla_x \log{p_{t \vert 0} (x_t \vert x_0)}] 
\end{aligned}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eI3DweQ.png" width="100%">
</div>

因此，考虑扩散过程的score function损失函数实际上就是去计算(学习)无条件情况下的score function，这使得我们计算通用情况下的score function变成了可能。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/EXgX0au.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/xpaMzzQ.png" width="100%">
</div>

## Unconditional to Conditional Sampling and Training

回到最开始的条件生成问题上，在很多场景中我们的需求是用户输入一些条件信息来引导模型从这些给定的信息中进行生成，这样的方式可以让我们对生成的结果有更多的控制。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Tykttkh.png" width="100%">
</div>

对于这样的问题只需要把数据$$x_0$$以及对应的条件$$c$$一起作为模型输入进行训练即可，本质和无条件的生成模型没有什么区别。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/McI2hyP.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/b46RIqZ.png" width="100%">
</div>

然而遗憾的是，在实践中这样训练出的模型在生成高维数据时的效果并不好。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/V0LWEhp.png" width="100%">
</div>

## Classifier Guidance and Classifier Free Guidance

### Classifier Guidance

回顾一下条件生成模型的score function，利用Bayes公式对它进行展开得到

$$
\nabla_x \log{p_t (x_t \vert c)} = \nabla_x \log{p_t (x_t)} + \nabla_x \log{p_t (c \vert x_t)}
$$

其中第一项$$\nabla_x \log{p_t (x_t)}$$是无条件情况下的score function，而第二项$$\nabla_x \log{p_t (c \vert x_t)}$$则对应一个分类器将数据映射为对应标签的概率，这表明我们可以在训练模型时同步训练一个分类器来指导条件生成模型。在实际操作中一般会在分类器一项加入一个系数$$\gamma$$表示来自分类器这一项的"强度"。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cxFyZF9.png" width="100%">
</div>

### Classifier Free Guidance

更进一步对分类器一项使用Bayes公式，实际上我们可以把训练分类器的步骤也给省略掉。此时只需要训练一个模型，这样的方式称为**Classifier Free Guidance**。在实践中通常会为无条件的情况设置一个专门的标签$$c=\emptyset$$，这样在训练时只需要按照概率随机将原始标签替换为空标签即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Lf9FBpx.png" width="100%">
</div>

目前主流的图片生成模型大多是基于Classifier Free Guidance的思路来进行训练的，和Classifier Guidance的结果相比它往往能够得到更加稳定和高质量的图片结果。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tSxac7b.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/NYW2d9w.png" width="100%">
</div>

至于为什么Classifier Free Guidance能够得到更高的生成质量，目前学术界也没有明确的结论，相关的理论仍在发展中。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/G92YVBv.png" width="100%">
</div>

## Introduction to Inverse Problems

本节课最后介绍了一下**逆问题(inverse problem)**。逆问题在工程中是一类非常常见的问题，在很多场景中我们无法获取原始数据$$x_0$$而只能获取被污染后的数据$$y = \mathcal{A}(x_0) + \sigma_y z$$，而我们的目标则是从污染后的数据$$y$$上来恢复原始数据$$x_0$$。像是黑白图像上色、图像编辑以及超分辨率这样的任务都可以归类为逆问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/a8JFSaA.png" width="100%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Ru6VFKF.png" width="100%">
</div>

## Reference

- [Lecture 3 - Conditioning and Guidance](https://www.youtube.com/watch?v=HYIhJGUycjQ)
- [Guidance: a cheat code for diffusion models](https://sander.ai/2022/05/26/guidance.html)