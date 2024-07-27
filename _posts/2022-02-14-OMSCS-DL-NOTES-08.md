---
layout: post
title: OMSCS-DL课程笔记08-Scalable Training
date: 2022-02-14
description: PyTorch的可拓展性
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍PyTorch的可拓展性。
{: .block-preface }


## Scaling Deep Learning from Experiment to Production

### Compute with PyTorch

在生产环境中我们希望程序有足够好的性能，因此在编写代码时需要一些技巧来进行优化。对于包含循环的代码一般可以通过向量化的方式来提高程序运行效率。

<div align=center>
<img src="https://i.imgur.com/ZlMjp0m.png" width="80%">
<img src="https://i.imgur.com/z44JiiI.png" width="80%">
</div>

同时，PyTorch也提供了一些函数方便测试代码的性能。

<div align=center>
<img src="https://i.imgur.com/2vkoCjf.png" width="80%">
</div>

### Script for Performance

默认情况下PyTorch使用**eager mode**来执行代码，eager mode适合在编写代码时进行调试但通常会损失一些性能。如果想要提升性能可以主动切换到**script mode**作为运行环境。

<div align=center>
<img src="https://i.imgur.com/XMofyoV.png" width="80%">
</div>

在script mode下同样代码的性能会得到显著的提升：

<div align=center>
<img src="https://i.imgur.com/Nk4AuYw.png" width="80%">
<img src="https://i.imgur.com/jEtJT8b.png" width="80%">
</div>

script mode会对代码进行优化来提高运行效率并降低内存负载，甚至还可以去掉GIL的限制进一步提升并行性能：

<div align=center>
<img src="https://i.imgur.com/93TOsJ2.png" width="80%">
</div>

同时PyTorch也允许通过C++自定义运算操作。

<div align=center>
<img src="https://i.imgur.com/64cPtvc.png" width="80%">
</div>

## Ingest Data

使用PyTorch训练模型时一般需要定义模型和优化器、读取数据、计算损失、反向传播更新模型等步骤。

<div align=center>
<img src="https://i.imgur.com/NDgynP4.png" width="80%">
</div>

在定义模数据集时建议使用PyTorch自带的`DataSet`类，除了对数据进行索引外它还允许在读取时对数据进行各种预处理。

<div align=center>
<img src="https://i.imgur.com/ZQ547X6.png" width="80%">
</div>

使用`DataSet`自定义数据集后就可以利用`DataLoader`来加载数据集。`DataLoader`会根据`batch_size`的大小自动获取指定批量的数据，同时还支持多线程的方式来加载数据提升性能。

<div align=center>
<img src="https://i.imgur.com/2OTMNyJ.png" width="80%">
<img src="https://i.imgur.com/xG8sYod.png" width="80%">
</div>

## Use Multiple GPUs and Machines

在生产环境中有时需要通过并行的方式来训练模型，这里首先对并行的模式进行区分。模型相同但数据分布在不同设备上的情况称为**data parallel**，而模型自身比较大必须分配到不同设备上的情况则称为**model parallel**。根据运行设备的数量还可以对它们继续细分。

<div align=center>
<img src="https://i.imgur.com/pb6Wq9l.png" width="80%">
<img src="https://i.imgur.com/BbvOT1Z.png" width="80%">
</div>

### Single Machine Data Parallel

最基本的情况是single machine data parallel。此时我们需要把一个batch的数据分配到不同的GPU上独立计算输出，然后由某个GPU收集计算出的数据并计算损失。

<div align=center>
<img src="https://i.imgur.com/Wc3OF5N.png" width="80%">
</div>

对于这种情况只需要调用`torch.nn.DataParallel(model)`即可。

<div align=center>
<img src="https://i.imgur.com/xQNWJZI.png" width="80%">
</div>

### Single Machine Model Parallel

对于single machine model parallel的情况，我们需要在定义模型时将不同的部件分配到不同的GPU上同时在定义前向传播时指定数据所在的GPU。

<div align=center>
<img src="https://i.imgur.com/0LEFtxa.png" width="80%">
<img src="https://i.imgur.com/e1RkAJR.png" width="80%">
</div>

### Distributed Data Parallel

对于distributed data parallel的情况，训练数据分布在不同机器的不同GPU上。

<div align=center>
<img src="https://i.imgur.com/9oCgiBt.png" width="80%">
</div>

在训练模型时需要指定机器和GPU的编号，然后通过`torch.multiprocessing.spawn()`开启训练。

<div align=center>
<img src="https://i.imgur.com/wZFxRsv.png" width="80%">
<img src="https://i.imgur.com/dVJUfqC.png" width="80%">
</div>

### Distributed Data Parallel with Model Parallel

把data parallel和model parallel结合到一起就得到了distributed data parallel with model parallel，此时数据和模型都处于不同机器的不同GPU上。

<div align=center>
<img src="https://i.imgur.com/URBVGWK.png" width="80%">
</div>

模型训练的代码与distributed data parallel类似，但在定义模型时需要指定模型不同组件分配的设备。

<div align=center>
<img src="https://i.imgur.com/fKLEkYe.png" width="80%">
</div>

## Distributed Model Parallel

在一些更复杂的场景中可能在不同的机器上都有不同的模型(组件)，此时就需要使用RPC来进行训练。

<div align=center>
<img src="https://i.imgur.com/RaUqifG.png" width="80%">
<img src="https://i.imgur.com/hXSD3Eu.png" width="80%">
<img src="https://i.imgur.com/Xd4yda6.png" width="80%">
</div>