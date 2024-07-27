---
layout: post
title: OMSCS-DL课程笔记15-Neural Machine Translation
date: 2022-03-21
description: 深度学习与机器翻译
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍深度学习在机器翻译中的应用。
{: .block-preface }


## Machine Translation

### Translation is a Hard Problem

**机器翻译(machine translation, MT)**的目标是利用算法将输入语句翻译为指定的语言。和传统分类问题不同，机器翻译的一大特点在于同一个输入语句存在多个等价的翻译形式。同时自然语言往往是含糊不清的，要进行翻译需要考虑语句的上下文。除此之外不同的语言之间往往有着不同的结构，因此在进行翻译时还要考虑不同语言的习惯用法。

<div align=center>
<img src="https://i.imgur.com/HXxD77u.png" width="80%">
</div>

### Sequence Generation

从数学角度上讲，机器翻译可以建模为一个序列搜索问题。对于一个输入序列，机器翻译等价于寻找到具有最大概率的目标语言序列。

<div align=center>
<img src="https://i.imgur.com/Dpi9xNL.png" width="80%">
</div>

利用条件概率我们可以对目标语言序列进行分解，这样就可以通过计算每一步对应的条件概率并且累乘起来得到最终需要的序列概率。不过需要注意的是即使进行了分解也不能直接进行暴力计算，这是因为计算所有可能序列的资源会根据序列长度成指数增长。因此实际计算概率时一般是使用beam search的方式来缩减搜索空间。

<div align=center>
<img src="https://i.imgur.com/jiUtVbp.png" width="80%">
</div>

### Sequence-to-Sequence Model

机器翻译的经典方法是使用**Seq2Seq模型**。此时输入序列首先会经过encoder提取序列的隐状态，然后decoder会利用这个隐状态进行翻译得到输出序列。

<div align=center>
<img src="https://i.imgur.com/KN87o1p.png" width="80%">
</div>

最后通过beam search的方法将输出序列转换为目标语言的token。

<div align=center>
<img src="https://i.imgur.com/wWXvuTg.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/sv4FFXS.png" width="80%">
</div>

除了标准Seq2Seq模型外，常用的机器翻译方法还包括使用双向RNN和LSTM作为encoder提取信息，或者使用一维CNN来代替RNN。近几年随着Transformer方法的流行很多工作还使用了Transformer作为基础架构。

<div align=center>
<img src="https://i.imgur.com/WsleHs0.png" width="80%">
</div>

## Inference Efficiency

在应用场景中机器翻译的计算代价可能是非常大的。这主要是由于机器翻译的对象是一个序列，我们必须按照顺序从前向后进行计算；即使使用了beam search的方法整个翻译过程的计算代价依然依赖于序列长度，当序列比较长时计算代价会变得非常巨大；同时计算模型往往是大规模神经网络，这类模型在推断时本身就需要很多的计算资源。因此人们设计出了很多策略来提高机器翻译的效率，常见的策略包括缩减词典、使用更高效的计算设备以及降低模型复杂度提高并行性等。

<div align=center>
<img src="https://i.imgur.com/vVA1pVa.png" width="80%">
</div>

### Vocabulary Reduction

在进行翻译时词典中的很多词是多余的，因此对计算过程进行加速的一种简单策略是对整个词典进行缩减，翻译每个单词时只考虑一个很小规模的词典。

<div align=center>
<img src="https://i.imgur.com/XFkeUjt.png" width="80%">
</div>

类似的做法是根据单词出现的频率把每个单词分解为subwords，这样也可以极大地缩小词典的规模。

<div align=center>
<img src="https://i.imgur.com/3zXA3DN.png" width="80%">
<img src="https://i.imgur.com/re82wH6.png" width="80%">
</div>

除此之外还可以考虑对模型进行剪枝，如在layer drop中没有使用训练时的所有层而是通过剪枝去掉一些性能不高的层来减小最终的模型规模。

<div align=center>
<img src="https://i.imgur.com/3uejCNq.png" width="80%">
</div>

### Hardware Considerations

计算设备对实际场景中的计算性能有着重要的影响，使用定制的计算设备(GPU、TPU等)可以极大地提高实际计算时的性能。不过需要注意的是这些设备往往是通过并行化来提高计算效率，而在机器翻译中由于任务天然的序列特性可能不会获得太高的性能提升。

<div align=center>
<img src="https://i.imgur.com/PTDDNQN.png" width="80%">
</div>

另一种提升计算效率的方法是牺牲一部分计算精度换取计算性能。

<div align=center>
<img src="https://i.imgur.com/M7UfHvl.png" width="80%">
</div>

最后，目前还有一些方法完全抛弃了Seq2Seq的序列结构直接对整句进行翻译。

<div align=center>
<img src="https://i.imgur.com/9VjCmvQ.png" width="80%">
</div>