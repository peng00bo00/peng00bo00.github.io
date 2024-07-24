---
layout: post
title: OMSCS-ML课程笔记06-SVM and Kernel Methods
date: 2021-09-13
description: 支持向量机
tags: CS7641-ML
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 机器学习课程([CS 7641: Machine Learning](https://omscs.gatech.edu/cs-7641-machine-learning))的同步课程笔记。课程内容涉及监督学习、无监督学习和强化学习三个部分，本节主要介绍监督学习中支持向量机的相关内容。
{: .block-preface }


## Linear SVM

**支持向量机(support vector machine, SVM)**是经典的二分类模型。从几何的角度上讲，二分类问题可以理解为在空间中寻找一个**超平面(hyperplane)**来将数据划分为两部分。在数据集是线性可分的情况下存在无数个可行的超平面将数据集正确划分，如下图中的$$H_2$$和$$H_3$$所示。显然我们希望从这些超平面中选择一个「最好」的使得模型对于噪声和扰动都具有足够的鲁棒性。

<div align=center>
<img src="https://upload.wikimedia.org/wikipedia/commons/b/b5/Svm_separating_hyperplanes_%28SVG%29.svg" width="40%">
</div>

### Maximal Margin Hyperplanes

SVM通过最大化两个类别数据到超平面的距离来求解这个的超平面。假设超平面方程为$$w^T x + b = 0$$，对于正负样本有：

$$
\begin{aligned}
w^T x_i + b &\geq +1,  & &y_i = +1 \\
w^T x_i + b &\leq -1, & &y_i = -1 \\
\end{aligned}
$$

换句话说

$$
y_i (w^T x_i + b) \geq 1
$$

当且仅当$$x_i$$到超平面的距离最小时取等号，此时的$$x_i$$称为**支持向量(support vector)**。

对于任意点$$x_i$$，它到超平面的距离为：

$$
\gamma_i = \frac{y_i (w^T x_i + b)}{\Vert w \Vert}
$$

将支持向量带入上式可以得到正负两类样本到超平面的最小距离为：

$$
\gamma = \frac{2}{\Vert w \Vert} \sim \frac{1}{\Vert w \Vert^2}
$$

进而得到SVM对应的约束优化问题：

$$
\begin{aligned}
\max_{w, b} \ \ & \frac{1}{\Vert w \Vert^2} \\
\text{s.t.} \  \ & y_i (w^T x_i + b) \geq 1
\end{aligned}
$$

由于最大化$$\frac{1}{\Vert w \Vert^2}$$与最小化$$\frac{1}{2} \Vert w \Vert^2$$是等价的，因此更常见的SVM优化形式是：

$$
\begin{aligned}
\min_{w, b} \ \ & \frac{1}{2} \Vert w \Vert^2 \\
\text{s.t.} \  \ & y_i (w^T x_i + b) \geq 1
\end{aligned}
$$

求解得到分类超平面后对应的决策函数为：

$$
f(x) = \text{sign} (w^T x + b)
$$

### Dual Problem

对于SVM的约束优化问题我们可以使用**拉格朗日乘子法(Lagrange multiplier)**进行求解。为此构造拉格朗日函数如下：

$$
L(w, b, \alpha) = \frac{1}{2} \Vert w \Vert^2 - \sum_{i=1}^N \alpha_i y_i (w^T x_i + b) + \sum_{i=1}^N \alpha_i
$$

其中$$\alpha = (\alpha_1, \alpha_2, ..., \alpha_N)$$为拉格朗日乘子向量。此时约束优化问题等价于拉格朗日函数的极小极大问题，称为**原始问题(primal problem)**：

$$
\min_{w. b} \max_{\alpha, \ \alpha_i \geq 0} L(w, b, \alpha)
$$

我们定义拉格朗日函数的极大极小问题为极小极大问题的**对偶问题(dual problem)**：

$$
\max_{\alpha, \ \alpha_i \geq 0} \min_{w. b} L(w, b, \alpha)
$$

对于线性可分情况SVM的原始问题与对偶问题有相同的解，因此我们可以通过求解对偶问题来获得原始问题的最优解。分别对$$w$$和$$b$$求偏导并令导数为0可以得到：

$$
\frac{\partial L}{\partial w} = w - \sum_{i=1}^N \alpha_i y_i x_i = 0 \Leftrightarrow w = \sum_{i=1}^N \alpha_i y_i x_i
$$

$$
\frac{\partial L}{\partial b} = - \sum_{i=1}^N \alpha_i y_i = 0 \Leftrightarrow \sum_{i=1}^N \alpha_i y_i = 0
$$

将上述条件带入$$L$$得到：

$$
\begin{aligned}
\min_{w. b} L(w, b, \alpha) &= \frac{1}{2} \sum_{i=1}^N \sum_{j=1}^N \alpha_i \alpha_j y_i y_j x_i^T x_j - \sum_{i=1}^N \alpha_i y_i \bigg( x_i^T \sum_{j=1}^N \alpha_j y_j x_j + b \bigg) + \sum_{i=1}^N \alpha_i \\
&= -\frac{1}{2} \sum_{i=1}^N \sum_{j=1}^N \alpha_i \alpha_j y_i y_j x_i^T x_j + \sum_{i=1}^N \alpha_i
\end{aligned}
$$

然后对$$L$$求极大得到关于$$\alpha$$的约束优化问题：

$$
\begin{aligned}
\max_\alpha \ \ & -\frac{1}{2} \sum_{i=1}^N \sum_{j=1}^N \alpha_i \alpha_j y_i y_j x_i^T x_j + \sum_{i=1}^N \alpha_i \\
\text{s.t.} \  \ & \sum_{i=1}^N \alpha_i y_i = 0 \\
& \alpha_i \geq 0
\end{aligned}
$$

更常见的形式是带约束的最小化问题：

$$
\begin{aligned}
\min_\alpha \ \ & \frac{1}{2} \sum_{i=1}^N \sum_{j=1}^N \alpha_i \alpha_j y_i y_j x_i^T x_j - \sum_{i=1}^N \alpha_i \\
\text{s.t.} \  \ & \sum_{i=1}^N \alpha_i y_i = 0 \\
& \alpha_i \geq 0
\end{aligned}
$$

此时的优化问题为关于$$\alpha$$的凸二次规划问题，可以使用二次规划的相关算法求解。最后将拉格朗日乘子向量$$\alpha$$带入约束条件即可计算出超平面参数：

$$
w = \sum_{i=1}^N \alpha_i y_i x_i
$$

$$
b = y_j - \sum_{i=1}^N \alpha_i y_i x_i^T x_j
$$

其中$$x_j$$和$$y_j$$需要满足对应的乘子$$\alpha_j > 0$$。

利用原始问题的KKT条件还可以进一步证明拉格朗日乘子向量$$\alpha$$仅在支持向量处存在非0值，因此$$\alpha$$实际上是非常稀疏的。换句话说我们只需要记录几个支持向量的乘子就可以计算分类超平面，而对应的决策函数则可以表示为支持向量内积的形式：

$$
f(x) = \text{sign} \bigg( \sum_{i=1}^N \alpha_i y_i x_i^T x + b \bigg)
$$

## Kernel Methods

对于线性不可分的情况使用SVM是不能得到分类超平面的。对于这样的问题常用的处理方式是构造一个到高维的映射$$\phi(x)$$然后在更高维的空间中使用SVM。以下图为例，在二维平面上不存在分类超平面但在三维空间中则可以很容易地对样本进行划分。

<div align=center>
<img src="https://i.imgur.com/uEvAnYQ.png" width="70%">
</div>

同样地，利用映射$$\phi(x)$$可以得到高维空间中的约束优化问题：

$$
\begin{aligned}
\min_\alpha \ \ & \frac{1}{2} \sum_{i=1}^N \sum_{j=1}^N \alpha_i \alpha_j y_i y_j \phi(x_i)^T \phi(x_j) - \sum_{i=1}^N \alpha_i \\
\text{s.t.} \  \ & \sum_{i=1}^N \alpha_i y_i = 0 \\
& \alpha_i \geq 0
\end{aligned}
$$

不难发现此时非线性的SVM与线性SVM没有任何本质区别，只需要将低维向量$$x$$映射到高维空间中即可。实际上我们并不需要去显式地定义映射$$\phi(x)$$，只需要内积项$$\langle \phi(x_i), \phi(x_j) \rangle = \phi(x_i)^T \phi(x_j)$$就可以求解SVM。我们定义核函数为低维向量在高维空间中的内积：

$$
K(x, y) = \langle \phi(x), \phi(y) \rangle
$$

利用核函数我们可以避免计算高维向量$$\phi(x)$$从而加速模型的训练，此时优化问题可以改写成核函数的形式：

$$
\begin{aligned}
\min_\alpha \ \ & \frac{1}{2} \sum_{i=1}^N \sum_{j=1}^N \alpha_i \alpha_j y_i y_j K(x_i, x_j)- \sum_{i=1}^N \alpha_i \\
\text{s.t.} \  \ & \sum_{i=1}^N \alpha_i y_i = 0 \\
& \alpha_i \geq 0
\end{aligned}
$$

求解拉格朗日乘子向量$$\alpha$$后带入约束条件得到偏置项：

$$
b = y_j - \sum_{i=1}^N \alpha_i y_i K(x_i, x_j)
$$

对应的决策函数为：

$$
f(x) = \text{sign} \bigg( \sum_{i=1}^N \alpha_i y_i K(x, x_i) + b \bigg)
$$

显然并不是任意的二元函数都可以作为核函数来使用，实际上核函数是**正定核函数(positive definite kernel function)**，它们满足对称正定的条件即对应的Gramm矩阵$$K$$为半正定矩阵。

在实践中一般很少从零开始构造核函数，往往是利用一些已有的核函数并在此基础上调整相关的参数。常用的核函数包括多项式核(polynomial kernel)、高斯核(Gaussian kernel)等。

## Back to Boosting

本节最后我们从margin的角度来分析boosting方法。boosting的决策函数可以表示为：

$$
f(x) = \text{sign} \bigg( \frac{\sum_i \alpha_i h_i(x)}{\sum_i \alpha_i} \bigg)
$$

注意到上式和[Ensemble Learning](/blog/2021/OMSCS-ML-NOTES-05/#boosting)一节中给出的形式有所区别，这里我们多了分母项$$\sum_i \alpha_i$$。由于任意$$\alpha_i > 0$$，使用上式定义的决策函数并不会改变函数的结果，同时加入分母项之后可以保证决策函数的输出在[-1, +1]区间内。

显然对于比较容易的样本$$f(x)$$的输出应该靠近[-1, +1]区间的两侧；而比较困难的样本$$f(x)$$的输出则会靠近原点。因此可以认为正负样本到原点的距离描述了模型对分类结果的把握，距离越大表示模型对结果越有信心。在boosting算法的训练过程中基分类器会更关注那些位于原点附近的困难样本，这使得加权后这些样本的类别会不断向区间端点移动。从这个角度看boosing相当于最大化正负样本之间的间隔，与SVM算法有异曲同工之妙。

<div align=center>
<img src="https://i.imgur.com/WtFdQwn.png" width="90%">
</div>


## Reference

- 第7章：支持向量机，统计学习方法（第2版）
- [Wikipedia: Support-vector machine](https://en.wikipedia.org/wiki/Support-vector_machine)
- [Wikipedia: Karush–Kuhn–Tucker conditions](https://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)