---
layout: post
title: OMSCS-GA课程笔记04-Max Flow
date: 2022-10-05
description: 最大流
tags: CS6515-GA
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 高级算法课程([CS 6515: Intro to Grad Algorithms](https://omscs.gatech.edu/cs-6515-intro-graduate-algorithms))的同步课程笔记。课程内容涉及动态规划、随机算法、分治、图算法、最大流、线性规划以及NP完备等高级算法的设计思想和相关应用，本节主要介绍最大流的相关算法。
{: .block-preface }


**最大流(max flow)**问题是图上的经典问题。对于有向图G，从节点s向节点t发送数据时每条边都具有一定的容量，显然我们希望最大化发送的数据量而且在每条边上不要超过相应的容量。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qK5OxwP.png" width="80%">
</div>

最大流问题更严格的定义如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7iDvGLO.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/KbyO2yw.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ltfQPRD.png" width="80%">
</div>

和其它图上的问题相比，最大流问题的特点在于我们允许图上出现环。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UhjVDCO.png" width="80%">
</div>

不过在最大流问题中我们不允许节点之间存在平行的边，即相互指向的边。如果出现这种情况则可以通过在这一对边上添加一个新节点的方式进行处理。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/l8kzaVR.png" width="80%">
</div>

## Ford-Fulkerson Algorithm

求解最大流的基本思想是利用边的容量来构造路径。我们首先初始化每条边的容量为0，然后利用边上剩余的容量来构造一张图并且在这张图上寻找s到t的路径。接着我们计算这条路径上最小的剩余容量，并且把剩余容量填满到路径中。最后不断重复上述过程直到无法构造s到t的路径为止。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/WbKTSHq.png" width="80%">
</div>

上述算法的缺陷在于反向的流量，实际上通过反向的流动我们才能最大化流量。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DxSSsTC.png" width="80%">
</div>

因此我们需要考虑反向的流量，即在原始流量的基础上构造**residual network**。在residual network中图的节点与原始图相同，而边的容量取决于原始图上边的流量。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/BLtDF9O.png" width="80%">
</div>

这样可以得到Ford-Fulkerson算法：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/79EFD7G.png" width="80%">
</div>

Ford-Fulkerson算法的复杂度为$O(mC)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/06De6CP.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7q9sY8S.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QNB9h4y.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5Z4VVCj.png" width="80%">
</div>

## Max-Flow = Min-Cut

### Ford-Fulkerson Recap

在介绍最大流与最小割问题的关系前我们先回顾一下Ford-Fulkerson算法。实际上Ford-Fulkerson算法的正确性依赖于如下定理：当residual network上没有可行的路径时说明已经实现了最大流。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UDGh5SE.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oGwPVVi.png" width="80%">
</div>

### Min-Cut Problem

回忆[图割](/2022/10/04/OMSCS-GA-NOTES-03.html#cuts)的概念是把图划分为两部分L和R，连接它们的边称为cut。对于最大流问题我们可以把图根据源点s和终点t进行划分，此时的图割问题称为**st-cut**。完成划分后L和R的capacity定义为连接它们的所有从L到R的边权重之和。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sC2VTlz.png" width="80%">
</div>

这样我们就可以定义图上的最小割问题，它的目标是寻找具有最小capacity的st-cut。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/C7IKp7V.png" width="80%">
</div>

### Max-Flow = Min ST-Cut

最大流问题的一个重要结论是最大流与最小st-cut的等价性，即最大流的流量等于最小st-cut的capacity。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mUcm7y3.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CRJo8ds.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/urYaORJ.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2d6QAPz.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/t0HdVsD.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7tVF0pA.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zz2sqyy.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fzRf4tf.png" width="80%">
</div>

## Image Segmentation

最大流理论在CV中的一个经典应用在于处理图像分割问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oIhUzn0.png" width="80%">
</div>

我们可以把图像视为一张图，图像中的每一个像素都对应图上的节点，相邻像素通过边进行连接。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/g6eiOxM.png" width="80%">
</div>

同时图上的每个顶点考虑其分类为前景或是背景的似然，而图的每一条边对应进行分割所需支付的代价。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tCWoiJq.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/g6iG8Bx.png" width="80%">
</div>

这样图像分割问题可以定义为图上的一个优化问题，我们希望最大化分类的似然同时减少进行分割必要的代价。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/D0WoPxl.png" width="80%">
</div>

实际上这个优化问题可以被建模为最小割问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2sP9FVN.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5SMOl3f.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/NiDAlGH.png" width="80%">
</div>

这样我们只需求解这个新的最小化问题即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Gr79Nic.png" width="80%">
</div>

更进一步，我们把原始图上的无向边转换成两条单向边，每条边上的capacity等于相应的分割代价。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qK3dPUl.png" width="80%">
</div>

同时，我们为前景和背景再分别设置一个节点表示源点和终点。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Uhq8Pwm.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TtaxyAV.png" width="80%">
</div>

最后我们就可以利用最大流的相关算法来求解图像分割问题，此时图上的最小割(最大流)恰为图像分割的目标函数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/WXgQHsv.png" width="80%">
</div>

总结一下，基于最大流的图像分割算法流程如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/8wYGchY.png" width="80%">
</div>

## Edmonds-Karp Algorithm

除了Ford-Fulkerson算法之外另一个求解最大流问题的经典方法是Edmonds-Karp算法。Edmonds-Karp算法的特点是其计算复杂度只依赖于网络的规模，而与capacity无关。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QGuexb7.png" width="80%">
</div>

Ford-Fulkerson算法的核心步骤在于构造residual network，然后在residual network上寻找起点s到终点t的通路。利用这条通路更新residual network直到网络中不存在任何s到t的路径，此时网络就达到了最大流。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/F0wKJ5w.png" width="80%">
</div>

Edmonds-Karp算法与Ford-Fulkerson算法非常类似，其主要区别在于寻找s到t的路径时必须要使用**广度优先搜索(breadth first search, BSF)**。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nJlSQNr.png" width="80%">
</div>

Edmonds-Karp算法的复杂度为$O(m^2 n)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TbR7Sm9.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RWp01Oa.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vdyQpKZ.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/NkYBsMd.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/W7yDaoH.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fSHC9Wi.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eV9rhA6.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oUR9Nl5.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/EMbvqM2.png" width="80%">
</div>

## Max-Flow Generalization

### Max-Flow with Demands

除了标准最大流之外，最大流问题还有一些变体。我们可以为每条边定义一个非负的量demand，同时希望每条边上的flow在小于等于capacity的基础上还有大于等于demand，此时图上的流称为**feasible flow**。显然我们希望能够最大化feasible flow，但在此之前我们首先需要回答feasible flow是否存在的问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YyYjJxD.png" width="80%">
</div>

### Reduction

基于**归约(reduction)**的思想，我们可以把寻找feasible flow的问题转换为计算最大流，然后使用相关的算法进行求解。具体来说，我们首先把包含capacity和demand的图G转换为一个只包含capacity的图G'，然后在G'上求解最大流问题，最后把G'上的最大流f'转换为原始图G上的feasible flow。因此寻找feasible flow的核心在于设计G和G'之间的转换函数g和h。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/F6aXhBP.png" width="80%">
</div>

首先来考虑G到G'的转换函数g。我们需要把原始边上的capacity减去demand，同时在原始的图上添加新的源点s'和终点t'以及新的边。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eBCI78P.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qHN1zXt.png" width="80%">
</div>

除此之外，我们还需要为t和s之间添加一条具有无穷大capacity的边。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Q7JbibQ.png" width="80%">
</div>

### Saturating Feasible

可以证明当G'上存在saturating flow时原始图G上具有feasible flow。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/WRW1iYw.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/abBdXd2.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/B2OFSId.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/aYp3GcG.png" width="80%">
</div>

### Feasible Saturating

当我们得到了一个feasible flow时，同样可以在G'上构造一个saturating flow。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/clTSbSR.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/OFGScJH.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TTA6Eat.png" width="80%">
</div>

### Max Feasible Flow

因此原始图G存在feasible flow等价于G'具有saturating flow，因此我们只需要在G'上通过最大流算法寻找saturating flow即可。得到了一个feasible flow之后可以通过在residual network上添加augment path的方式来进行最大化，不过需要注意的是此时构造residual network的方式与[Ford-Fulkerson算法](/2022/10/05/OMSCS-GA-NOTES-04.html#ford-fulkerson-algorithm)有所不同。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/oamskOF.png" width="80%">
</div>

## Reference

- [Graphs](https://teapowered.dev/assets/ga-notes.pdf#page=32)
- [网络流问题基础](https://www.youtube.com/watch?v=6DFWUgV5Osc&list=PLvOO0btloRnsbnIIbX6ywvD8OZUTT0_ID&index=8)
- [Ford-Fulkerson Algorithm 寻找网络最大流](https://www.youtube.com/watch?v=8sLON0DqLZo&list=PLvOO0btloRnsbnIIbX6ywvD8OZUTT0_ID&index=9)
- [最小割 Min-Cut](https://www.youtube.com/watch?v=Ev_lFSIzNh4&list=PLvOO0btloRnsbnIIbX6ywvD8OZUTT0_ID&index=12)
- [Edmonds–Karp Algorithm 寻找网络最大流](https://www.youtube.com/watch?v=l-5W0ffPsDo&list=PLvOO0btloRnsbnIIbX6ywvD8OZUTT0_ID&index=10&ab_channel=ShusenWang)