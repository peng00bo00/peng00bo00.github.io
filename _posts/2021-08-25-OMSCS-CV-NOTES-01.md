---
layout: post
title: OMSCS-CV课程笔记01-Linear Image Processing
date: 2021-08-25
description: 图像处理基础
tags: CS6476-CV
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 计算机视觉课程([CS 6476: Computer Vision](https://omscs.gatech.edu/cs-6476-computer-vision))的同步课程笔记。课程内容涉及图像处理以及传统计算机视觉的相关理论和方法，本节主要介绍图像处理的基础知识。
{: .block-preface }


## Image as Functions

图像处理是对图像进行操作，此时我们会把图像看做是一个二元函数$$f(x,y)$$。对于灰度图像可以认为图像的每一点是一个标量，因此$$f$$可以表示为：

$$
f:[a, b] \times [c, d] \rightarrow [\min, \max]
$$

其中$$[a, b]$$和$$[c, d]$$表示图像的大小范围，$$[\min, \max]$$表示灰度的范围。对于彩色图像则可以认为$$f$$的值域是一个向量：

$$
f(x, y) = 
\begin{bmatrix}
r(x, y) \\ g(x, y) \\ b(x, y)
\end{bmatrix}
$$

其中$$r(x, y)$$，$$g(x, y)$$，$$b(x, y)$$分别表示三个通道上的色彩值。

由于计算机不能表示连续的对象，计算机中的数字图像实际上是经过离散后的结果，此时我们也可以把图像理解为一个数组或是矩阵。

<div align=center>
<img src="https://i.imgur.com/Pxskgo5.png" width="32%">
<img src="https://i.imgur.com/Pa10JG2.png" width="40%">
</div>

将图像视为函数之后就可以用函数的方法对图像进行操作，其中最简单的操作是对函数进行加减。比如说我们可以把对图像添加噪声的操作理解为原函数$$f(x, y)$$与噪声函数noise相加。常见的噪声函数包括：

- 椒盐噪声(salt-and-pepper noise)：随机出现的黑白噪点；
- 脉冲噪声(impulse noise)：随机出现的白噪点；
- 高斯噪声(Gaussian noise)：服从正态分布$$N(0, \sigma^2)$$的噪点。

<div align=center>
<img src="https://i.imgur.com/xGQc9uM.png" width="60%">
</div>

对于高斯噪声，方差$$\sigma^2$$控制了噪声的强度：方差越大噪声越明显。

## Filtering

去除图像噪声的过程称为**滤波(filtering)**。尽管精确地还原出无噪声图像是非常困难的，但滤波可以使图像变得更加平滑，也可以实现图像模糊的视觉效果。为了便于讨论我们对噪声做出以下两条假定：

- 每一个像素点不带噪声的真值与它周围的像素值相近；
- 每一个像素点添加的噪声相互独立。

在这样的假设下我们可以使用滑动平均的方法对图像进行滤波，具体地针对每个像素点取它周围像素的平均值作为滤波的结果。假设我们已知图像$$F(x,y)$$，对它使用滑动平均进行滤波的过程如下：

<div align=center>
<img src="https://i.imgur.com/iCybD4n.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/VGJ9mqJ.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/SBpay1T.png" width="50%">
</div>

<div align=center>
<img src="https://i.imgur.com/YaSgCw1.png" width="50%">
</div>

以上过程写成数学表达式为：

$$
G(i, j) = \frac{1}{(2k+1)^2} \sum_{u=-k}^k \sum_{v=-k}^k F(i+u, j+v)
$$

其中$$k$$为邻域的半径。上述公式表明像素点的邻域对中心的贡献是相同的，每个相邻像素点都具有相同权重。实际中更常用的方法是对邻域中的像素赋予不同的权重，此时公式可以改写为：

$$
G(i, j) = \sum_{u=-k}^k \sum_{v=-k}^k H(u, v) F(i+u, j+v)
$$

其中$$H(u, v)$$满足$$\sum H(u, v) = 1$$，它代表邻域内不同像素具有的权重，称为滤波器的**核(kernel)**。上述的滤波过程称为**相关滤波(correlation filtering)**，记为$$G = H \otimes F$$。

使用**方块核(box filter)**进行滤波可以图像模糊的效果如下：

<div align=center>
<img src="https://i.imgur.com/ubX9JEI.png" width="30%">
<img src="https://i.imgur.com/GQa2SBz.png" width="30%">
</div>

仔细观察可以发现图像中有很多的方块状伪影。产生这种现象的原因是方块核并不能正确地反映图像模糊的过程：我们可以想象眼前有一个亮点不断离我们远去，随着距离的增加图像应该是中间亮边缘暗的，如下图所示：

<div align=center>
<img src="https://i.imgur.com/23SLRc2.png" width="20%">
</div>

因此要正确的实现图像模糊的效果我们需要类似于上图的核。实际中比较常用的是**高斯核(Gaussian kernel)**，它可以看做是高斯函数的离散形式：

<div align=center>
<img src="https://i.imgur.com/2WtpHvU.png" width="70%">
</div>

使用高斯核代替方块核可以得到更加合理的模糊效果：

<div align=center>
<img src="https://i.imgur.com/ubX9JEI.png" width="30%">
<img src="https://i.imgur.com/zVfSVk4.png" width="30%">
</div>

## Linearity and Convolution

接下来讨论相关滤波的性质。设$$f_1$$和$$f_2$$为函数，$$a$$为常量，称系统$$H$$为线性系统(算子)如果它满足可加性和数乘性质：

- $$H(f_1 + f_2) = H(f_1) + H(f_2)$$
- $$H(a \cdot f) = a \cdot H(f)$$

然后引入**脉冲函数(impulse)**的概念：在离散情况下某个给定位置取1的函数称为脉冲函数。

<div align=center>
<img src="https://i.imgur.com/BxOVn7u.png" width="30%">
</div>

将脉冲函数序列输入到系统$$H$$中可以得到响应$$h(t)$$，如果$$H$$是线性系统我们还可以通过响应$$h(t)$$来描述系统的性质。

<div align=center>
<img src="https://i.imgur.com/Lmzpryw.png" width="60%">
</div>

可以证明相关滤波是线性系统。因此可以将脉冲函数$$F(x, y)$$输入到任意滤波器$$H(u, v)$$中，得到响应$$G(x, y)$$：

<div align=center>
<img src="https://i.imgur.com/i0hpXOb.png" width="70%">
</div>

不难发现响应$$G(x, y)$$实际上就是对滤波器$$H(u, v)$$进行了翻转。类似于相关滤波的运算我们可以定义**卷积(convolution)**运算：

$$
G(i, j) = \sum_{u=-k}^k \sum_{v=-k}^k H(u, v) F(i-u, j-v)
$$

上式记为$$G = H * F$$，容易验证使用卷积来代替相关滤波就可以使脉冲函数的响应与卷积核相同。实际上卷积还具有非常有用的性质，包括：

- 线性且平移不变；
- 交换性：$$f * g = g * f$$；
- 结合性：$$f * (g * h) = (f * g) * h$$；
- 恒等式：$$e * f = f$$，其中$$e$$为脉冲函数；
- 微分性质：$$\frac{\partial}{\partial x} (f * g) = \frac{\partial f}{\partial x} * g$$

显然对于对称的卷积核，卷积运算与相关滤波是等价的；而对于不对称的滤波核则可以通过调整卷积核的形式来达到一样的效果。因此我们可以用卷积来描述滤波的过程，同时也不再区分卷积和相关滤波。

利用卷积的结合性还可以降低计算的复杂度。假设卷积核$$G$$可以通过两个一维卷积核表示：

$$
G = c * r
$$

因此使用$$G$$直接进行滤波与分别使用一维卷积核$$c$$和$$r$$进行滤波是等价的：

$$
G * F = (c * r) * F = c * (r * F)
$$

假设$$G$$的尺寸为$$W \times W$$，$$F$$的尺寸为$$H \times H$$，则直接进行卷积的计算复杂度为$$O(W^2 H^2)$$而分解后的计算复杂度则为$$O(W H^2)$$。因此使用小的卷积核来替代大的卷积核可以极大地提高程序的性能。

## Filters as Templates

我们还可以把滤波的过程看做是用给定的**模板(template)**在图像上进行搜索。以一维信号为例，假设我们希望从已知信号中找到一个给定的模板。我们可以把该模板作为滤波器在原始信号上进行滤波，在滤波窗口中原始信号与滤波器越相近响应越强。因此只需在响应信号中寻找最大值即可找到模板在原始信号中的位置：

<div align=center>
<img src="https://i.imgur.com/NaY5Pdx.png" width="52%">
<img src="https://i.imgur.com/PXVHfw1.png" width="20%">
<img src="https://i.imgur.com/k2lrpEQ.png" width="50%">
</div>

在图像上使用模板进行滤波的过程也称为**模板匹配(template matching)**。模板匹配的结果表示图像对应区域与模板的相似程度，匹配的响应越强表示该区域与模板越相似。

<div align=center>
<img src="https://i.imgur.com/U4y0rHi.png" width="60%">
</div>

需要额外说明的是进行模板匹配时需要保证待搜寻的目标与模板的大小、方向以及外观尽可能相似，否则可能会出现无法匹配的情况。

## Edge Detection: Gradients

人眼可以通过图像的边缘来认识图像的内容，因此边缘是一种重要的图像特征。边缘可以有各种各样的定义，在图像处理中最常见的是把图像灰度值发生剧烈变化的区域定义为边缘，因此可以把图像导数取极值的位置视为图像的边缘。

<div align=center>
<img src="https://i.imgur.com/v4rZcWL.png" width="60%">
</div>

由于图像是二元函数，实际上这里"导数"指的是图像的**梯度(gradient)**：

$$
\nabla f = 
\begin{bmatrix}
\frac{\partial f}{\partial x} \\ \frac{\partial f}{\partial y}
\end{bmatrix}
$$

梯度的方向为：

$$
\theta = \arctan \bigg(\frac{\nabla_y f}{\nabla_x f}\bigg)
$$

对应的模长为：

$$
\Vert \nabla f \Vert = \sqrt{\bigg(\frac{\partial f}{\partial x}\bigg)^2 + \bigg(\frac{\partial f}{\partial y}\bigg)^2}
$$

在图像上使用差分代替微分得到图像上的梯度算子：

$$
\frac{\partial f(x, y)}{\partial x} \approx f(x+1, y) - f(x, y)
$$

$$
\frac{\partial f(x, y)}{\partial y} \approx f(x, y+1) - f(x, y)
$$

显然我们可以把差分的形式通过滤波进行表示从而得到相应的滤波核。实际应用中更为常用的梯度算子包括Sobel算子、Prewitt算子、Roberts算子等，它们对应的滤波核如下：

<div align=center>
<img src="https://i.imgur.com/nsaDue1.png" width="60%">
</div>

需要说明的是这里"滤波"指的是相关滤波，如果使用卷积滤波来计算的话则需要对滤波核进行相应的处理。在实际应用中由于噪声的存在直接计算梯度可能会产生较大的误差，因此往往需要首先对图像进行降噪然后在计算梯度。以一维信号为例，假设我们使用高斯核对信号进行降噪，利用卷积的微分性质可以把高斯滤波和边缘检测合并成一个算子：

<div align=center>
<img src="https://i.imgur.com/6POXCm5.png" width="70%">
</div>

因此只需要使用高斯核的一阶导数进行滤波即可。为了获得边缘的位置我们需要在响应信号上再次求导，导数为0的位置即为边缘。再次利用卷积的微分性质将求导运算整合到卷积核中，得到高斯核的二阶导。使用高斯核的二阶导进行滤波，响应信号的过0点即为边缘：

<div align=center>
<img src="https://i.imgur.com/yCDgJoa.png" width="70%">
</div>

## Edge detection: 2D Operators

最后来讨论图像上的边缘检测。类似于一维信号，我们同样可以将高斯核与求梯度进行结合从而加速计算：

<div align=center>
<img src="https://i.imgur.com/snLlyTQ.png" width="70%">
</div>

通常情况下寻找图像中的边缘可以采用如下的流程：

1. 对图像进行滤波并计算梯度；
2. 对梯度进行阈值化保留梯度较强的区域；
3. 对当前得到的边缘进行细化；
4. 将细化后的边缘重新连接得到最终的边缘。

<div align=center>
<img src="https://i.imgur.com/BoiLtDt.png" width="30%">
<img src="https://i.imgur.com/4HtBHwl.png" width="30%">
<img src="https://i.imgur.com/pG4n0sU.png" width="30%">
</div>

图像边缘检测算法中最常用的是Canny算法同样采用了这样一套流程。在边缘的细化过程中Canny算法通过**非极大值抑制(non-maximum suppression)**来清除杂乱的边缘，而在最后一步则使用了2套阈值来连接细化后的边缘：

- 利用高阈值和低阈值将边缘划分为强边缘(大于高阈值)、弱边缘(介于高阈值和低阈值之间)和非边缘(小于低阈值)；
- 对于强边缘进行保留，并对非边缘进行抑制；
- 对于弱边缘，如果它附近存在强边缘则进行保留否则进行抑制。

<div align=center>
<img src="https://i.imgur.com/MZMukk7.png" width="30%">
<img src="https://i.imgur.com/bzfOUUk.png" width="30%">
<img src="https://i.imgur.com/M0YhYbD.png" width="30%">
<img src="https://i.imgur.com/kmqd4Px.png" width="30%">
<img src="https://i.imgur.com/OjbfqJN.png" width="30%">
</div>

此外值得一提的是高斯模糊过程中方差的选取也会对边缘检测的最终效果产生一定的影响：使用较大的$$\sigma$$会得到图像中较为显著的边缘，反之较小的$$\sigma$$则会保留图像中细节部分的边缘。因此$$\sigma$$的取值还需要结合实际需求进行选择。

<div align=center>
<img src="https://i.imgur.com/UBDvzKp.png" width="70%">
</div>

除了一阶梯度算子外我们也可以使用二阶梯度算子(**Laplace算子**)来进行边缘检测，其定义为函数在两个方向上二阶导数的和。我们同样可以将高斯滤波与Laplace算子进行结合，得到**LoG算子(Laplacian of Gaussian)**。类似于一维信号的边缘检测，使用LoG算子进行滤波后图像的过0点即为所需边缘。

$$
\nabla^2 h = \frac{\partial^2 f}{\partial x^2} + \frac{\partial^2 f}{\partial y^2}
$$

<div align=center>
<img src="https://i.imgur.com/mCzoRkl.png" width="36%">
<img src="https://i.imgur.com/vg6nar8.png" width="30%">
</div>