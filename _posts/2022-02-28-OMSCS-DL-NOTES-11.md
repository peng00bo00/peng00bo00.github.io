---
layout: post
title: OMSCS-DL课程笔记11-Introduction to Structured Representations
date: 2022-02-28
description: 深度学习的结构化表示
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍如何利用深度学习的进行结构化表示。
{: .block-preface }


## Neural Network Overview

在前面的课程中我们介绍过神经网络和深度学习的基本概念，以及CNN在图像识别中的应用。

<div align=center>
<img src="https://i.imgur.com/QGbqRwM.png" width="80%">
<img src="https://i.imgur.com/tX9ZBty.png" width="80%">
</div>

## Hierarchical Compositionality

深度学习的一个特点在于它具有结构化特征的偏好(bias)。以图像识别为例，网络会对浅层的图像特征进行组合逐渐学习到高层的语义特征。

<div align=center>
<img src="https://i.imgur.com/CKVbnpi.png" width="80%">
</div>

除了图像识别外，很多现实中的任务都有着类似的结构化特征。对于语音识别、自然语言处理等领域，数据往往天然就具有一定的结构。因此我们希望能够使用神经网络来学习数据的这种结构特征。

<div align=center>
<img src="https://i.imgur.com/wWXk7C1.png" width="80%">
</div>

## Relationships are Important

实际上这样的结构化特征是非常常见的。对于一张图像我们可以将图像中的物体用图的方式来描述它们之间的关系；对于一个句子来说不同词之间的语义和语法关系也可以用图结构来进行描述；甚至对于知识和概念本身也可以用类似的形式进行表达。

<div align=center>
<img src="https://i.imgur.com/Wlj3rNy.png" width="80%">
<img src="https://i.imgur.com/XrntHor.png" width="80%">
</div>

## The Space of Architectures

为了适应这些更加复杂的结构关系，人们在标准神经网络的基础上还开发出了**循环神经网络(Recurrent Neural Networks, RNN)**、**注意力网络(Attention-Based Networks)**以及**图神经网络(Graph Neural Networks, GNN)**等更加复杂的网络形式。

<div align=center>
<img src="https://i.imgur.com/HLVgNzx.png" width="80%">
</div>

## Graph Embeddings

显然，我们需要利用**图(graph)**这种数据结构来描述不同实体之间的相互关系。某种意义上讲，结构化表示的核心就是利用图结构来学习不同实体的**嵌入(embedding)**，这样每个实体就可以使用一个特征向量来进行表示。

<div align=center>
<img src="https://i.imgur.com/uMGGlW1.png" width="80%">
</div>

## Propagating Information

使用图进行结构化表达时需要注意一些问题，包括如何去定义系统的状态、如何建立图上不同节点之间的关系、以及如何通过节点之间的消息传播来更新系统的状态。

<div align=center>
<img src="https://i.imgur.com/5vDhYmW.png" width="80%">
</div>

在进行消息传播时一种流行的方式是使用注意力机制。注意力机制往往会使用softmax()函数为每个节点分配不同的注意力权重，这样在进行消息传播时会更加高效。

<div align=center>
<img src="https://i.imgur.com/KVUPMK7.png" width="80%">
</div>