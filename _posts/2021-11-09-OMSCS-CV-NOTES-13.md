---
layout: post
title: OMSCS-CV课程笔记13-Discriminative Models
date: 2021-11-09
description: 判别式模型
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍判别式模型的相关内容。
{: .block-preface }


## Classification: Discriminative Models

在上一节中我们介绍了分类问题中的生成式模型，它通过计算类别的先验以及似然来估计样本的类别。生成式模型有非常好的理论保证，但如何去表示高维数据的概率密度是十分困难的。同时在很多情况下我们并不关心数据的产生过程，只要有一个好的决策模型就可以满足我们的要求。因此在现代机器学习和计算机视觉中更加常用的是对决策边界进行建模的**判别式模型(discriminative model)**。

要使用判别式模型我们需要对数据进行如下假设：

- 每个样本所属的可能类别是确定的；
- 每个类别都拥有足够多的数据样本；
- 分类器可能犯的错误都具有相同的代价；
- 我们使用特征来表示每个样本，但我们事先并不知道哪些特征对于分类问题是更有效的。

在这样的假设下我们可以把求解图像识别的问题划分为训练和测试两个阶段：

- 在训练阶段中我们需要设计合适的特征来表示样本，并利用这些特征来训练分类器；
- 在测试阶段中我们需要将输入图像分解成若干的候选图像(candidate)，然后对这些图像进行打分来实现图像识别。

### Window-Based Models

在图像识别任务中常用的图像特征包括边缘、轮廓、图像梯度等，在构造图像特征时往往会将图像划分成网格然后在每个格子内计算这些特征的直方图作为最终的图像特征。

<div align=center>
<img src="https://i.imgur.com/yYwRoKH.png" width="80%">
</div>

以车辆检测为例，我们需要使用这些图像特征训练一个二元分类器来回答输入图像是否是一辆汽车。

<div align=center>
<img src="https://i.imgur.com/sh7SbyV.png" width="80%">
<img src="https://i.imgur.com/yqyBkUo.png" width="80%">
</div>

当模型训练好后，我们就可以在图像上进行滑窗来检测出图像中车汽车的位置。

<div align=center>
<img src="https://i.imgur.com/8gI2u1z.png" width="80%">
</div>

在计算机视觉中常用的分类模型如下：

<div align=center>
<img src="https://i.imgur.com/z0s2Y9O.png" width="80%">
</div>

## Boosting and Face Detection

### Boosting: Intuition

Boosting是一种常用的集成学习模型，它的基本思想是将一系列**弱学习器(weak learner)**组合到一起形成一个强大的模型。在训练过程中，boosting会提高当前学习器分类错误的样本权重从而在下一轮训练中更加关注这些错误的样本：

<div align=center>
<img src="https://i.imgur.com/lmUv2Ry.png" width="40%">
<img src="https://i.imgur.com/SofYVOc.png" width="40%">
</div>

<div align=center>
<img src="https://i.imgur.com/xrfsjLp.png" width="40%">
<img src="https://i.imgur.com/j158nFe.png" width="40%">
</div>

<div align=center>
<img src="https://i.imgur.com/JJViXfo.png" width="40%">
</div>

当训练完成时，最终的模型即为全部弱学习器的线性组合。每个学习器的权重则取决于所使用的boosting算法，如Adaboost中会根据分类器的错误率来调节弱学习器的权重，错误率越低则权重越大。

<div align=center>
<img src="https://i.imgur.com/UdSjdAl.png" width="60%">
</div>

### Viola-Jones Face Detector

Boosting在计算机视觉中的经典应用是Viola-Jones人脸检测算法，它的基本思想是使用一系列方块形滤波器构造出人脸的特征并训练出一个二分类模型进行人脸检测。

<div align=center>
<img src="https://i.imgur.com/uZtQgJ1.png" width="80%">
</div>

在原始论文中检测窗口固定为$$24 \times 24$$，根据滤波器的位置、尺寸和形状总共有超过180,000种可能的特征(滤波器)。

<div align=center>
<img src="https://i.imgur.com/6AhjIQq.png" width="60%">
</div>

然后使用Adaboost算法来训练人脸分类器，其中每个弱学习器只在几个少量的特征上进行分类，即每个学习器都只选择当前最有效的特征进行学习。

<div align=center>
<img src="https://i.imgur.com/TFYT6FB.png" width="60%">
</div>

同时在实践中还发现图像中大部分位置都不是人脸，因此为了提高效率Viola-Jones人脸检测算法还设计了一个**级联分类器(cascade classifier)**来过滤掉图像中非人脸的部分。它的思想是将一系列分类器串联起来，当输入窗口图像被当前分类器划分为负样本时直接拒绝它，换句话说只有通过所有分类器的窗口才是人脸。

<div align=center>
<img src="https://i.imgur.com/ZSj1WK3.png" width="60%">
</div>

Viola-Jones人脸检测算法的基本框架如下：

<div align=center>
<img src="https://i.imgur.com/zEzL6s8.png" width="80%">
</div>

Viola-Jones人脸检测算法是第一个大规模应用的人脸检测算法，一些检测结果如下：

<div align=center>
<img src="https://i.imgur.com/awv2J1G.png" width="80%">
</div>

## Support Vector Machines

### Linear Classifiers

**支持向量机(support vector machines, SVM)**是计算机视觉中另一种非常常用的分类模型。它的基本思想是寻找一条直线(超平面)将正负两类样本进行分隔，同时使得样本到这条直线的**间隔(margin)**尽可能大。

<div align=center>
<img src="https://i.imgur.com/HOupgMR.png" width="40%">
</div>

假设超平面方程为$$y = w^T x + b$$，正负两类样本到超平面的间隔为1。则对于任意样本$$x_i$$有：

$$
y_i (w^T x_i + b) \geq 1
$$

其中取等号的样本称为**支持向量(support vector)**。

<div align=center>
<img src="https://i.imgur.com/mSvIyfF.png" width="50%">
</div>

对于任意样本$$x_i$$，它到超平面的距离为：

$$
\frac{\Vert w^T x_i + b \Vert}{\Vert w \Vert}
$$

显然正负两侧支持向量的距离为：

$$
M = \bigg\vert \frac{1}{\Vert w \Vert} - \frac{-1}{\Vert w \Vert} \bigg\vert = \frac{2}{\Vert w \Vert}
$$

我们希望能够最大化这个距离$$M$$，因此可以得到约束优化问题：

$$
\begin{aligned}
\max \ \ & \frac{2}{\Vert w \Vert} \\
\text{s.t.} \  \ & y_i (w^T x_i + b) \geq 1
\end{aligned}
$$

更常见的形式为：

$$
\begin{aligned}
\min \ \ & \frac{1}{2} w^T w \\
\text{s.t.} \  \ & y_i (w^T x_i + b) \geq 1
\end{aligned}
$$

可以证明上式定义的约束优化问题其解的形式为：

$$
w = \sum_i \alpha_i y_i x_i
$$

且系数$$\alpha_i$$仅在支持向量位置有非0值。对应的截距项$$b$$可通过带入支持向量$$x_i$$进行求解：

$$
b = y_i - w^T x_i
$$

这说明我们只需要记录少量的几个支持向量和对应的系数就可以表示整个SVM模型。当我们需要进行分类时，数据$$x$$的类别为：

$$
\begin{aligned}
f(x) &= \text{sign} (w^T x + b) \\
&= \text{sign} (\sum_i \alpha_i y_i x_i \cdot x + b)
\end{aligned}
$$

### Non-Linear SVM

SVM的一个主要的限制是它要求训练数据必须是线性可分的，当数据不满足这个条件时显然SVM无法得到正确的解。在这种情况下一般会考虑将数据映射到更高维使得原本线性不可分的数据在高维空间中变得可分。

<div align=center>
<img src="https://i.imgur.com/NCxoYOa.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/P79ksfL.png" width="70%">
</div>

显式地将数据映射到高维再训练SVM往往会极大地提高训练成本，实践中更常见的方法是使用**核函数(kernel function)**来进行处理。它的基本思想是在低维空间中利用核函数来表示高维空间中的内积：

$$
K(x_i, x_j) = \phi(x_i) \cdot \phi(x_j)
$$

实际上使用核函数的SVM与线性SVM并没有很大的差异，我们只需要将线性SVM所有的内积计算替换成核函数即可。比如说此时的分类模型可以表达为：

$$
f(x) = \text{sign} (\sum_i \alpha_i y_i K(x_i, x) + b)
$$

在实践中最常用的核函数是RBF核：

$$
K(x_i, x_j) = \exp \bigg\{ -\frac{\Vert x_i - x_j \Vert^2}{2 \sigma^2} \bigg\}
$$

同时可以证明RBF核等价于无穷维向量的内积：

$$
\exp \bigg\{ -\frac{1}{2} \Vert x - x' \Vert^2 \bigg\} = \sum_{i=1}^\infty \frac{(x^T x')^i}{i!} \exp \bigg \{ -\frac{1}{2} \Vert x \Vert^2 \bigg \} \exp \bigg \{ -\frac{1}{2} \Vert x' \Vert^2 \bigg \}
$$

### Multi-Class SVMs

需要说明的是SVM是定义在二分类问题上的分类器，它不能直接处理多分类的问题。使用SVM来处理多分类问题一般有两种策略：

- 对每个类别训练一个二分类SVM，此时把其他类别的样本都视为负样本，这种方式称为**one vs. all**
- 将所有类别进行组合，对每个类别组合对训练一个二分类SVM，这种方式称为**one vs. one**

最后这里简单总结一下SVM的优点和缺点，SVM的优点包括：

- 实践中有非常多现成的实现，一般不需要从头开始编写；
- 使用核函数的SVM非常强大，可以处理各种非线性的分类问题；
- 通常情况下数据集上只会有少量的支持向量，这使得SVM在测试阶段有非常好的性能；
- 即使数据集很小，SVM也往往会有很好的表现。

相对的，SVM的缺点包括：

- SVM无法直接处理多分类问题，必须要借助二分类来进行处理；
- 选择核函数时往往需要进行不断地调试才能取得比较好的效果；
- SVM的训练过程往往需要耗费大量的计算资源。

## Bag of Visual Words

本节最后我们来介绍一些**词袋模型(bag of words)**。词袋模型是文本处理中的一种常见方法，我们统计文本中的单词出现的频率然后构造出这些单词的直方图。对于内容相似的文本可以假定它们的单词直方图是相似的，因此我们就可以把这种直方图作为特征来训练机器学习模型。而在计算机视觉中我们可以利用相同的思想，把图像特征或是一些特定的区域视为"单词"，然后通过统计图像上这些"单词"的直方图来表示整个图片。

<div align=center>
<img src="https://i.imgur.com/PlTqpaH.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/FnUA4EW.png" width="80%">
</div>

显然在图像上使用词袋模型的难点在于如何计算图像中"单词"出现的频率，这个可以通过计算图像的相似性来实现：

<div align=center>
<img src="https://i.imgur.com/w30vDMs.png" width="80%">
</div>

## Reference

- [Wikipedia: AdaBoost](https://en.wikipedia.org/wiki/AdaBoost)
- [Wikipedia: Viola–Jones object detection framework](https://en.wikipedia.org/wiki/Viola%E2%80%93Jones_object_detection_framework)
- 第7章：支持向量机，统计学习方法（第2版）
- [Wikipedia: Support-vector machine](https://en.wikipedia.org/wiki/Support-vector_machine)
- [Wikipedia: Karush–Kuhn–Tucker conditions](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)