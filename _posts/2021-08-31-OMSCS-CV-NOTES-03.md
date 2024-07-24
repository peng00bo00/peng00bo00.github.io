---
layout: post
title: OMSCS-CV课程笔记03-Frequency Domain Analysis
date: 2021-08-31
description: 频域分析
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍频域分析的相关内容。
{: .block-preface }


## Fourier Transform

在图像处理中我们往往把数字图像视为关于空间位置的函数，实际上从频域角度上来看我们还可以把数字图像看做是一系列给定频率函数的组合，不同频率的函数表示图像在相应方向上的变化快慢。从频域角度认识和分析图像的方法称为频域分析。

以一维信号为例，通过**傅里叶变换(Fourier transform)**我们可以把函数$$f(x)$$在频域进行表示，得到频域函数$$F(u)$$：

$$
F(u) = \int_{-\infty}^\infty f(x) e^{-i 2 \pi u x} dx
$$

$$F(u)$$表示$$f(x)$$在对应频率$$u$$上的权重，它的实部和虚部分别表示频率为$$u$$的余弦和正弦信号成分。

<div align=center>
<img src="https://i.imgur.com/Fif1pte.png" width="90%">
</div>

与之对应的，通过**逆傅里叶变换(inverse Fourier transform)**可以将频域函数$$F(u)$$变换为时域函数$$f(x)$$：

$$
f(x) = \int_{-\infty}^\infty F(u) e^{i 2 \pi u x} du
$$

需要说明的是傅里叶变换仅在函数可积时成立，即函数$$f(x)$$需要满足：

$$
\int_{-\infty}^\infty \vert f(x) \vert dx < \infty
$$

将傅里叶变换推广到在离散情况，可以得到信号处理中常用的**离散傅里叶变换(discrete Fourier transform, DFT)**：

$$
F(k) = \frac{1}{N} \sum_{x=0}^{N-1} f(x) e^{-i \frac{2 \pi k x}{N}}
$$

对于数字图像我们需要将傅里叶变换应用到二维平面上，得到连续和离散形式的二维傅里叶变换：

$$
F(u, v) = \int_{-\infty}^\infty \int_{-\infty}^\infty f(x, y) e^{-i 2 \pi (ux + vy)} dx dy
$$

$$
F(k_x, k_y) = \frac{1}{N} \sum_{x=0}^{N-1} \sum_{y=0}^{N-1} f(x, y) e^{-i \frac{2 \pi (k_x x + k_y y)}{N}}
$$

在数字图像的频域分析中通常只关注频域图像$$F(k_x, k_y)$$的幅度而忽略相位，将$$F(k_x, k_y)$$的幅度进行可视化就得到了数字图像的**频谱(frequency spectrum)**。图像的频谱在某个位置的响应越强表示图像在该方向变化越剧烈：

<div align=center>
<img src="https://i.imgur.com/Z5Wg0Rh.png" width="25%">
<img src="https://i.imgur.com/HbGBH7u.png" width="25%">
</div>

<div align=center>
<img src="https://i.imgur.com/2YogQt5.png" width="25%">
<img src="https://i.imgur.com/BHKDIb2.png" width="25%">
</div>

同时由于傅里叶变换具有可加性，两幅图像相加后它们的频谱也会相加：

<div align=center>
<img src="https://i.imgur.com/AeQ2ND1.png" width="70%">
</div>

此外对频谱进行操作也会反映到图像上。删去频谱的高频成分相当于对图像进行模糊，而删去低频成分则相当于提取图像的边缘。

<div align=center>
<img src="https://i.imgur.com/811Fa2o.png" width="40%">
<img src="https://i.imgur.com/FLtAPqa.png" width="40%">
</div>

## Convolution in Frequency Domain

卷积运算与频域分析有着深刻的联系。以一维信号为例，假设信号$$g$$为$$f$$和$$h$$的卷积$$g = f * h$$，此时$$g$$的频域形式为：

$$
\begin{aligned}
G(u) &= \int_{-\infty}^\infty g(x) e^{-i 2 \pi u x} dx \\
&= \int_{-\infty}^\infty \int_{-\infty}^\infty f(\tau) h(x - \tau) e^{-i 2 \pi u x} d \tau dx \\
&=\int_{-\infty}^\infty \int_{-\infty}^\infty [f(\tau) e^{-i 2 \pi u \tau}] [h(x - \tau) e^{-i 2 \pi u (x - \tau)}] d \tau dx \\
&= \int_{-\infty}^\infty f(\tau) e^{-i 2 \pi u \tau} d \tau \int_{-\infty}^\infty h(x') e^{-i 2 \pi u x'} dx' \\
&=F(u) H(u)
\end{aligned}
$$

上式说明在时域函数的卷积相当于频域函数的乘积，称为**卷积定理(convolution theorem)**。类似地可以证明频域函数的卷积等价于时域函数的乘积。

通过卷积定理我们还可以从频域的角度来认识卷积滤波。以高斯滤波为例，从频域上看使用高斯核进行滤波相当于保留了图像的低频成分同时抑制了高频成分。由于滤波去掉了高频信息(细节)，图像自然会变得模糊。

<div align=center>
<img src="https://i.imgur.com/4SbCZaU.png" width="70%">
</div>

除了卷积定理外，傅里叶变换的其他常用性质包括：

- Linearity：$$c_1 f(x) + c_2 g(x) \Leftrightarrow c_1 F(x) + c_2 G(x)$$
- Scaling：$$f(ax) \Leftrightarrow \frac{1}{\vert a \vert} F(\frac{u}{a})$$
- Differentiation：$$\frac{d^n}{d x^n} f(x) \Leftrightarrow (i 2 \pi u)^n F(u)$$

同时一些常用函数的傅里叶变换如下：

<div align=center>
<img src="https://i.imgur.com/JnEx6sX.png" width="70%">
</div>

## 	Aliasing

本节最后从频域角度来理解信号和图像的混淆问题。由于计算机不能直接表示连续的信号，我们需要通过采样来对连续信号进行离散。当采样的频率低于信号自身的频率时采样的结果就不能完全反映原始信号的信息，更严重的问题是我们无法区分采样的信号是来自于信号原始信号还是更高频的信号，这样的现象称为**混淆(aliasing)**。

<div align=center>
<img src="https://i.imgur.com/5Ru2Jth.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/Bpty2ef.png" width="70%">
</div>

为了更好的理解混淆的现象，首先需要引入**脉冲序列(impulse train)**的相关概念：

$$
\text{comb}_M (x) = \sum_{k=-\infty}^\infty \delta(x - k M)
$$

<div align=center>
<img src="https://i.imgur.com/lfbQB0T.png" width="50%">
</div>

显然脉冲序列$$\text{comb}_M (x)$$是一个周期为$$M$$的函数，因此我们可以用**傅里叶级数(Fourier series)**将其展开：

$$
\text{comb}_M (x) = \sum_{k=-\infty}^\infty A_k e^{i \frac{2 \pi}{M} k x}
$$

$$
A_k = \frac{1}{M} \int_{-\frac{M}{2}}^{\frac{M}{2}} \delta(x - k M) e^{-i \frac{2 \pi}{M} k x} d x = \frac{1}{M}
$$

这样得到使用傅里叶级数表示的脉冲序列：

$$
\text{comb}_M (x) = \frac{1}{M} \sum_{k=-\infty}^\infty e^{i \frac{2 \pi}{M} k x}
$$

再通过傅里叶变换得到脉冲序列的频域表示：

$$
\begin{aligned}
\int_{-\infty}^\infty \text{comb}_M (x) \cdot e^{-i 2 \pi u x} dx &= \frac{1}{M} \sum_{k=-\infty}^\infty \int_{-\infty}^\infty e^{i \frac{2 \pi}{M} k x} \cdot e^{-i 2 \pi u x} dx \\
&= \frac{1}{M} \sum_{k=-\infty}^\infty \delta(u -\frac{k}{M}) \\
&= \frac{1}{M} \text{comb}_\frac{1}{M} (u)
\end{aligned}
$$

上式说明周期为$$M$$的脉冲序列经过傅里叶变换后得到周期为$$\frac{1}{M}$$的脉冲序列。

<div align=center>
<img src="https://i.imgur.com/3xHfTeg.png" width="80%">
</div>

采样的过程可以表示为原始信号与脉冲序列的乘积，在时域上得到一系列幅值变化的脉冲信号。根据卷积定理，采样后的信号从频域上看相当于对原始信号的频谱进行了平移，平移的间隔与采样间隔成反比。在脉冲序列频率足够高的情况下平移后的频谱间不会相互影响，此时不会出现混淆的问题。

<div align=center>
<img src="https://i.imgur.com/WjfMniG.png" width="70%">
</div>

但是当原始信号存在较多的高频成分或是脉冲序列的频率不够高时，采样后的频谱会相互叠加到一起进而出现混淆的情况。此时频谱中的高频成分会混入到低频成分中，使得我们丢失了信号中的各种细节。

<div align=center>
<img src="https://i.imgur.com/RKSVMwU.png" width="70%">
</div>

因此我们可以通过增加采样频率的方式来避免信号出现混淆。实际中更见用的处理方法是在采样前首先利用一个低通滤波器过滤掉原始信号中的高频成分，然后再对处理过的信号进行采样。这样做虽然会损失掉原始信号中的一些高频信息，但可以保证采样后不会出现混淆的问题。

<div align=center>
<img src="https://i.imgur.com/dSRgULN.png" width="70%">
</div>

混淆的现象在图像处理中并不少见。以图像的缩放为例，如果在每次缩放时只是删去一半的行和列就很容易出现混淆。

<div align=center>
<img src="https://i.imgur.com/FICsGsU.png" width="70%">
</div>

处理的方法也很简单，只要在每次缩放前先对图像进行高斯滤波即可。

<div align=center>
<img src="https://i.imgur.com/ZNRtnzd.png" width="70%">
</div>

## Reference

- [Wikipedia: Fourier transform](https://en.wikipedia.org/wiki/Fourier_transform)
- [Wikipedia: Convolution theorem](https://en.wikipedia.org/wiki/Convolution_theorem)
- [Sampling Theory 101](https://web.cs.ucdavis.edu/~okreylos/PhDStudies/Winter2000/SamplingTheory.html)