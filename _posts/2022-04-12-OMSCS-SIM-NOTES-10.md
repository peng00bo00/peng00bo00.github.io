---
layout: post
title: OMSCS-SIM课程笔记10-Comparing Systems
date: 2022-04-12
description: 系统评价
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节介绍利用随机模拟的结果对系统进行对比和评价的方法。
{: .block-preface }


## Introduction and Review of Classical Confidence Intervals

**置信区间(confidence intervals, CI)**是系统评价最常用的方法，因此我们首先来回顾一下CI的概念和性质。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/EumBLyk.png" width="80%">
</div>

### Confidence Intervals for the Mean

假设有一系列独立同分布的正态分布随机变量$$\{ X_i \}$$，我们可以计算它们的样本均值$$\bar{X}_n$$和方差$$S_X^2$$。在此基础上，我们构造统计量$$T$$：

$$
T = \frac{\bar{X}_n - \mu}{\sqrt{S_X^2 / n}} = \frac{\frac{\bar{X}_n - \mu}{\sqrt{\sigma^2 / n}}}{\sqrt{S_X^2 / \sigma^2}}
$$

可以证明统计量$$T$$服从参数为$$n-1$$的$$t$$分布，$$T \sim t(n-1)$$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/8N1fMJu.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/kbIHPDX.png" width="80%">
</div>

利用统计量$$T$$可以得到总体方差$$\sigma^2$$未知条件下样本均值的置信区间：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/b2WhMJY.png" width="80%">
</div>

### Confidence Intervals Difference Two Means

接下来考虑样本来自两个正态分布的情况。假设$$\{ X_i \}$$来自正态分布$$N(\mu_X, \sigma_X^2)$$，$$\{ Y_i \}$$来自$$N(\mu_Y, \sigma_Y^2)$$，根据我们已知的信息可以构造出不同的置信区间。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/VBrJuRQ.png" width="80%">
</div>

如果$$\{ X_i \}$$和$$\{ Y_i \}$$有着相同但是未知的方差，可以构造出它们均值差异的置信区间为：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Gn8UV4V.png" width="80%">
</div>

如果$$\{ X_i \}$$和$$\{ Y_i \}$$有着不相同的方差，可以构造出它们均值差异的置信区间为：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oyENHzR.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lF1CvDR.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PoSROpM.png" width="80%">
</div>

### Paired CI Difference Two Means

现实中另一种常见的情况是$$\{ X_i \}$$和$$\{ Y_i \}$$自身满足独立同分布，但每一对$$(X_i, Y_i)$$则存在相互关联。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GEHZFIb.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ToB43Cj.png" width="80%">
</div>

在这种情况下可以构造出差异$$D$$的置信区间：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ImQXF8T.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qnjKNtY.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/s2y3hOT.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YWzkXPM.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lgPM2VU.png" width="80%">
</div>

## Comparison of Simulated Systems

在随机模拟中数据之间往往不满足独立同分布假设，在这种情况下我们需要一些额外的处理来推导出置信区间。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/U0KJPLl.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PAy7qws.png" width="80%">
</div>

### Confidence Intervals for Mean Differences

利用独立重复试验，我们可以估计两个系统自身的均值和方差：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cLoKVKK.png" width="80%">
</div>

得到均值和方差后就可以计算两个系统差异的置信区间：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/rWSWTo3.png" width="80%">
</div>

另一种可行的策略是把两个系统的试验看成是一对，然后利用[Paired CI Difference](#paired-ci-difference-two-means)的方法来估计置信区间：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UPcwkd5.png" width="80%">
</div>

### Variance Reduction Techniques

#### Common Random Numbers

在比较不同系统时我们可以通过对仿真过程的控制来减少随机模拟的方差。比如说我们可以将不同系统的随机输入设置为相同的值，这样就可以比较它们在同样条件下的性能。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/i1sk5bD.png" width="80%">
</div>

进一步可以证明这种使用相同随机输入的方法可以减少估计的方差并且能够给出更紧的置信区间。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/pf8tFQe.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QvUW7TZ.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/R5Ji7zy.png" width="80%">
</div>

#### Antithetic Random Numbers

减少方差的另一种方法是使用**对立变量(antithetic random numbers)**，它的思想是主动引入负相关的随机变量来减少估计的方差。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UlVNRWi.png" width="80%">
</div>

以Monte Carlo积分为例，假设待估计的积分为$$\int_1^2 \frac{1}{x} \ dx$$，直接进行采样可以得到如下结果：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eoPOkrJ.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eEWR3pu.png" width="80%">
</div>

接下来我们构造这些随机样本的对立变量$$1-U_i$$重新进行估计，然后对两次估计取均值可以得到一个更准确的积分估计值：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/LHyJXNm.png" width="80%">
</div>

#### Control Variates

除此之外还可以使用**控制变量(control variates)**来减少方差。假设我们已知一个与$$X$$正相关的随机变量$$Y$$，我们可以利用$$Y$$来构造一个关于$$X$$的估计：

$$
C = \bar{X} - \beta (Y - \mathbb{E}[Y])
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7rE8L0q.png" width="80%">
</div>

可以证明这个新的估计$$C$$是无偏的，而且有着比$$\bar{X}$$更小的方差。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/v2R0spq.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/NpzM75w.png" width="80%">
</div>

## Ranking and Selection Methods

很多任务中我们需要从多个系统中挑选出其中最好的那个，此时就需要对这些系统进行排序。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/h2T6V7H.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/OtDABLW.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PtPwSrr.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fgXXhdq.png" width="80%">
</div>

### Find the Normal Distribution with the Largest Mean

#### Introduction

在排序问题中最常见的情况是从k个未知参数的正态分布中选出均值最大的那个分布。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/smqAcry.png" width="80%">
</div>

此时我们可以使用**无差异区域(indifference zone)**的方法进行选择。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sLvPcdS.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/a7xAhVi.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/BCI0rYb.png" width="80%">
</div>

#### Single-Stage Procedures

indifference zone方法中最基本的是single-stage procedure。single-stage procedure非常简单：我们假设k个系统都具有相同的方差，此时只需要根据样本均值进行排序并且选出均值最大的那个系统即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Lj38AqS.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PZKsHO9.png" width="80%">
</div>

single-stage procedure的难点在于计算统计样本均值时所需的样本数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lezBojK.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/xpmzCWn.png" width="80%">
</div>

所需样本数$$n$$的推导过程比较复杂，这里简单记录如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/wcR9bWm.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fAzB8o0.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9rkewdE.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CvKoWZZ.png" width="80%">
</div>

#### Two-Stage Procedures

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nS4zS5R.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PnrN0KH.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Vvc1XtC.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Q8lkQp5.png" width="80%">
</div>

#### Multi-Stage Procedure

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mc4gCmK.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Bi5Scfd.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vpda3qR.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Mbkol8K.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YpqMGJ6.png" width="80%">
</div>

### Find the Bernoulli with the Largest Success Probability

对于Bernoulli系统，我们希望能够挑选出概率最大的那个。这里同样使用了indifference zone方法进行排序。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/gNo1drC.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ot2jgXj.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oQkgkrT.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/z4S4h2W.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4g4xkiP.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tkZSJgV.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cVNPIxj.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/i0F4ofP.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CKOnNB4.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ne2nMzH.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/iLuJvfh.png" width="80%">
</div>

### Find the Most Probable Multinomial Cell

对于multinomial分布的系统，我们希望了解系统的哪个输出具有最高的概率。这样的问题同样可以使用indifference zone方法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Wdo7MJe.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QlPePm3.png" width="80%">
</div>

根据多项分布的性质，我们可以计算产生样本的概率以及基于样本进行选择时结果正确的概率。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IAbtIj4.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2VpqymO.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yELslCs.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/KO7gvL7.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XJCPV95.png" width="80%">
</div>

在进行选择时同样需要考虑所需的样本数$$n$$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/r8aC3n6.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ZRuPqrp.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vWsbHNj.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/iv00aKG.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/d4gmdqz.png" width="80%">
</div>

## Reference

- [Comparing Systems](https://www2.isye.gatech.edu/~sman/courses/6644/Module10-ComparingSystems-201128.pdf)