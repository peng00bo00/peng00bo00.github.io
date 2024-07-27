---
layout: post
title: OMSCS-DL课程笔记13-Embeddings
date: 2022-03-07
description: 深度学习中的嵌入
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍各种嵌入技术。
{: .block-preface }


**嵌入(embedding)**的目的是获得不同对象的结构化表示。在深度学习中embedding一般是指通过一个神经网络来将不同类型的对象转换为向量表示，embedding的对象可以是词、图片、句子、概念等等。一个好的embedding需要将具有相似属性的对象映射到相邻的空间中，因此embedding问题的核心是如何训练神经网络来获得合适的embedding向量。

<div align=center>
<img src="https://i.imgur.com/ZIPMwWv.png" width="80%">
<img src="https://i.imgur.com/uf1ba4q.png" width="80%">
</div>

## Word Embeddings

**词嵌入(word embedding)**是NLP中的一个基础方法。它的基本原理是**语义的分布假设(distributional semantics)**，即每个单词的语义是由它的上下文语境来决定的，因此可以通过单词的上下文来建立词向量。

<div align=center>
<img src="https://i.imgur.com/7cZY7xb.png" width="80%">
</div>

实际上词嵌入并不是一个特别新的技术，早在03年就提出了通过大量语料训练词向量的方法。不过词嵌入的大规模应用则要归功于13年提出的**word2vec**方法。

<div align=center>
<img src="https://i.imgur.com/eN2HULc.png" width="80%">
</div>

在介绍word2vec前我们简单了解一下传统的词向量是如何训练的。它的基本思路是把词向量的训练转换成一个二分类问题：我们可以把一个句子中出现的单词看做是正样本，把随机替换单词后的句子看做负样本，然后通过SVM来训练得到词向量。

<div align=center>
<img src="https://i.imgur.com/Tiuwv82.png" width="80%">
</div>

### Word2vec

目前更加流行的词嵌入方法是基于word2vec来训练词向量。word2vec使用skip-gram的方式来选取正样本，即对于中心词$$w_t$$把它前后相邻$$m$$距离内的词作为正样本。模型的目标在于给定中心词$$w_t$$的条件下预测窗口内单词出现的概率$$P(w_{t+i} \vert w_t)$$。

<div align=center>
<img src="https://i.imgur.com/oY5Z1tz.png" width="80%">
<img src="https://i.imgur.com/V1DAv7E.png" width="80%">
</div>

在训练word2vec时我们只需要通过滑窗的方式来遍历语料，然后最大化正样本的联合概率即可，这种方式等价于最小化似然函数的负对数损失。

<div align=center>
<img src="https://i.imgur.com/Qc7vNEF.png" width="80%">
</div>

### Negative Sampling

word2vec使用了softmax函数来计算正样本的概率。对于中心词$$w_t$$和上下文单词$$w_{t+j}$$，这一对单词的倾向性由词向量的内积给出。因此在给定中心词$$w_t$$的条件下出现单词$$w_{t+j}$$的概率$$P(w_{t+j} \vert w_t)$$可以由softmax函数来计算，其中分母需要对所有可能的单词对进行求和。

<div align=center>
<img src="https://i.imgur.com/T2S6W01.png" width="80%">
</div>

显然直接计算分母的代价过于巨大，因此实践中更常用的方法是通过**负样本采样(negative sampling)**来进行训练。在构造负样本时可以通过随机采样的方式选取正样本$$k$$倍的随机词来取代上下文词，然后训练一个二分类模型来获得最终所需的词向量。此时整个训练过程相当于最大化正样本出现的概率同时最小化负样本出现的概率。

<div align=center>
<img src="https://i.imgur.com/1yawVC0.png" width="80%">
</div>

当然除了word2vec之外还有很多其它的词嵌入技术。

<div align=center>
<img src="https://i.imgur.com/sDfZMHO.png" width="80%">
</div>

### Evaluation

得到词向量之后我们还需要去评估词向量的性能。目前词向量的平均方法包括intrinsic和extrinsic两类，其中intrinsic是在给定的任务上计算性能指标而extrinsic则是在真实的任务上进行评价。

<div align=center>
<img src="https://i.imgur.com/NPbTjMu.png" width="80%">
</div>

一般来说extrinsic的计算代价会比较大，因此目前最常用的评价方法仍然是intrinsic相关的方法。intrinsic方法在评估词向量时会使用analogy test，即测试具有相似概念的两对单词它们的距离是否相似。具体来说我们可以选择man和woman这一对单词并且计算它们词向量空间上的距离，然后把这个距离作用在单词king上并寻找和该点最接近的单词。如果找到的单词是queen则说明词向量学习到了类比的性质。

<div align=center>
<img src="https://i.imgur.com/DMVYXEo.png" width="80%">
</div>

## Graph Embeddings

很多现实生活中的数据可以使用图结构来进行表示，通过**图嵌入(graph embedding)**技术我们可以研究图上不同之间的关系。

<div align=center>
<img src="https://i.imgur.com/3FTOTT4.png" width="80%">
</div>

图数据一个常见的例子是**推荐系统(recommender system)**。这样的图上包含用户和商品两类节点，而用户对商品的评价构成了图的边。当我们利用图嵌入学习到不同用户和商品的表示后就可以更有针对性地为用户推荐商品。

<div align=center>
<img src="https://i.imgur.com/z1IDuwo.png" width="80%">
</div>

图嵌的目标是获得图上每个节点的表示，使得有边相连接的节点有相似的节点向量。同时图嵌入的学习过程更类似于**无监督学习(unsupervised learning)**，我们无法事先得知每个节点的嵌入信息而是通过图结构来获取它们的嵌入。获得图嵌入后除了分析节点之间的关系外还可以使用节点向量来辅助下游的各种任务。

<div align=center>
<img src="https://i.imgur.com/peelTX1.png" width="80%">
<img src="https://i.imgur.com/juh6MkB.png" width="80%">
</div>

在训练图嵌入时可以使用图上的边作为单位进行采样。首先我们需要去度量节点之间的相似度，一种常用的方法是通过cos距离来度量起点嵌入$$\theta_s$$和终点嵌入$$\theta_d$$之间的差异，如果图上有不止一种边的类型则还需要考虑不同边的种类$$\theta_r$$。这样我们可以把图上已有的边作为正样本，随机调整这些边得到负样本，通过训练SVM获得每个节点和边类型的嵌入。

<div align=center>
<img src="https://i.imgur.com/LEaJKzV.png" width="80%">
<img src="https://i.imgur.com/V1GKE7K.png" width="80%">
<img src="https://i.imgur.com/yu3lyh2.png" width="80%">
</div>

对于包含多种类型边的图，我们可以使用不同的方式对这些节点关系进行建模。

<div align=center>
<img src="https://i.imgur.com/kQOu7Gq.png" width="80%">
</div>

目前图嵌入已经在现实生活中取得了一定的应用。最常见的例子是知识图谱和问答系统，通过对知识库进行嵌入我们可以通过对图进行查询的方法来回答提问者的问题。除此之外直接对图上的节点进行可视化也能够发现嵌入后的节点具有一些聚类的性质。

<div align=center>
<img src="https://i.imgur.com/qUBNsLd.png" width="80%">
<img src="https://i.imgur.com/vrgUhlL.png" width="80%">
</div>

对于大规模的图，如何计算负样本得分对于训练效率有着尤为重要的影响。试验表明，通过一些批量化的方法可以上百倍地提高训练的效率。

<div align=center>
<img src="https://i.imgur.com/UsQzf0D.png" width="80%">
</div>

## Applications

接下来我们介绍各种嵌入技术的应用。

### TagSpace, PageSpace

TagSpace的目标是获得用户文本对应的标签，而PageSpace则描述了不同用户主页之间的相互关系。它们在社交媒体和门户网站中有着重要的应用价值。

<div align=center>
<img src="https://i.imgur.com/RAsZz1g.png" width="80%">
<img src="https://i.imgur.com/sSV9U56.png" width="80%">
</div>

### VideoSpace

类似地，我们可以对视频进行嵌入。每个视频包含的信息包括上传用户的个人信息、视频的图像信息以及文字描述信息。通过获取每个视频的嵌入我们可以更好地对视频内容进行甄别，同时也便于把视频推送给感兴趣的用户。

<div align=center>
<img src="https://i.imgur.com/uO2PUXV.png" width="80%">
</div>

### world2vec

更进一步，我们可以把用户的任意信息通过图来表示，然后获取图上节点的嵌入。

<div align=center>
<img src="https://i.imgur.com/KUHroQQ.png" width="80%">
</div>

得到嵌入向量后就可以实现各种不同的功能，比如说分析用户的喜好、检测用户的异常行为、推送用户可能感兴趣的信息等。

<div align=center>
<img src="https://i.imgur.com/vBJ2K6A.png" width="80%">
</div>

同时图嵌入本身也有很多有价值的信息，比如说检测图上的不同聚类、把嵌入向量作为下游分类器的输入等。

<div align=center>
<img src="https://i.imgur.com/FgjW7DL.png" width="80%">
</div>

整个图嵌入的训练过程可以参考下图。

<div align=center>
<img src="https://i.imgur.com/Fg1iVu0.png" width="80%">
</div>

对于大规模的图数据，可以使用PyTorch-BigGraph来进行处理。

<div align=center>
<img src="https://i.imgur.com/hMnFnPt.png" width="80%">
<img src="https://i.imgur.com/6WZXToK.png" width="80%">
<img src="https://i.imgur.com/0rRs6r1.png" width="80%">
</div>

## Additional Topics

除了标准的图嵌入方法之外，一些近期的研究还试图学习节点之间的结构化嵌入。

<div align=center>
<img src="https://i.imgur.com/DOuP9ap.png" width="80%">
</div>

同时一些研究还表明嵌入可能是有倾向性的，如何识别并去除数据集的偏见也是一个热门的研究领域。

<div align=center>
<img src="https://i.imgur.com/DTYTD4P.png" width="80%">
<img src="https://i.imgur.com/RbDCxuz.png" width="80%">
</div>