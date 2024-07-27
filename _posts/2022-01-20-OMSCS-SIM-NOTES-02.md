---
layout: post
title: OMSCS-SIM课程笔记02-Calculus, Probability, and Statistics Primers
date: 2022-01-20
description: 微积分与概率论基础
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节主要复习微积分以及概率论的相关知识。
{: .block-preface }


## Calculus Primer

在进一步介绍随机模拟的内容前首先来回顾一下微积分中的基本内容。

<div align=center>
<img src="https://i.imgur.com/EOUMhdr.jpg" width="80%">
<img src="https://i.imgur.com/8IpC0tR.jpg" width="80%">
<img src="https://i.imgur.com/Q44MQnh.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/5fWyWHB.jpg" width="78%">
<img src="https://i.imgur.com/LQ3i4He.jpg" width="78%">
<img src="https://i.imgur.com/qW8WkjV.jpg" width="78%">
</div>

一些常用函数的导数如下：

$$
[x^k]' = k x^{k-1}
$$

$$
[e^x]' = e^x
$$

$$
[\sin (x)]' = \cos (x)
$$

$$
[\cos (x)]' = -\sin (x)
$$

$$
[\ln (x)]' = \frac{1}{x}
$$

$$
[\arctan (x)]' = \frac{1}{1 + x^2}
$$

同时，导数的运算法则为：

$$
[a f(x) + b]' = a f'(x)
$$

$$
[f(x) + g(x)]' = f'(x) + g'(x)
$$

$$
[f(x) g(x)]' = f'(x) g(x) + f(x) g'(x)
$$

$$
\bigg[ \frac{f(x)}{g(x)} \bigg]' = \frac{g(x) f'(x) - f(x) g'(x)}{g^2 (x)}
$$

$$
[f(g(x))]' = f'(g(x)) g'(x)
$$

函数$$f(x)$$仅在其导数为0的位置取极值，而借助于二阶导数我们还可以进一步判断极值点的类型。

<div align=center>
<img src="https://i.imgur.com/O4C9DTm.jpg" width="80%">
<img src="https://i.imgur.com/l99hEFq.jpg" width="80%">
<img src="https://i.imgur.com/yRTCQs6.jpg" width="80%">
<img src="https://i.imgur.com/gapqWiE.jpg" width="80%">
</div>

## Finding Zeros

在很多时候我们需要去计算函数的零点。对于复杂的函数可能无法解析地求得函数的零点，在这种情况下需要使用一些数值计算方法来求解。最基本的方法是**二分法(bisection)**：

<div align=center>
<img src="https://i.imgur.com/FK7YfdR.jpg" width="80%">
</div>

除了二分法之外我们还可以通过**Newton's method**来迭代计算函数的零点。

<div align=center>
<img src="https://i.imgur.com/kWZpm2U.jpg" width="80%">
</div>

$$
x_{i+1} = x_i - \frac{g(x_i)}{g'(x_i)}
$$

## Integration

除了微分之外，我们还需要计算积分。

<div align=center>
<img src="https://i.imgur.com/koVKCub.jpg" width="80%">
<img src="https://i.imgur.com/VZNQw5I.jpg" width="80%">
</div>

常用函数的积分公式如下：

$$
\int x^k dx = \frac{x^{k+1}}{k+1} + C, k \neq -1
$$

$$
\int \frac{dx}{x} dx = \ln \vert x \vert + C
$$

$$
\int e^x dx = e^x + C
$$

$$
\int \cos (x) dx = \sin (x) + C
$$

$$
\int \frac{dx}{1 + x^2} = \arctan (x) + C
$$

(定)积分的常用性质为：

$$
\int_a^a f(x) dx = 0
$$

$$
\int_a^b f(x) dx = - \int_b^a f(x) dx
$$

$$
\int_a^b f(x) dx = \int_a^c f(x) dx + \int_c^b f(x) dx
$$

$$
\int [f(x) + g(x)] dx = \int f(x) dx + \int g(x) dx
$$

$$
\int f(x) g'(x) dx = f(x) g(x) - \int g(x) f'(x) dx
$$

$$
\int f(g(x)) g'(x) dx = \int f(u) du
$$

### Riemann Sums

对于复杂的函数，有时可能很难通过解析的形式来计算定积分。此时我们可以将函数拆分成若干个小区间然后在每个区间上计算函数包围的面积，最后把它们加起来近似定积分的值。这种做法称为黎曼和(Riemann sums)：

<div align=center>
<img src="https://i.imgur.com/U3BPt2S.jpg" width="80%">
</div>

### Taylor Series Expansion

在微积分中**泰勒级数展开(Taylor series expansion)**是一种非常常用的方法：

<div align=center>
<img src="https://i.imgur.com/x6MpwFu.jpg" width="80%">
</div>

在$$a=0$$处进行展开得到的级数称为Maclaurin级数，常用函数的Maclaurin级数为：

$$
\sin (x) = \sum_{k=0}^\infty \frac{(-1)^k x^{2k + 1}}{(2k + 1)!}
$$

$$
\cos (x) = \sum_{k=0}^\infty \frac{(-1)^k x^{2k}}{(2k)!}
$$

$$
e^x = \sum_{k=0}^\infty \frac{x^k}{k!}
$$

另一个常用的工具是**洛必达法则(L'Hospital's rule)**，它指出当极限存在时可以利用函数的导数来代替函数值计算极限：

<div align=center>
<img src="https://i.imgur.com/bCweIqe.jpg" width="70%">
</div>

## Probability Basics

### Probabilities

接下来复习一下概率论的相关知识。

<div align=center>
<img src="https://i.imgur.com/WdPvNR2.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/7i2shoo.jpg" width="80%">
</div>

### Random Variables

<div align=center>
<img src="https://i.imgur.com/RzUg9Ja.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/JiXLH3c.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/DoxdVLe.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/twpvTdQ.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/vt84Thk.jpg" width="80%">
</div>

## Simulating Random Variables

那么如何使用计算机来产生各种类型的随机变量呢？对于任意连续型随机变量$$X$$我们可以通过它的CDF来进行采样，实际上$$F(X)$$服从(0, 1)区间上的均匀分布：

<div align=center>
<img src="https://i.imgur.com/ONHtqto.jpg" width="70%">
</div>

稍后我们会来证明这个定理。由于$$F(X) \sim \text{Unif} (0, 1)$$，我们可以通过对(0, 1)区间上的均匀分布进行采样再映射回$$X$$实现对任意分布的采样，这种采样方法称为**逆变换方法(inverse transform method)**。以参数为$$\lambda$$的指数分布为例，假设$$X \sim \text{Exp} (\lambda)$$则它的CDF为$$F(X) = 1 - e^{-\lambda X}$$，根据逆变换方法可以得到采样公式为：

$$
X = - \frac{1}{\lambda} \ln (1 - U), \ U \sim \text{Unif} (0, 1)
$$

对于逆变换方法我们需要生成(0, 1)区间上的均匀分布的随机数，我们可以利用上节课介绍的[伪随机数生成算法](/2022/01/16/OMSCS-SIM-NOTES-01.html#generating-randomness)来实现：

<div align=center>
<img src="https://i.imgur.com/99sOakO.jpg" width="80%">
</div>

## Great Expectations

期望是随机变量的一个重要性质，离散和连续型随机变量的期望分别定义为：

<div align=center>
<img src="https://i.imgur.com/mlQQeF1.jpg" width="80%">
</div>

常用分布的期望如下：

<div align=center>
<img src="https://i.imgur.com/Ld2D6SJ.jpg" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/jmRFcSj.jpg" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/0xhI99V.jpg" width="68%">
</div>

### LOTUS

对于作用在随机变量上的函数，我们可以利用**law of the unconscious statistician(LOTUS)**来计算它的期望：

<div align=center>
<img src="https://i.imgur.com/Q1BfpMI.jpg" width="80%">
</div>

函数$$h(X)$$可以是任意形式，而对于幂函数我们称对应的期望为$$X$$的**n阶矩($$n$$th moment of $$X$$)**。除了幂函数我们还可以利用LOTUS来定义**n阶中心矩($$n$$th central moment of $$X$$)**和**方差(variance)**：

<div align=center>
<img src="https://i.imgur.com/cqiZ8pe.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/e3SZJE8.jpg" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/laWzyB9.jpg" width="70%">
</div>

期望和方差的一个重要性质是线性性：

<div align=center>
<img src="https://i.imgur.com/2GXb4pA.jpg" width="80%">
</div>

### Moment Generating Functions

和期望有关的另一个函数是**动量母函数(moment generating functions, MGF)**：

<div align=center>
<img src="https://i.imgur.com/Cg2Cs2J.jpg" width="70%">
</div>

常用分布的MGF为：

<div align=center>
<img src="https://i.imgur.com/uZ9CZCv.jpg" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/cMgct85.jpg" width="70%">
</div>

我们可以通过对MGF求导来计算$$X$$的n阶矩：

<div align=center>
<img src="https://i.imgur.com/5rwFJMQ.jpg" width="60%">
</div>

## Functions of a Random Variable

当我们把某个函数$$h$$作用在随机变量$$X$$上时，输出$$h(X)$$也会是一个随机变量，它有着自己的PDF/CDF。

<div align=center>
<img src="https://i.imgur.com/OYlz7AK.jpg" width="50%">
</div>

想要计算任意函数$$h$$作用后得到的随机变量是十分困难的，但对于某些特定的函数我们可以解析地计算出$$h(X)$$的分布。实际上逆变换方法可以看做是$$h$$取CDF情况下$$h(X)$$的分布：

<div align=center>
<img src="https://i.imgur.com/sYlQlWS.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/seI0VHO.jpg" width="60%">
</div>

更进一步，如果$$h$$的性质比较好我们还可以利用链式法则来计算$$h(X)$$的PDF：

<div align=center>
<img src="https://i.imgur.com/ghgotnM.jpg" width="80%">
</div>

实际上我们可以利用这个定理来证明LOTUS：

<div align=center>
<img src="https://i.imgur.com/Dgu8fJc.jpg" width="80%">
</div>

## Jointly Distributed Random Variables

### Joint Distribution

我们把一维随机变量拓展到多维可以得到多元随机变量和它们的联合概率分布。

<div align=center>
<img src="https://i.imgur.com/5MxLRpQ.jpg" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/MFxszhU.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/eSB6qxQ.jpg" width="73%">
</div>

对于联合概率，我们可以通过**边缘化(marginalization)**来获得每个随机变量单独的概率分布。

<div align=center>
<img src="https://i.imgur.com/9O9SwwE.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/lzujXYe.jpg" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/p1LF5dn.jpg" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/eIHGuYD.jpg" width="70%">
</div>

如果两个随机变量相互独立，则它们的联合概率还可以写成因子相乘的形式。

<div align=center>
<img src="https://i.imgur.com/hlvdX8T.jpg" width="80%">
</div>

### Conditional Distribution

利用联合概率我们可以计算多元随机变量的条件概率。

<div align=center>
<img src="https://i.imgur.com/8x7VypM.jpg" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/oMyKGW3.jpg" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/y8ZVCay.jpg" width="75%">
</div>

## Conditional Expectation

在条件概率的基础上我们可以定义**条件期望(conditional expectation)**。条件期望$$E[Y \vert X]$$可以理解为一个关于随机变量$$X$$的函数，它是$$Y$$在条件概率$$f(Y \vert X)$$下的期望。

<div align=center>
<img src="https://i.imgur.com/sLpzn2Y.jpg" width="75%">
</div>

### Double Expectations

条件期望的一个重要应用是利用条件期望$$E[Y \vert X]$$来计算$$Y$$自身的期望，这称为**全期望定理(law of total expectation)**。

<div align=center>
<img src="https://i.imgur.com/1svvJse.jpg" width="75%">
</div>

在很多情况下直接计算期望比较困难，但条件期望的计算时相对容易的。在这种情况下使用全期望定理就可以方便地计算原始随机变量的期望。

<div align=center>
<img src="https://i.imgur.com/6diNKR3.jpg" width="75%">
<img src="https://i.imgur.com/BVu78Zm.jpg" width="75%">
</div>

类似地，我们还可以把$$Y$$定义为某个事件发生的概率，这样就可以利用全期望定理来计算概率。

<div align=center>
<img src="https://i.imgur.com/pDghvs3.jpg" width="75%">
<img src="https://i.imgur.com/OcCF6dP.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/QWFfild.jpg" width="75%">
</div>

### Variance Eecomposition

类似于全期望定理，我们也可以对$$Y$$的方差进行分解，称为**全方差定理(law of total variance)**。

<div align=center>
<img src="https://i.imgur.com/BnZqHDY.jpg" width="75%">
</div>

## Covariance and Correlation

在进一步介绍多元随机变量前我们首先把LOTUS进行推广。

<div align=center>
<img src="https://i.imgur.com/n4lBDQB.jpg" width="75%">
</div>

同时我们还需要引入**独立同分布(independent and identically distributed, iid)**的概念。

<div align=center>
<img src="https://i.imgur.com/n4lBDQB.jpg" width="75%">
</div>

### Covariance

在LOTUS的基础上就可以定义多元随机变量的**协方差(covariance)**，它描述了随机变量之间的相关性。不过需要注意的是协方差为0并不表示随机变量相互独立。

<div align=center>
<img src="https://i.imgur.com/cmQMWRL.jpg" width="75%">
</div>

协方差具有线性性。

<div align=center>
<img src="https://i.imgur.com/Ulc2zTG.jpg" width="75%">
</div>

### Correlation

与协方差类似的还有**相关系数(correlation)**，它是一个规范化的系数来度量随机变量的相关性。

<div align=center>
<img src="https://i.imgur.com/C6a9c1x.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/j76qhhR.jpg" width="75%">
</div>

## Some Probability Distributions

接下来我们总结一些常用概率分布的性质。

### Bernoulli Distribution

<div align=center>
<img src="https://i.imgur.com/t0l5IhL.jpg" width="75%">
</div>

### Geometric Distribution

<div align=center>
<img src="https://i.imgur.com/PG7Y59s.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/eNrhnVj.jpg" width="75%">
</div>

### Poisson Distribution

<div align=center>
<img src="https://i.imgur.com/FkeL9HQ.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/id6L9fX.jpg" width="75%">
</div>

### Uniform Distribution

<div align=center>
<img src="https://i.imgur.com/RS9JpHB.jpg" width="75%">
</div>

### Exponential Distribution

<div align=center>
<img src="https://i.imgur.com/h31msA9.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/nyRoYo4.jpg" width="75%">
</div>

### Gamma Distribution

<div align=center>
<img src="https://i.imgur.com/bXwtSEs.jpg" width="75%">
</div>

### Triangular Distribution

<div align=center>
<img src="https://i.imgur.com/MwfXBZZ.jpg" width="75%">
</div>

### Beta Distribution

<div align=center>
<img src="https://i.imgur.com/IsJLZ2E.jpg" width="75%">
</div>

### Normal Distribution

<div align=center>
<img src="https://i.imgur.com/bZelF70.jpg" width="75%">
</div>

## Limit Theorems

<div align=center>
<img src="https://i.imgur.com/WuiJnY7.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/fs9B8EX.jpg" width="75%">
</div>

### Central Limit Theorem

**中心极限定理(central limit theorem, CLT)**指出，当样本数目比较大的时候相互独立随机变量的均值经标准化后依分布收敛于标准正态分布。

<div align=center>
<img src="https://i.imgur.com/dwcUSFS.jpg" width="75%">
</div>

## Estimation

我们可以利用随机变量来估计概率分布的参数。不过需要注意的是随机变量的函数仍然是一个随机变量，因此我们的估计值也是一个随机变量。

<div align=center>
<img src="https://i.imgur.com/qA7igAN.jpg" width="75%">
<img src="https://i.imgur.com/lHFYG7F.jpg" width="75%">
</div>

由于参数的估计值是一个随机变量，我们希望它能够近似真实的参数而且具有的比较小的方差。

<div align=center>
<img src="https://i.imgur.com/9O1bkE7.jpg" width="75%">
<img src="https://i.imgur.com/K3HIokz.jpg" width="75%">
</div>

### Unbiased Estimators

**无偏估计(unbiased estimation)**是指参数估计值的期望等于真实的参数。最常用的无偏估计是样本均值，显然对于独立同分布的样本其均值的期望等于样本分布的期望。

<div align=center>
<img src="https://i.imgur.com/xfcVtkm.jpg" width="75%">
</div>

样本方差与之类似，不过需要注意计算样本方差时的分母为$$n-1$$。

<div align=center>
<img src="https://i.imgur.com/8iBN3aG.jpg" width="75%">
<img src="https://i.imgur.com/AUzwfh1.jpg" width="75%">
</div>

对于两个无偏估计，我们认为方差小的估计更好。

<div align=center>
<img src="https://i.imgur.com/Cz2zNu1.jpg" width="75%">
<img src="https://i.imgur.com/XHOVloj.jpg" width="75%">
</div>

### Mean Squared Error

由于方差的存在有时无偏估计也不是一个好的估计。我们定义估计值与真实值之间平方差的期望为**平均平方误差(mean squared error, MSE)**，它度量了估计值与真实值之间的误差。

<div align=center>
<img src="https://i.imgur.com/shJERz5.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/s4oJnJ7.jpg" width="75%">
</div>

### Maximum Likelihood Estimation

**极大似然估计(maximum likelihood estimation, MLE)**是一种非常常用的参数估计方法。通过对样本的似然函数求极值我们可以得到最有可能生成这些样本的参数。

<div align=center>
<img src="https://i.imgur.com/iSoUiBZ.jpg" width="75%">
</div>

有时直接计算似然函数的极值比较困难，我们还可以通过对数函数来进行处理。

<div align=center>
<img src="https://i.imgur.com/o3pm95g.jpg" width="75%">
<img src="https://i.imgur.com/UGAM0a1.jpg" width="75%">
<img src="https://i.imgur.com/vGaoipV.jpg" width="75%">
</div>

## Confidence Intervals

最后，我们可以利用一些已知的分布来估计参数的置信区间。

<div align=center>
<img src="https://i.imgur.com/Ea0xS9R.jpg" width="75%">
</div>

<div align=center>
<img src="https://i.imgur.com/iJtEIgL.jpg" width="75%">
<img src="https://i.imgur.com/fSZGGI1.jpg" width="75%">
</div>

## Reference

- [Calculus, Probability, and Statistics Primers](https://studio.edx.org/assets/courseware/v1/c6b6e79b9ddda262529c17b784e1052e/asset-v1:GTx+ISYE6644x+2T2019+type@asset+block/Module02-CalcProbStatsReviewSlides-181230.pdf)