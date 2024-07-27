---
layout: post
title: OMSCS-SIM课程笔记08-Input Analysis
date: 2022-03-29
description: 输入分析
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节介绍对随机模拟的输入进行分析的方法。
{: .block-preface }


## Intro to Input Analysis

到目前为止我们已经介绍过如何进行随机模拟以及如何生成各种类型的随机变量，但进行具体的模拟前我们还需要判断该选择什么类型的随机变量进行模拟，确定使用随机变量的过程称为**输入分析(input analysis)**。

<div align=center>
<img src="https://i.imgur.com/7yRA8U7.png" width="80%">
<img src="https://i.imgur.com/tTfPT1l.png" width="80%">
</div>

显然输入分析对于随机模拟非常重要，它的基本流程包括收集数据、估计随机变量的分布以及验证概率分布是否符合收集到的数据。

<div align=center>
<img src="https://i.imgur.com/MjUJW0l.png" width="80%">
</div>

**直方图(histogram)**是输入分析最基本的方法，利用直方图我们可以直观地显示随机变量的分布情况。实际上当收集到的数据足够多时，直方图会收敛到数据真实分布的PDF上。

<div align=center>
<img src="https://i.imgur.com/CzihpnL.png" width="80%">
</div>

**茎叶图(stem-and-leaf diagram)**是输入分析的另一种常用方法，它可以看做是直方图的一个变种。

<div align=center>
<img src="https://i.imgur.com/irEXgj9.png" width="80%">
</div>

在确定随机变量的分布时需要思考随机变量是离散变量还是连续变量、是一维变量还是多维变量等等。

<div align=center>
<img src="https://i.imgur.com/oGO5FJD.png" width="80%">
</div>

常用随机变量分布的应用场景如下：

<div align=center>
<img src="https://i.imgur.com/4EElTCW.png" width="80%">
<img src="https://i.imgur.com/qR8CV1h.png" width="80%">
</div>

总结一下，输入分析的基本思路是根据收集到的数据来选择一个概率分布，然后利用一些统计检验来验证我们的选择时合理的。

<div align=center>
<img src="https://i.imgur.com/Qz1HxXF.png" width="80%">
</div>

## Point Estimation

### Intro to Estimation

在介绍具体的参数估计方法前我们需要引入一些相关的数学概念。我们定义**统计量(statistic)**为观测值到样本的函数，常用的样本均值和方差都是统计量。

<div align=center>
<img src="https://i.imgur.com/Hwhw19E.png" width="80%">
</div>

显然统计量也是随机变量，而我们则可以通过统计量来估计概率分布的参数。此时得到的参数估计称为是一个**点估计(point estimator)**。

<div align=center>
<img src="https://i.imgur.com/QWtOSmp.png" width="80%">
</div>

我们熟悉的样本均值和方差都是常见的点估计。一个好的点估计需要保证它的期望尽可能等于参数的真值，而且有比较小的方差。

<div align=center>
<img src="https://i.imgur.com/VCdtulE.png" width="80%">
</div>

### Unbiased Estimation

如果点估计的期望等于参数的真值，则称其为**无偏估计(unbiased estimator)**。可以证明样本均值和样本方差都是无偏估计。

<div align=center>
<img src="https://i.imgur.com/sujwSeP.png" width="80%">
<img src="https://i.imgur.com/KFIbhQi.png" width="80%">
<img src="https://i.imgur.com/jOnwMne.png" width="80%">
<img src="https://i.imgur.com/6Yj8ZIW.png" width="80%">
</div>

### Mean Squared Error

无偏估计的期望等于参数的真值但有时会有很大的方差，这种情况下的无偏估计仍然不是一个好的估计。为了同时考虑方差的影响，我们引入**均方误差(mean squared error)**的概念。均方误差是点估计与参数真值之间平方误差的期望，可以证明均方误差可以分解为估计的方差与偏差平方之和。

<div align=center>
<img src="https://i.imgur.com/7vZdTuR.png" width="80%">
<img src="https://i.imgur.com/RtpVcvh.png" width="80%">
</div>

显然均方误差越低的估计性能越好。

<div align=center>
<img src="https://i.imgur.com/YQryEbA.png" width="80%">
<img src="https://i.imgur.com/N7oaTyu.png" width="80%">
</div>

### Maximum Likelihood Estimation

**极大似然估计(maximum likelihood estimation, MLE)**是最常用的参数估计方法。它的思想是利用样本的联合概率密度函数来估计产生这样样本最有可能的参数。

<div align=center>
<img src="https://i.imgur.com/0cYryyn.png" width="80%">
</div>

以指数分布为例，我们定义样本的**似然函数(likelihood function)** $$L(\lambda)$$为每个样本PDF的乘积，然后通过最大化似然函数来获得参数$$\lambda$$。有时直接处理似然函数会比较困难，因此常用的技巧是对似然函数取对数并最大化对数似然函数。

<div align=center>
<img src="https://i.imgur.com/3kHEhFi.png" width="80%">
<img src="https://i.imgur.com/kpizHXA.png" width="80%">
<img src="https://i.imgur.com/8nAuIaA.png" width="80%">
</div>

类似地我们可以计算Bernoulli分布的MLE，不难证明此时MLE是一个无偏估计。

<div align=center>
<img src="https://i.imgur.com/v3bBG8Y.png" width="80%">
<img src="https://i.imgur.com/WNhd4Vm.png" width="80%">
</div>

正态分布的MLE要稍微复杂一些，不过容易证明$$\mu$$的MLE与样本均值相同。但需要注意的是$$\sigma^2$$的MLE不是样本方差，不过当样本数量很大时它们之间的差异可以忽略。

<div align=center>
<img src="https://i.imgur.com/mEp6TYo.png" width="80%">
<img src="https://i.imgur.com/oyKL7iI.png" width="80%">
<img src="https://i.imgur.com/1boS1uH.png" width="80%">
<img src="https://i.imgur.com/LM4wwKj.png" width="80%">
</div>

Gamma分布的MLE还要更复杂一些，实际上我们没有办法找到MLE的解析解

<div align=center>
<img src="https://i.imgur.com/ngwR5iP.png" width="80%">
<img src="https://i.imgur.com/fhPUBs6.png" width="80%">
<img src="https://i.imgur.com/XuM57qA.png" width="80%">
</div>

均匀分布的MLE等于样本中的最大值。

<div align=center>
<img src="https://i.imgur.com/MS6YbuV.png" width="80%">
</div>

MLE的一个特点是具有不变性，$$h(\theta)$$的MLE等于直接把$$h$$作用在$$\hat{\theta}$$上。

<div align=center>
<img src="https://i.imgur.com/5mNslPX.png" width="80%">
</div>

但需要注意的是这样的变换不能保证无偏性，无偏MLE经过$$h$$作用后不一定是无偏的。

<div align=center>
<img src="https://i.imgur.com/TSu3aLd.png" width="80%">
</div>

利用MLE的不变性我们甚至还可以直接估计随机变量的CDF。

<div align=center>
<img src="https://i.imgur.com/Ajvuvuf.png" width="80%">
</div>

### Method of Moments

除了点估计和MLE之外，**矩估计(method of moments, MOM)**也是一种常用的参数估计方法。它的思想是令样本的$$k$$阶矩等于概率分布的$$k$$阶矩，并以此求解出参数。

<div align=center>
<img src="https://i.imgur.com/W4WHis8.png" width="80%">
<img src="https://i.imgur.com/flKe4E3.png" width="80%">
</div>

对于Poisson分布我们可以选择均值或是方差进行矩估计，此时估计出的参数$$\hat{\lambda}$$是不同的。

<div align=center>
<img src="https://i.imgur.com/oaIAXqI.png" width="80%">
</div>

和MLE相比矩估计在推导和计算上往往要容易很多。

<div align=center>
<img src="https://i.imgur.com/VJx1rnS.png" width="80%">
<img src="https://i.imgur.com/IjIWM6Z.png" width="80%">
<img src="https://i.imgur.com/0tPDzW1.png" width="80%">
</div>

## Goodness-of-Fit Tests

完成参数估计后我们还需要验证样本是否来自估计出的分布，此时就需要使用**拟合优度检验(goodness-of-fit tests)**。

<div align=center>
<img src="https://i.imgur.com/ArDwEyW.png" width="80%">
</div>

我们前面介绍过[$$\chi^2$$拟合优度检验](/2022/03/01/OMSCS-SIM-NOTES-06.html#chi2-goodness-of-fit-test)，这里把它推广到任意形式的概率分布上。

<div align=center>
<img src="https://i.imgur.com/1En7tN9.png" width="80%">
<img src="https://i.imgur.com/SsjAVc5.png" width="80%">
<img src="https://i.imgur.com/kAoZqqG.png" width="80%">
</div>

这里我们使用一个几何分布的例子来说明从参数估计到拟合优度检验的流程：

<div align=center>
<img src="https://i.imgur.com/LyqsJYk.png" width="80%">
<img src="https://i.imgur.com/Vz16nCG.png" width="80%">
<img src="https://i.imgur.com/4cQDRMv.png" width="80%">
<img src="https://i.imgur.com/AHiewUK.png" width="80%">
<img src="https://i.imgur.com/jC6as39.png" width="80%">
</div>

### Exponential Example

对于连续型随机变量我们同样可以使用$$\chi^2$$拟合优度检验。

<div align=center>
<img src="https://i.imgur.com/Ynov9DD.png" width="80%">
<img src="https://i.imgur.com/0llj6Hw.png" width="80%">
<img src="https://i.imgur.com/gUhesJP.png" width="80%">
</div>

### Weibull Example

这里我们再举一个Weibull分布的例子。

<div align=center>
<img src="https://i.imgur.com/2KOLLnT.png" width="80%">
<img src="https://i.imgur.com/UxfLUzi.png" width="80%">
<img src="https://i.imgur.com/QpsB1pN.png" width="80%">
<img src="https://i.imgur.com/SbfCE6D.png" width="80%">
<img src="https://i.imgur.com/i99SOB2.png" width="80%">
<img src="https://i.imgur.com/sUqQnwv.png" width="80%">
<img src="https://i.imgur.com/FOe29qo.png" width="80%">
<img src="https://i.imgur.com/mpVDyDa.png" width="80%">
</div>

### More Goodness-of-Fit Test

除了$$\chi^2$$拟合优度检验之外还有很多不同形式的检验方法。这里介绍一下**Kolmogorov-Smirnov拟合优度检验(Kolmogorov-Smirnov goodness-of-fit test，K-S test)**，不同于$$\chi^2$$拟合优度检验K-S检验通过样本的经验分布的CDF来验证其是否来自给定的分布。当样本数量足够大时，经验分布的CDF应该趋向于真实分布的CDF。

<div align=center>
<img src="https://i.imgur.com/N5tED4r.png" width="80%">
<img src="https://i.imgur.com/vBq20zi.png" width="80%">
</div>

实际上**Glivenko–Cantelli定理(Glivenko–Cantelli theorem)**指出，当样本数趋于无穷时经验分布的CDF会收敛到真实分布的CDF。因此我们可以计算经验分布和目标分布之间的最大差异，当这个最大差异足够小时即可认为样本确实来自我们估计的分布。

<div align=center>
<img src="https://i.imgur.com/QQgggC3.png" width="80%">
</div>

以均匀分布Unif(0, 1)为例，K-S检验的流程如下：

<div align=center>
<img src="https://i.imgur.com/57fvZ0d.png" width="80%">
<img src="https://i.imgur.com/Q36QR61.png" width="80%">
<img src="https://i.imgur.com/zSvcLnk.png" width="80%">
</div>

总结一下，K-S检验适用于各种不同类型的概率分布而且在计算时都比较容易。除了K-S检验之外其它常用的拟合优度检验还包括Anderson-Darling检验、Cramér-von Mises检验、Shapiro-Wilk检验等。

<div align=center>
<img src="https://i.imgur.com/qNit3Ma.png" width="80%">
</div>

## Problem Children

本节课最后讨论了实际进行输入分析时的常见问题和解决方法。

<div align=center>
<img src="https://i.imgur.com/PLh1avb.png" width="80%">
</div>

### No / Little Data

最常见的问题是收集数据时没有办法获得足够多的数据，此时可以咨询一些专家来获得数据分布的统计特征并以此来生成随机样本。

<div align=center>
<img src="https://i.imgur.com/aMkL65K.png" width="80%">
<img src="https://i.imgur.com/hQUR36n.png" width="80%">
</div>

### Nonstandard / Goofy / Mixture Distributions

有时样本的分布不是常见的分布，甚至可能是由多个分布混合而成的。此时可以使用一些混合模型来估计这个分布。

<div align=center>
<img src="https://i.imgur.com/YCvZxjH.png" width="80%">
</div>

### Nonstationary Data

有时分布的参数不是一个固定的值，需要考虑参数随时间的变化。在这种情况下可以考虑使用非齐次Poisson过程的相关方法进行模拟。

<div align=center>
<img src="https://i.imgur.com/wfElYzG.png" width="80%">
</div>

### Multivariate / Correlated Data

另外样本之间可能是多维的，而且不一定满足独立同分布假设。此时可以使用多元随机变量或是时间序列的相关方法进行处理。

<div align=center>
<img src="https://i.imgur.com/6ZtCxp4.png" width="80%">
<img src="https://i.imgur.com/E6WIoQm.png" width="80%">
</div>

## Reference

- [Input Analysis](https://www2.isye.gatech.edu/~sman/courses/6644/Module08-InputAnalysis-210814.pdf)