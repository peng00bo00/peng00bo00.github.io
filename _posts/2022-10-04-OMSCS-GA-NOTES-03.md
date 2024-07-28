---
layout: post
title: OMSCS-GA课程笔记03-Graph Algorithms
date: 2022-10-04
description: 图算法
tags: CS6515-GA
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 高级算法课程([CS 6515: Intro to Grad Algorithms](https://omscs.gatech.edu/cs-6515-intro-graduate-algorithms))的同步课程笔记。课程内容涉及动态规划、随机算法、分治、图算法、最大流、线性规划以及NP完备等高级算法的设计思想和相关应用，本节主要介绍图上的相关算法。
{: .block-preface }


图是一种重要的数据结构，很多现实中的问题都可以抽象为对图的操作。本节课我们首先从图的遍历入手，在介绍图遍历的基础上推导图上的其它算法。

<div align=center>
<img src="https://i.imgur.com/45uIrUu.png" width="80%">
</div>

## Strongly Connected Components

### DFS

**深度优先搜索(depth-first search, DFS)**是对图进行遍历的基本方法。假设图G是一个无向图，通过DFS可以得到图上所有相连的节点。无向图上的DFS算法伪代码如下：

```
DFS(G):
  cc = 0

  for v in V:
    visited[v] = False
	
  for v in V:
    if not visited[v]:
      cc += 1
      Explore(v)

Explore(z):
  ccnum[z] = cc
  visited[z] = True

  for (z, w) in E:
    if not visited[w]:
      Explore(w)
```

DFS的复杂度为$$O(n+m)$$。

<div align=center>
<img src="https://i.imgur.com/hz0Sfru.png" width="80%">
<img src="https://i.imgur.com/cg2ss85.png" width="80%">
</div>

DFS是很多更高级图算法的基础。除了计算联通节点之外还可以使用DFS来计算联通节点之间的路径，不过此时需要对DFS算法进行一些修改，在进行遍历时使用`prev[v]`来记录当前节点的上一个节点：

```
DFS(G):
  cc = 0

  for v in V:
    visited[v] = False
    prev[v] = None
	
  for v in V:
    if not visited[v]:
      cc += 1
      Explore(v)

Explore(z):
  ccnum[z] = cc
  visited[z] = True

  for (z, w) in E:
    if not visited[w]:
      Explore(w)
      prev[w] = z
```

<div align=center>
<img src="https://i.imgur.com/cBT2xKm.png" width="80%">
</div>

对于有向图我们同样可以使用DFS来计算联通节点。不过此时我们需要引入`pre`和`post`两个数组来记录节点的访问顺序：

```
DFS(G):
  clock = 1

  for v in V:
    visited[v] = False

  for v in V:
    if not visited[v]:
      Explore(v)

Explore(z):
  pre[z] = clock
  clock += 1

  visited[z] = True

  for (z, w) in E:
    if not visited[w]:
      Explore(w)

  post[z] = clock
  clock += 1
```

<div align=center>
<img src="https://i.imgur.com/qPUkH4f.png" width="80%">
</div>

### Types of Edges

在有向图上使用DFS相当于把图上的节点按照树进行展开。

<div align=center>
<img src="https://i.imgur.com/Wna4aoz.png" width="80%">
</div>

根据`pre`和`post`两个数组上记录的值可以对原始图上的边进行分类：对于有向边`(z, w)`，树上的反向边会出现`post[z] < post[w]`，而其它情况下则满足`post[z] > post[w]`。

<div align=center>
<img src="https://i.imgur.com/wfgAMUt.png" width="80%">
</div>

### Topological Sorting

当图G的DFS tree存在反向边时称图上存在环。

<div align=center>
<img src="https://i.imgur.com/4WU5Y7S.png" width="80%">
</div>

如果有向图G上没有环则称其为一个**有向无环图(directed acyclic graph, DAG)**。对于DAG来说我们可以根据访问顺序对图上的节点进行排序，使得排序后的节点都是从低序号到高序号。具体来说，只需要使用DFS来计算节点顺序然后按照`post`数组的逆序进行排序即可。

<div align=center>
<img src="https://i.imgur.com/6v252OC.png" width="80%">
</div>

这里我们额外需要说明的是拓扑排序后的图上会有两类节点，其中一类没有边进入称为source vertex，还有一类节点没有指向其它节点的边称为sink vertex。对于DAG来说，图上至少有一个source vertex和一个sink vertex。

<div align=center>
<img src="https://i.imgur.com/AqFf4zk.png" width="80%">
</div>

同时注意到source vertex总是具有最大的post数值而sink vertex总是具有最小的post数值，因此另一种可行的拓扑排序算法是不断寻找图上的sink vertex然后删除。

<div align=center>
<img src="https://i.imgur.com/AqFf4zk.png" width="80%">
</div>

### Connectivity

图上的**强联通分量(strongly connected components, SCC)**是图上节点的集合，其中集合内的任意一对节点v和w之间存在相互联通的路径。

<div align=center>
<img src="https://i.imgur.com/ztjYvxm.png" width="80%">
</div>

以下图为例，图上存在5个强连通分量。

<div align=center>
<img src="https://i.imgur.com/2XU5zQj.png" width="80%">
</div>

有向图的一个重要性质在于其SCC之间构成一个DAG，称为metagraph。

<div align=center>
<img src="https://i.imgur.com/iOeu7my.png" width="80%">
</div>

因此在有向图上计算SCC的基本思想在于利用metagraph是一个DAG的性质。我们可以尝试寻找metagraph上的sink SCC然后从图上删除掉，不断重复这样的过程直至图为空。由于选择的是sink SCC，从其中任意一个节点进行访问都只能访问到SCC内的其它节点。

<div align=center>
<img src="https://i.imgur.com/G6qcX2j.png" width="80%">
</div>

那么如何取寻找sink SCC呢？最直观的想法是从`post`数组中选择具有最小数值的那个节点。但遗憾的是具有最小`post`数值的节点不一定位于一个sink SCC中。

<div align=center>
<img src="https://i.imgur.com/GeWMsIQ.png" width="80%">
</div>

实际上具有最大`post`数值的节点一定位于一个source SCC中。

<div align=center>
<img src="https://i.imgur.com/sEvfCZR.png" width="80%">
</div>

因此计算SCC的第一步在于反转图上的边，然后在反转图上寻找source SCC。此时找的source SCC在原始图上是一个sink SCC。

<div align=center>
<img src="https://i.imgur.com/L6P5ANX.png" width="80%">
</div>

这样有向图上计算SCC的算法主要流程为：

1. 反转边的指向得到反转图
2. 在反转图上使用DFS得到节点排序
3. 在原始图上利用排序对节点进行遍历

<div align=center>
<img src="https://i.imgur.com/PXb3YCa.png" width="80%">
<img src="https://i.imgur.com/0uJGpBQ.png" width="80%">
<img src="https://i.imgur.com/iLodiQG.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/x1hJqVe.png" width="80%">
</div>

当然计算SCC算法依赖于最大`post`数值的节点位于source SCC中这一性质。

<div align=center>
<img src="https://i.imgur.com/UKjcCwk.png" width="80%">
<img src="https://i.imgur.com/fdZfGpg.png" width="80%">
</div>

## 2-Satisfiability

### SAT Problem

SCC算法的一个应用是求解**布尔可满足性问题(satisfiability, SAT)**。在SAT问题中有n个布尔变量，我们希望能够找到满足语句要求的变量值：

<div align=center>
<img src="https://i.imgur.com/oi66qd5.png" width="80%">
</div>

从更严格的角度来说，对于包含n个布尔变量和m个从句的语句我们希望能够找到一个对布尔变量的赋值使得语句的布尔值为True。

<div align=center>
<img src="https://i.imgur.com/jm9w1CR.png" width="80%">
</div>

### k-SAT

根据每个从句的规模我们可以定义k-SAT问题，其中每个从句包含最多k个布尔变量。对于k-SAT问题人们已经证明在k大于2的情况下它是[NP-complete](/blog/2022/OMSCS-GA-NOTES-06/#3sat)，而对于2-SAT问题则存在多项式复杂度的解法。

<div align=center>
<img src="https://i.imgur.com/bs0xoCX.png" width="80%">
</div>

对于2-SAT问题我们可以简化语句，首先满足只包含1个变量的从句然后从整个语句中删除它。

<div align=center>
<img src="https://i.imgur.com/W8Pq29A.png" width="80%">
</div>

通过不断地简化我们最终会得到一个新的语句。显然两个语句的可满足性是等价的，因此只需要验证新的语句是否是可满足的即可。

<div align=center>
<img src="https://i.imgur.com/uR6qiDX.png" width="80%">
</div>

### Graph of Implications

语句化简后我们可以把它等价地表示为一张图，称为**graph of implications**。具体地，我们把n个变量的布尔取值表示为图上的节点，而m个从句则可以表示为2条对应的边。

<div align=center>
<img src="https://i.imgur.com/yNMISSg.png" width="80%">
</div>

如果语句是可满足的，那么graph of implications上不能存在任意变量$$x$$到$$\bar{x}$$的路径，否则会产生矛盾。换句话说，如果$$x$$和$$\bar{x}$$位于同一个SCC中，则语句是无法满足的。

<div align=center>
<img src="https://i.imgur.com/OXvX2Gn.png" width="80%">
</div>

### SCC's

这样我们就可以利用SCC的算法来解决2-SAT问题，如果每一对布尔变量都位于不同的SCC中则说明语句是可以满足的。

<div align=center>
<img src="https://i.imgur.com/BSbd4nA.png" width="80%">
</div>

首先我们在图上寻找一个sink SCC，记为$$S$$。然后满足$$S$$中的所有条件，从图上删除SCC并不断重复上面的过程。这个算法的缺陷在于没有考虑S的补集$$\bar{S}$$，当$$S$$得到满足时$$\bar{S}$$可能是无法满足的。

<div align=center>
<img src="https://i.imgur.com/wjwNeSz.png" width="80%">
</div>

为了解决这样的问题，我们可以考虑在图上寻找一个source SCC，记为$$S'$$。然后把$$S'$$设为False，并且从图上删除$$S'$$。最后寻找$$S'$$的补集$$\bar{S'}$$，其必为一个sink SCC，然后将其设为True即可。

<div align=center>
<img src="https://i.imgur.com/Z5nvgok.png" width="80%">
</div>

### 2-SAT Algorithm

总结一下，利用SCC算法来求解2-SAT问题的方法如下：

<div align=center>
<img src="https://i.imgur.com/tWsP35X.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/zlSxQ7q.png" width="80%">
<img src="https://i.imgur.com/05RJdi8.png" width="80%">
<img src="https://i.imgur.com/Y2hDAv0.png" width="80%">
</div>

## Minimum Spanning Tree

### Greedy Approach

**贪心(greedy)**是经典的算法设计思想，在贪心算法中我们把整个问题分解为若干个子问题然后通过最优化每一个子问题来试图优化原始问题。当然并不是所有的问题都可以使用贪心这样的方法来进行优化，比如说[knapsack问题](/blog/2022/OMSCS-GA-NOTES-01/#knapsack)就无法通过贪心来进行求解。

<div align=center>
<img src="https://i.imgur.com/cYqRFpX.png" width="80%">
</div>

### MST Problem

**最小生成树(minimum spanning tree, MST)**是图上的经典问题，我们希望对于无向图G可以找到一个最小的子图来包含原始图的所有节点。显然这个最小的子图一定是一个树结构，称为最小生成树。

<div align=center>
<img src="https://i.imgur.com/SgyiwM6.png" width="80%">
</div>

在介绍计算MST的算法前我们先来回顾一下树这种数据结构的性质。首先树是一个DAG，而且对于包含n个节点的树其边的数量为n-1，同时树上任意两个节点之间有且只有一条路径连接它们。关于树最重要的一条性质是如果一张图的边和节点数满足$$\vert E \vert = \vert V \vert - 1$$，那么它一定是一棵树。

<div align=center>
<img src="https://i.imgur.com/Tl0RmUX.png" width="80%">
</div>

### Greedy Approach for MST

MST可以使用贪心的思想来进行计算。我们可以不断地在添加图上具有最小权重的边，同时保证生成的图是一棵树即可。这种计算MST的算法称为**Kruskal算法(Kruskal's algorithm)**，它的复杂度为$$O(m \log{n})$$。

<div align=center>
<img src="https://i.imgur.com/WYYmazs.png" width="80%">
<img src="https://i.imgur.com/ZJvv0Pc.png" width="80%">
</div>

Kruskal算法的正确性则可以使用归纳法来进行证明。

<div align=center>
<img src="https://i.imgur.com/ZaYKVJ3.png" width="80%">
</div>

### Cuts

我们还可以借助**图割(cut)**的概念来处理MST问题。cut是指把图上的节点分为两个集合$$S$$和$$\bar{S}$$，其中连接两个集合的边称为cut edge。

<div align=center>
<img src="https://i.imgur.com/2lASuxt.png" width="80%">
</div>

基于cut的概念，Kruskal算法可以理解为不断地寻找当前图上的最小cut edge。

<div align=center>
<img src="https://i.imgur.com/vndNPfm.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/02FNcZr.png" width="80%">
<img src="https://i.imgur.com/FEgdwaZ.png" width="80%">
<img src="https://i.imgur.com/UXHDbG7.png" width="80%">
<img src="https://i.imgur.com/AlAzKMz.png" width="80%">
</div>

### Prim's Algorithm

除了Kruskal算法之外，**Prim算法(Prim's algorithm)**同样是计算MST的经典算法。

<div align=center>
<img src="https://i.imgur.com/3G3NeeJ.png" width="80%">
</div>

## Markov Chains and PageRank

**PageRank**是图算法的重要应用，早期Google的搜索引擎就是基于PageRank来实现的。要理解PageRank还需要引入**Markov链(Markov chain)**的概念，很多工程问题都会使用Markov链来进行建模和求解。

<div align=center>
<img src="https://i.imgur.com/3M5A8dA.png" width="80%">
</div>

### Markov Chains

Markov链是一种概率模型，它可以表示为一张有向图。此时图上的顶点表示系统可能的状态，图上的边和权重分别表示状态转移的方向和概率。具体来说，节点$$i$$到$$j$$的有向边权重表示系统从状态$$i$$转移到状态$$j$$的概率：

$$
P(i, j) = Pr(j \vert i)
$$

同时它还要满足归一化条件：

$$
\sum_j P(i, j) = 1, \ \forall i
$$

$$
0 \leq P(i, j) \leq 1, \ \forall i, j
$$

<div align=center>
<img src="https://i.imgur.com/Yw87Ki4.png" width="80%">
</div>

实际上Markov链的状态转移概率还可以表示为一个矩阵，称为**状态转移矩阵(transition matrix)**。它的每个元素即为上面定义的状态转移概率，因此每一行都具有归一性。

<div align=center>
<img src="https://i.imgur.com/q8TKUrP.png" width="80%">
</div>

#### k-Step Transition

如果我们忽略状态转移矩阵中具体的概率，将全部非零项设置为1，就能得到图的**邻接矩阵(adjacency matrix)**。邻接矩阵的一个重要性质是它的$$k$$次幂$$A^k$$中任意元素$$A^k(i, j)$$表示从$$i$$出发经过$$k$$步移动到$$j$$的路径数量。

<div align=center>
<img src="https://i.imgur.com/2fVoxDt.png" width="80%">
</div>

状态转移矩阵也有类似的性质：$$P^k$$中任意元素$$P^k(i, j)$$表示从$$i$$出发经过$$k$$步移动到$$j$$的概率。

<div align=center>
<img src="https://i.imgur.com/r8AuVFS.png" width="80%">
<img src="https://i.imgur.com/z6kQTic.png" width="80%">
</div>

#### Stationary Distribution

当步数$$k$$比较大时$$P^k$$中每一行都会收敛到同一个行向量上。

<div align=center>
<img src="https://i.imgur.com/TDWtpOC.png" width="80%">
</div>

实际上可以证明当步数趋于无穷时，$$P^k$$中每一行都会收敛到某个向量$$\pi$$上。这表示无论从图上的哪个节点出发，经过足够多的步数后落到状态$$j$$的概率都是$$\pi(j)$$。这个行向量(概率分布)称为Markov链的**平稳分布(stationary distribution)**。

<div align=center>
<img src="https://i.imgur.com/gY9INtW.png" width="80%">
</div>

我们关心Markov链何时具有平稳分布，是否只具有唯一的平稳分布，以及多快可以收敛到平稳分布。

<div align=center>
<img src="https://i.imgur.com/zMDnhDJ.png" width="80%">
</div>

#### Linear Algebra View

从线性代数的角度来看，从初始状态分布$$\mu_0$$通过Markov链进行状态转移相当于行向量$$\mu_0$$与矩阵$$P$$的乘积：

$$
\mu_0 P = \mu_1
$$

对于平稳分布$$\pi$$则有：

$$
\pi P = \pi
$$

即$$\pi$$是矩阵(线性变换)$$P$$的一个不动点，或者说$$\pi$$是矩阵$$P$$对应特征值1的特征向量。实际上1还是矩阵$$P$$的最大特征值，即$$\pi$$是矩阵$$P$$的**主特征向量(principal eigenvector)**。需要额外说明的是矩阵$$P$$可能有多个主特征向量，即平稳分布$$\pi$$可能是不唯一的。

<div align=center>
<img src="https://i.imgur.com/Yl0R0Ox.png" width="80%">
</div>

#### Bipartite Markov Chain

当Markov链具有如下所示的二分图结构时，初始状态会决定最终收敛到的平稳分布。要避免这种情况可以为图上的每个节点添加一个自循环边，这相当于为矩阵$$P$$添加了对角元。我们称此时的Markov链是**非周期(aperiodic)**的。

<div align=center>
<img src="https://i.imgur.com/hT99qlB.png" width="80%">
</div>

#### Multiple SCCs

另一种需要避免的情况是图上具有若干个不同的强连通分量，如果我们移动到某个SCC中时会陷入其中无法离开。因此我们希望图上只有一个强连通分量，这可以为图上的每一对节点赋予有向边来实现，相当于保证任意两个状态都是可以相互转换的。我们称此时的Markov链是**不可约(irreducible)**的。

<div align=center>
<img src="https://i.imgur.com/euSWV3P.png" width="80%">
</div>

#### Ergodic MC

当Markov链满足非周期和不可约条件时称其为**遍历(ergodic)**的，此时的Markov链具有唯一的平稳分布$$\pi$$。对于PageRank算法来说，它相当于在一张由网页组成的巨型图上进行随机游走，当这张图满足相应的条件时无论从何处开始游走最终都会收敛到平稳分布$$\pi$$上。此时某个页面的重要性即为$$\pi$$对应位置的概率。

<div align=center>
<img src="https://i.imgur.com/u4xI2sF.png" width="80%">
<img src="https://i.imgur.com/dphlVuj.png" width="80%">
</div>

当然大多数情况下平稳分布$$\pi$$是没有办法通过解析的手段来求解的。不过对于对称的状态转移矩阵，其平稳分布有非常简单的形式：

$$
\pi(i) = \frac{1}{N}
$$

<div align=center>
<img src="https://i.imgur.com/mUW3CfJ.png" width="80%">
</div>

### PageRank

在Markov链的基础上我们开始介绍PageRank算法，它的基本思想是为每一个网页赋予一定的重要性。当我们在搜索引擎中进行查询时，我们希望搜索引擎可以对相关的网页进行排序，越重要的网页排在越前面。

<div align=center>
<img src="https://i.imgur.com/lBk1Nuw.png" width="80%">
</div>

同时还需要说明的是互联网上的网页可以表示为一张图，每一个页面对应图上的一个顶点，而页面之间跳转的超链接则对应图上的有向边。记$$\pi(x)$$表示页面$$x$$的重要性，它可以使用节点的入度或是出度来进行描述。

<div align=center>
<img src="https://i.imgur.com/TadFqN0.png" width="80%">
</div>

#### First Idea

对于学术文章来说，我们可以使用文章引用数来描述某篇文章的重要性。从图的角度来看这表示使用节点的入度来作为重要性，一般来说还会结合一些归一化来为入边赋予相应的权重。

<div align=center>
<img src="https://i.imgur.com/syLAMBv.png" width="80%">
<img src="https://i.imgur.com/jgH9lZc.png" width="80%">
</div>

#### Second Idea

类似地，我们可以把节点的出度也作为重要性度量。

<div align=center>
<img src="https://i.imgur.com/RrBA6cR.png" width="80%">
</div>

但实际上这种做法假定了每个节点的价值是相同的，而在现实生活中这往往是不成立的。

<div align=center>
<img src="https://i.imgur.com/xoRN0Ds.png" width="80%">
</div>

#### Rank Definition

因此更合理的做法是让有价值的节点为与它相邻的节点赋予更多的重要性，这样我们可以得到价值向量$$\pi$$需要满足的方程：

$$
\pi(x) = \sum_{y \in \text{In} (x)} \frac{\pi(y)}{\vert \text{Out} (y) \vert}
$$

<div align=center>
<img src="https://i.imgur.com/eTTWIJt.png" width="80%">
</div>

实际上$$\pi$$的递归定义与Markov链有着深刻的联系。首先不难发现网页之间的跳转等价于图上的随机游走。

<div align=center>
<img src="https://i.imgur.com/hGNn9pw.png" width="80%">
</div>

这个随机游走对应一个平稳分布$$\pi(x)$$。从线性代数的角度来看，平稳分布满足矩阵方程：

$$
\pi(x) = \sum_{y \in V} \pi(y) P(y, x)
$$

这表示$$\pi$$的每一位都是状态转移矩阵$$P$$的对应列按照$$\pi$$加权的和。如果进一步假定节点$$j$$转移到相邻节点上的概率是相同的，我们就可以推导出价值向量的定义：

$$
\pi(x) = \sum_{y \in V} \pi(y) P(y, x) = \sum_{y \in \text{In} (x)} \frac{\pi(y)}{\vert \text{Out} (y) \vert}
$$

这表示价值向量$$\pi$$即为图上随机游走的平稳分布。

<div align=center>
<img src="https://i.imgur.com/E2u4OPv.png" width="80%">
<img src="https://i.imgur.com/ipplebX.png" width="80%">
</div>

#### Random Surfer

在上一小节我们讨论过何时Markov链有唯一的平稳分布。为了保证PageRank的正确性，我们需要为图上每一对顶点添加一对边，这相当于假设用户在浏览网页时会以概率$$1-\alpha$$进行随机跳转。

<div align=center>
<img src="https://i.imgur.com/6fdMcLp.png" width="80%">
<img src="https://i.imgur.com/jSYt9p6.png" width="80%">
</div>

此时的状态转移矩阵具有如下的形式：

<div align=center>
<img src="https://i.imgur.com/Diu3Xxp.png" width="80%">
<img src="https://i.imgur.com/wDuKFbY.png" width="80%">
</div>

对于某些没有指向其它页面的网页，我们可以通过引入自循环边、删除节点或是将$$\alpha$$置为0的方式来进行处理。

<div align=center>
<img src="https://i.imgur.com/bpvxlTZ.png" width="80%">
</div>

#### Ergodic

显然$$\alpha$$对于随机游走的行为起着重要的作用。当$$\alpha$$趋于1时随机游走会接近于在原始图上的行为，而当$$\alpha$$趋于0时随机游走会接近于在一张全联通图上的行为。除此之外，当$$\alpha$$比较小时随机游走的收敛速度往往比较快。

<div align=center>
<img src="https://i.imgur.com/HqJfz6a.png" width="80%">
</div>

<div align=center>
<img src="https://i.imgur.com/PrdYEM3.png" width="80%">
</div>

## Reference

- [Graphs](https://teapowered.dev/assets/ga-notes.pdf#page=32)
- [最小生成树 Minimum Spanning Trees](https://www.youtube.com/watch?v=KsobpcI3dN0&list=PLvOO0btloRnsbnIIbX6ywvD8OZUTT0_ID&index=5)
- [Prim算法 寻找最小生成树 Prim's Algorithm for Minimum Spanning Trees](https://www.youtube.com/watch?v=GLlIaT_PxVE&list=PLvOO0btloRnsbnIIbX6ywvD8OZUTT0_ID&index=6)
- [Kruskal算法 寻找最小生成树 Kruskal's Algorithm for Minimum Spanning Trees](https://www.youtube.com/watch?v=Z4jm4o2bt28&list=PLvOO0btloRnsbnIIbX6ywvD8OZUTT0_ID&index=7)