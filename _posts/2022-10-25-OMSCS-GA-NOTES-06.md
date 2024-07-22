---
layout: post
title: OMSCS-GA课程笔记06-NP Completeness
date: 2022-10-25
description: NP完备理论
tags: CS6515-GA
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 高级算法课程([CS 6515: Intro to Grad Algorithms](https://omscs.gatech.edu/cs-6515-intro-graduate-algorithms))的同步课程笔记。课程内容涉及动态规划、随机算法、分治、图算法、最大流、线性规划以及NP完备等高级算法的设计思想和相关应用，本节主要介绍NP完备理论。
{: .block-preface }


## Computational Complexity

在本节课中我们会介绍**NP完备(NP-completeness)**的概念、P=NP以及P≠NP的意义、以及如何证明一个问题是**难以求解(intractable)**的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GP0NR3H.png" width="80%">
</div>

### Complexity Classes

我们定义NP问题是所有**搜索(search)**问题的集合，这里搜索是指可以在多项式时间内对它的解进行验证的问题。而P问题则是指可以在多项式时间内进行求解的搜索问题集合。显然P问题是NP问题的一个子集，这是因为能在多项式时间内求解的问题必然能在多项式时间内进行验证。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DUw1b0Q.png" width="80%">
</div>

因此P=NP是指验证一个问题和求解这个问题具有相同的难度，而P≠NP则是指求解一个问题要比验证这个问题要困难一些。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yWyTnxY.png" width="80%">
</div>

### Search Problem

对于搜索问题，我们可以形式化地定义如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/I0M3Iv8.png" width="80%">
</div>

#### SAT

我们在前面课程中介绍的[SAT问题](/2022/10/04/OMSCS-GA-NOTES-03.html#sat-problem)就是一个NP问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ciU96OZ.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/PIfCD67.png" width="80%">
</div>

#### GCP

**图着色问题(graph coloring problem, GCP)**是图论中的经典问题，对于给定的k种颜色我们希望可以保证着色后相邻节点有着不同的颜色。显然GCP同样也是NP问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/xG7fwDS.png" width="80%">
</div>

#### MST

[最小生成树](/blog/2022/OMSCS-GA-NOTES-03/#minimum-spanning-tree)同样是NP问题，实际上它还是P问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lp1UxVR.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Q6CKnrb.png" width="80%">
</div>

#### Knapsack

而[knapsack问题](/blog/2022/OMSCS-GA-NOTES-01/#knapsack)可以利用动态规划进行求解，但目前人们无法证明knapsack问题是否是一个NP问题，也无法证明它是否是一个P问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/AN8ou24.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GGQz3Mi.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/SyhJz94.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/be0Hlv5.png" width="80%">
</div>

不过knapsack问题存在一个变体，我们引入一个新的变量g同时要求背包内物品的价值之和至少为g。此时的knapsack问题则是一个NP问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tk0usPq.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qClPNE1.png" width="80%">
</div>

### NP-Completeness

总结一下，在计算复杂度理论中P是指多项式时间(polynomial time)，而NP则是指nondeterministic polynomial time。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ekjOJc8.png" width="80%">
</div>

由于P是NP的子集，我们关心的是是否每个NP问题同样是P问题，即P是否等于NP。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/W7gCrui.png" width="80%">
</div>

如果P≠NP那么在NP问题中一定存在一些难以求解的问题，称为**NP完备(NP-completeness)**问题。用Venn图来表示的话NP完备问题是NP问题和P问题的差集。NP完备问题是NP问题中最困难的问题，显然如果P≠NP那么NP完备问题一定不是P问题。它的逆否命题则说明如果能够找到一个NP完备问题在多项式时间内的解，那么所有NP问题都是P问题，即P=NP。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/esCVci4.png" width="80%">
</div>

如果我们承认P≠NP，那么上面提到的SAT问题就是一个典型的NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ZF34chs.png" width="80%">
</div>

### Reductions

要证明一个问题是NP完备需要使用**归约(reduction)**的技巧。具体来说对于两个问题A和B，我们需要证明求解问题A可以转换为求解问题B，或者求解问题A最多相当于求解问题B，即求解B问题一定可以求解出A问题。如果我们可以在多项式时间内解决B问题，那么一定可以在多项式时间内解决A问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/iYR0qxW.png" width="80%">
</div>

假设问题A为图着色问题，问题B为SAT问题。利用归约可以证明图着色问题为NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sD6Xuta.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ul4S1vS.png" width="80%">
</div>

总结一下证明某个问题是NP完备问题需要两步：

- 证明该问题是NP问题
- 证明所有NP问题可以归约到该问题

由于我们已知SAT问题是NP完备问题，因此对于第二步可以通过归约将SAT归约到所需问题上来进行证明。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/wdFDqAt.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DUovPSe.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4uDUir0.png" width="80%">
</div>

## 3SAT

这一节我们开始证明3SAT问题是NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yRgz4FU.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4mh461I.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/B3kZ8oa.png" width="80%">
</div>

首先我们需要证明3SAT是NP问题。显然验证3SAT的解只需要$$O(m)$$的时间复杂度，因此3SAT是NP问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Am3iGQn.png" width="80%">
</div>

接下来证明SAT可以归约到3SAT上，具体来说我们需要证明任意SAT问题可以转换为求解一个3SAT问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/waCrPMC.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JA1bOeJ.png" width="80%">
</div>

假设有一个包含4个变量的从句$$C$$：

$$
C = ( \bar{x}_2 \vee x_3 \vee \bar{x}_1 \vee \bar{x}_4 )
$$

我们定义一个新的从句$$C'$$，它由2个只包含3个变量的从句构成：

$$
C' = ( \bar{x}_2 \vee x_3 \vee y ) \wedge ( \bar{y} \vee \bar{x}_1 \vee \bar{x}_4 )
$$

可以证明$$C$$和$$C'$$是等价的，这样一个4SAT问题就转换成了3SAT。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/1ztyGfr.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lGxX2C8.png" width="80%">
</div>

实际上对于包含k个变量的从句$$C$$我们都可以把它转换为只包含3个变量的从句$$C'$$：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IUVHmN5.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/HdJHynK.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Iam7DFu.png" width="80%">
</div>

这样就可以把SAT问题归约到3SAT上，即3SAT是NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/H6Eq1ne.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/uLkrjbb.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dHSydop.png" width="80%">
</div>

## Graph Problems

很多图上的问题都是NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zsRyUzK.png" width="80%">
</div>

### Independent Sets

**独立集(independent set, IS)**是指在图上没有公共边的顶点集合。寻找小的独立集比较容易，但寻找大的独立集要困难很多。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RkIYGFc.png" width="80%">
</div>

实际上寻找图上的最大独立集不是NP问题。不过我们可以对它进行一些修正，只要求独立集中顶点的数量大于g，此时的独立集问题是一个NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5IwJMpY.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CDoAEmo.png" width="80%">
</div>

显然此时的独立集问题可以在多项式时间内进行验证，这说明它是一个NP问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fw9J5qb.png" width="80%">
</div>

接下来证明3SAT问题可以归约到独立集问题上。首先我们把每个从句转换成图上的一个团，然后为任意包含互斥变量的顶点对添加一条边。这样求解3SAT问题就转换为了在图上寻找独立集的问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FBc1mn9.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zEToDmk.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zxnJa1j.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zxnJa1j.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TzHaLya.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5R1RMHy.png" width="80%">
</div>

这样我们就证明了独立集问题是一个NP完备问题。回到最大独立集的问题，利用归约可以证明最大独立集问题至少和独立集问题一样难。因此对于最大独立集问题我们称其为**NP-hard**，表示它至少和任意NP问题一样困难。如果能够找到NP-hard问题的多项式时间解法，那么所有的NP问题都可以在多项式时间内解决。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Xiancwk.png" width="80%">
</div>

### Clique

图上的**团(clique)**是指图上相互连接的一组节点，而最大团问题则是指寻找图上最大的一个团。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/HMLQDg6.png" width="80%">
</div>

类似于独立集问题，我们同样先构造一个关于团的搜索问题。此时要求团的大小至少为g，而这个团搜索问题实际上也是一个NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/iuQavzq.png" width="80%">
</div>

显然团搜索问题是一个NP问题，我们只需要验证团中的每一对顶点是否相连即可。而在第二步则可以将独立集问题归约到团搜索上来进行证明，实际上独立集与团搜索是相互对立的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7WR1I4K.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/wweB0xp.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/BLVIaTA.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/0aG2ZhR.png" width="80%">
</div>

### Vertex Cover

**顶点覆盖(vertex cover, VC)**问题是指在图上寻找一组顶点使得与它们相连的边可以覆盖到图上的所有边。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ZfOMtDC.png" width="80%">
</div>

我们同样把VC问题修改为满足一定约束的搜索问题，此时要求顶点集合的大小最多为b。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cZvuCXH.png" width="80%">
</div>

可以证明此时的VC搜索问题是一个NP完备问题。首先容易证明验证VC搜索问题的一个解只需要多项式时间，然后可以把独立集问题归约到VC搜索上。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2jxSXEF.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/idWmhE0.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/sSWJV2V.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mZs8oqN.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/pcrEUH5.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/j9hUESX.png" width="80%">
</div>

## Knapsack

前面我们利用SAT证明了3SAT以及IS都是NP完备问题，而在这一小节中我们会证明knapsack同样是NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qUJVnsF.png" width="80%">
</div>

### Subset-Sum

首先我们需要证明subset-sum是NP完备问题。subset-sum问题是指在正整数集合$$S = \{ a_1, ..., a_n \}$$中找出和为$$t$$的一个子集。显然这个问题可以使用动态规划进行求解，它的复杂度为$$O(nt)$$。不过需要注意的是类似于knapsack问题，subset-sum不是一个P问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/GmN1cwP.png" width="80%">
</div>

接下来我们开始证明subset-sum是NP完备问题。显然我们可以在多项式时间内验证subset-sum的解，因此它是一个NP问题；然后利用归约可以证明3SAT会归约到subset-sum上，这样就证明了subset-sum是NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XvMxKy0.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Q532gfL.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/kkc1IIB.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RadW3E1.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/T0Fovw2.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bUumj7N.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JQgV3Do.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TIz8A2k.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RIGcn1v.png" width="80%">
</div>

在subset-sum的基础上可以证明Knapsack是NP完备问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/BMctfAn.png" width="80%">
</div>

## Halting Problem

### Undecidable

NP完备问题是在计算上非常困难的一类问题。如果我们承认P≠NP，那么NP完备问题不存在对于任意输入的多项式时间解法。而**不可判定问题(undecidable problem)**则是更加困难的问题，对于任意输入不可判定问题即使给予无限的时间和空间都不存在求解的算法。图灵在1936年指出**停机问题(halting problem)**就是这样的一个不可判定问题。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/gbPihU3.png" width="80%">
</div>

### Halting Problem

停机问题是指判断一个程序是否能够在有限时间内终止。对于给定程序P和输入I，停机问题要求在P能够终止的情况下输出True，否则输出False。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/u7JsZOz.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Oe5EjuY.png" width="80%">
</div>

接下来我们利用反证法来证明停机问题是不可判定问题：假设停机问题存在算法`TERMINATOR(P, I)`进行求解，我们可以构造一个新的程序Q以及输入J使得`TERMINATOR`无法给出正确的解。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/hgbcP8O.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tuLU5O4.png" width="80%">
</div>

显然如此构造的程序`Harmful(J)`在`TERMINATOR(J, J)`为True时会无限循环下去，而在`TERMINATOR(J, J)`为False时终止运行。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/695BpfT.png" width="80%">
</div>

最后令J为所定义的`Harmful`，此时就产生了一个悖论：

- 如果`Harmful(J)`能够终止，带入`J=Harmful`会得到`Harmful(Harmful)`无法终止。
- 如果`Harmful(J)`不能终止，带入`J=Harmful`会得到`Harmful(Harmful)`可以终止。

这样我们就利用反证法证明了停机问题不存在任何解法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Jxwwr6t.png" width="80%">
</div>

## Reference

- [Computational Complexity](https://teapowered.dev/assets/ga-notes.pdf#page=82)
- [NP reduction from subset sum to Knapsack](https://www.youtube.com/watch?v=pK8VQd6U7BI)