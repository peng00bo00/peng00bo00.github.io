---
layout: post
title: OMSCS-DL课程笔记07-Visualization
date: 2022-02-14
description: 神经网络可视化
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍深度学习中的可视化方法。
{: .block-preface }


## Visualization of Neural Networks

当我们训练好模型后可以通过一些可视化方法去理解网络在训练过程中到底学习到了什么内容。常用的可视化方法可以分为对模型权重(参数)的可视化、对输入数据响应的可视化、基于梯度的可视化以及对输入数据进行扰动来测试模型鲁棒性几种类型。

<div align=center>
<img src="https://i.imgur.com/CBswuv4.png" width="80%">
</div>

### Visualizing Weights

最基础的可视化方法是对网络的权重进行可视化。以全连接层为例，我们可以把全连接层的参数reshape成与输入图像一致的尺寸，然后将参数的范围规范化到[0, 255]区间上进行可视化。通过这种方法可以发现不同类别的分类器对应的权重都类似于相应类别的图片，这说明网络在训练过程中学习到了不同类别在图像上的表示。而对于卷积层，我们可以直接把每个卷积核的取值进行规范化，然后按照图像的格式进行显示。可视化的结果显示卷积核会对某些特定类型的图像特征有着强烈的响应。

<div align=center>
<img src="https://i.imgur.com/YffEij3.png" width="80%">
</div>

### Visualizing Output Maps

我们同样可以提取网络的一些中间层特征图进行可视化。需要注意的是由于池化的存在深层输出的尺寸可能都比较小，因此这种方法只适用于一些浅层的输出。

<div align=center>
<img src="https://i.imgur.com/cv8kezw.png" width="80%">
</div>

可视化的结果显示不同的卷积核会对输入图像上的特定区域有着比较强的相应，而这些区域也往往对应着图像上的一些特定的内容(眼睛、面孔等)。

<div align=center>
<img src="https://i.imgur.com/bWasUgz.png" width="80%">
</div>

### Dimensionality Reduction

除此之外我们还可以使用一些降维的方法来可视化高维的输出数据。由于这些数据往往具有很强的非线性关系，一般会使用t-SNE这样的降维算法来进行可视化。可视化的结果显示具有相同类别的特征向量往往会出现在同一个聚类中。

<div align=center>
<img src="https://i.imgur.com/Pz5Ifua.png" width="80%">
</div>

上述这些可视化方法可以帮助我们理解神经网络学习到了什么内容，但有时这些可视化的结果可能是难以解释甚至是带有误导性质的。同时需要注意的是神经网络学习到的特征往往是分布式的，单个神经元可能无法完整的表达某个特征的全部内容，这就使得对神经网络进行解释更加困难。

<div align=center>
<img src="https://i.imgur.com/4xuzUdC.png" width="80%">
</div>

## Gradient-Based Visualizations

在训练网络时我们利用反向传播算法来计算损失函数的梯度，而在可视化中我们同样可以利用反向传播来理解网络学习到的内容。根据反向传播的起点和终点主要有两种可视化方法：从损失出发计算网络所有层的梯度，以及从某一层出发计算输入图像的梯度。

<div align=center>
<img src="https://i.imgur.com/Exh5trk.png" width="80%">
</div>

### Gradient of Loss w.r.t. Image

从损失函数出发通过反向传播算法可以得到所有中间层的梯度，每一个梯度值对应损失函数在该处的响应，梯度越大表示该点对于损失函数越重要，每一层上梯度响应图称为一张**saliency map**。在实践中往往不会直接使用损失函数，而是使用模型最后分类层上某个类别的得分作为起点，然后计算类别得分关于输入图像的梯度作为saliency map。进行可视化时还会对梯度取绝对值并把所有通道上的梯度进行累加来获得输入图像有明显响应的区域。

<div align=center>
<img src="https://i.imgur.com/tvYAsTt.png" width="80%">
</div>

#### Object Segmentation for Free!

利用saliency map我们可以使用分类模型对图像进行分割，从而得到指定类别的区域。

<div align=center>
<img src="https://i.imgur.com/VOUwaRi.png" width="80%">
</div>

#### Detecting Bias

同时saliency map也可以帮助我们检查数据集是否是有偏(biased)的，训练好的模型在无偏的数据集上不会在非目标区域上有很强的响应。

<div align=center>
<img src="https://i.imgur.com/T6KrHWy.png" width="80%">
</div>

### Gradient of Activation w.r.t. Input

另一种基于梯度的可视化方法是选择中间层的一个神经元利用反向传播计算它关于输入图像的梯度。通过这种方法我们可以观察选中的神经元对输入图像上哪些区域有比较强的响应。

<div align=center>
<img src="https://i.imgur.com/90bJdqk.png" width="80%">
</div>

#### Guided Backprop

除了标准反向传播外还可以使用一些变体来提升可视化的效果，如deconvnet会过滤掉非负的梯度，而guided backpropagation则同时过滤了非负的梯度和前向输入值。

<div align=center>
<img src="https://i.imgur.com/y3Rw9ZO.png" width="80%">
</div>

利用guided backpropagation我们可以可视化任意神经元在输入图像上感兴趣的区域，从而理解它学习到的内容。

<div align=center>
<img src="https://i.imgur.com/MHLOoEH.png" width="80%">
</div>

利用类似的方法我们可以可视化神经网络每一层上的每一个神经元。可视化的结果显示在浅层神经元会关注一些简单的图像信息(颜色、纹理等)，而随着层数的增加神经元则会关注图像上更加抽象和复杂的内容。

<div align=center>
<img src="https://i.imgur.com/vNIBfJh.png" width="80%">
<img src="https://i.imgur.com/WhUOgEN.png" width="80%">
<img src="https://i.imgur.com/k8EEj07.png" width="80%">
<img src="https://i.imgur.com/cXa39zm.png" width="80%">
</div>

#### Grad-CAM

在此基础上人们还开发出了更加复杂的可视化方法来研究神经网络感兴趣的区域。比如说Grad-CAM就是一种对梯度反向传播图进行一些额外操作以方便可视化的方法。

<div align=center>
<img src="https://i.imgur.com/ZKCby2j.png" width="80%">
<img src="https://i.imgur.com/0XqWqI7.png" width="80%">
<img src="https://i.imgur.com/FX8U20X.png" width="80%">
<img src="https://i.imgur.com/vMOuGSG.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/ewjgphr.png" width="80%">
</div>

## Optimizing the Input Images

利用反向传播我们不仅可以优化损失函数训练网络，还可以对输入图像进行优化来生成新的图像。

<div align=center>
<img src="https://i.imgur.com/rgij8NM.png" width="80%">
</div>

### Gradient Ascent on the Scores

最基本的方法是把一张随机噪声图像作为输入，然后从某个类别得分进行反向传播并利用梯度上升来优化输入图像，在实际操作时还往往会结合一些正则项来提升优化后的图像质量。

<div align=center>
<img src="https://i.imgur.com/RQ61gI2.png" width="80%">
<img src="https://i.imgur.com/gXTW31T.jpg" width="80%">
</div>

除此之外还有一些技巧比如说对梯度进行裁剪或是对输出进行高斯模糊来提升可视化的视觉效果。

<div align=center>
<img src="https://i.imgur.com/rZFYhIM.png" width="80%">
<img src="https://i.imgur.com/xPACK55.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/ooxolUC.png" width="80%">
</div>

## Testing Robustness

### Adversarial Noise

如果对真实的图像使用梯度上升，人们发现只需要少量的像素扰动就可以影响模型的识别结果。

<div align=center>
<img src="https://i.imgur.com/CCi1wk9.png" width="80%">
</div>

一些深入的研究显示这样的现象不止存在与深度学习中，实际上很多模型都会受到这种随机扰动的影响。目前的主流观点认为导致这种现象的原因主要是模型的线性部分。

<div align=center>
<img src="https://i.imgur.com/a7fAZjH.png" width="80%">
</div>

在此基础上甚至还发展出了一个新的研究领域称为**对抗攻击和防御(adversarial attacks/defenses)**。人们通过对针对性的训练来生成攻击样本从而影响模型的性能，同时研究如何设计更好的防御手段使模型对于这些攻击样本更加鲁棒。

<div align=center>
<img src="https://i.imgur.com/izwT8Un.png" width="80%">
<img src="https://i.imgur.com/O1yzaND.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/KmlVFa3.png" width="80%">
</div>

### Analyzing Bias

同时人们还利用类似的方法来研究CNN的偏好。通过实验人们发现CNN更倾向于使用纹理来识别物体，而人眼则倾向于使用形状。

<div align=center>
<img src="https://i.imgur.com/U80PnWM.png" width="80%">
<img src="https://i.imgur.com/XbLrjiF.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/MK38dEI.png" width="80%">
</div>

## Style Transfer

基于梯度的可视化方法还可以用来实现**风格迁移(style transfer)**。风格迁移的难点是保证内容的基础上获得其它图像的风格，目前的常用做法是使用训练好的神经网络作为特征提取器，然后利用内容图像和风格图像的中间特征图构造损失并对输入图像进行反向传播和优化。

<div align=center>
<img src="https://i.imgur.com/KE4IV8m.png" width="80%">
</div>

为了保证图像内容的一致性，可以直接对内容图像的特征图进行拟合作为**内容损失(content loss)**。

<div align=center>
<img src="https://i.imgur.com/mGk5ApE.png" width="80%">
</div>

为了获得风格，一般会首先提取风格图像和输入图像特征图的Gram矩阵然后对Gram矩阵进行拟合得到**风格损失(style loss)**。最后把内容损失和风格损失组合到一起并对输入图像进行优化就可以实现风格迁移的效果。

<div align=center>
<img src="https://i.imgur.com/bnmLe45.png" width="80%">
<img src="https://i.imgur.com/SFAYqO0.png" width="80%">
</div>

一些风格迁移的案例如下：

<div align=center>
<img src="https://i.imgur.com/lNKeX97.jpg" width="80%">
<img src="https://i.imgur.com/xgs8yxu.jpg" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/RXelSJL.png" width="80%">
</div>