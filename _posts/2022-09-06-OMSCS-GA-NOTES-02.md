---
layout: post
title: OMSCS-GA课程笔记02-Divide and Conquer
date: 2022-09-06
description: 分治算法
tags: CS6515-GA
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 高级算法课程([CS 6515: Intro to Grad Algorithms](https://omscs.gatech.edu/cs-6515-intro-graduate-algorithms))的同步课程笔记。课程内容涉及动态规划、随机算法、分治、图算法、最大流、线性规划以及NP完备等高级算法的设计思想和相关应用，本节主要介绍分治。
{: .block-preface }


**分治(Divide and Conquer)**是算法设计中非常重要的一种思想，很多常用的算法都是基于分治来设计和实现的。

## Fast Integer Multiplication

分治的一个基本应用是计算高位整数的乘法。当数字的位数高到无法直接计算时，我们仍然可以利用分治的思想来进行求解。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bdKavlE.png" width="80%">
</div>

### Gauss's Idea

在大多数情况下数字加减的计算代价是要小于乘除的，因此在进行计算时我们希望尽可能多地减少乘除计算的次数。以复数的乘法为例，两个复数的乘积需要进行4次乘法计算。但通过合理的算法我们可以把乘法计算降低到3次。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GVgFnPp.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sZlpuXw.png" width="80%">
</div>

### Naive Approach

回到高位整数乘法的问题，我们可以把数字拆成两半然后分别求解乘法。这样就可以得到一个递归的算法：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/KYNNu2s.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mctTDhl.png" width="80%">
</div>

递归算法的伪代码如下：

```
EasyMultiply(x, y):
  xL = first n/2 bits of x
  xR = last n/2 bits of x
  yL = first n/2 bits of y
  yR = last n/2 bits of y

  A = EasyMultiply(xL, yL)
  B = EasyMultiply(xR, yR)
  C = EasyMultiply(xL, yR)
  D = EasyMultiply(xR, yL)

  z = A << n + (C + D) << (n/2) + B

  return z
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/VXV71gO.png" width="80%">
</div>

利用主定理可以得到`EasyMultiply(x, y)`的复杂度为$O(n^2)$：

$$
T(n) = 4T(n/2) + O(n) = O(n^2)
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/n58W1jH.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/n58W1jH.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lB86OST.png" width="80%">
</div>

### Improved Approach

通过减少递归计算的次数可以降低整个算法的复杂度。具体地，我们利用[复数乘法](/2022/09/06/OMSCS-GA-NOTES-02.html#gausss-idea)的技巧来减少一次递归乘法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7Jl4b9D.png" width="80%">
</div>

此时的伪代码如下：

```
FastMultiply(x[], y[]):
  xL = first n/2 bits of x
  xR = last n/2 bits of x
  yL = first n/2 bits of y
  yR = last n/2 bits of y

  A = FastMultiply(xL, yL)
  B = FastMultiply(xR, yR)
  C = FastMultiply(xL+xR, yL+yR)

  z = A << n + (C - A - B) << (n/2) + B

  return z
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/EQfdgzn.png" width="80%">
</div>

改进后的计算复杂度为$O(n^{\log_2 3})$：

$$
T(n) = 3T(n/2) + O(n) = O(n^{\log_2 3})
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/04ipg9c.png" width="80%">
</div>

## Linear-Time Median

分治算法的另一个经典应用是用来寻找数组的中位数。对于更一般的情况，我们可以使用分治的思想来寻找数组中第k个最小的数值。最直接的实现方法是首先对数组进行排序然后选择第k个值，此时算法的复杂度为$O(n \log{n})$；而实际上通过合理的设计算法可以在$O(n)$时间内实现查询。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2rnFXNu.png" width="80%">
</div>

### QuickSort

在介绍线性时间内的查询算法前我们先回顾一下快速排序算法。在快速排序算法中，每一步我们需要选择一个pivot然后将整个数组划分为小于pivot和大于pivot的两部分，接下来分别对两边的数组进行排序。显然快速排序的性能取决于pivot的选择，我们希望pivot可以尽可能接近数组的中位数使得子问题的规模尽可能相等。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/59n6U1E.png" width="80%">
</div>

### QuickSelect

利用快速排序的思想就可以设计出快速选择算法。具体来说，当我们选择了某个pivot后需要将数组划分为小于pivot、等于pivot以及大于pivot三部分，然后利用这三个数组的大小和k值的关系继续递归求解子问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5vJvtm4.png" width="80%">
</div>

这样可以得到快速选择算法的伪代码如下：

```
Select(A[], k):
  Choose a pivot p
  Partition A[] to A<p, A=p, A>p

  if k <= len(A<p):
    return Select(A<p[], k)
  elif len(A<p) < k <= len(A<p) + len(A=p):
    return p
  else:
    return Selct(A>p[], k - (len(A<p) + len(A=p)))
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ulVpT4s.png" width="80%">
</div>

### Good Pivot

显然快速选择算法的核心在于如何选取合适的pivot，如果我们希望得到线性时间内的算法则需要保证p恰为数组的某个分位数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zKB3oRJ.png" width="80%">
</div>

在快速选择算法中我们要求pivot位于数组的25%到75%范围中，同时选取pivot的代价不超过$O(n)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mph57qz.png" width="80%">
</div>

假设我们随机选择数组中的一个元素作为pivot，则它位于25%到75%范围中的概率为1/2。通过对数组进行遍历我们可以在$O(n)$时间内验证pivot是否满足我们的需求，如果不满足则可以再随机选择一个pivot，理论上我们需要2次随机选择就可以得到合适的pivot。由于随机选择以及验证的复杂度不超过$O(n)$，这样设计的快速选择算法已经能够满足线性时间的需求。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cNfQJLA.png" width="80%">
</div>

### Recursive Pivot

对于随机选择pviot的策略，其单步算法的平均复杂度为$O(n)$，而我们希望能够在最差的情况下也能有$O(n)$复杂度的pivot选择方法。这样整个算法设计就归结于寻找一个在$O(n)$时间内寻找合适pivot的问题。具体来说，在快速选择算法中会额外抽样出一个原始数组大小1/5的子数组，然后在子数组上寻找中位数作为原始数组的pivot，这样整个算法的复杂度仍然是$O(n)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/s2QcL7j.png" width="80%">
</div>

当然我们仍然需要考虑如何抽样出子数组的问题。假设我们直接选择数组的前1/5作为子数组，此时是无法获得合适的pivot。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/s7S3GzY.png" width="80%">
</div>

显然我们希望子数组可以在某种程度上代表原来的数组。具体来说我们每5个元素一组将原始数组划分为n/5组，然后在每一组上选择其中的中位数重新构成子数组。由于每一组上只有5个元素，选择中位数的复杂度为$O(1)$。这样构造的子数组就是原始数组的一个好的代表。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/b1hXA5s.png" width="80%">
</div>

总结一下pivot选择策略就可以得到最终的快速选择算法：

```
FastSelect(A[], k):
  Break A[] into n/5 groups G1, G2, ...

  for i=1:n/5:
    sort(Gi)
    mi = median(Gi)

  S = [m1, m2, ...]

  p = FastSelect(S[], n/10)

  Partition A[] to A<p, A=p, A>p

  if k <= len(A<p):
    return FastSelect(A<p[], k)
  elif k > len(A<p) + len(A=p):
    return FastSelect(A>p[], k - (len(A<p) + len(A=p)))
  else:
    return p
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uYr3ZgH.png" width="80%">
</div>

此时的算法复杂度仍然为$O(n)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XabEiAG.png" width="80%">
</div>

最后需要证明的是这样选择出的pivot是一个好的pivot。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GlFQ2pi.png" width="80%">
</div>

## Solving Recurrences

分治算法的核心在于构造递归问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UPw8X8i.png" width="80%">
</div>

在分析算法复杂度时可以利用几何级数来进行考虑：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uxDGtbz.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2STHrYi.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/59tnnSD.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uPej1mc.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/AElHcuT.png" width="80%">
</div>

对于更一般的情况，可以利用递归次数以及子问题的规模来分析整个算法的复杂度：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/36tW5l5.png" width="80%">
</div>

## FFT

### Polynomial Multiplication

前面我们介绍过高位数字以及矩阵乘法的加速算法，而对于多项式我们使用它的系数来进行表示。此时两个$n-1$次多项式的乘法是一个$2n-2$次多项式，其系数可以使用两个多项式系数的卷积来计算：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/21gGrnL.png" width="80%">
</div>

对于更一般的情况，我们定义多项式的乘法为对应系数的卷积。显然可以在$O(n^2)$的时间内来完成计算，但实际上我们可以在$O(n \log n)$时间内完成卷积。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Y9HQBHd.png" width="80%">
</div>

### Convolution Applications

卷积运算在工程中有着大量的应用，比如说信号处理中的各种滤波器就是使用卷积来定义和实现的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/3TRmA58.png" width="80%">
</div>

### Polynomial Basics

回到多项式的计算，我们可以使用系数来表达多项式也可以使用它在一系列点上的值来进行表示。在数学上可以证明这两种表达形式是等价的，而且可以通过FFT来相互转换。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/1afFnbL.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9wLnONK.png" width="80%">
</div>

更进一步，通过采样点来表示的多项式更适合进行卷积运算。我们只需要把对应位置上的数值相乘即可：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/MfabNoG.png" width="80%">
</div>

### FFT

FFT的核心是选择合适的采样点。我们假设采样点是对称布置的$x_{i+n}=-x_i$，然后把多项式的系数$a$拆分成奇次项$a_\text{odd}$和偶次项$a_\text{even}$。这样我们可以构造两个新的多项式：

$$
A_\text{even} (y) = a_0 + a_2 y + \dots + a_{n-2} y^{\frac{n-2}{2}}
$$

$$
A_\text{odd} (y) = a_1 + a_3 y + \dots + a_{n-1} y^{\frac{n-2}{2}}
$$

而原始多项式$A(x)$可以表示为两个新多项式的和：

$$
A(x) = A_\text{even} (x^2) + x A_\text{odd} (x^2)
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Kv0GnQc.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/L0LlyLs.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/0pZFI0F.png" width="80%">
</div>

同时，由于采样点是对称布置的我们可以得到对称采样点上的函数关系：

$$
A(x_i) = A_\text{even} (x_i^2) + x_i A_\text{odd} (x_i^2)
$$

$$
A(x_{n+i}) = A(-x_i) = A_\text{even} (x_i^2) - x_i A_\text{odd} (x_i^2)
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/WEAMvEs.png" width="80%">
</div>

这样可以得到递归算法：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nhHNV10.png" width="80%">
</div>

### Complex Numbers

需要注意的是FFT在选择采样点时要考虑复数的情况：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UJYJY0H.png" width="80%">
</div>

利用欧拉公式可以把复数表示为极坐标的形式，而且这样的形式更适合乘法运算。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uaWpYXK.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uaWpYXK.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jFnoloH.png" width="80%">
</div>

这样多项式方程$z^n = 1$的根就可以用复平面上的角度来理解。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IHhXdxR.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ST1tvKC.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qDNusHp.png" width="80%">
</div>

同时不难发现$z^n = 1$的根还具有对称性和平方性质，因此我们可以把这些根作为FFT的采样点。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/B4POOC1.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7FnvM2f.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/H7eawI2.png" width="80%">
</div>

### High-Level

结合复数上根的性质就得到了完整的FFT算法：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lSJlhT6.png" width="80%">
</div>

其伪代码如下：

```
FFT(a[], w):
  if len(a) == 1:
    return a[0]

  a_even = [a0, a2, ...]
  a_odd  = [a1, a3, ...]

  s = FFT(a_even, w^2)
  t = FFT(a_odd, w^2)

  for j=1:(n/2-1):
    r[j]    = s[j] + t[j] * w[j]
    r[j+n/2]= s[j] - t[j] * w[j]

  return r
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ziPwiDZ.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TPzrvSI.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FaQTL5C.png" width="80%">
</div>

而利用FFT实现多项式乘法只需要先将两个多项式转换成采样点的表示形式，然后在对应采样点上相乘即可。注意如果要获得系数形式的多项式最后还需要使用inverse FFT。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9b2yjsw.png" width="80%">
</div>

### Linear Algebra View

在介绍inverse FFT之前我们先来看一下矩阵视角下的FFT。实际上FFT的过程可以理解为矩阵乘法：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DhASPFa.png" width="80%">
</div>

带入复数的根可以发现系数矩阵还具有一定的结构。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zcvSJ41.png" width="80%">
</div>

因此从矩阵的角度来理解inverse FFT只需要计算系数矩阵的逆阵即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oloFhdm.png" width="80%">
</div>

### Inverse FFT

实际上利用系数矩阵的结构可以很方便的得到它的逆阵。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CUwXyaR.png" width="80%">
</div>

系数矩阵的这个性质可以证明如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ZHebGuj.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bfvo2gR.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IzXkmlf.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7OqokeB.png" width="80%">
</div>

这样就可以利用FFT来实现inverse FFT：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oHsbHUf.png" width="80%">
</div>

而多项式乘法的完整过程如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/LMrEGBZ.png" width="80%">
</div>

## Reference

- [Divide & Conquer](https://teapowered.dev/assets/ga-notes.pdf#page=22)