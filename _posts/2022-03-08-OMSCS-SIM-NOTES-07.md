---
layout: post
title: OMSCS-SIM课程笔记07-Random Variate Generation
date: 2022-03-08
description: 常用的随机变量生成方法
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节介绍常用的随机变量生成方法。
{: .block-preface }


利用均匀分布U(0, 1)我们可以构造出各种不同类型的随机变量。

<div align=center>
<img src="https://i.imgur.com/FgpAKEH.png" width="80%">
</div>

## Inverse Transform Method

最基本的随机变量生成算法是**逆变换方法(inverse transform method)**，它的理论基础是**逆变换定理(inverse transform theorem)**：对于任意连续型随机变量$$X$$，把CDF作用在$$X$$上会得到均匀分布U(0, 1)。

<div align=center>
<img src="https://i.imgur.com/KSJ0H5A.png" width="80%">
<img src="https://i.imgur.com/9sEWTO8.png" width="80%">
</div>

要使用逆变换方法进行采样只需要从均匀分布中采样一个随机数$$U$$，然后利用CDF的反函数即可$$X = F^{-1}(U)$$。

<div align=center>
<img src="https://i.imgur.com/1f8kK7N.png" width="80%">
</div>

### Continuous Examples

很多常见分布的CDF都具有比较简单的解析形式，因此我们可以使用逆变换方法来生成来自这些分布的随机数。

<div align=center>
<img src="https://i.imgur.com/AGkD6WP.png" width="80%">
<img src="https://i.imgur.com/r0RmonG.png" width="80%">
<img src="https://i.imgur.com/w0xgHBh.png" width="80%">
</div>

但需要注意的是，正态分布的CDF是不具有解析形式的。如果一定要使用逆变换方法生成正态分布的随机数则可以通过一些近似方法来近似标准正态分布的CDF。

<div align=center>
<img src="https://i.imgur.com/XggONpi.png" width="80%">
<img src="https://i.imgur.com/pbtiPO3.png" width="80%">
</div>

得到标准正态分布的随机数后只要通过简单变形就可以获得其它均值和方差所定义的正态分布。

<div align=center>
<img src="https://i.imgur.com/mdzV34T.png" width="80%">
</div>

### Discrete Examples

对于离散型随机变量，可以把CDF转换成一个分段函数然后利用逆变换方法来进行采样。

<div align=center>
<img src="https://i.imgur.com/RDUVgEF.png" width="80%">
<img src="https://i.imgur.com/R5Cv7FR.png" width="80%">
</div>

对于几何分布Geom($$p$$)则可以通过取整函数来辅助进行采样：

<div align=center>
<img src="https://i.imgur.com/LtMQYFj.png" width="80%">
</div>

不过更常用的方法是利用几何分布和Bernoulli分布的关系来进行采样。

<div align=center>
<img src="https://i.imgur.com/EJRrLAq.png" width="80%">
</div>

而对于Poisson分布Pois($$\lambda$$)则可以根据需要动态拓展CDF表格，直到随机数落在指定的区间上。

<div align=center>
<img src="https://i.imgur.com/PA6dxAi.png" width="80%">
<img src="https://i.imgur.com/EQmrEUC.png" width="80%">
</div>

### Empirical Distributions

在很多情况下我们无法使用已知的分布来表示随机变量，此时可以利用**经验分布(empirical distributions)**来近似它的CDF。

<div align=center>
<img src="https://i.imgur.com/bIoV68A.png" width="80%">
</div>

得到均匀分布随机数$$U$$后可以在对应的区间上进行线性插值生成所需的随机数。

<div align=center>
<img src="https://i.imgur.com/2RPNuQ9.png" width="80%">
</div>

## Convolution Method

这里**卷积(convolution)**是指把随机变量加起来构造新的随机变量。比如说二项分布Bin($$n$$, $$p$$)可以看做是$$n$$个参数为$$p$$的Bernoulli分布之和。

<div align=center>
<img src="https://i.imgur.com/Sstcadj.png" width="80%">
</div>

类似地，三角分布Triangular(0, 1, 2)等于两个均匀分布U(0, 1)的和。

<div align=center>
<img src="https://i.imgur.com/4cimUqd.png" width="80%">
</div>

Erlang分布则是指数分布的和。

<div align=center>
<img src="https://i.imgur.com/nUyqVFC.png" width="80%">
</div>

根据CLT我们还可以利用大量的均匀分布来构造出标准正态分布。

<div align=center>
<img src="https://i.imgur.com/AZGPBYL.png" width="80%">
</div>

使用卷积来构造的其它常用分布如下：

<div align=center>
<img src="https://i.imgur.com/qm0ERsu.png" width="80%">
</div>

## Acceptance-Rejection Method

为了解决随机变量CDF无法计算的问题人们还开发出了**接受-拒绝方法(acceptance-rejection method)**来进行采样。接受-拒绝方法的思想是首先从一个已知的分布进行采样，如果生成的样本满足要求就接受它。

<div align=center>
<img src="https://i.imgur.com/v5BJhuj.png" width="80%">
</div>

接受-拒绝方法严格的定义如下：设我们想要采样的PDF为$$f(x)$$，已知的PDF为$$h(x)=\frac{t(x)}{c}$$，且它们满足

$$
t(x) \geq f(x)
$$

我们从$$h(x)$$采样一个样本$$Y$$，同时从均匀分布采样一个样本$$U$$。如果$$U$$满足

$$
U \leq \frac{f(Y)}{t(Y)}
$$

则接受样本$$Y$$，且认为$$Y$$来自于分布$$f(y)$$。

<div align=center>
<img src="https://i.imgur.com/n97MFdO.png" width="80%">
<img src="https://i.imgur.com/00zpMHQ.png" width="80%">
<img src="https://i.imgur.com/TGx62UA.png" width="80%">
</div>

### Proof

接下来我们证明一下接受-拒绝方法的正确性。记事件$$A$$为接受样本$$Y$$，那么按照接受-拒绝方法得到的样本具有CDF：

$$
P(X \leq x) = P(Y \leq x \vert A) = \frac{P(A, Y \leq x)}{P(A)}
$$

而对于事件$$A$$则有：

$$
\begin{aligned}
P(A \vert Y = y) &= P(U \leq g(Y) \vert Y = y) \\
&= P(U \leq g(y) \vert Y = y) \\
&= P(U \leq g(y)) \\
&= g(y)
\end{aligned}
$$

利用全概率公式可以得到联合概率：

$$
\begin{aligned}
P(A, Y \leq x) &= \int_{-\infty}^\infty P(A, Y \leq x \vert Y = y) h(y) \ dy \\
&= \int_{-\infty}^x P(A \vert Y = y) h(y) \ dy \\
&= \frac{1}{c} \int_{-\infty}^x P(A \vert Y = y) t(y) \ dy \\
&= \frac{1}{c} \int_{-\infty}^x g(y) t(y) \ dy \\
&= \frac{1}{c} \int_{-\infty}^x f(y) \ dy
\end{aligned}
$$

令$$x \to \infty$$可以得到：

$$
P(A) = \frac{1}{c} \int_{-\infty}^\infty f(y) \ dy = \frac{1}{c}
$$

把上面的这些式子带入CDF有：

$$
P(X \leq x) = \frac{P(A, Y \leq x)}{P(A)} = \int_{-\infty}^x f(y) \ dy
$$

因此$$X$$的PDF即为$$f(x)$$：

$$
\frac{d P(X \leq x)}{dx} = \frac{d}{dx} \int_{-\infty}^x f(y) \ dy = f(x)
$$

<div align=center>
<img src="https://i.imgur.com/q8zPyLo.png" width="80%">
<img src="https://i.imgur.com/Yr1tzb9.png" width="80%">
<img src="https://i.imgur.com/8K0NvPu.png" width="80%">
</div>

### Continuous Examples

在使用接受-拒绝方法进行采样时一般要求已知分布$$h(y)$$可以进行快速的采样，同时常数$$c$$需要尽可能接近1，这样可以保证不会拒绝掉太多的样本。

<div align=center>
<img src="https://i.imgur.com/cLPUtVc.png" width="80%">
</div>

对于CDF不可逆的函数我们无法使用逆变换方法进行采样，此时仍然可以使用接受-拒绝方法：

<div align=center>
<img src="https://i.imgur.com/IKD0SPg.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/AFRfc74.png" width="80%">
</div>

### Poisson Distribution

接受-拒绝方法的经典应用是生成Poisson分布的随机数，这里介绍一种接受-拒绝方法的变体。根据定义，Poisson分布的样本可以理解为计数过程单位时间内的到达数量，参数$$\lambda$$为单位时间内的平均到达数量。

<div align=center>
<img src="https://i.imgur.com/VMuDFMM.png" width="80%">
</div>

因此可以利用计数过程来生成Poisson分布的随机数。同时注意到相邻到达的时间间隔服从指数分布Exp($$\lambda$$)，这样就可以利用指数分布的随机数序列来模拟计数过程。

<div align=center>
<img src="https://i.imgur.com/BmaBBg8.png" width="80%">
</div>

上面的推导说明我们只需要从均匀分布中采样出一个随机数序列，选择它们累乘恰好小于$$e^{-\lambda}$$时的序数即为来自Poisson分布的样本。

<div align=center>
<img src="https://i.imgur.com/FklndIq.png" width="80%">
<img src="https://i.imgur.com/Hk4F1dO.png" width="80%">
</div>

对于参数$$\lambda$$比较大的情况还可以利用正态分布的随机数来近似Poisson分布。

<div align=center>
<img src="https://i.imgur.com/EpGjC8w.png" width="80%">
<img src="https://i.imgur.com/wFzb7WD.png" width="80%">
</div>

## Composition Method

**composition方法(composition method)**是指利用利用两个随机变量来构造出一个新的随机变量。如果随机变量$$X$$的CDF可以分解为某个随机变量的概率密度和另一个随机变量CDF乘积的形式，则可以使用composition方法通过对这两个随机变量进行采样来构造$$X$$。

<div align=center>
<img src="https://i.imgur.com/CAs9772.png" width="80%">
<img src="https://i.imgur.com/S2G2lLW.png" width="80%">
</div>

对于**Laplace分布(Laplace distribution)**，我们可以把它的CDF利用Bernoulli分布进行分解。进行采样时只需要根据Bernoulli样本的结果选择对应的CDF进行采样即可。

<div align=center>
<img src="https://i.imgur.com/LOCjxAd.png" width="80%">
<img src="https://i.imgur.com/vQFsQef.png" width="80%">
</div>

## Special-Case Techniques

对于某些特定的分布人们还开发出了一些特殊的采样方法。

### Box–Muller Method

**Box–Muller方法(Box–Muller method)**是利用两个独立均匀分布来构造标准正态分布的方法。进行采样时只需要先采样出两个相互独立的均匀分布样本$$U_1$$和$$U_2$$，然后进行简单的变形即可。

<div align=center>
<img src="https://i.imgur.com/j6s9up1.png" width="80%">
</div>

利用Box–Muller方法还可以证明正态分布相关的一些性质。比如说$$\chi^2(2)$$分布等价于指数分布Exp(1/2)、利用正切函数来构造Cauchy分布等。

<div align=center>
<img src="https://i.imgur.com/5e8aYxX.png" width="80%">
<img src="https://i.imgur.com/51thSX0.png" width="80%">
</div>

除了Box–Muller方法外也可以使用**Polar方法(Polar method)**来构造标准正态分布，试验表明Polar方法的计算效率要比Box–Muller方法略高一些。

<div align=center>
<img src="https://i.imgur.com/vXBxCDl.png" width="80%">
</div>

### Order Statistics

**次序统计量(order statistics)**定义为独立同分布的随机变量序列$$X_1, \dots , X_n$$中最小的那个，即$$Y = \min \{ X_1, \dots, X_n \}$$。显然$$Y$$也是一个随机变量，它的CDF记为$$G(y)$$可以通过$$X_i$$的CDF来进行构造。

<div align=center>
<img src="https://i.imgur.com/rRJHDGb.png" width="80%">
<img src="https://i.imgur.com/JCELEHK.png" width="80%">
</div>

$$G(y)$$看起来比较复杂，但当$$X_i$$取某些特定的概率分布时$$G(y)$$往往具有十分简洁的形式。比如说当$$X_i$$来自指数分布Exp($$\lambda$$)时，$$Y$$服从参数为$$n \lambda$$的指数分布。

<div align=center>
<img src="https://i.imgur.com/6poWTqI.png" width="80%">
</div>

对于一些常用概率分布有时可能无法直接计算它们的CDF。此时可以通过概率分布之间的关系，利用简单的概率分布来构造出复杂的概率分布。

<div align=center>
<img src="https://i.imgur.com/Vx7tdPF.png" width="80%">
</div>

## Multivariate Normal Distribution

前面我们只介绍了如何生成一维随机变量的样本，在这一小节中则关注高维的正态分布。首先考虑二维的情况，对于二维正态分布我们需要两个维度上的均值、方差以及它们的相关系数来描述整个概率分布。

<div align=center>
<img src="https://i.imgur.com/OssdtVY.png" width="80%">
</div>

对于更高维度的情况我们只需要把均值和方差的概念推广到矩阵即可，因此描述高维正态分布只需要一个均值向量$$\pmb{\mu}$$和协方差矩阵$$\pmb{\Sigma}$$。

<div align=center>
<img src="https://i.imgur.com/oW5iw6I.png" width="80%">
</div>

从高维正态分布直接进行采样是比较困难的，但可以利用标准正态分布进行构造。记标准正态分布为$$\pmb{Z} \sim \text{Nor}_k (\mathbf{0}, \pmb{I})$$，我们可以通过一个线性变换将标准正态分布转换为所需的正态分布：

$$
\pmb{X} = \pmb{\mu} + \pmb{C} \pmb{Z}
$$

其中$$\pmb{C}$$为对协方差矩阵$$\pmb{\Sigma}$$进行Cholesky分解的结果：

$$
\pmb{\Sigma} = \pmb{C} \pmb{C}^T
$$

因此对$$\pmb{X}$$进行采样时只需要从$$\pmb{Z}$$中采样然后带入线性变换即可。

<div align=center>
<img src="https://i.imgur.com/HnM4jVy.png" width="80%">
</div>

大多数软件中都有Cholesky分解的实现，直接调用即可。

<div align=center>
<img src="https://i.imgur.com/TSInW87.png" width="80%">
</div>

综合上面的步骤可以得到多元正态分布的采样算法如下：

<div align=center>
<img src="https://i.imgur.com/ygjBAXW.png" width="80%">
</div>

## Generating Stochastic Process

本节最后我们来介绍如何对随机过程进行采样。

### Markov Chains

**马尔科夫链(Markov chain)**是一类重要的随机过程。简单来说马尔科夫链假设了系统在发生状态转移时的转移概率只与前一时刻的状态有关，与转移的历史过程无关。因此对离散马尔科夫链进行采样时只需要在每一步单独进行采样即可。

<div align=center>
<img src="https://i.imgur.com/YFjfmvA.png" width="80%">
<img src="https://i.imgur.com/EZJNouZ.png" width="80%">
</div>

连续的情况与离散类似，只需要针对前一时刻的状态重新进行采样即可。考虑**齐次Poisson过程(homogeneous Poisson process)**，我们可以利用指数分布采样出连续的到达时间间隔，然后就可以通过记数的方式来对齐次Poisson过程进行采样。

<div align=center>
<img src="https://i.imgur.com/C4UMeOU.png" width="80%">
</div>

某些情况下我们已知[a, b]时间段上的到达数总量，目标是对到达时刻进行采样。此时只需要从均匀分布U(0, 1)进行采样，然后排序并变换到区间[a, b]上即可。

<div align=center>
<img src="https://i.imgur.com/wT1e5lu.png" width="80%">
</div>

### Nonhomogeneous Poisson Process

对于**非齐次Poisson过程(nonhomogeneous Poisson Process)**，参数$$\lambda$$不再是一个常量而是关于时间的函数$$\lambda(t)$$，同时[a, b]时间内的到达数服从Poisson分布：

$$
N(b) - N(a) = \text{Poi} \bigg( \int_a^b \lambda(t) \ dt \bigg)
$$

<div align=center>
<img src="https://i.imgur.com/3n85lsP.png" width="80%">
</div>

因此对非齐次Poisson过程进行采样的一种方法是利用$$\lambda(T_i)$$对时间间隔进行采样。然而这种做法是**错误**的，这是因为$$\lambda(T_i)$$和$$\lambda(t)$$并不是同步的，我们不能认为$$T_i$$和$$T_{i+1}$$时间段内的$$\lambda$$是一个常数。

<div align=center>
<img src="https://i.imgur.com/Xm0HdcT.png" width="80%">
</div>

正确的做法是使用**Thinning算法**进行采样，此时我们直接使用时间段上最大的$$\lambda^{*}$$来采样时间间隔，然后按照概率$$\frac{\lambda(t)}{\lambda^{*}}$$保留采样的样本。

<div align=center>
<img src="https://i.imgur.com/vJiwpQv.png" width="80%">
<img src="https://i.imgur.com/6szcjJa.png" width="80%">
<img src="https://i.imgur.com/x8ephBl.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/HZhxxJB.png" width="80%">
</div>

### Time Series

对于**时间序列(time series)**一般可以直接根据定义来进行采样。

#### MA

<div align=center>
<img src="https://i.imgur.com/kTfuZd0.png" width="80%">
<img src="https://i.imgur.com/KVCRf5h.png" width="80%">
</div>

#### AR

<div align=center>
<img src="https://i.imgur.com/2twthOO.png" width="80%">
<img src="https://i.imgur.com/kAImDFO.png" width="80%">
</div>

#### ARMA

<div align=center>
<img src="https://i.imgur.com/z9k4ZsA.png" width="80%">
</div>

#### EAR

<div align=center>
<img src="https://i.imgur.com/cAkceHD.png" width="80%">
</div>

#### ARP

<div align=center>
<img src="https://i.imgur.com/pgDQrJJ.png" width="80%">
<img src="https://i.imgur.com/NNsuYBs.png" width="80%">
</div>

### Queuing

对于排队问题我们也有非常简单的采样方法。只要采样出了到达时刻和服务时间，其它的参数都可以通过定义计算出来。

<div align=center>
<img src="https://i.imgur.com/rRmEXyq.png" width="80%">
</div>

### Brownian Motion

本节最后介绍了如何对**布朗运动(Brownian motion)**进行采样。布朗运动在自然科学、金融市场等领域中都有着大量的应用，标准布朗运动的定义如下：

<div align=center>
<img src="https://i.imgur.com/y1yj8b3.png" width="80%">
<img src="https://i.imgur.com/qWmQekj.png" width="80%">
</div>

对布朗运动进行采样的理论基础是**Donsker定理(Donsker's theorem)**，它指出均值为0方差为1的独立同分布随机变量序列经过变换后会收敛到标准布朗运动上：

<div align=center>
<img src="https://i.imgur.com/HZl9BFI.png" width="80%">
</div>

在此基础上就可以利用Bernoulli分布或是标准正态分布的样本来构造出布朗运动。

<div align=center>
<img src="https://i.imgur.com/6UL6erB.png" width="80%">
</div>

布朗运动的一些常用性质如下：

<div align=center>
<img src="https://i.imgur.com/MxjLgYu.png" width="80%">
</div>

#### Geometric Brownian Motion

布朗运动的一个常见变体是**几何布朗运动(geometric Brownian motion)**，它可以用来对股票价格进行建模。

<div align=center>
<img src="https://i.imgur.com/TKq7r31.png" width="80%">
<img src="https://i.imgur.com/ea5EhFN.png" width="80%">
<img src="https://i.imgur.com/veukhhG.png" width="80%">
</div>

## Reference

- [Random Variate Generation](https://www2.isye.gatech.edu/~sman/courses/6644/Module07-RandomVariateGenerationSlides-201026.pdf)