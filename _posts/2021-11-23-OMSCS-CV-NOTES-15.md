---
layout: post
title: OMSCS-CV课程笔记15-Color Spaces and Segmentation
date: 2021-11-23
description: 颜色空间与图像分割
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍颜色空间与图像分割的相关内容。
{: .block-preface }


## Color Spaces

### Human Vision System

人眼对颜色的感知是通过视网膜上的棒状(rods)和锥形(cones)细胞来实现的。在视网膜上分布着6,000,000-7,000,000个锥形细胞，它们形成了我们用肉眼看到的图像。同时这些锥形细胞根据它们对颜色的感知可以分为3种：64%对红色敏感，32%对绿色敏感，2%对蓝色敏感，这样的分布构成了"三原色"的生理学基础。

<div align=center>
<img src="https://i.imgur.com/xTBWTkJ.png" width="35%">
<img src="https://i.imgur.com/IfcYSWh.png" width="51%">
</div>

从物理学的角度上讲，这些不同类型的锥形细胞对不同波长的光存在不同的响应。从响应的绝对值上讲人眼对红光最为敏感，其次是绿光，最后才是蓝光。

<div align=center>
<img src="https://i.imgur.com/HNJgono.png" width="32%">
<img src="https://i.imgur.com/Y3oRcPi.png" width="50%">
</div>

还需要说明的是与颜色相比人眼对于亮度的变化更为敏感：下方左边图片字体和背景使用了相同的颜色但是具有不同的亮度，我们可以轻松地识别出图上的文字；而右边的图片则使用了具有相同亮度不同颜色的文字和背景，我们要识别出文字则困难得多。

<div align=center>
<img src="https://i.imgur.com/In8LsVR.png" width="40%">
<img src="https://i.imgur.com/EZUocv5.png" width="40%">
</div>

### CIE Color Space

显然我们希望能够从物理的角度定量描述不同颜色的差异，这样的想法促成了**CIE RGB颜色空间(CIE RGB color space)**的诞生。之后人们对空间进行了规范化，得到了**CIE XYZ颜色空间(CIE XYZ color space)**。人眼能够感知到的颜色在XYZ颜色空间中呈一个马蹄形：

<div align=center>
<img src="https://i.imgur.com/RuTtJup.png" width="35%">
</div>

除此之外还有**LAB颜色空间(CIELAB color space)**，其中L表示亮度，A和B分别表示颜色坐标。不同亮度下的颜色有不同的显示效果，因此LAB颜色空间也可以认为是一个柱体。

<div align=center>
<img src="https://i.imgur.com/ReCGhhC.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/KX7dwZQ.png" width="30%">
</div>

其它常用的颜色空间还包括HSV以及HSL空间等，当然最常见的颜色空间还是RGB空间。

<div align=center>
<img src="https://i.imgur.com/zZaMvi6.png" width="60%">
</div>

<div align=center>
<img src="https://i.imgur.com/E0kbqe8.png" width="60%">
</div>

还需要注意的一点是不同的颜色空间、软件以及显示设备存在不同**色域(color gamut)**，因此不是所有的颜色都能够在不同的设备上显示出来。一些常见软硬件的色域可参考下图：

<div align=center>
<img src="https://i.imgur.com/6hKXnMM.png" width="30%">
<img src="https://i.imgur.com/dTa9sEl.png" width="33%">
<img src="https://i.imgur.com/RGqVta9.png" width="28%">
</div>

### Color Vectors

对于彩色图像我们可以把图像上每个点的颜色用一个向量来表示：

<div align=center>
<img src="https://i.imgur.com/cgX5apG.png" width="70%">
</div>

进一步可以将图像上的所有像素放置在颜色空间中，得到图像的颜色分布：

<div align=center>
<img src="https://i.imgur.com/HaeIPmM.png" width="70%">
</div>

我们可以在图像上使用颜色进行滤波，从而识别图像上的不同物体：

<div align=center>
<img src="https://i.imgur.com/E5aN2w2.png" width="70%">
</div>

但这样做的缺陷在于同一个颜色在不同光照条件下会产生不同的效果：

<div align=center>
<img src="https://i.imgur.com/XOCVyez.png" width="50%">
</div>

因此更合理的做法是将亮度从颜色向量中分离出来，仅对颜色进行滤波：

<div align=center>
<img src="https://i.imgur.com/TskwA6J.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/DtmzThU.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/8mvtHY6.png" width="70%">
</div>

分离亮度后可以得到图像的颜色分布如下：

<div align=center>
<img src="https://i.imgur.com/pThycY2.png" width="70%">
</div>

最后在YUV空间中进行滤波就能够得到更好的分割结果：

<div align=center>
<img src="https://i.imgur.com/kM69g24.png" width="70%">
</div>

## Segmentation

**图像分割(image segmentation)**是计算机视觉中的重要任务之一，我们希望能够将图像上属于同一物体的区域聚合到一起：

<div align=center>
<img src="https://i.imgur.com/geOutwL.png" width="40%">
<img src="https://i.imgur.com/tmOTCBb.png" width="40%">
</div>

图像分割的经典应用是抠图，我们希望能够将图像中非背景的部分从背景中分离出来：

<div align=center>
<img src="https://i.imgur.com/DWQPTjZ.png" width="20%">
<img src="https://i.imgur.com/bC762K1.png" width="20%">
<img src="https://i.imgur.com/WtN1Zjt.png" width="20%">
</div>

图像分割的另一个重要应用是**超像素(superpixel)**。超像素类似于马赛克的效果，它将图像划分成若干个区域这样就可以用这些区域来描述原来的图像：

<div align=center>
<img src="https://i.imgur.com/G3RGOw5.png" width="30%">
<img src="https://i.imgur.com/QtUwB6O.png" width="30%">
</div>

### Clustering

实现图像分割最简单的方式是利用直方图进行阈值化。以下图为例，我们可以通过直方图发现图像存在3个区域，每个区域对应不同的物体。

<div align=center>
<img src="https://i.imgur.com/pBOSAkR.png" width="70%">
</div>

然而当图像存在噪声时就不能使用这样的方法了，此时每个物体不再具有特定的颜色而是在颜色附近波动。因此图像分割的目标是从图像中找到这些代表颜色来表示不同的物体。

<div align=center>
<img src="https://i.imgur.com/sl9z07X.png" width="70%">
</div>

从机器学习的角度上讲这样的过程称为**聚类(clustering)**，我们希望从数据集中寻找到若干个"代表"作为中心，进而将数据集划分为不同的区域。对于像素而言，只需要把它们看做是一般的向量使用SSD作为误差度量即可。

<div align=center>
<img src="https://i.imgur.com/EbdTcry.png" width="60%">
</div>

假设我们知道了每个样本的类别，那么只需要对每个类别取平均就可以得到该类别的"代表"；而如果我们知道了每个类别的"代表"，则可以通过计算样本与每个"代表"的距离来获得它的类别。从这个角度上看聚类问题实际上是一个chicken-egg问题：我们既不知道样本的类别，也不知道聚类的中心，因此无法直接进行求解。

### K-Means

k-means是解决聚类问题的经典算法，它的流程如下：

1. 初始化聚类中心$$c_1$$, $$c_2$$, ..., $$c_K$$
2. 计算数据集上的每个点与聚类中心的距离，然后将它划分到距离最近的类别中
3. 将聚类中心更新为该类别中样本的平均值
4. 重复第2步到第3步直到收敛

使用k-means进行图像分割需要注意选择合适的特征空间。以彩色图像为例，我们可以在灰度化的图像上进行聚类也可以在颜色空间中聚类，聚类的结果往往会具有一些差异。

<div align=center>
<img src="https://i.imgur.com/lqWrasJ.png" width="70%">
</div>

除此之外我们还可以把像素的坐标也加到特征空间中，这样在聚类时也会考虑像素空间位置的相似性：

<div align=center>
<img src="https://i.imgur.com/mING289.png" width="50%">
</div>

k-means是非常简单高效的聚类算法，但需要注意的是k-means的聚类结果与初值有关因此容易显然局部最优。此外k-means对内存的需求较高，需要提前指定k的取值，而且只能得到"球形"分布的聚类结果。

<div align=center>
<img src="https://i.imgur.com/LmkEDUU.png" width="40%">
<img src="https://i.imgur.com/0cLfCLM.png" width="40%">
</div>

## Mean Shift Segmentation

除了k-means之外，mean shift也是图像分割中常用的分割算法。mean shift的本质是在特征空间中寻找样本的众数或是局部极大值，它的主要流程如下：

<div align=center>
<img src="https://i.imgur.com/Mkj4DJB.png" width="40%">
<img src="https://i.imgur.com/9iSazdy.png" width="40%">
<img src="https://i.imgur.com/jIwYr4D.png" width="40%">
<img src="https://i.imgur.com/FkR219M.png" width="40%">
<img src="https://i.imgur.com/coGjDID.png" width="40%">
<img src="https://i.imgur.com/rqWaMo1.png" width="40%">
<img src="https://i.imgur.com/rBeq4aY.png" width="40%">
</div>

使用mean shift进行聚类时需要将图像上每个像素转换成特征空间中的点，然后把每个点作为起点进行mean shift。最终算法收敛到的局部极值即为聚类中心：

<div align=center>
<img src="https://i.imgur.com/HwR6QEY.png" width="50%">
</div>

一些实验结果表明使用mean shift进行分割可以得到非常好的效果：

<div align=center>
<img src="https://i.imgur.com/z8owpte.png" width="40%">
<img src="https://i.imgur.com/W7aEpDI.png" width="40%">
</div>

<div align=center>
<img src="https://i.imgur.com/k5NYIIY.png" width="40%">
<img src="https://i.imgur.com/eoDrJRd.png" width="40%">
</div>

除了颜色之外我们还可以使用"纹理"作为特征来构造特征空间，这样对于图像中颜色接近的区域也能取得很好的分割结果。所谓"纹理"可以理解为局部的图像统计信息，通常可以使用局部梯度的直方图来表示。

<div align=center>
<img src="https://i.imgur.com/F55YyMa.png" width="70%">
</div>

对于某些图片使用纹理空间会得到更好的分割结果

<div align=center>
<img src="https://i.imgur.com/Gv6WgJ9.png" width="70%">
</div>

## Segmentation by Graph Partitioning

更高级的图像分割方法是基于graph partitioning的分割方法。此时我们不再把图像视为二维的数组，而是把图像看成是一张全连接图，图上任意两点$$p$$和$$q$$之间的边权重$$w_{pq}$$可由下式计算：

$$
w_{pq} = \exp \bigg( -\frac{1}{2} \text{dist}(x_p, x_q)^2 \bigg)
$$

其中距离函数$$\text{dist}(x_p, x_q)$$描述了$$p$$、$$q$$两点在颜色和位置上的差异。

<div align=center>
<img src="https://i.imgur.com/FSlf9e6.png" width="30%">
<img src="https://i.imgur.com/GaBEkEC.png" width="30%">
</div>

从图论的角度上看，图像分割的实质是将原始的图划分成若干个子图。因此我们只需要从图上删除连接不同区域间权重较小的边即可，这样相似的节点(像素)会属于同一个子图(分割)。

<div align=center>
<img src="https://i.imgur.com/X6WuyF7.png" width="50%">
</div>

基于图论进行图像分割的经典算法是**图割(graph cut)**，它的基本思路是将原始图分割成互不相连的两部分，同时每删除一条边就要支付这条边的权重作为代价。显然一个好的分割要有尽可能小的代价，因此这种算法也称为minimum cut。

<div align=center>
<img src="https://i.imgur.com/A5vRrUt.png" width="40%">
</div>

实践中发现直接使用minimum cut容易产生过小且互不联通的区域，因此需要进行一些改进。在normalized cut算法中对优化目标进行了规范化从而避免出现过分割的问题：

<div align=center>
<img src="https://i.imgur.com/ulyipzf.png" width="70%">
</div>

normalized cut算法的流程如下：

<div align=center>
<img src="https://i.imgur.com/zbKqY39.png" width="70%">
</div>

使用normalized cut进行图像分割的一些结果如下：

<div align=center>
<img src="https://i.imgur.com/XIA8Vuo.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/WO5HJWk.png" width="70%">
</div>

## Reference

- [Wikipedia: CIE 1931 color space](https://en.wikipedia.org/wiki/CIE_1931_color_space#:~:text=The%20CIE%20RGB%20color%20space%20is%20one%20of%20many%20RGB,John%20Guild%20with%20seven%20observers.)
- [Wikipedia: CIELAB color space](https://en.wikipedia.org/wiki/CIELAB_color_space)
- [Wikipedia: Gamut](https://en.wikipedia.org/wiki/Gamut)
- [Wikipedia: k-means clustering](https://en.wikipedia.org/wiki/K-means_clustering)
- [Wikipedia: Graph cuts in computer vision](https://en.wikipedia.org/wiki/Graph_cuts_in_computer_vision)