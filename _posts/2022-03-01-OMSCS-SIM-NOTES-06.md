---
layout: post
title: OMSCS-SIM课程笔记06-Random Number Generation
date: 2022-03-01
description: 均匀分布随机数的生成方法
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节介绍各种均匀分布随机数的生成方法。
{: .block-preface }


生成(0, 1)区间上的均匀分布随机数是各种随机模拟技术的基础，利用Uniform(0,1)可以构造出各种不同类型的随机变量。因此随机数生成算法的目标是产生一个伪随机数序列且它们服从相互独立的均匀分布。

<div align=center>
<img src="https://i.imgur.com/zYV9bH4.png" width="80%">
</div>

## Some lousy generators

早期的随机数生成算法是基于物理过程来产生真实的随机数，这类方法产生的结果有很好的随机性但几乎无法进行重复试验。在此基础上人们还整理了随机数表格然后通过查表的方式来产生可复现的随机数。

<div align=center>
<img src="https://i.imgur.com/4zHNcm9.png" width="80%">
</div>

后来von Neumann发明了**平方取中法(mid-square method)**来生成随机数，然而这种方法生成的随机数效果并不好。

<div align=center>
<img src="https://i.imgur.com/fO6ZTxP.png" width="80%">
</div>

人们还发明了**加同余法(additive congruential method)**来生成随机数，但它同样具有一些问题。

<div align=center>
<img src="https://i.imgur.com/RnzAqxm.png" width="80%">
</div>

## Linear Congruential Generators

前面介绍的这些随机数生成算法现在已经弃用，目前最常用的算法是**线性同余法(linear congruential generator, LCG)**。LCG的一个缺陷在于它会产生循环出现的随机数，因此在选择参数时要额外小心。

<div align=center>
<img src="https://i.imgur.com/WUHPKKY.png" width="80%">
<img src="https://i.imgur.com/UcAMLJJ.png" width="80%">
<img src="https://i.imgur.com/LWJKrZp.png" width="80%">
<img src="https://i.imgur.com/yxP7QHi.png" width="80%">
</div>

## Tausworthe Generator

除了LCG外目前常用的随机数生成器还包括**Tausworthe生成器(Tausworthe generator)**。Tausworthe生成器定义了一个比特序列$$B_i$$，它通过位运算来生成新的比特。

<div align=center>
<img src="https://i.imgur.com/fUsCb5m.png" width="80%">
<img src="https://i.imgur.com/WoaYgk8.png" width="80%">
</div>

要获得随机数只需要先将比特序列视为二进制数，再转换成小数即可。

<div align=center>
<img src="https://i.imgur.com/mssmCig.png" width="80%">
</div>

## Generalizations of LCGs

在实际使用LCG时一般会进行一些拓展，最常用的拓展方法是将线性变换推广成向量乘法，即每次取前$$q$$次的结果来计算新的随机数。

<div align=center>
<img src="https://i.imgur.com/AKylKQm.png" width="80%">
</div>

另一种常用的拓展方法是将两个LCG组合起来形成一个新的随机数生成器。

<div align=center>
<img src="https://i.imgur.com/uOJgFOJ.png" width="80%">
<img src="https://i.imgur.com/E3eJG9J.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/5pFxexT.png" width="80%">
</div>

## Some Theory

关于LCG还有很多理论上的性质，这里直接介绍相关的结论。

<div align=center>
<img src="https://i.imgur.com/hhHHnjz.png" width="80%">
<img src="https://i.imgur.com/tmwrFpS.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/UHgwC7v.png" width="80%">
<img src="https://i.imgur.com/5z087un.png" width="80%">
</div>

## Statistical Tests

衡量随机数生成器的好坏需要一些统计检验的相关知识，这里简单介绍一下。对于我们的问题主要涉及两类统计检验，其一是**拟合优度检验(goodness-of-fit test)**用来检验样本是否来自均匀分布Uniform(0, 1)，另一类是**独立性检验(independence test)**用来检验样本之间是否相互独立。如果我们的生成器能够通过这两类检验那么就可以认为生成器是一个好的算法。

<div align=center>
<img src="https://i.imgur.com/zVQWFor.png" width="80%">
</div>

在统计检验中我们几乎无法去证明一个假设，因此合适的做法是使用反证法，即选择一个假设然后去否定它。这个待否定的假设称为**原假设(null hypothesis)**，而它的对立假设称为**备选假设(alternative hypothesis)**。统计检验的基本策略是假定原假设是正确的然后构造一个小概率事件，如果这个小概率事件发生在样本上则说明原假设错误，拒绝原假设。

<div align=center>
<img src="https://i.imgur.com/KtwSsVG.png" width="80%">
<img src="https://i.imgur.com/iWBmPS7.png" width="80%">
</div>

因此在假设检验中存在两种可能的错误：其一是$$H_0$$正确的情况下拒绝了$$H_0$$，这称为第I类错误，它发生的概率记为$$\alpha$$；另一种情况是$$H_0$$错误的情况下没有拒绝它，这称为第II类错误，它发生的概率记为$$\beta$$。在大多数问题中我们只关心第一类错误的概率，$$\alpha$$也称为**显著性水平(significance level)**。

<div align=center>
<img src="https://i.imgur.com/rtMgOTm.png" width="80%">
</div>

### $$\chi^2$$ Goodness-of-Fit Test

最常用的拟合优度检验是$$\chi^2$$拟合优度检验。假设观测到的样本服从均匀分布Uniform(0, 1)，接着把(0, 1)区间划分为$$k$$份，此时样本$$R_j$$落在任意区间上的概率为$$\frac{1}{k}$$。记第$$i$$个区间中的样本数为$$O_i$$，显然$$O_i$$服从二项分布Bin($$n$$, $$\frac{1}{k}$$)。这样就可以构造统计量$$\chi_0^2$$来描述$$O_i$$和它的期望$$E_i$$之间的差异：

$$
\chi_0^2 = \sum_{k=1}^n \frac{(O_i - E_i)^2}{E_i}
$$

<div align=center>
<img src="https://i.imgur.com/HZ97b9H.png" width="80%">
<img src="https://i.imgur.com/p8IReoJ.png" width="80%">
</div>

显然$$\chi_0^2$$越大说明原假设越有可能是错误的。实际上$$\chi_0^2$$服从自由度为$$k-1$$的$$\chi^2$$分布，因此我们可以使用$$\chi^2$$分布的分位数进行检验：如果$$\chi_0^2 \gt \chi_{\alpha, k-1}^2$$说明假设错误，样本不服从均匀分布Uniform(0, 1)。

<div align=center>
<img src="https://i.imgur.com/N4XL2fB.png" width="80%">
<img src="https://i.imgur.com/n2vOrWg.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/opR28tO.png" width="80%">
</div>

### Tests for Independence

介绍独立性检验前我们首先需要引入**run**的概念：我们把随机序列中相似的一段子序列称为一个run。直观上来看，如果一个随机序列中的run过多或是过少都说明这个序列中的样本不是相互独立的。

<div align=center>
<img src="https://i.imgur.com/5ypSUtA.png" width="80%">
<img src="https://i.imgur.com/njP9Co1.png" width="80%">
</div>

对于均匀分布，我们可以先用相邻数字之间的相对关系把数字转换成布尔值然后来获得run，这样的检验方法称为**up and down run test**。此时序列中run的数量$$A$$服从正态分布Normal($$\frac{2n-1}{3}$$, $$\frac{16n-29}{90}$$)，因此可以利用标准正态分布来进行假设检验。

<div align=center>
<img src="https://i.imgur.com/PvvgoMt.png" width="80%">
<img src="https://i.imgur.com/bOsmOe1.png" width="80%">
</div>

另一种常见的做法是把0.5作为阈值来构造出run，此时同样可以利用标准正态分布来进行检验。

<div align=center>
<img src="https://i.imgur.com/xTSpzGG.png" width="80%">
</div>

除了使用run外也可以利用相关性来检验样本之间是否相互独立，这种方法称为**相关性检验(correlation test)**。

<div align=center>
<img src="https://i.imgur.com/3q27VBq.png" width="80%">
</div>

## Reference

- [Random Number Generation](https://www2.isye.gatech.edu/~sman/courses/6644/Module06-RandomNumberGenerationSlides-200322.pdf)