---
layout: post
title: OMSCS-DL课程笔记04-Data Wrangling
date: 2022-01-29
description: 数据预处理
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍深度学习中的数据预处理方法。
{: .block-preface }


## Data Wrangling

数据预处理是机器学习的重要一环。无论是什么类型的机器学习任务，我们都需要先对原始数据进行一些处理才能进行后续的模型训练、调优等工作。数据预处理的基本流程如下图所示。

<div align=center>
<img src="https://i.imgur.com/dZu816B.png" width="80%">
</div>

数据预处理的第一步是确定数据的"总体"并从中采样出一定的样本。

<div align=center>
<img src="https://i.imgur.com/UdanrvN.png" width="80%">
</div>

在进行采样时有很多种不同的策略。最基本的方法是进行随机采样，即每个数据点都具有相同的概率被采样到；有时也会用到分层采样的方法，此时会首先把总体划分成若干组然后从每一组中进行随机采样。

<div align=center>
<img src="https://i.imgur.com/kVyxNlK.png" width="80%">
</div>

## Cross Validation and Class Imbalance

获得数据样本后可以需要将这些样本划分为训练集和测试集，一般还会结合**交叉验证(cross validation)**来评估模型的性能。

<div align=center>
<img src="https://i.imgur.com/uB26tnu.png" width="80%">
</div>

### Cross Validation

交叉验证是在机器学习建立模型和验证模型参数时常用的办法。在交叉验证中我们进一步将训练数据拆分为训练集合测试集，其中训练集用来训练模型而测试集用来评估模型的性能。交叉验证中最常用的方法是k-fold交叉验证，此时我们把所有训练数据均等分为k份，每次选择其中一份作为验证集其它作为训练集，然后使用模型在全部验证集上的性能作为最终的性能估计。

<div align=center>
<img src="https://i.imgur.com/PW9bkxh.png" width="80%">
<img src="https://i.imgur.com/RRFaz9O.png" width="80%">
<img src="https://i.imgur.com/6F7NjSp.png" width="80%">
<img src="https://i.imgur.com/aCz97PR.png" width="80%">
<img src="https://i.imgur.com/Ln6w8et.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/mad6kQl.png" width="80%">
</div>

### Class Imbalance

**类别不均衡(class imbalance)**是训练模型时的一个常见问题。大部分的机器学习理论都建立在不同类别的数据量都比较接近的假设上，因此不均衡的数据分布会严重影响机器学习模型的性能。

在现实中的机器学习任务中类别不均衡是一个很常见的问题。以计算机视觉中的目标检测为例，常用的检测模型都是假设物体会出现在图像上均匀分布的**锚框(anchor)**附近，然后模型会对这些锚框进行分类和回归来提取物体的信息。显然图像上的大部分锚框都不会是真正的物体而是背景，这种情况下就导致了严重的正负样本不均衡问题。

<div align=center>
<img src="https://i.imgur.com/Nur5BbB.png" width="80%">
</div>

要解决类别不均衡的问题一种方法是使用自适应的损失函数使得模型不会过于关注某一种类别产生的损失。在目标检测中常用**Focal Loss**来计算前景和背景的分类损失。

<div align=center>
<img src="https://i.imgur.com/wWtS1UV.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/HYQEq3h.png" width="60%">
</div>

当然我们也可以在采样时进行一些处理来避免出现类别不均衡，不过需要注意在采样时不要出现数据泄露等问题。

<div align=center>
<img src="https://i.imgur.com/qrv4f9q.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/C4mBpZ5.png" width="80%">
</div>

## Prediction and Evaluation

### Model Evaluation Statistics

预处理的第三步是选择合适的指标来评价模型的性能。

<div align=center>
<img src="https://i.imgur.com/zAjQOMi.png" width="80%">
</div>

对于分类任务常用的性能度量包括**精度(precision)**、**召回率(recall)**、**正确率(accuracy)**等，而对于回归问题则常用**MSE**等度量。

<div align=center>
<img src="https://i.imgur.com/NDuNZfw.png" width="80%">
</div>

同时我们还需要设置一个性能度量的**基准线(baseline)**，如果模型的性能低于基准线则说明训练得到的模型是无意义的。

<div align=center>
<img src="https://i.imgur.com/HuCGA6e.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/7yLw3ou.png" width="80%">
</div>

### Datasheets for Datasets

最后我们还需要考虑如何重复整个试验过程。这一步比较简单，往往只需要用文档记录下一些试验的过程和细节即可。

<div align=center>
<img src="https://i.imgur.com/LJptziA.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/ELZdTTR.png" width="80%">
</div>

## Data Cleaning for Machine Learning

在准备数据阶段一般需要对收集到的数据进行一些清理工作。

<div align=center>
<img src="https://i.imgur.com/55xdkfE.png" width="80%">
</div>

### Missing Data

在收集数据时最常见的问题是数据缺失，根据缺失的模式可以分为三种不同类型。

<div align=center>
<img src="https://i.imgur.com/Oor7bBl.png" width="80%">
</div>

对缺失的数据进行处理时最直接的方法是将缺失的数据删除，不过这样会丢失一些信息。

<div align=center>
<img src="https://i.imgur.com/RHvcbm8.png" width="80%">
</div>

因此实践中更常用的做法是使用特征的统计值来填充缺失的数据。

<div align=center>
<img src="https://i.imgur.com/ToXNDyq.png" width="80%">
</div>

### Transform

获得完整的数据后需要根据数据的特征以及任务的类型将它们转换成合适的格式。

<div align=center>
<img src="https://i.imgur.com/p9p8XR9.png" width="80%">
</div>

### Pre-process

有时还会进一步对数据进行一些归一化处理以方便模型的训练。

<div align=center>
<img src="https://i.imgur.com/qIjni2a.png" width="80%">
</div>

### Case Study: Depth Estimation

以**深度估计(depth estimation)**为例，下面展示了整个数据集构建的流程。

<div align=center>
<img src="https://i.imgur.com/lB11hkN.png" width="80%">
<img src="https://i.imgur.com/ggdu5Au.png" width="80%">
<img src="https://i.imgur.com/aq7Mn8X.png" width="80%">
</div>

## Managing Bias

最后需要说明的是在构建数据集时要尽可能避免使用带有偏见或是歧视性的信息。

<div align=center>
<img src="https://i.imgur.com/SBI3sTE.png" width="80%">
</div>