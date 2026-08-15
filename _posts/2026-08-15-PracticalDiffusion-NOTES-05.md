---
layout: post
title: MIT 6.S183课程笔记5-Applications of Diffusion Models
date: 2026-08-15
description: 扩散模型的应用
tags: DL Diffusion
categories: MIT-6.S183
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


> 这个系列是[MIT 6.S183 - A Practical Introduction to Diffusion](https://www.practical-diffusion.org/)的同步课程笔记。本门课程面向对扩散模型感兴趣的学生和研究者，从最底层开始逐步介绍扩散模型的数学原理以及各种应用。本节课主要介绍扩散模型的一些应用。
{: .block-preface }

## Recap

对于扩散模型的前向过程，我们通过不断地添加噪声来将原始数据扩散为纯粹的噪声。而在图像这样的数据，添加细微的噪声实际上并不会干扰太多图像的识别过程，只有当噪声的比例很大时噪声的效果才会比较明显。因此对于图像生成这样的任务，我们不需要太过关注噪声水平比较低时的情况。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Jky4awW.png" width="100%">
</div>

实际上在图像生成任务中，我们常常会使用**噪声调度器(noise scheduler)**来控制不同时间步上的噪声水平。一些高级的调度器甚至会在训练和生成采样节点使用不同的调度策略，从而提高训练效率以及生成数据的质量。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/L2p87ip.png" width="100%">
</div>

除了噪声调度外，另一种处理噪声的方式是修改训练的目标函数。在标准的扩散模型中，一般是使用噪声的误差作为训练目标；而在更现代的一些模型中，我们还可以使用预测的数据或是速度场来作为训练目标。实际上这些训练目标函数在数学上都是相互等价的，但在实践中它们会由于噪声水平$$\sigma$$而产生一定的差异，进而对训练过程产生影响。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nDtp5sC.png" width="100%">
</div>

## Applications

### High Resolution Image Generation

图像生成任务中的一个现实需求是生成高分辨率的图像。处理这样的任务一个简单的想法是直接训练更大的扩散模型来实现，但更大的模型往往意味着更多的计算需求以及训练难度；同时，对于图像这样的数据，很多高频的信息往往是不太重要的。这些都说明直接使用像素空间进行模型训练并不是一个高效的处理方法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/adjjeMv.png" width="100%">
</div>

目前处理高分辨率图像生成的核心技术是**latent diffusion**，它的主要思想是在低分辨率的latent space中实现生成的过程，从而规避直接在高分辨率上生成的计算代价。具体来说，latent diffusion的训练过程可以大致分为三步:

1. 在高分辨率图像上训练一个AutoEncoder，其中encoder将高分辨率图像编码到低分辨率的latent space上，而decoder则将latent space中的样本解码回高分辨空间。

2. 固定encoder，在对应的latent space上面训练一个扩散模型。

3. 完成训练后，把扩散模型生成的结果输入到decoder中，最终获得高分辨率的图像。

stable diffusion就是采用latent diffusion这样的技术路线。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JwmvPo2.png" width="100%">
</div>

当然在训练latent diffusion时还需要很多trick，比如说在训练AutoEncoder的时候使用了大量VAE以及GAN的成熟经验，这样才能找到合适高效的latent space。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/37JFr33.png" width="100%">
</div>

同时值得一提的是latent diffusion本身也具有一些缺陷，比如说latent space只是对原始像素空间的一个压缩，它本身也不一定是非常高效的信息表达。如何更高效地生成高分辨率图像仍然是目前学术界以及工业级积极探索的一个领域。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/50GLG2d.png" width="100%">
</div>

### Further Applications

除了图像生成外，扩散模型在CV领域还有很多让人惊喜的应用。文生视频在最近几年有了很大的突破，它的底层技术架构与latent diffusion是差不多的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/D50qCKO.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ZGP6noK.png" width="100%">
</div>

另一个应用场景是对生成图像进行编辑，以及生成更加可控的图像生成结果。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/01gZi6w.png" width="100%">
</div>

扩散模型还可以用来生成一些让人产生视觉错觉的图像。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cdZloIY.png" width="100%">
</div>

对于平面设计相关的场景，很多时候设计师想要获得对生成图像更加精细的控制能力。ControlNet以及后续工作就提供了更加丰富的控制信号类型，方便设计师基于不同模态的输入数据来生成更加个性化的结果。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QguJA9y.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ol2M7wB.png" width="100%">
</div>

类似的思路同样可以用在视频中，从而获得更加可控的视频生成结果。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qVOdYtX.png" width="100%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lLcyGsr.png" width="100%">
</div>

在视频生成的基础上，更有野心的场景是**世界模型(world model)**。我们希望能基于现有的海量视频数据直接模拟整个世界的物理过程，这同样是目前最火爆的AI应用领域之一。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lnNQ4Qv.png" width="100%">
</div>

## Why does Diffusion Work So Well?


## Reference

- [Lecture 5 - Applications of Diffusion Models](https://www.youtube.com/watch?v=idJcN28lw80)