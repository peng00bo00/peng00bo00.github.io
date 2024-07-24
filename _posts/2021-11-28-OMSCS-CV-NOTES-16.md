---
layout: post
title: OMSCS-CV课程笔记16-Binary Morphology
date: 2021-11-28
description: 数学形态学
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍数学形态学的相关内容。
{: .block-preface }


## Binary Image Analysis

**二值图像(binary image)**是指仅包含0和1的图像。二值图像虽然简单但在计算机视觉中有着广泛的应用，我们可以使用0来表示背景并且用1来表示前景，这样就可以表示图像中的物体。除此之外，我们还可以对不相连的前景进行计数来获得图像中物体的数量、大小等信息。

<div align=center>
<img src="https://i.imgur.com/DZVDBXI.png" width="40%">
</div>

### Thresholding

获得二值图像最简单的方法是对灰度图像进行**阈值化(thresholding)**。所谓"阈值化"是指选择一个阈值将图像划分为前景和背景两部分，通常情况下小于阈值的部分划分背景(0)而大于阈值的为前景(1)。以下图为例，我们希望从图像中提取到水果的区域，首先需要统计图像的灰度直方图然后选择一个合适的灰度值进行阈值化：

<div align=center>
<img src="https://i.imgur.com/qeQiLV9.png" width="70%">
</div>

那么如何选择合适的阈值呢？一个直观的想法是遍历所有可能的灰度取值，然后选择其中最好的那个作为阈值。我们假设灰度直方图有两个峰值，然后对每个阈值$$t$$计算阈值化后前景和背景的灰度方差，选择方差之和最小的作为最终的分割阈值。这种算法称为**大津算法(Otsu's method)**，在二值图像分割中有着重要的应用。

<div align=center>
<img src="https://i.imgur.com/FQ25P46.png" width="40%">
</div>

### Connected Components Labeling

在很多情况下我们希望能够获得二值图像中互不相连的前景物体，这个过程称为**连通分量标记(connected-component labeling)**。求解连通分量标记的经典方法是进行两次扫描：首先逐行进行扫描，每当遇到同一行上互不相连的前景就令标记编号加1，如果当前位置的邻域已经标记过就选择邻域最小的编号：

<div align=center>
<img src="https://i.imgur.com/TSwI7Yo.png" width="70%">
</div>

然后进行第二次扫描，每当发现当前位置上方的标记大于自身时需要重新对上面的标记赋值，同时以它为起点将新的标记传播上去：

<div align=center>
<img src="https://i.imgur.com/vX46pYs.png" width="70%">
</div>

重新标记后就图像中互不连通的物体就获得了相应的标记：

<div align=center>
<img src="https://i.imgur.com/s32q2oU.png" width="70%">
</div>

连通分量标记在现实生活中有着非常多的应用，比如说在医学影像中来标记人体的不同器官，或是将标记映射为不同的颜色来实现一些图像效果：

<div align=center>
<img src="https://i.imgur.com/pE8hyYi.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/7iZnird.png" width="70%">
</div>

## Mathematical Morphology

### Dilation and Erosion

**数学形态学(mathematical morphology)**是研究对二值图像的形态进行变换的学科，它的基本操作包括**膨胀(dilation)**和**腐蚀(erosion)**两种运算。膨胀会扩张物体的区域，可以用来增大图形或是填充孔洞。

<div align=center>
<img src="https://i.imgur.com/8zGrcCJ.png" width="40%">
</div>

而腐蚀则可以看成与膨胀相反的操作，它会缩减图形的区域：

<div align=center>
<img src="https://i.imgur.com/wI6V1kH.png" width="40%">
</div>

要严格的定义膨胀和腐蚀操作我们需要引入**结构单元(struct element)**的概念。结构单元是一个带原点的二值掩码，常用的结构单元包括矩形单元、六边形单元以及圆形单元等。

<div align=center>
<img src="https://i.imgur.com/v1uVSoy.png" width="70%">
</div>

通过结构单元我们可以将膨胀运算定义如下：把结构单元的原点放置在二值图像上并与该区域进行或运算，如果结构单元接触到了图像上的1则将该处标记为1，最后遍历二值图像的每一个像素就得到了膨胀后的图像：

<div align=center>
<img src="https://i.imgur.com/ubnodcM.png" width="70%">
</div>

对二值图像进行膨胀后的结果可参考下图：

<div align=center>
<img src="https://i.imgur.com/ErNMf6k.png" width="70%">
</div>

类似地，腐蚀则是令结构单元与局部区域进行与运算，如果结构单元内都是前景则将该处标记为1：

<div align=center>
<img src="https://i.imgur.com/gZ8dmFP.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/QMYfVyC.png" width="70%">
</div>

需要注意的是膨胀和腐蚀不是互逆的运算，对图像先进行膨胀然后腐蚀或是先腐蚀后膨胀不会恢复到初始状态。

### Opening and Closing

在膨胀和腐蚀的基础上我们可以定义**开运算(opening)**和**闭运算(closing)**。开运算是先对图像进行腐蚀然后再使用相同的结构单元进行膨胀，可以证明开运算相当于求解二值图像$$A$$中所有包含结构单元$$B$$的区域：

<div align=center>
<img src="https://i.imgur.com/6mdvM5I.png" width="70%">
</div>

如果我们使用一个非常大的结构单元，那么可以用开运算来提取二值图像的中心区域：

<div align=center>
<img src="https://i.imgur.com/Cz5AxLI.png" width="70%">
</div>

类似地，闭运算是先腐蚀然后再进行膨胀。它可以理解为结构单元$$B$$在二值图像$$A$$外侧运动时所不能接触到的区域：

<div align=center>
<img src="https://i.imgur.com/bUoaV0B.png" width="70%">
</div>

闭运算的一个重要应用是填充阈值化后图像内部的孔洞：

<div align=center>
<img src="https://i.imgur.com/eOuogUB.png" width="70%">
</div>

同时开运算和闭运算都是**幂等(idempotent)**的，重复对图像进行开运算或是闭运算都只会获得相同的结果。此外类似于膨胀和腐蚀的关系，开运算与闭运算也不是互逆的运算，先进行开运算然后闭运算不能恢复原图：

<div align=center>
<img src="https://i.imgur.com/UFGXROF.png" width="70%">
</div>

将开运算和闭运算结合起来我们可以实现不同的形态操作：

<div align=center>
<img src="https://i.imgur.com/OAMskJx.png" width="70%">
</div>

### Basic Morphological Algorithms

将这些形态学操作与布尔运算结合起来我们还可以得到更多的形态学算法，包括提取边界、细化、加粗等：

<div align=center>
<img src="https://i.imgur.com/O0IiEOm.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/yRc7SLU.png" width="40%">
</div>

<div align=center>
<img src="https://i.imgur.com/wQWLbMP.png" width="70%">
</div>

<div align=center>
<img src="https://i.imgur.com/4rdTGo0.png" width="65%">
</div>

## Reference

- [Wikipedia: Otsu's method](https://en.wikipedia.org/wiki/Otsu%27s_method)
- [Wikipedia: Connected-component labeling](https://en.wikipedia.org/wiki/Connected-component_labeling)
- [Wikipedia: Mathematical morphology](https://en.wikipedia.org/wiki/Mathematical_morphology)