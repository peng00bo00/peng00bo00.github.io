---
layout: post
title: OMSCS-DL课程笔记14-Neural Attention Models
date: 2022-03-14
description: 注意力模型
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍深度学习中的注意力模型。
{: .block-preface }


## Softmax and Preview of Attention

**注意力模型(attentaion model)**是最近几年深度学习领域非常火爆的模型。简单来说attentaion模型的核心是为输入分配不同的计算权重，权重的大小一般可由softmax函数来动态确定。因此在介绍attentaion模型的原理之前我们先回顾一下softmax函数。

<div align=center>
<img src="https://i.imgur.com/OY3k0MU.png" width="80%">
</div>

### Review of Softmax Function

softmax函数是在分类问题中非常常用的函数，它可以把给定的向量转换成一个概率分布。

<div align=center>
<img src="https://i.imgur.com/gqKUD0q.png" width="80%">
<img src="https://i.imgur.com/NMT6YyU.png" width="80%">
<img src="https://i.imgur.com/tqQ7GyI.png" width="80%">
</div>

### Softmax and Index Selection

在分类问题中我们一般会选择概率最大的索引作为最终的类别。但需要注意的是softmax的整个计算过程是可微的，而且我们也可以对softmax进行采样来获取类别标签。

<div align=center>
<img src="https://i.imgur.com/YS6wgwr.png" width="80%">
<img src="https://i.imgur.com/Xjg4KVW.png" width="80%">
<img src="https://i.imgur.com/lPwdFa3.png" width="80%">
</div>

### Selecting a Vector from a Set

在很多应用场景中我们需要根据一个查询向量$$q$$来获取给定向量集合$$$$U=\{ u_1, ..., u_N\}$$$$中最接近的一个向量。最直观的做法是把$$q$$和$$U$$中的每一个向量做内积然后选择其中最大的那个作为输出；而利用softmax函数我们也可以对$$U$$计算概率来实现类似的功能。这种使用softmax函数来计算权重的方法称为**softmax attention**。

<div align=center>
<img src="https://i.imgur.com/tmeLaHy.png" width="80%">
<img src="https://i.imgur.com/6M9pYke.png" width="80%">
<img src="https://i.imgur.com/uVDwCZd.png" width="80%">
<img src="https://i.imgur.com/lq37Ebh.png" width="80%">
</div>

### Softmax Attention and MLP Outputs

在softmax attention中我们会把softmax后的结果作为中间隐层，然后利用$$U$$的概率分布实现不同的功能。

<div align=center>
<img src="https://i.imgur.com/nJ7RElp.png" width="80%">
</div>

## Attention

attention的核心是计算一个关于输入的分布，根据对这个分布的使用方法可以分为hard和soft两类。目前在深度学习中常用的attention方法是soft attention，即把概率分布当做是加权求和的权重进行处理。

<div align=center>
<img src="https://i.imgur.com/78Zb5d4.png" width="80%">
</div>

### Attention in Machine Vision

实际上attention方法也不是一个特别新的概念。在机器视觉领域，人们很早就提出了类似的方法对图像中的ROI进行定位。

<div align=center>
<img src="https://i.imgur.com/hq8jaJv.png" width="80%">
</div>

### Attention in NLP

甚至在NLP领域中attention的思想也不是近几年才出现的。以机器翻译为例，在90年代人们就发现了翻译中的词和词之间不一定是一一对应的，因此可以利用一个概率分布来辅助机器翻译的过程。这种alignment的方式可以看做是attention的雏形。

<div align=center>
<img src="https://i.imgur.com/4GMuTuC.png" width="80%">
</div>

### Attention as a Layer

目前attention的主流实现方法是把它作为网络的一个中间层。一般来说attention层包括一个输入向量集合$$U$$以及一个查询向量$$q$$，进行前向计算时利用softmax函数计算$$U$$和$$q$$内积的概率分布，然后再对$$U$$进行加权得到输出。

<div align=center>
<img src="https://i.imgur.com/gNgQ2FC.png" width="80%">
<img src="https://i.imgur.com/B9xqtmv.png" width="80%">
</div>

从这样的角度来看attention层和fully connection层没有什么本质区别。把attention层串联在一起就得到了一种新的网络架构。

<div align=center>
<img src="https://i.imgur.com/YsKAiBp.png" width="80%">
</div>

在不同的应用中需要注意如何取设置向量集合$$U$$以及查询向量$$q$$，比如说在NLP中往往会把$$U$$设置为词向量而把$$q$$设置为隐状态。同时如果需要考虑输入元素的相对位置关系，还可以显式地添加position encoding进行训练。

<div align=center>
<img src="https://i.imgur.com/SitYnuc.png" width="80%">
<img src="https://i.imgur.com/82TgmEj.png" width="80%">
</div>

## Transformers

attention模型的大规模应用得益于Transformer在NLP领域的展现出的强大性能。和传统的attention模型相比，Transformer的改进可以归结为self-attention、multi-head attention以及residual connections, layerNorm等几部分。

<div align=center>
<img src="https://i.imgur.com/K6lDFIA.png" width="80%">
</div>

### Self-Attention

self-attention直接使用$$U$$中的每个元素$$u_i$$来计算查询向量。

<div align=center>
<img src="https://i.imgur.com/Mrock1b.png" width="80%">
</div>

### Multi-Head Attention

在multi-head attention中则会在一个attention层中同时计算多个加权向量，最后把它们用一个全连接层把它们组合起来作为整个attention层的输出。

<div align=center>
<img src="https://i.imgur.com/dmN5plS.png" width="80%">
<img src="https://i.imgur.com/KWA4DP8.png" width="80%">
</div>

residual connections和layerNorm跟attention模型的关系不大，这里就不多做介绍了。除了标准Transformer外在不同的NLP任务中还会对模型进行一定的调整，根据任务自身的特点可以为Transformer添加position encoding、在训练language model时可以引入causal attention遮住句子中的一部分内容，以及在训练时使用一些加速的技巧等。

<div align=center>
<img src="https://i.imgur.com/d1PaCzm.png" width="80%">
</div>

## Reference

- [The Illustrated Transformer](http://jalammar.github.io/illustrated-transformer/)