---
layout: post
title: OMSCS-SIM课程笔记09-Output Analysis
date: 2022-04-05
description: 输出分析
tags: ISYE6644-SIM
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 仿真和建模课程([ISYE 6644: Simulation and Modeling for Engineering and Science](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science))的同步课程笔记。课程内容涉及计算机模拟在统计分析和建模中的应用，本节介绍对随机模拟的输出结果进行分析的方法。
{: .block-preface }


## Introduction

回忆进行仿真的流程，当我们完成了试验设计并且进行了模拟后就需要对收集到的数据进行分析。

<div align=center>
<img src="https://i.imgur.com/8VIDnKq.png" width="80%">
</div>

对输出结果进行分析的原因之一在于我们在进行模拟时的输入是随机的，因此系统的输出也必然是随机变量。这样就需要考虑采样误差对于系统输出结果的影响。

<div align=center>
<img src="https://i.imgur.com/ig99nEc.png" width="80%">
</div>

对随机输出结果进行分析的常用指标包括均值、方差、分位数等，同时我们也会使用点估计或是置信区间的方法来进行分析。

<div align=center>
<img src="https://i.imgur.com/PGfzLvg.png" width="80%">
</div>

输出结果分析的难点在于样本之间往往是不满足独立同分布假定的。以排队问题为例，相邻用户之间的等待时间显然是相互关联的。在这种情况下之间套用一些常用的估计方法会导致额外偏差。

<div align=center>
<img src="https://i.imgur.com/t3ME1kT.png" width="80%">
<img src="https://i.imgur.com/GyPIrFg.png" width="80%">
</div>

总之，我们在对仿真结果进行分析时需要考虑样本之间的相互影响。直接使用经典统计分析的方法可能是不合适的。

<div align=center>
<img src="https://i.imgur.com/0mjwa9M.png" width="80%">
</div>

在本节课中我们将主要介绍两种分析方法：其一是**finite-horizon simulation**，它比较适合分析系统短期的行为；另一种是**steady-state analysis**，它则更关注系统的长期行为。

<div align=center>
<img src="https://i.imgur.com/X2aDH6w.png" width="80%">
<img src="https://i.imgur.com/YoqZ0bI.png" width="80%">
</div>

## A Mathematical Interlude

在正式介绍具体的结果分析方法前我们首先来看一下当样本之间存在相关性时会产生什么样的问题。这里我们假设样本具有相同的分布，但并不是相互独立的。

<div align=center>
<img src="https://i.imgur.com/MvYpTwQ.png" width="80%">
</div>

在这种情况下样本均值$$E[\bar{Y}_n]$$仍然等于分布的均值$$\mu$$，我们考虑它的方差可以得到如下公式：

<div align=center>
<img src="https://i.imgur.com/Wyqf0sE.png" width="80%">
<img src="https://i.imgur.com/SNOxW1V.png" width="80%">
<img src="https://i.imgur.com/0u5zuCq.png" width="80%">
</div>

当样本之间相互独立时，参数$$\sigma^2$$恰好等于分布的方差。但在样本之间相互关联的情况下$$\sigma^2$$往往会远大于真实的方差，这会导致在计算置信区间时产生非常大的误差。

<div align=center>
<img src="https://i.imgur.com/FeXCHqh.png" width="80%">
</div>

举一个具体的例子，我们假设样本由自回归过程产生。在这种条件下样本的均值会有非常巨大的方差。

<div align=center>
<img src="https://i.imgur.com/FyAJqeS.png" width="80%">
</div>

当我们使用样本均值对分布的其它参数进行估计时，样本之间的相关性同样会产生巨大的影响。以样本方差为例，如果我们直接套用独立同分布条件下的样本方差公式则会导致样本方差的期望远小于样本均值的方差。

<div align=center>
<img src="https://i.imgur.com/QqHoGcs.png" width="80%">
<img src="https://i.imgur.com/bmtWNc9.png" width="80%">
<img src="https://i.imgur.com/0fEj2DA.png" width="80%">
</div>

如果错误地使用了样本方差公式则会计算出远小于真实情况下的置信区间。

<div align=center>
<img src="https://i.imgur.com/exreS2E.png" width="80%">
</div>

## Finite-Horizon Simulation

### Finite-Horizon Analysis

finite-horizon simulation的目的是确定系统在相对短期内的行为。

<div align=center>
<img src="https://i.imgur.com/SV3I1g4.png" width="80%">
<img src="https://i.imgur.com/wTpPJY2.png" width="80%">
</div>

其中，最简单的任务是使用样本均值来估计观测的结果。但需要注意的是由于样本之间不是独立同分布的，我们不能直接使用样本方差来来估计总体的方差。

<div align=center>
<img src="https://i.imgur.com/R0VTudP.png" width="80%">
<img src="https://i.imgur.com/ZZgxjwJ.png" width="80%">
</div>

要解决这个问题也很简单，我们只需要进行**独立重复试验(independent replications, IR)**即可。

<div align=center>
<img src="https://i.imgur.com/QFZXu1I.png" width="80%">
</div>

通过独立重复试验，我们可以得到满足独立同分布假定的观测均值$$Z_i$$，此时就可以使用标准的参数估计方法来估计总体方差了。

<div align=center>
<img src="https://i.imgur.com/URqJmm4.png" width="80%">
<img src="https://i.imgur.com/JY6uHPN.png" width="80%">
</div>

当每次试验中的观测数足够大时我们还可以使用中心极限定理来估计均值的置信区间。

<div align=center>
<img src="https://i.imgur.com/34nb4sJ.png" width="80%">
</div>

### Finite-Horizon Extensions

在独立重复试验的基础上如果想要得到一个更紧的置信区间则可以考虑增加试验的次数。可以证明区间的半径与试验次数成平方反比关系，换句话说如果想要把区间半径减小一半需要4倍次数的试验。

<div align=center>
<img src="https://i.imgur.com/j5202pb.png" width="80%">
</div>

另一方面，我们还可以去估计均值以外其它统计量的置信区间。以分位数为例，我们可以推导出分位数的置信区间如下：

<div align=center>
<img src="https://i.imgur.com/hRJR82X.png" width="80%">
<img src="https://i.imgur.com/Qipo6XC.png" width="80%">
<img src="https://i.imgur.com/4ohSGjj.png" width="80%">
<img src="https://i.imgur.com/HWFpkmT.png" width="80%">
<img src="https://i.imgur.com/cWd4uj6.png" width="80%">
<img src="https://i.imgur.com/Nj3SuEq.png" width="80%">
</div>

## Initialization Problems

在进行仿真前我们还需要考虑系统的初始化问题。实际上初始化也会对最终的仿真结果产生影响，对于steady-state analysis而言初始化的过程尤为重要。

<div align=center>
<img src="https://i.imgur.com/wLSRutX.png" width="80%">
<img src="https://i.imgur.com/drrF2Ej.png" width="80%">
</div>

检验初始化效果的常用方法如下：

<div align=center>
<img src="https://i.imgur.com/9yBjPtc.png" width="80%">
</div>

当我们检测到初始化过程存在误差时，可以通过截断或是延长试验时间的方式来修正误差。

<div align=center>
<img src="https://i.imgur.com/MWT6Ryl.png" width="80%">
<img src="https://i.imgur.com/Z0uan4f.png" width="80%">
<img src="https://i.imgur.com/fgHcmNs.png" width="80%">
</div>

## Steady-State Analysis

### Batch Means

在steady-state analysis中我们关心系统的长期行为，这里我们借用finite-horizon simulation中独立重复试验的一些概念来进行参数估计。

<div align=center>
<img src="https://i.imgur.com/2mi25t2.png" width="80%">
<img src="https://i.imgur.com/TaZoD3R.png" width="80%">
</div>

在估计观测的均值时，我们把整个观测序列划分成若干个batch。当batch的长度足够大时可以认为每个batch上的均值服从独立同分布假设。

<div align=center>
<img src="https://i.imgur.com/vtfpsBC.png" width="80%">
<img src="https://i.imgur.com/FHo7eTR.png" width="80%">
</div>

类似于独立重复试验，我们可以得到参数$$\sigma^2$$的估计如下：

<div align=center>
<img src="https://i.imgur.com/6Mc8yZc.png" width="80%">
</div>

进一步的分析可以发现$$\hat{V}_B$$并不是一个无偏估计，而是渐进无偏的。同样地，我们可以得到$$\hat{V}_B$$的方差。

<div align=center>
<img src="https://i.imgur.com/a47WvJY.png" width="80%">
<img src="https://i.imgur.com/FgRQgf3.png" width="80%">
</div>

这样，就可以利用MSE得到$$\hat{V}_B$$总体的误差：

<div align=center>
<img src="https://i.imgur.com/EltPkGG.png" width="80%">
</div>

通过最小化MSE我们可以得到MSE的最优值：

<div align=center>
<img src="https://i.imgur.com/VSJgMzN.png" width="80%">
<img src="https://i.imgur.com/vTzf2zi.png" width="80%">
</div>

我们同样可以计算均值$$\mu$$的置信区间，从形式上看它和finite-horizon simulation并没有什么区别。但需要注意的是finite-horizon simulation是通过独立重复试验来进行计算，而steady-state analysis则是把一个长的序列划分为batch进行计算。

<div align=center>
<img src="https://i.imgur.com/Yvxrspj.png" width="80%">
</div>

置信区间的一些常用性质如下：

<div align=center>
<img src="https://i.imgur.com/G90FgBs.png" width="80%">
<img src="https://i.imgur.com/xonIMqX.png" width="80%">
<img src="https://i.imgur.com/oVb6PGy.png" width="80%">
</div>

### Other Methods

batch means并不是steady-state analysis的唯一方法，实际上有些方法有着更好的性质。比如说我们可以把独立重复试验应用在steady-state analysis中，然后在每次试验时再进行batch mean来获得更稳定的估计。

<div align=center>
<img src="https://i.imgur.com/KqEI3tS.png" width="80%">
</div>

另一种估计方法是使用有重叠的batch来进行估计。可以证明当序列和batch数量比较大时，有重叠的batch会比无重叠的估计方法有更好的性能。

<div align=center>
<img src="https://i.imgur.com/oPaowkj.png" width="80%">
<img src="https://i.imgur.com/aEXYlDB.png" width="80%">
<img src="https://i.imgur.com/xKn4v2p.png" width="80%">
</div>

除了上面介绍的两种方法之外还有一些更加复杂的估计方法。

<div align=center>
<img src="https://i.imgur.com/DDReSaG.png" width="80%">
<img src="https://i.imgur.com/V7l70gX.png" width="80%">
<img src="https://i.imgur.com/MrFuF3t.png" width="80%">
</div>

## Reference

- [Output Analysis](https://studio.edx.org/assets/courseware/v1/ddb0b74e23388195b0ac265c61c7c3c7/asset-v1:GTx+ISYE6644x+2T2019+type@asset+block/Module09-OutputAnalysis_171019.pdf)