---
layout: post
title: OMSCS-DL课程笔记03-Optimization of Deep Neural Networks
date: 2022-01-25
description: 深度学习中的优化算法
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍深度学习中的优化算法。
{: .block-preface }


## Architectural Considerations

在深度学习中模型的结构对于最终的性能起着至关重要的作用。在设计网络结构时一般需要考虑不同的数据类型和数据自身的特征，同时还要考虑反向传播时梯度传播的形式以免在训练时产生问题。

<div align=center>
<img src="https://i.imgur.com/5Qo5RFQ.png" width="80%">
</div>

### Linear and Non-Linear Modules

根据计算的形式我们可以把模型中的层分为线性层和非线性层两类。其中线性层是指对输入数据进行线性变换，典型的例子是使用矩阵乘法来表示全连接层；而非线性层则表示对输入数据进行非线性变换，典型的非线性层是网络的激活函数层。需要注意的是线性层的组合仍然是一个线性层，因此非线性层对于提升网络的性能至关重要。

<div align=center>
<img src="https://i.imgur.com/hMmXFMH.png" width="80%">
</div>

### Analysis of Non-Linear Function

接下来我们介绍一些常用的非线性层(激活函数)以及它们的性质。

<div align=center>
<img src="https://i.imgur.com/aDPI60G.png" width="80%">
</div>

#### Sigmoid Function

sigmoid函数是传统神经网络最常用的激活函数，它的特点是能够将任意输入数值映射到(0, 1)区间上。sigmoid函数的缺陷在于当输入数值很大或是很小的时候会出现**饱和(saturation)**的现象，即局部的导数会趋于0容易产生模型无法训练的问题。除此之外sigmoid函数在计算时还涉及对指数函数的运算，因此它的计算效率相对较低。

<div align=center>
<img src="https://i.imgur.com/1zZ2fcX.png" width="80%">
</div>

#### Tanh Function

tanh函数也是传统神经网络常用的一种激活函数。类似于sigmoid，tanh会将输入数值映射到(-1, 1)区间上，而且当函数值较大或较小的时候也会出现饱和的问题。

<div align=center>
<img src="https://i.imgur.com/Jku5UHt.png" width="80%">
</div>

#### Rectified Linear Unit

ReLU函数是现代神经网络最常用的激活函数之一。它会抑制输入数据小于0的部分并将它们映射为0，而对于大于0的部分则保持不变。因此ReLU函数没有饱和的问题，同时在计算时只涉及比较大小它的计算效率要比sigmoid和tanh高得多。

<div align=center>
<img src="https://i.imgur.com/NaaEaHy.png" width="80%">
</div>

#### Leaky ReLU

ReLU函数的缺陷在于反向传播时它会将输入数据小于0的部分导数映射为0。为了解决这样的问题人们开发出了Leaky ReLU这种激活函数，它通过一个可学习参数$$\alpha$$来对数据进行线性变换。Leaky ReLU在计算时的开销要稍大于ReLU，但仍然是非常高效的。

<div align=center>
<img src="https://i.imgur.com/XqamkgT.png" width="80%">
</div>

最后需要说明的是目前不存在所谓"最好"的激活函数，在实践中一般需要根据不同的任务类型对不同的激活函数进行调试来选择。在大多数情况下ReLU函数都具有比较好的性能，而sigmoid函数由于存在饱和的问题因此往往只在需要对数据进行规范化时才会使用。

## Initialization

在训练模型前我们还需要为模型赋予一定的初值，这样的过程称为**初始化(initialization)**。初始化对于模型的训练过程有着重要的影响，一个好的初始化不仅可以加快模型的训练过程，还往往会提升模型最终的性能

<div align=center>
<img src="https://i.imgur.com/EBt3z6M.png" width="80%">
</div>

### A Poor Initialization

一个不好的初始化是将模型的所有参数初始化为一个常数(比如说0)，在这种情况下模型会出现退化的问题。具体来说在进行反向传播时每一个节点得到的梯度都是相同的，因此模型在更新时所有的参数都会更新为相同的值。显然这种情况下模型的性能不会很好。

<div align=center>
<img src="https://i.imgur.com/fVHh6Er.png" width="80%">
</div>

### Gaussian/Normal Initialization

因此更加常用的初始化方法是进行随机初始化，比如说利用均匀分布或是正态分布来初始化参数。这样初始化会使模型参数都分布在一个比较小的区间上，这意味着模型对于不同类型的特征没有明显的偏好，也即所有的输入特征都是相同重要的。

<div align=center>
<img src="https://i.imgur.com/NMZLcxE.png" width="80%">
</div>

还需要说明的是深层网络对于模型的初始化十分敏感。研究发现随着网络层数的增加每一层输出的标准差会不断缩小，这就会导致反向传播时浅层的参数获得的梯度比较小使得训练大型神经网络更加困难。而如果一味地增加初始化的值则容易导致饱和的问题，同样无法训练大型的神经网络。

<div align=center>
<img src="https://i.imgur.com/97M2VGh.png" width="80%">
</div>

### Xavier Initialization

为了解决这样的问题人们提出了**Xavier初始化(Xavier initialization)**，它的思想是通过调整初始化参数使得每一层的输入和输出都具有类似的分布。

<div align=center>
<img src="https://i.imgur.com/QpsUES9.png" width="80%">
</div>

Xavier初始化利用($$-\frac{\sqrt{6}}{\sqrt{n_j + n_{j+1}}}$$, $$\frac{\sqrt{6}}{\sqrt{n_j + n_{j+1}}}$$)上的均匀分布来进行初始化。在实践中人们还发现只需要对正态分布初始化稍作变换也能够达到类似的效果。

<div align=center>
<img src="https://i.imgur.com/QyLt38i.png" width="80%">
</div>

## Normalization

### Preprocessing

除了模型参数外输入数据自身的分布也会对模型的训练过程产生影响，因此在深度学习中我们还会对输入数据进行一定的**规范化(normalization)**来使得模型的训练过程更加稳定。常用的规范化方法包括直接减去均值并除以标准差，以及利用**白化(whitening)**将数据各个方向都进行规范化等。

<div align=center>
<img src="https://i.imgur.com/HI9AUBt.png" width="80%">
</div>

### Batch Normalization

除此之外在深度学习中我们还可以单独设计一个规范化层来实现对任意数据的规范化。**batch normalization**就是这样的一种规范化方法，它会首先统计当前输入批次数据的均值和方差并对它们进行标准化。

<div align=center>
<img src="https://i.imgur.com/yBWXQzG.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/h7U1TyX.png" width="80%">
</div>

然后batch normalization利用两个可学习的参数$$\gamma$$和$$\beta$$将标准化后的数据规范化到指定的区间上。和传统的规范化方法相比batch normalization利用可学习参数来调节规范化后的范围，因此往往具有更好的效果。

<div align=center>
<img src="https://i.imgur.com/ijIBzzx.png" width="80%">
</div>

batch normalization在现代神经网络中有着大量的应用。一般可以将它放置在激活函数前来避免激活函数的输入过大从而导致的饱和问题。

<div align=center>
<img src="https://i.imgur.com/7VYbmKj.png" width="80%">
</div>

需要说明的是batch normalization在模型训练和推断时有着不同的行为。在进行推断时不会再计算当前数据上的均值和方差，而是在训练过程中存储它们的估计值来表示总体数据集上的行为。同时每一批次数据的量对于batch normalization也有着重要的影响，如果数量比较小则可能无法获得稳定的估计值。而对于数量很大的情况有时无法使用单个GPU来存储数据，此时需要使用一些分布式的方法来进行训练。

<div align=center>
<img src="https://i.imgur.com/23KPPyp.png" width="80%">
</div>

## Optimizers

我们知道神经网络的训练过程等价于一个优化问题。和其它常见的优化问题相比，神经网络的优化具有明显的非凸性和非线性，因此神经网络的优化更加复杂目前也没有比较好的理论来进行指导。

<div align=center>
<img src="https://i.imgur.com/A4SQMN5.png" width="80%">
</div>

### Loss Landscape

目前对神经网络进行训练的主流方法仍是基于梯度下降的相关算法。由于神经网络的目标函数有着明显的非凸性，在进行优化时往往会遇到大量的局部极值。除此之外一些常见的问题还包括梯度的噪声、鞍点以及目标函数的病态。

<div align=center>
<img src="https://i.imgur.com/rIWjpBw.png" width="80%">
</div>

#### Noisy Gradients

在计算梯度时我们会取一部分数据来计算损失函数并进行反向传播，然后它们的梯度取平均作为当前批次数据的梯度。这种计算梯度的方法是无偏的，但往往具有比较大的方差。体现在优化上就是函数不会沿着一个确定的优化方向，而是在最优方向附近不断摆动前进。

<div align=center>
<img src="https://i.imgur.com/sac9PJF.png" width="80%">
</div>

#### Loss Surface Geometry

在优化过程中根据优化函数的几何性质可以把极值点分为局部极值、plateaus以及鞍点几种。如果训练时函数陷入到这些位置上则很难进行进一步的优化。

<div align=center>
<img src="https://i.imgur.com/lnwtvOE.png" width="80%">
</div>

### Adding Momentum

为了克服局部极值的问题我们可以在优化过程中引入**动量项(momentum)**。所谓"动量项"是指对过去的梯度进行一定的平均，这样即使当前的梯度很小通过动量项仍然可以获得足够大的更新值从而越过局部极值。

<div align=center>
<img src="https://i.imgur.com/4yfwG4g.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/n8CJ5j2.png" width="75%">
</div>

如果对动量项进行展开，不难发现动量项本质是对过去的梯度进行了指数加权平均。越老的梯度具有的权重越小，越近的梯度具有的权重越大。

<div align=center>
<img src="https://i.imgur.com/4yugSQv.png" width="80%">
</div>

另一种常用的动量项是**Nesterov动量(Nesterov momentum)**，它的方式是沿着当前速度方向移动一步然后再计算梯度。

<div align=center>
<img src="https://i.imgur.com/TKZVf6k.png" width="80%">
</div>

### Per-Parameter Learning Rate

除了在更新过程中引入动量项之外，我们还可以动态地调整学习率从而加速模型的训练过程。常见的算法包括Adagrad、RMSProp以及Adam等。

<div align=center>
<img src="https://i.imgur.com/268aOKZ.png" width="80%">
</div>

#### Adagrad

Adagrad的思想是利用梯度的统计值来衰减实际学习率。如果梯度在某个方向上有着很大的累计值，那么说明优化函数在这个方向上有着较大的曲率，因此我们需要缩减该方向上移动的步长。

<div align=center>
<img src="https://i.imgur.com/u1KhvPA.png" width="80%">
</div>

#### RMSProp

Adagrad的缺陷在于随着训练轮数的增加梯度累计值会不断增长，使得在所有方向上移动的步长都会比较小。为了克服这个问题RMSProp使用了滑动平均的方式来更新梯度累计值，从而避免实际学习率过小的问题。

<div align=center>
<img src="https://i.imgur.com/KhkYb3T.png" width="80%">
</div>

#### Adam

更进一步，Adam算法提出了使用梯度的一阶矩和二阶矩来进行更新的方式。

<div align=center>
<img src="https://i.imgur.com/Jqx4jYv.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/B9Xh6UI.png" width="80%">
</div>

#### Behavior of Optimizers

近期的研究显示，不同的优化算法在不同的优化函数上的行为有着巨大的差异。而标准SGD+Momentum的方式可能有着更好的泛化性能，当然和上面提到过的自适应方法相比标准SGD+Momentum需要进行更多的调试才能得到比较好的效果。

<div align=center>
<img src="https://i.imgur.com/bx7K9zJ.png" width="80%">
</div>

### Learning Rate Schedules

最后，我们还可以显式指定学习率的更新策略来加速训练过程。基本的思想是在一开始选择比较大的学习率，然后当损失函数收敛后使用比较小的学习率来进行更进一步的优化。同时一些近期的研究还显示周期性的学习率也能够获得比较好的训练效果。

<div align=center>
<img src="https://i.imgur.com/ClewPfY.png" width="80%">
</div>

## Regularization

在训练神经网络时我们往往还需要引入一些**正则项(regularization)**来避免模型出现过拟合的问题。

<div align=center>
<img src="https://i.imgur.com/YcgIE03.png" width="80%">
</div>

最简单的正则项是把模型参数的范数加入到损失函数中，这样可以避免出现过大的参数。

<div align=center>
<img src="https://i.imgur.com/xEnBBXA.png" width="80%">
</div>

### Dropout

另一种常用的正则化方法是**dropout**，它会在训练过程中将模型的一部分输出设置为0从而限制模型的学习能力。

<div align=center>
<img src="https://i.imgur.com/dxoCq8n.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/1HaQAPA.png" width="80%">
</div>

类似于batch normalization，dropout在进行推断时也有着和训练时不同的行为。由于训练时将一部分输出设置为了0，我们需要保证在推断时模型的参数的分布与训练时保持一致。因此常用的方法是在推断时将所有参数乘以$$p$$，或是在训练时将每一层的输出除以$$p$$。

<div align=center>
<img src="https://i.imgur.com/jVThPPj.png" width="80%">
</div>

最后关于dropout背后的原理目前有两种解释：其一是认为dropout避免了模型过于依赖某些特定的特征从而防止出现过拟合；而另一种解释则是认为通过dropout我们实际上训练了$$2^n$$个模型，这样最终的模型等价于利用集成学习的方式来减轻过拟合。

<div align=center>
<img src="https://i.imgur.com/v4TlPV9.png" width="80%">
</div>

## Data Augmentation

**数据增强(data augmentation)**是一种常用的训练技巧，我们可以对原始数据进行各种各样的变换从而提高数据集的丰富性。以图像分类为例，常用的数据增强包括随机翻转、裁剪、染色、几何变换以及它们的复合等。

<div align=center>
<img src="https://i.imgur.com/7zEKy92.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/Gt6fPKZ.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/S21Q5fv.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/vYlNZVB.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/TySSbnu.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/3mmXB4f.png" width="80%">
</div>

## Training Neural Networks

### The Process of Training

本节课最后讨论了训练神经网络时的各种注意事项。总结一下，在训练神经网络时我们需要去监视模型的各种曲线，包括损失曲线、正确率曲线等，这样就可以利用曲线的行为来帮助判断训练的程度。同时还需要注意在训练时不要使用**测试集(test set)**上的数据，如果想要调整模型的超参数则可以使用**验证集(validation set)**的数据或是利用**交叉验证(cross-validation)**来调整参数。

<div align=center>
<img src="https://i.imgur.com/T7novsS.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/QtnKyah.png" width="80%">
</div>

### Sanity Checking

我们需要关注损失曲线在训练过程中的行为从而保证模型的训练过程是正确的。对于大多数常见的损失函数的取值范围往往是非负的，而且随机初始化的模型在训练开始阶段模型的行为是可以预测的。如果损失函数不满足这些性质则说明模型可能存在一些问题。一个常用的技巧是在正式训练之前首先用一个小的数据集让模型进行过拟合从而保证模型自身没有问题。

<div align=center>
<img src="https://i.imgur.com/i6cka2Q.png" width="80%">
</div>

### Loss

如果在训练过程中发现损失曲线下降缓慢则说明当前学习率比较低，而如果出现损失上升或是NaN的情况则说明学习率可能是过大的。

<div align=center>
<img src="https://i.imgur.com/qqbuNnR.png" width="80%">
</div>

### Overfitting

我们还可以利用模型在训练集合验证集上两条损失曲线来判断模型训练的程度。如果出现了训练集上损失不断下降但在验证集上损失上升则表明出现了过拟合的问题；而如果两条曲线非常接近或者损失都很大则说明出现了欠拟合，模型需要进一步的训练。

<div align=center>
<img src="https://i.imgur.com/K2WJQRN.png" width="80%">
</div>

### Hyper-Parameter Tuning

在深度学习中我们往往需要对模型的超参数进行调参。最基本的调参方法是首先划定一个大致的范围进行训练，然后在其中选择性能较好的部分继续进行调参。当然目前也有一些自动化的调参方法，但这些方法往往有着比较高的计算代价。

<div align=center>
<img src="https://i.imgur.com/BqjteHv.png" width="80%">
</div>

需要注意的是超参数之间往往存在着一些依赖关系，因此在调参时不能够单独调整每个参数再将结果组合起来。

<div align=center>
<img src="https://i.imgur.com/rVAGZie.png" width="80%">
</div>

### Loss and Other Metrics

最后还需要说明的是尽管我们优化的是损失函数，但实际关心的则是模型的其它性能度量，比如说正确率、精度等。一般来说模型的损失越小性能度量就越好，但二者的关系实际上非常复杂。

<div align=center>
<img src="https://i.imgur.com/xyKaUVN.png" width="80%">
</div>

一个简单的例子是交叉熵与分类正确率。我们知道交叉熵越小模型的分类正确率就越高，但在训练时经常发生的现象是收敛后损失函数只发生了很小的变化而精度却会有明显的提升。造成这种现象的原因是交叉熵中的对数函数，当概率比较高时继续提高概率只会导致对数函数微小的变化。

<div align=center>
<img src="https://i.imgur.com/EpkOH05.png" width="80%">
</div>

其它常用的性能度量包括TPR/FPR曲线、PR曲线、AUC等。这些度量与损失函数的关系更为复杂，因此在进行训练时往往还会同时监视这些性能度量来指示模型的训练程度。

<div align=center>
<img src="https://i.imgur.com/MFgNGoX.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/oWn0vQQ.png" width="80%">
</div>