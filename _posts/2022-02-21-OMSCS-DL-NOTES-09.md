---
layout: post
title: OMSCS-DL课程笔记09-Advanced Computer Vision Architectures
date: 2022-02-21
description: 图像分割与目标检测
tags: CS7643-DL
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 深度学习课程([CS 7643: Deep Learning](https://omscs.gatech.edu/cs-7643-deep-learning))的同步课程笔记。课程内容涉及深度学习的基本理论方法以及它在计算机视觉、自然语言处理以及决策理论等领域中的应用，本节主要介绍深度学习在图像分割以及目标检测中的应用。
{: .block-preface }


除了前面介绍过的图像分类，深度学习在很多更高级的计算机视觉任务中也有着大量的应用。

<div align=center>
<img src="https://i.imgur.com/82iqo1Y.png" width="80%">
</div>

## Image Segmentation Networks

### Segmentation Tasks

**图像分割(image segmentation)**是计算机视觉中的高级视觉任务，对于给定点输入图片我们希望获得它每个像素对应的标签。而图像分类还可以细分为**语义分割(semantic segmentation)**和**实例分割(instance segmentation)**两类，语义分割只需要获得像素的类别标签而实例分割还需要获得每个像素的物体编号。

<div align=center>
<img src="https://i.imgur.com/yYn9svy.png" width="80%">
</div>

显然对于图像分割任务我们的输出不再是一个类别编号，而是一张包含每个像素类别的特征图。因此对于图像分割网络我们需要一些额外的处理方式来获得类别特征图。

<div align=center>
<img src="https://i.imgur.com/ZJRS8zG.png" width="80%">
</div>

### Fully-Convolutional Network

**FCN(Fully-Convolutional Network)**是最早使用深度学习来解决图像分割任务的模型。它的基本思想是使用卷积层来替换掉分类网络最后的全连接层，同时最后一个卷积层的输出对应每个像素在不同类别上的概率。由于卷积层仍然是进行了线性运算，替换后的效果和全连接层是类似的，但此时我们则可以获得每个像素对应的类别。

<div align=center>
<img src="https://i.imgur.com/3sJetcj.png" width="80%">
<img src="https://i.imgur.com/whFObXs.png" width="80%">
</div>

卷积层的特点是参数量与输入的尺寸无关，因此FCN可以适应任意尺寸的图像作为输入。当输入图像的分辨率比较高时，通过FCN可以直接获得每个像素在不同类别上的概率(热力图)。

<div align=center>
<img src="https://i.imgur.com/8PQrS7N.png" width="80%">
<img src="https://i.imgur.com/MZv0mNK.png" width="80%">
</div>

### DeConvolution and UnPooling

FCN的缺陷在于输入图像经过网络后分辨率会缩减，造成一部分信息的损失。而对于现代的图像分割网络则一般会通过一些**反卷积(deconvolution)**和**反池化(unpooling)**的方式来恢复原始分辨率。

<div align=center>
<img src="https://i.imgur.com/FaXiu8B.png" width="80%">
</div>

#### UnPooling

反池化的运算类似于池化在反向传播中的计算流程。首先需要记录下池化时最大元素的位置，然后在反池化中将元素填入到对应位置从而恢复池化后的分辨率。

<div align=center>
<img src="https://i.imgur.com/1PlxbJB.png" width="80%">
<img src="https://i.imgur.com/M2NIMDp.png" width="80%">
<img src="https://i.imgur.com/ZtWkUjU.png" width="80%">
</div>

因此反池化要求将网络对称布置，形成**编码器-解码器(encoder-decoder)**的结构形式。

<div align=center>
<img src="https://i.imgur.com/wFgfzKJ.png" width="80%">
</div>

#### Transposed Convolution

类似于池化和反池化的关系，卷积运算的逆运算称为反卷积或**转置卷积(transposed convolution)**。在转置卷积运算中需要把卷积核乘以输入图像每个位置上的值，然后放置到输出图像的对应位置上。

<div align=center>
<img src="https://i.imgur.com/lEr5JL2.png" width="80%">
<img src="https://i.imgur.com/Vyo9Zlb.png" width="80%">
</div>

我们同样把转置卷积和卷积对称布置，这样整个网络输出的尺寸就和输入图像一致了。

<div align=center>
<img src="https://i.imgur.com/xHu0xsW.png" width="80%">
</div>

### Transfer Learning

从网络结构的角度上看，encoder部分的网络与一般的图像分类网络没有任何差异。因此我们可以使用迁移学习的方式用分类网络进行预训练，从而提高图像分割的准确率。

<div align=center>
<img src="https://i.imgur.com/VSeElHI.png" width="80%">
</div>

除此之外我们也可以借鉴分类网络的设计思想，把skip connection这样的结构应用在图像分割网络中来提高模型的性能。

<div align=center>
<img src="https://i.imgur.com/y0v9Nij.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/MTXmwYI.png" width="80%">
</div>

## Single-Stage Object Detection

### Object Detection Tasks

目标检测是一种常见的计算机视觉任务，在目标检测中我们希望使用矩形框来标出图像中不同物体的位置和类别。目标检测的难点在于图像中物体的数量是未知的，因此需要一些额外的方法来提取图像中物体可能出现的位置。

<div align=center>
<img src="https://i.imgur.com/1KRU47R.png" width="80%">
</div>

目前基于深度学习的目标检测可以大致分为single-stage和two-stage两类，这里首先来介绍single-stage的相关方法。类似于图像分类，在目标检测中往往首先会通过一个CNN来提取图像的信息得到特征图，然后利用这张特征图进行后续的分类和回归操作。对于single-stage方法，模型会在特征图上同时进行bounding box的分类和回归，即图像特征在两个任务上是共享的。

<div align=center>
<img src="https://i.imgur.com/QkCjI7m.png" width="80%">
<img src="https://i.imgur.com/JQS5hpf.png" width="80%">
</div>

显然目标检测的核心是获得图像中不同物体的region proposal。在一些早期的工作中往往是使用传统计算机视觉的技术获得不同尺度不同位置下大量的bounding box，然后对这些proposal进行分类并通过NMS获得最终的bounding box。

<div align=center>
<img src="https://i.imgur.com/24fXuIO.jpg" width="80%">
</div>

### Single-Shot Detector (SSD)

SSD是single-stage的经典模型。在提取图像特征时它会生成一系列不同尺度下的特征图，然后在每个尺度下使用预先定义的**锚框(anchor)**作为region proposal，最后通过对这些锚框进行分类和回归来实现目标检测。

<div align=center>
<img src="https://i.imgur.com/pfRB4yS.png" width="80%">
</div>

### You Only Look Once (YOLO)

YOLO也是经典的single-stage模型。为了提升模型的运行速度，YOLO没有使用类似于SSD的多尺度结构而是在单一尺度上进行目标检测。

<div align=center>
<img src="https://i.imgur.com/vi0n0ST.png" width="80%">
</div>

### Evaluation

在目标检测中一般会使用mAP来评价模型的准确性。mAP是PR曲线下的面积，它的计算方法如下：

<div align=center>
<img src="https://i.imgur.com/CK9JUue.png" width="80%">
</div>

除了mAP外很多模型还考虑了运行时的计算效率，因此在一些注重实时性的场景中FLOPs和FPS也是常用的性能指标。

<div align=center>
<img src="https://i.imgur.com/qld0nqG.png" width="80%">
</div>

## Two-Stage Object Detectors

single-stage方法没有显式的region proposal阶段，而是直接使用特征图来提取bounding box。而two-stage方法包含region proposal和bounding box classification两个阶段，整个目标检测流程可以分解为提取图像中的ROI然后对ROI进行分类。

<div align=center>
<img src="https://i.imgur.com/u6we4Yr.png" width="80%">
</div>

### R-CNN

早期的two-stage模型使用selective search来获得ROI，然后使用CNN对这些区域进行分类作为最终的bounding box。这种方式的缺陷在于selective search的计算效率是非常低下的，基本无法做到实时计算。

<div align=center>
<img src="https://i.imgur.com/kIBfL5v.png" width="80%">
</div>

同时在对ROI进行分类时每个proposal都会使用CNN进行计算，这又导致了计算资源的浪费。

<div align=center>
<img src="https://i.imgur.com/StKWejr.png" width="80%">
</div>

### Fast R-CNN

为了提升R-CNN的计算效率，在Fast R-CNN中首先将图像送入网络生成特征图，而在后续进行分类时则直接从特征图上提取ROI对应的区域作为分类特征。

<div align=center>
<img src="https://i.imgur.com/NeEDsGo.png" width="80%">
</div>

这种做法确实避免了图像特征的重复计算，但由于ROI的尺寸是不确定的无法直接将特征向量输入到分类器中。为了解决这个问题在Fast R-CNN中还提出了**ROI pooling**运算，它会将ROI划分为指定尺寸的网格并进行池化运算，这样就保证了任意大小的ROI都会生成固定尺寸的特征向量。

<div align=center>
<img src="https://i.imgur.com/oMVqUMa.png" width="80%">
</div>

这样就实现了端对端的目标检测。

<div align=center>
<img src="https://i.imgur.com/xB00K2g.png" width="80%">
</div>

### Faster R-CNN

在Faster R-CNN中更进一步提出了**RPN(region proposal network)**来代替selective search进行region proposal。RPN的思想是在特征图上进行二分类来获得proposal，而后续的分类网络则是在特征图上对proposal进行分类。

<div align=center>
<img src="https://i.imgur.com/Iomwm0d.png" width="80%">
</div>

在RPN中同样使用了anchor的概念，每个anchor对应object得分以及bounding box的位置。在训练RPN时需要对object得分进行分类同时对bounding box的位置进行回归。

<div align=center>
<img src="https://i.imgur.com/H2MEtwd.png" width="80%">
</div>

### Mask R-CNN

目前R-CNN系列模型已经发展到了Mask R-CNN，它不仅可以进行目标检测还可以对检测到的目标进行实例分割。

<div align=center>
<img src="https://i.imgur.com/TVPYpGe.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/MmQ5HM3.png" width="80%">
</div>