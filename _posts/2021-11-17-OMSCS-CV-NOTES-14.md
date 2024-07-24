---
layout: post
title: OMSCS-CV课程笔记14-Action Recognition
date: 2021-11-17
description: 视频分析和动作识别
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍视频分析和动作识别的相关内容。
{: .block-preface }


## Video Analysis

### Background Subtraction

在视频分析中我们把图像序列看成是一个三维的数组$$I(x, y, t)$$，通过视频分析我们可以从图像序列中识别出运动的物体。视频分析的一个经典应用是**背景差分(background subtraction)**，我们可以从视频中对背景图像进行建模并通过差分的方式来获得前景图像：

<div align=center>
<img src="https://i.imgur.com/2GM3pf6.png" width="70%">
</div>

背景差分在交通摄像头监控、行为识别、目标跟踪等很多领域都有相关的应用，它的基本流程如下：

1. 估计$$t$$时刻的背景图像；
2. 将当前图像与背景图像相减，得到差分图像；
3. 对差分图像进行阈值化，得到前景图像的掩码。

显然背景差分最重要的一步是对背景图像进行建模，最简单的方式是使用前一帧的图像作为背景：

$$
B(x, y, t) = I(x, y, t-1)
$$

然而这种方法容易收到运动物体的外观、速度等因素的影响，因此实际中基本不会直接使用之前的图像作为背景。比较常用的背景建模方式是使用均值或者中值图像，通过前$$n$$帧的图像进行均值滤波或是中值滤波来获得背景：

$$
B(x, y, t) = \frac{1}{n} \sum_{i=1}^n I(x, y, t-i)
$$

$$
B(x, y, t) = \mathop{\text{median}} \{ I(x, y, t-i) \}
$$

背景差分算法的优势在于实现简单、运行效率高、同时对于可变的背景具有一定的适应性；但它的缺陷在于算法的效果容易收到物体运动速度以及视频帧率的影响，而且中值滤波需要存储之前的图像对内存有更高的要求，同时阈值如何设置需要进行不断的调试才能确定。

### Epipolar Plane Images

本节课还介绍了另一种有趣的视频分析应用，称为**极几何图像(epipolar plane images)**。假设物体不动，相机沿直线运动，则空间点$$P$$会位于同一条极线上：

<div align=center>
<img src="https://i.imgur.com/bU466oT.png" width="50%">
</div>

我们可以把这些图片叠到一起，可以得到图像的长方体：

<div align=center>
<img src="https://i.imgur.com/5O0cmBg.png" width="90%">
</div>

<div align=center>
<img src="https://i.imgur.com/9lXIahO.png" width="30%">
</div>

如果从Y轴进行切片则可以发现空间中同一个点在切片上会变成一条直线，而这条直线的斜率则对应了该点的深度。

<div align=center>
<img src="https://i.imgur.com/Ezggmmc.png" width="30%">
</div>

类似地，我们同样可以对运动的物体构造极几何图像。此时运动的物体会在切片图像上留下特定的图案，一些研究表明我们可以利用这种图案来进行动作的识别。

<div align=center>
<img src="https://i.imgur.com/fm1BG99.png" width="80%">
</div>

## Activity Recognition

### Human Activity in Video

对视频中人的行为进行识别是比较困难的任务，一般来说没有一个通用的解决方法。常用的方法包括对单帧图像进行识别或是结合一些运动的信息进行识别等。从算法层面来讲进行行为识别可以分为**基于模型的动作识别(model based action recognition)**以及**基于模型的行为识别(model based activity recognition)**两种：动作识别是对人体的不同部位以及姿态进行识别，而行为识别则是在动作的基础上根据模型来识别人的行为

<div align=center>
<img src="https://i.imgur.com/YgHcGCo.png" width="80%">
</div>

目前行为识别主流方式是通过结合时空的模式来进行分析。一般来说这类方法不需要显式地对人体进行跟踪，只需要从视频中提取物体的运动模式并训练一个分类器即可。

### Motion History Images

要进行行为识别就需要将物体的运动记录下来，一种经典的处理方法是使用**运动历史图(motion history image, MHI)**来保存运动信息。MHI通过对图像中的像素进行更新来记录物体的运动信息：

$$
I_\tau (x, y, t) = 
\left\{
\begin{aligned}
& \tau, & & \text{if moving} \\
& \max \{I_\tau (x, y, t-1), 0 \}, & & \text{otherwise}
\end{aligned}
\right.
$$

<div align=center>
<img src="https://i.imgur.com/q5azR6T.png" width="30%">
</div>

在MHI的基础上进行阈值化就可以得到**运动能量图(motion energy image, MEI)**，MEI和MHI共同组成了运动在时空上的模板：

<div align=center>
<img src="https://i.imgur.com/bj1cevE.png" width="70%">
</div>

对于视频中运动的物体我们利用这种时空上的模板进行模板匹配从而实现简单的行为识别：

<div align=center>
<img src="https://i.imgur.com/zO8xkNf.png" width="70%">
</div>

### Image Moments

除了直接使用模板外我们还可以利用模板的统计特征来训练分类器从而实现行为识别。最简单的图像特征是**矩(moment)**，它定义为：

$$
M_{ij} = \sum_x \sum_y x^i y^j I(x, y)
$$

类似地，可以定义图像的**中心矩(central moments)**为：

$$
\mu_{pq} = \sum_x \sum_y (x - \bar{x})^p (y - \bar{y})^q I(x, y)
$$

$$
\bar{x} = \frac{M_{10}}{M_{00}}, \bar{y} = \frac{M_{01}}{M_{00}}
$$

中心矩是一个平移不变量，同样的形状无论发生什么样的平移它的中心矩保持不变。实践中更常用的图像矩特征是**Hu矩(Hu moment)**，它是一个包含7个矩的特征，而且具有平移和尺度不变性。Hu矩的定义如下：

<div align=center>
<img src="https://i.imgur.com/48cDuns.png" width="70%">
</div>

有了特征后就可以利用各种机器学习模型来完成行为的识别，主要流程如下：

<div align=center>
<img src="https://i.imgur.com/E4JE3Fj.png" width="70%">
</div>

## Hidden Markov Models

### Markov Models

本节课最后介绍了**隐马尔科夫模型(hidden Markov model, HMM)**在行为识别中的应用。在正式介绍HMM前首先需要引入马尔科夫模型的概念，简单来说马尔科夫模型是满足一阶马尔科夫性的模型，它由3部分组成：

- **状态(state)**：$$\{ S_1, S_2, ..., S_n \}$$
- **状态转移概率(state transition probabilities)**：$$a_{ij} = P(q_{t+1} = S_i \vert q_t = S_j)$$
- **初始状态分布(Initial state distribution:)**：$$\pi_i = P(q_1 = S_i)$$

<div align=center>
<img src="https://i.imgur.com/8sLmxaT.png" width="50%">
</div>

在马尔科夫模型的基础上，HMM中指系统状态不可见但是我们可以对系统进行观测，系统状态到观测结果的过程由**发射概率(emission probabilities)**控制：

$$
b_j (k) = P( o_t = k \vert q_t = S_j)
$$

因此，HMM可以看做是由状态转移、发射概率以及初始状态分布构成的三元组$$\lambda = (A, B, \pi)$$。

### 3 Computational Problems of HMMs

HMM可以求解三种问题：

- Evaluation：给定模型$$\lambda = (A, B, \pi)$$，计算观测序列$$O = \{ o_1, o_2, ..., o_T \}$$的概率；
- Decoding：给定模型$$\lambda = (A, B, \pi)$$和观测序列$$O = \{ o_1, o_2, ..., o_T \}$$，计算产生该观测序列最有可能的状态序列；
- Learning：给定观测序列$$O = \{ o_1, o_2, ..., o_T \}$$，学习产生该序列最有可能的模型$$\lambda = (A, B, \pi)$$。

以上问题都有非常经典的求解方法，本节课中只介绍了evaluation的求解过程。evalution可以形式化为计算如下定义的概率：

$$
P(O \vert \lambda) = \sum_q P(O \vert q, \lambda) P(q \vert \lambda)
$$

其中，

$$
P(O \vert q, \lambda) = \prod_t P(o_t \vert q_t, \lambda) = b_{q_1} (o_1) b_{q_2} (o_2) ... b_{q_T} (o_T)
$$

$$
P(q \vert \lambda) = \pi_{q_1} a_{q_1 q_2} a_{q_2 q_3} ... a_{q_{T-1} q_T}
$$

因此直接求解的方法是对所有可能的状态序列$$\{ q_1, q_2, ..., q_T \}$$进行求和。然而这种直接求解的计算复杂度为$$O(TN^T)$$，在实际计算中是不可行的。

实际中解决evaluation问题使用的是前后向算法，它分为前向计算和后向计算两个步骤。前向计算通过递归的方式来计算前向概率$$\alpha_t$$：

$$
\alpha_t (i) = P(o_1, ..., o_t, q_t=i \vert \lambda)
$$

它表示到$$t$$时刻为止观测到$$\{ o_1, ..., o_t \}$$且状态为$$q_t = i$$的概率，它可以通过递归的方式来求解：

$$
\alpha_{t+1} (j) = \bigg[ \sum_{i=1}^N \alpha_t (i) a_{ij} \bigg] b_j(o_{t+1})
$$

当递推到$$T$$时刻时，$$P(O \vert \lambda)$$可通过对前向概率求和来计算：

$$
P(O \vert \lambda) = \sum_{i=1}^N \alpha_T (i)
$$

使用前向概率递推计算的复杂度为$$O(N^2 T)$$，远低于直接计算的复杂度。类似地，我们可以定义后向概率为：

$$
\beta_t (i) = P(o_{t+1}, o_{t+2}, ..., o_T \vert q_t = i, \lambda)
$$

递推时首先初始化：

$$
\beta_T (i) = 1
$$

然后从后向前递推：

$$
\beta_t (i) = \sum_{i=1}^N a_{ij} b_j (o_{t+1}) \beta_{t+1} (j)
$$

当递推到$$t=1$$时刻只需要再求和即可：

$$
P(O \vert \lambda) = \sum_{i=1}^N \pi_i b_1 (o_1) \beta_1 (i)
$$

## Reference

- [Wikipedia: Hidden Markov model](https://en.wikipedia.org/wiki/Hidden_Markov_model)
- 第10章：隐马尔科夫模型，统计学习方法（第2版）