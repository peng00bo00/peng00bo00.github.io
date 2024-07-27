---
layout: post
title: OMSCS-DL课程笔记02-Neural Networks
date: 2022-01-19
description: 神经网络基础
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍神经网络的基本知识。
{: .block-preface }


## Neural Network View of a Linear Classifier

**线性分类器(linear classifier)**是最简单的机器学习模型之一，它通过一个线性变换将输入$$X$$映射为输出$$y$$，同时我们还需要一个损失函数来对它进行训练。

<div align=center>
<img src="https://i.imgur.com/uzlx5J8.png" width="80%">
</div>

而简单的神经网络和线性分类器具有相似的结构，每个**神经元(neuron)**接收来自其它神经元的信号并进行组合，如果累计的信号超过一定的阈值则会激活，然后向下一层的神经元发射信号。不过激活的过程往往具有一定的非线性，比如说可以利用sigmoid函数来表示神经元激活后发射的信号。

<div align=center>
<img src="https://i.imgur.com/c0tK5gL.png" width="80%">
<img src="https://i.imgur.com/5Uc12vU.png" width="80%">
</div>

在计算机中，我们通过矩阵乘法来表示相互连接的神经元发射接收信号的过程。神经元接收信号并判断是否激活可以通过权重矩阵$$W$$和偏置向量$$b$$来描述。我们把神经元这样的计算封装成一个层，称为**全连接层(fully connected layer)**。

<div align=center>
<img src="https://i.imgur.com/eMMoZ1O.png" width="80%">
</div>

我们还可以通过一张图来表示神经网络：每个神经元对应层输入/输出层的一个节点，神经元之间的连接则构成了图的边。同时不难发现，基本的线性分类器等价于将输入层和输出层直接相连的神经网络，当然还需要去除掉输出层的激活函数。

<div align=center>
<img src="https://i.imgur.com/tBssqP0.png" width="80%">
</div>

如果把更多的层组合到一起就得到了通常意义下的"神经网络"，其中输入层和输出层直接的称为**隐层(hidden layer)**。在大多数情况下统计神经网络的层数都不包含输入层，因此最基本的神经网络模型是仅包含一个隐层的双层神经网络。

<div align=center>
<img src="https://i.imgur.com/9qsFaya.png" width="80%">
</div>

显然增加隐层的数量可以提高神经网络的表达能力，但实际上简单的三层神经网络就足以描述任何连续的函数，这称为**通用逼近定理(universal approximation theorem)**。需要说明的是尽管理论上三层的神经网络可以近似任何形式的连续函数，对于复杂的情况可能需要指数级或是更多数量的神经元才能进行近似，同时训练这样的大型网络也往往是更加困难的。因此层数更深的网络要比神经元更多的网络有着更加广泛的应用。

<div align=center>
<img src="https://i.imgur.com/oalCB8z.png" width="80%">
</div>

## Computation Graphs

我们不仅可以为神经网络添加全连接层，理论上任意可导的函数都可以添加到网络中。同时，在模型的末端还需要添加一个可导的损失函数以便训练网络。

<div align=center>
<img src="https://i.imgur.com/9yUOxDc.png" width="80%">
</div>

对于复杂的模型，我们还需要关注如何前向计算损失函数以及如何计算损失函数对于每一层参数的导数。

<div align=center>
<img src="https://i.imgur.com/cdbpq9z.png" width="80%">
</div>

为了开发出通用的算法，我们需要把整个模型看做是一张**计算图(computation graph)**。

<div align=center>
<img src="https://i.imgur.com/HMV1b0m.png" width="80%">
</div>

一些函数的计算图表示如下：

<div align=center>
<img src="https://i.imgur.com/eSjWbdS.png" width="40%">
<img src="https://i.imgur.com/SmzDlVE.png" width="43%">
</div>

## Backpropagation

对于给定的计算图，模型的训练包括**前向计算(forward pass)**和**反向计算(backward pass)**两个步骤。在前向计算中，我们把数据带入到图上的输入节点并依次计算节点的输出，然后在反向计算中从损失函数出发从后向前计算损失函数的导数。这个算法称为**反向传播(backpropagation)**算法。

<div align=center>
<img src="https://i.imgur.com/r9YNsd6.png" width="80%">
</div>

### Forward Pass

前向计算的过程比较简单，我们只需要按照节点的拓扑顺序从前到后计算节点输出即可。不过在进行前向计算时可能需要保存一些中间结果以方便反向计算时计算导数。

<div align=center>
<img src="https://i.imgur.com/rxOAwDW.png" width="80%">
</div>

### Backward Pass

在进行反向计算时我们需要计算损失函数关于每一层参数的导数。从模型的最后一层开始，首先计算损失函数对于输出层的导数$$\frac{\partial L}{\partial h^l}$$、$$\frac{\partial L}{\partial W}$$，并把输入项的导数$$\frac{\partial L}{\partial h^l}$$传给上一层。

<div align=center>
<img src="https://i.imgur.com/KY7B6oF.png" width="80%">
</div>

然后考虑损失函数关于其它层的导数。实际上我们不会直接计算损失函数对于某一层的导数，而是首先计算该层的输出关于输入以及参数的导数$$\frac{\partial h^l}{\partial h^{l-1}}$$、$$\frac{\partial h^l}{\partial W}$$。由于计算图上每一层都是可导的，我们可以解析地计算这些局部导数。

<div align=center>
<img src="https://i.imgur.com/X9IQUFw.png" width="80%">
</div>

得到局部导数后再结合**链式法则(chain rule)**将后一层计算的导数乘以当前层的局部导数就可以得到损失函数关于当前层的导数。

<div align=center>
<img src="https://i.imgur.com/AloXoaO.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/gD9Wh6x.png" width="80%">
</div>

这样从后向前依次计算就得到了损失函数关于模型每一层的导数。

<div align=center>
<img src="https://i.imgur.com/N1VW4fN.png" width="80%">
</div>

### Update with Gradients

得到导数后再结合梯度优化算法就可以更新模型的参数。综合以上步骤可以发现，反向传播算法的本质就是利用链式法则和计算图来计算损失函数关于模型参数的导数。

<div align=center>
<img src="https://i.imgur.com/ZqGeoDT.png" width="80%">
</div>

## Automatic Differentiation

反向传播算法不仅仅适用于链式模型，实际上对于任意拓扑结构的计算图只要它是**有向无环图(directed acyclic graph, DAG)**都可以利用反向传播来计算导数。这种利用图的结构以及链式法则来计算导数的方法称为**自动微分(automatic differentiation)**。

<div align=center>
<img src="https://i.imgur.com/JTJQuH6.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/5i8i01w.png" width="80%">
</div>

### A Simple Example

举一个简单的例子，我们可以把$$f(x_1, x_2) = x_1 x_2 + \sin (x_2)$$等价地表示为包含两个输入节点和一个输出节点的计算图如下：

<div align=center>
<img src="https://i.imgur.com/zPhSNqe.png" width="80%">
</div>

在计算导数时在每个节点上将后一层节点传过来的导数以及自身的局部导数相乘即可获得所需的导数。还需要注意的一点是在反向计算时每个节点需要把所有的路径上的导数相加才是输出关于当前节点的导数。

<div align=center>
<img src="https://i.imgur.com/grEj8gF.png" width="80%">
</div>

### Patterns of Gradient Flow

在进行反向计算时不同的运算有着不同的传播模式。对于相加运算，它会把自身的导数直接复制给所有参与计算的节点。

<div align=center>
<img src="https://i.imgur.com/9Py0QsJ.png" width="80%">
</div>

对于相乘运算，每个参与节点接收到的导数等于相乘后的导数乘以另一个参与节点。

<div align=center>
<img src="https://i.imgur.com/7YZ8ihx.png" width="80%">
</div>

而对于最大值运算，导数只会传递给取最大值处的节点，其它节点接收到的导数为0。

<div align=center>
<img src="https://i.imgur.com/pxuSDhW.png" width="80%">
</div>

### Computational Implementation

深度学习框架的本质就是大量事先定义好的计算节点，这样用户在定义模型时实际上定义了一个巨大的计算图。在模型训练时框架会调用反向传播算法来计算损失函数关于每个节点的导数，从而实现模型的更新。

<div align=center>
<img src="https://i.imgur.com/MtLjbGm.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/ZCa3bn2.png" width="80%">
</div>

## Vectorization and Jacobians

本节课最后讨论了**向量化(vectorization)**在深度学习中的应用。实际上在现代深度学习中我们几乎不会使用标量形式的节点来进行计算，通过向量化可以充分利用硬件的性能进而提高计算效率。

<div align=center>
<img src="https://i.imgur.com/m1DYdnV.png" width="80%">
</div>

对于全连接层，矩阵形式的前向计算非常简单，只需要使用矩阵乘法来代替标量乘法即可。

<div align=center>
<img src="https://i.imgur.com/DKDM8Fy.png" width="80%">
</div>

而在反向计算时则要复杂一些。输出向量关于输入向量的局部导数等于系数矩阵$$W$$，在计算导数时只需要和后面传过来的导数相乘即可。真正的难点在于计算输出向量关于系数矩阵的局部导数，通过维度分析不难发现这个局部导数实际上是一个张量，我们需要通过张量内积的方式来计算导数矩阵。不过我们可以把系数矩阵$$W$$看成是若干个行向量$$w_i$$，这样就可以分别计算每一个行向量的局部导数矩阵而无需引入张量内积。更进一步的分析发现，每个行向量的局部导数矩阵是一个稀疏矩阵，仅在对应行上存在元素，这样就可以进一步化简反向计算的过程。

<div align=center>
<img src="https://i.imgur.com/csxaiqx.png" width="90%">
</div>

除了全连接层之外，非线性激活函数也需要进行向量化。由于激活函数是单独作用在每个元素上的，在进行前向计算时需要进行逐元素的操作。

<div align=center>
<img src="https://i.imgur.com/uogorRw.png" width="80%">
</div>

类似地，激活函数在反向计算时也需要进行向量化。由于前向计算时是对每个元素单独进行的计算，因此局部的导数矩阵是一个稀疏的对角矩阵。利用这一点同样可以提高反向计算时的效率。

<div align=center>
<img src="https://i.imgur.com/oskVWQ9.png" width="80%">
</div>

## Reference

- [Wikipedia: Universal approximation theorem](https://en.wikipedia.org/wiki/Universal_approximation_theorem)