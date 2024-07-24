---
layout: post
title: OMSCS-CV课程笔记08-Feature and Matching
date: 2021-09-28
description: 图像特征与匹配
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍图像特征与匹配的相关内容。
{: .block-preface }


## Feature Detection

### Introduction to Features

在前面的课程中我们已经介绍过如何对图像进行几何变换，我们可以通过两幅图像上对应点的坐标来将其中一幅图像变换到另一幅图像上，而在本节课中我们将介绍如何寻找这样的特征点以及对图像上的特征点进行匹配。通常情况下图像的特征点包含两部分：特征点的坐标和相应的**描述子(descriptor)**。对于不同图像中的同一个点，它在不同图像上的坐标互不相同但都具有相似的描述子。理想情况下特征点的描述子需要具有以下特性：

- Repeatability/Precision：同样的特征在几何和光照变换下保持不变；
- Saliency/Matchability：所有的特征都是独一无二的；
- Compactness and Efficiency：描述子需要足够简洁，数量要远小于图像像素数；
- Locality：描述子表达了图像的局部性质，并且对遮挡有足够的鲁棒性。

<div align=center>
<img src="https://i.imgur.com/Km7h6Us.png" width="70%">
</div>

### Corner Detection

**角点(corner)**是最基本的图像特征，它指的是在图像的各个方向上都有显著变化的像素点。

<div align=center>
<img src="https://i.imgur.com/5fKhGq9.png" width="70%">
</div>

从数学上来看，假设我们对图像上的点进行一个微小的平移$$(u, v)$$，平移前后图像像素值的变化可以表示为：

$$
E(u, v) = \sum_{x, y} w(x, y) [I(x+u, y+v) - I(x, y)]^2
$$

其中$$w(x,y)$$为窗口函数，它表示邻域像素点对于中心像素贡献的权重，一般可取常数1或高斯函数。显然位移$$(u,v)$$越大，像素变化越大；当$$(u,v) = (0, 0)$$时像素的变化值为$$E(0, 0) = 0$$。

<div align=center>
<img src="https://i.imgur.com/Nj3DVL6.png" width="50%">
</div>

由于角点是图像的局部特征，我们更关心$$E(u, v)$$在原点附近的变化情况。因此利用泰勒公式在原点附近展开：

$$
E(u, v) \approx E(0, 0) + 
\begin{bmatrix}
E_u(0, 0) & E_v(0, 0)
\end{bmatrix}
\begin{bmatrix}
u \\ v
\end{bmatrix} + 
\frac{1}{2} 
\begin{bmatrix}
u & v
\end{bmatrix}
\begin{bmatrix}
E_{uu}(0, 0) & E_{uv}(0, 0) \\
E_{vu}(0, 0) & E_{vv}(0, 0)
\end{bmatrix}
\begin{bmatrix}
u \\ v
\end{bmatrix}
$$

对原始公式进行求导可以得到：

$$
E_u (u, v) = \sum_{x, y} 2 w(x, y) [I(x+u, y+v) - I(x, y)] I_x(x+u, y+v)
$$

$$
E_u (0, 0) = E_v (0, 0)  = 0
$$

类似地，可以证明Hessian矩阵满足：

$$
E_{uu} (0, 0) = \sum_{x, y} 2 w(x, y) I_x(x, y) I_x(x, y)
$$

$$
E_{vv} (0, 0) = \sum_{x, y} 2 w(x, y) I_y(x, y) I_y(x, y)
$$

$$
E_{uv} (0, 0) = E_{vu} (0, 0) = \sum_{x, y} 2 w(x, y) I_x(x, y) I_y(x, y)
$$

把它们代回原式可以得到化简后的形式：

$$
E(u, v) \approx 
\begin{bmatrix}
u & v
\end{bmatrix}
M
\begin{bmatrix}
u \\ v
\end{bmatrix}
$$

$$
M = \sum_{x, y} w(x, y)
\begin{bmatrix}
I_x I_x & I_x I_y \\
I_x I_y & I_y I_y
\end{bmatrix}
$$

矩阵$$M$$也称为图像的**二阶矩矩阵(second moment matrix)**。二阶矩矩阵$$M$$的两个主方向对应图像梯度变化最大和最小的两个方向，对应的特征值表示变化的剧烈程度。如果两个特征值都接近于0，说明图像在所有的方向上变化都不明显，对应比较平坦的区域；如果两个特征值都很大，说明图像在所有的方向上都有剧烈的变化，即为所需的角点。

在Harris角点检测算法中定义了响应值$$R$$来判断某个点是否是角点：

$$
\begin{aligned}
R &= \lambda_1 \lambda_2 - \alpha \cdot (\lambda_1 + \lambda_2)^2 \\
&= \det (M) - \alpha \cdot \text{tr}^2 (M) \\
\end{aligned}
$$

其中$$\alpha$$为一个常数，一般取$$\alpha$$介于0.04~0.06。当响应值$$R$$大于指定的阈值时即可认为是一个可能的角点，而在实际中往往还会结合非极大值抑制来筛选出局部极值作为最终的角点。

<div align=center>
<img src="https://i.imgur.com/nHpkDzl.png" width="30%">
</div>

综合以上所有的步骤，得到Harris角点检测算法流程如下：

<div align=center>
<img src="https://i.imgur.com/cezLQYM.png" width="80%">
</div>

### Scale Invariance

图像角点对于平移、旋转以及光照变化都具有一定的不变性，但对于尺度变换却不具有不变性。简单来说当我们对图像进行缩放时，图像的边缘部分可能会转换成角点；而当我们对图像进行放大时，原来的角点则可能会转换成边缘。

<div align=center>
<img src="https://i.imgur.com/wHmtQVl.png" width="70%">
</div>

显然我们希望图像的特征点对于尺度变换也具有一定的不变性，为此我们首先需要对"尺度"进行定义。所谓"尺度"可以理解成图像的邻域性质，我们可以在不同的图像上使用尺寸不同的邻域进行观察，同样的特征应该会得到相同的结果。

<div align=center>
<img src="https://i.imgur.com/hByCzs6.png" width="50%">
</div>

接下来我们的问题就是如何在不同的图像上选择相应的邻域尺寸。一种解决办法是把特征定义成一个与邻域尺寸有关的函数，然后在不同大小的邻域上计算它，这样我们总能在某个尺度上找到所需的特征。同时这样的特征还需要在对应尺度上有稳定的响应，这样我们就可以把它在不同尺度上响应的最大值作为尺度不变的特征。

实践中一般可以通过图像卷积来构造尺度不变特征，比较常用的卷积核包括**LoG算子(Laplacian of Gaussian)**和**DoG算子(Difference of Gaussians)**：

$$
LoG = \sigma^2 (G_{xx} (x, y, \sigma) + G_{yy} (x, y, \sigma))
$$

$$
DoG = G(x, y, k \sigma) - G(x, y, \sigma)
$$

<div align=center>
<img src="https://i.imgur.com/f4IAqeI.png" width="40%">
</div>

其中不同的方差项$$\sigma$$对应不同的尺度。这样在特征点检测时同时在不同尺度上的每张图像都进行检测并在不同尺度上选择最大响应的特征就构造出了尺度不变的特征点。比如说Harris-Laplacian角点检测算法中利用LoG算子来构造尺度空间，它要求角点不仅要在图像空间上是局部极值还要在相邻尺度上也是一个局部极值，这样使得检测出的角点也具有尺度不变性。

<div align=center>
<img src="https://i.imgur.com/z74t3TC.png" width="50%">
</div>

## Feature Descriptors

通过特征点检测可以从图像中获得我们感兴趣的点坐标，但却不能对特征点进行分辨。以单应矩阵计算为例，我们可以利用角点检测来得到两幅图像上不同位置的角点，但却不知道如何将左边图像的角点与右边图像中的角点对应起来。

<div align=center>
<img src="https://i.imgur.com/f4WkCEb.png" width="70%">
</div>

因此我们需要为图像中每个特征点指定一个描述符作为标识，称之为描述子。我们希望特征点的描述子是独一无二的，且对于图像变换具有一定的不变性，这样在相机位置发生变化后空间中同一个点的描述子在变化前后保持不变从而方便我们寻找特征点的匹配并计算图像的变换。

### SIFT Descriptor

**SIFT(scale-invariant feature transform)**是最出名的图像描述子之一，它的核心思想是为特征点构造一个对平移、旋转、缩放以及光照变化不变的向量作为特征点的描述子。它的主要流程包括构造尺度空间、特征点定位、旋转对齐以及构造特征描述子四个步骤。

<div align=center>
<img src="https://i.imgur.com/3LdGYoT.png" width="70%">
</div>

前两步是对图像上的极值点进行检测，这里不再赘述。第三步旋转对齐的目的是检测出极值点梯度变化的主方向，然后以该方向为初始方向计算该点邻域上其它方向上的梯度直方图。由于计算的起点都是梯度变化的主方向，最终得到的梯度直方图具有旋转不变性。当然在实际计算中并不需要真的进行对直方图进行旋转，只需要计算出标准的直方图后以响应最大的方向即可。

<div align=center>
<img src="https://i.imgur.com/kMCDzqA.png" width="30%">
</div>

SIFT算法的最后一步是为特征点指定描述子，这里使用了梯度直方图作为描述子。对于每个特征点，我们在相应的尺度上选择它的邻域并计算邻域上的梯度向量并旋转到特征点的主方向上。然后将邻域划分成若干个小格并在每个小格上统计梯度的直方图，并用高斯核进行加权。此外为了避免某个方向上梯度过大，往往还会对旋转后的梯度进行裁剪。最后将每个格子的梯度直方图拼接到一起并进行规范化就得到了一个高维的特征向量作为SIFT描述子。

<div align=center>
<img src="https://i.imgur.com/csrslWA.png" width="70%">
</div>

SIFT描述子本身比较复杂，同时在实现时有很多的技巧来对计算进行加速，因此大多数情况下直接调用OpenCV等视觉库的现成实现即可。

### Matching Feature Points

有了描述子就可以对进行特征点进行匹配了。特征匹配的本质是在高维空间中寻找最相近的向量，最简单的方法是暴力匹配即对于给定的查询向量计算它与所有已知向量的距离，然后选择距离最小的向量作为匹配。这种暴力匹配的方法在特征点比较多的情况下计算效率会非常低，因此在实际应用中暴力匹配这种算法并不多见。更常用的方法是使用kd树这种数据结构来对匹配进行加速，而SIFT算法的作者还提出了一种BBF算法来进一步加速匹配过程。

<div align=center>
<img src="https://i.imgur.com/YyWsIk1.png" width="70%">
</div>

除了kd树外还可以使用哈希的思想来对匹配进行加速，如使用小波来对特征描述子进行哈希或是利用局部敏感哈希(locality sensitive hashing)来缩减特征描述子的维度，从而大大加快特征匹配的过程。

<div align=center>
<img src="https://i.imgur.com/f8Ptl89.png" width="30%">
</div>

## Feature Robustness

### Robust Error Functions

特征匹配的一个常见问题是错误的匹配：即使使用了具有不变性的描述子，当图像特征点本身就具有相似性时我们仍然可能会获得很多错误的匹配：

<div align=center>
<img src="https://i.imgur.com/4ueNeTx.png" width="70%">
</div>

从更广义的角度上来看，特征匹配的目的是利用已有数据去拟合一个模型使得模型在数据上的误差足够小。由于错误匹配的存在，拟合后的模型在某些数据点上会存在非常显著的误差，这些点称为**离群点(outliers)**。显然我们希望在进行模型拟合前先将这些离群点找到并去除，然后在剩下的数据点上进行拟合，这样就能够保证拟合的模型具有足够的精度。

当然在实际应用中一般很难对这些离群点进行识别，因此比较常用的处理方法是对这些离群点造成的误差进行约束，使模型对这些离群点不会特别敏感。我们可以使用一个**鲁棒核函数(robust kernel function)**来处理每个数据点上的拟合误差：

$$
\rho(u, \sigma) = \frac{u^2}{\sigma^2 + u^2}
$$

其中$$u = y - f(x)$$为数据$$x$$在模型$$f$$下的残差向量，参数$$\sigma$$控制了核函数的作用的范围。当残差非常大时，核函数作用后的函数值会接近于1，远小于平方误差函数产生的值。因此鲁棒核函数可以约束模型的拟合误差从而减轻离群点产生的影响。

<div align=center>
<img src="https://i.imgur.com/JkvZH9F.png" width="40%">
</div>

最后需要说明的是使用鲁棒核函数会额外引入一个超参数$$\sigma$$，当$$\sigma$$的取值过大或过小时都会对影响最终的拟合效果。

### RANSAC

特征点匹配更常用的算法是RANSAC算法，它的思想是从数据中抽样出一部分数据点来拟合模型并用拟合的模型来判断数据集上的点是否是离群点。当我们采样的次数足够多时，我们总能够拟合到一个好的模型使得数据集上大部分数据点都能够被模型很好的描述。

以直线拟合为例，RANSAC算法首先会在数据集上挑选出拟合模型所需的数量最少的点，这里会随机挑选出2个点用于拟合。

<div align=center>
<img src="https://i.imgur.com/B4otgS2.png" width="70%">
</div>

然后使用这两个点拟合得到直线如下图所示。

<div align=center>
<img src="https://i.imgur.com/7OiYzc2.png" width="70%">
</div>

利用当前拟合的直线来计算数据集上所有数据的拟合误差，误差小于阈值的称为**内点(inliers)**。

<div align=center>
<img src="https://i.imgur.com/Llrh6xd.png" width="70%">
</div>

最后RANSAC算法会重复以上三步并选择内点数目最多的直线作为最终的模型。

<div align=center>
<img src="https://i.imgur.com/nyX209W.png" width="70%">
</div>

我们把直线拟合问题推广到更一般的情况，就得到了广义上的RANSAC算法：

<div align=center>
<img src="https://i.imgur.com/z8sVKeu.png" width="70%">
</div>

最后一个问题是我们需要多少次抽样才能得到合理的模型，这里需要做一点简单的数学推导。假设我们每次需要$$s$$个数据点来进行拟合，数据中离群点的比例为$$e$$。那么在一次采样时采样点都是内点的概率为：

$$
(1 - e)^s
$$

而采样点中至少包含一个离群点的概率为：

$$
1 - (1 - e)^s
$$

因此进行$$N$$次采样，每次都包含离群点的概率为：

$$
[1 - (1 - e)^s]^N
$$

这种情况下我们认为算法失败了，令这个概率小于$$(1-p)$$得到：

$$
[1 - (1 - e)^s]^N \lt (1 - p)
$$

求解这个不等式得到采样次数的下界：

$$
N \gt \frac{\log (1-p)}{\log \big( 1 - (1 - e)^s \big)}
$$

在实际应用中一般可以事先指定几个常数$$p$$、$$e$$和$$s$$来计算采样的次数$$N$$。如果对离群点的比例$$e$$没有估计，还可以使用自适应的RANSAC算法在每次迭代中对迭代次数进行动态更新：

<div align=center>
<img src="https://i.imgur.com/2LoChyl.png" width="60%">
</div>