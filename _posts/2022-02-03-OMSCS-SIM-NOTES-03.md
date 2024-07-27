---
layout: post
title: OMSCS-SIM课程笔记03-Hand and Spreadsheet Simulations
date: 2022-02-03
description: 随机模拟的基本方法
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节介绍随机模拟的基本方法。
{: .block-preface }


## Stepping Through a Differential Equation

在正式介绍随机模拟前我们首先来看一下如何通过模拟的方法来求解微分方程。假设我们已知函数$$f(x)$$的导数$$f'(x)$$，利用导数的定义我们可以使用差分进行近似：

$$
f'(x) \approx \frac{f(x+h) - f(x)}{h}
$$

整理后可以得到迭代形式：

$$
f(x+h) \approx f(x) + h f'(x)
$$

这种求解微分方程的方法称为**Euler方法(Euler’s method)**。对于给定的边界条件，我们可以利用Euler方法进行迭代来获得$$f(x)$$的表格。当步长$$h$$比较小而且在边界附近时Euler方法有比较好的近似结果。

## Monte Carlo Integration

接下来我们考虑积分的情况。对于任意可积函数$$g(x)$$，假设我们需要计算它在[a, b]区间上的积分：

$$
I = \int_a^b g(x) \ dx
$$

通过换元可以把它转换成[0, 1]标准区间上的积分：

$$
I = \int_a^b g(x) \ dx = (b-a) \int_0^1 g(a + (b-a) u) \ du
$$

其中，$$u = \frac{x-a}{b-a}$$。当然我们可以利用**梯形公式(trapezoidal rule)**或是**Gauss–Laguerre积分(Gauss–Laguerre quadrature)**来进行计算，不过当$$g(x)$$比较复杂时使用数值积分可能是比较困难的。在这种情况下我们可以使用随机模拟来进行计算，这种通过生成随机数来计算积分的方法称为**蒙特卡洛积分(Monte Carlo integration, MC)**。

使用MC时我们首先从(0, 1)区间上的均匀分布进行采样，然后定义$$I_i$$为使用样本$$U_i$$计算得到的函数值：

$$
I_i = (b-a) \cdot g(a + (b-a) U_i)
$$

其中$$U_i \sim \text{Unif}(0, 1)$$。显然$$I_i$$是一个随机变量，而它的均值满足：

$$
\bar{I}_n = \frac{1}{n} \sum_{i=1}^n I_i = \frac{b-a}{n} \sum_{i=1}^n g(a + (b-a) U_i)
$$

进一步对$$\bar{I}_n$$求期望得到：

$$
\begin{aligned}
E[\bar{I}_n] &= (b-a) \cdot E[g(a + (b-a) U_i)] \\
&= (b-a) \cdot \int_0^1 g(a + (b-a)u) f(u) \ d u \\
&= (b-a) \cdot \int_0^1 g(a + (b-a)u) \ d u \\
&= I
\end{aligned}
$$

也就是说$$\bar{I}_n$$是积分$$I$$的一个无偏估计，类似地可以证明它的方差满足$$V(\bar{I}_n) = O(\frac{1}{n})$$。依据大数定理我们可以认为当$$n \to \infty$$时，$$\bar{I}_n$$会收敛到$$I$$($$\bar{I}_n \to I$$)。

更进一步我们还可以估计$$\bar{I}_n$$的置信区间。根据CLT有

$$
\bar{I}_n \approx \text{Nor}(E[\bar{I}_n], V[\bar{I}_n]) \sim \text{Nor}(I, V(I_i / n))
$$

那么$$I$$在$$100(1-\alpha)\%$$水平下的置信区间为：

$$
I \in \bar{I}_n \pm \sqrt{\frac{S_I^2}{n}} \cdot z_{\alpha/2}
$$

其中$$z_{\alpha/2}$$为标准正态分布$$\alpha/2$$处对应的分位数，$$S_I^2$$为样本方差$$S_I^2 = \frac{1}{n-1} \sum_{i=1}^n (I_i - \bar{I}_n)^2$$。

## Making Some $$\pi$$

MC的一个经典应用是估计圆周率$$\pi$$的值。假设$$U_1$$和$$U_2$$服从均匀分布$$\text{Unif}(0, 1)$$且相互独立，则点$$(U_1, U_2)$$位于圆内的条件为：

$$
\bigg(U_1 - \frac{1}{2} \bigg)^2 + \bigg(U_2 - \frac{1}{2} \bigg)^2 \leq \frac{1}{4}
$$

因此我们可以建立一个指示函数$$I(u_1, u_2)$$表示样本点是否落在圆内：

$$
I(u_1, u_2) =
\left\{
\begin{aligned}
& 1, & & \text{if} \ \bigg(u_1 - \frac{1}{2} \bigg)^2 + \bigg(u_2 - \frac{1}{2} \bigg)^2 \leq \frac{1}{4} \\
& 0, & & \text{otherwise} 
\end{aligned}
\right.
$$

显然$$I(u_1, u_2)$$的积分为$$\frac{\pi}{4}$$。通过MC我们只需要生成大量的样本然后计算指示函数并取均值即可完成对$$\frac{\pi}{4}$$的估计。

## Single-Server Queue

现实中的很多问题使用解析的方法来精确求解可能是比较困难的，在这种情况下使用模拟的方法可以帮助我们进行求解。首先考虑一个排队问题，假设有一个先入先出(FIFO)的队列，我们引入一些记号来方便描述：

- 用户$$i$$和$$i-1$$之间到达的时间间隔为$$I_i$$
- 用户$$i$$到达的时刻为$$A_i = \sum_{j=1}^i I_i$$
- 用户$$i$$的服务时刻为$$T_i$$，离开的时刻为$$D_i$$，它们满足$$T_i = \max (A_i, D_{i-1})$$
- 用户$$i$$的等待时间为$$W_i^Q = T_i - A_i$$
- 用户$$i$$在队列中的总时间$$W_i = D_i - A_i$$
- 用户$$i$$的服务时间为$$S_i$$，它与离开队列时刻$$D_i$$的关系为$$D_i = T_i + S_i$$

依据这些关系我们可以生成一个用户序列如下，通过模拟结果可以计算出用户的平均等待时间为$$\frac{\sum_{i=1}^6 W^Q_i}{6} = 7.33$$。

<div align=center>
<img src="https://i.imgur.com/qVNk37G.png" width="60%">
</div>

接下来我们考虑系统中在单位时间的平均用户数，我们记$$L(t)$$为$$t$$时刻系统中用户的数目，它包含正在服务以及在队列中等候的用户数。

<div align=center>
<img src="https://i.imgur.com/Uwyq3tO.png" width="35%">
<img src="https://i.imgur.com/CyYMnX8.png" width="58%">
</div>

因此单位时间的平均用户数为：

$$
\bar{L} = \frac{1}{29} \int_0^{29} L(t) \ dt = \frac{70}{29}
$$

另一种计算$$\bar{L}$$的方法是对每个用户在系统中的时长进行求和然后再对时间进行平均：

$$
\bar{L} = \frac{\sum_{i=1}^6 D_i - A_i}{29} = \frac{70}{29}
$$

如果使用后入先出(LIFO)的策略，对于同样的样本我们可以得到仿真结果如下：

<div align=center>
<img src="https://i.imgur.com/9fKfeZL.png" width="60%">
</div>

计算得到此时用户的平均等待时间为5.33，单位时间系统中的平均用户数为2，因此后入先出的队列比先入先出的队列更加高效。

## (s; S) Inventory System

接下来考虑一个库存管理系统。假设每件商品的售价为$$d$$，库存系统的管理策略为保证每一天开始库存中商品的数量至少为$$s$$，如果当天结束时库存小于$$s$$则需要进行补货将库存量提到$$S$$。记第$$i$$天结束后的库存量为$$I_i$$，当天的进货量为$$Z_i$$，它满足

$$
Z_i = 
\left\{
\begin{aligned}
& S-I_i, & & \text{if} \ I_i < s \\
& 0, & & \text{otherwise} 
\end{aligned}
\right.
$$

假设第$$i$$天进货的花费为$$K+c Z_i$$，每件商品在库存中的保管费用为$$h$$，商店的需求为$$D_i$$，如果当天库存量小于需求单位商品还会导致$$p$$的损失。在这些条件下我们可以建立商店的利润模型：

<div align=center>
<img src="https://i.imgur.com/SoZOQEl.png" width="80%">
</div>

通过模拟我们可以得到商店每天的利润：

<div align=center>
<img src="https://i.imgur.com/JGLLHvR.png" width="80%">
</div>

## Simulating Random Variables

本节课最后讨论了模拟随机变量的方法。对于离散型随机变量我们可以通过对[0, 1]区间上的均匀分布进行采样，然后利用逆变换采样来生成离散变量。

<div align=center>
<img src="https://i.imgur.com/IIAf0Pm.png" width="80%">
<img src="https://i.imgur.com/kraSXdf.png" width="80%">
</div>

而对于连续型随机变量，只需要解出CDF的反函数再进行逆变换采样即可。

<div align=center>
<img src="https://i.imgur.com/sunRzaj.png" width="80%">
</div>

## Reference

- [Hand and Spreadsheet Simulations](https://www2.isye.gatech.edu/~sman/courses/6644/Module03-HandSimulationSlides-210816.pdf)
- [Wikipedia: Euler’s method](https://en.wikipedia.org/wiki/Euler_method#:~:text=The%20Euler%20method%20is%20a,proportional%20to%20the%20step%20size.)
- [Wikipedia: Trapezoidal rule](https://en.wikipedia.org/wiki/Trapezoidal_rule)
- [Wikipedia: Gauss–Laguerre quadrature](https://en.wikipedia.org/wiki/Gauss%E2%80%93Laguerre_quadrature)