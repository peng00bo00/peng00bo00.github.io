---
layout: post
title: OMSCS-GA课程笔记01-Dynamic Programming
date: 2022-08-23
description: 动态规划
tags: CS6515-GA
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 高级算法课程([CS 6515: Intro to Grad Algorithms](https://omscs.gatech.edu/cs-6515-intro-graduate-algorithms))的同步课程笔记。课程内容涉及动态规划、随机算法、分治、图算法、最大流、线性规划以及NP完备等高级算法的设计思想和相关应用，本节主要介绍动态规划。
{: .block-preface }


**动态规划(dynamic programming, DP)**是算法设计中非常重要的一种思想，很多复杂的问题都可以基于动态规划来进行求解。

## Fibonacci Numbers

计算Fibonacci数列是一个经典的算法问题。Fibonacci数列的递推式为：

$$
F_0 = 0, \ F_1 = 1 \\
F_n = F_{n-2} + F_{n-1}, \ \text{for} \ n \geq 2
$$

我们的目标是对于任意的整数$n \geq 0$，输出第$n$项数值$F_n$。

### Recursive Algorithm

显然我们可以使用递归的方式来求解这个问题：

```
Fib1(n):
  if n == 0
    return 0
  if n == 1
    return Fib1(n-1) + Fib1(n-2)
```

根据主定理我们可以计算这个算法的复杂度：

$$
T(n) \leq O(1) + T(n-1) + T(n-2)
$$

$$
T(n) \geq F_n \approx \frac{\phi^n}{\sqrt{5}}
$$

其中$\phi=\frac{1+\sqrt{5}}{2} \approx 1.618$，即该算法有指数级的复杂度。

显然`Fib1(n)`不是一个好的算法，其问题在于它会反复计算数列前面的项。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QBssznj.png" width="80%">
</div>

### DP Algorithm

接下来我们使用动态规划来改进之前的算法。具体地，我们使用一个数组来存储中间的计算结果然后从前向后进行计算：

```
Fib2(n):
  F[0] = 0
  F[1] = 1

  for i=2:n
    F[i] = F[i-1] + F[i-2]
    
  return F[n]
```

显然此时算法的复杂度为$O(n)$，远小于之前的复杂度。

从这个例子可以看出动态规划的特点：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/MmQlu6J.png" width="80%">
</div>

## Longest Increasing Subsequence

LIS问题的目标是在给定序列中寻找递增子列的长度，注意这里我们允许对原始序列进行删减来获得子列。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/InNHqpl.png" width="80%">
</div>

### Subproblem Attempt

使用DP的步骤是首先定义一个subproblem，然后依次求解subproblem。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/OrvDRcO.png" width="80%">
</div>

对于LIS问题可以进行形式化如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nH3zrPE.png" width="80%">
</div>

### Recurrence Attempt

求解LIS问题的核心在于记录下序列中每个元素结尾时递增子列的长度。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UpO1clv.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/wecKkrK.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mSrPH3M.png" width="80%">
</div>

### DP Algorithm

因此我们可以基于DP来设计算法：

```
LIS(a[]):
  for i=1:n
    L[i] = 1

    for j=1:i-1
      if a[j] < a[i] & L[i] < 1+L[j]
        L[i] = 1 + L[j]

  max = 1
  for i=2:n
    if L[i] > L[max]
      max = i

  return max
```

此时算法的复杂度为$O(n^2)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/AhZwakT.png" width="80%">
</div>

## Longest Common Subsequence

LCS问题的目标是计算两个序列中最长的公共子列：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/bA8GydQ.png" width="80%">
</div>

### Subproblem Attempt1

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7kqzCSh.png" width="80%">
</div>

### Recurrence Attempt1

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2q0zgaA.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JTJvkbb.png" width="80%">
</div>

### Subproblem Attempt2

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7ts0xtJ.png" width="80%">
</div>

### Recurrence Attempt2

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/hW9KwqY.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YJjLKVP.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YueOEWA.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dLDg8o9.png" width="80%">
</div>

### DP Algorithm

因此求解LCS的算法为：

```
LCS(X[], Y[]):
  for i=0:n
    L[i, 0] = 0
  for j=0:n
    L[0, j] = 0

  for i=1:n
    for j=1:n
      if X[i] == Y[j]
        L[i, j] = 1 + L[i-1, j-1]
      else
        L[i, j] = max(L[i, j-1], L[i-1, j])

  return L[n, n]
```

此时算法的复杂度为$O(n^2)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ycZbes0.png" width="80%">
</div>

如果想要获得最长公共子列，还可以从L的右下角开始向上进行追溯：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6KeLTki.png" width="80%">
</div>

## Knapsack

knapsack是经典的优化问题，我们希望在一定的重量约束下最大化背包中物品的价值：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/AqMxn1p.png" width="80%">
</div>

### Greedy Algorithm

贪心算法是求解knapsack问题的一种经典解法，不过需要注意的是贪心算法往往不能得到问题的最优解。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4RaHtnI.png" width="80%">
</div>

### Attempt1

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/rGEnwFr.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/l53NiJl.png" width="80%">
</div>

### Attempt2

求解knapsack的核心在于构造一个二维数组$K[i, b]$，它表示使用物品序列$$\{ 1, \dots, i \}$$且重量约束为$b$条件下背包中物品的最大价值。显然knapsack问题的解即为数组的最后一个元素$K[n, B]$，而子问题$K[i, b]$的递归形式则依赖于$i$号物品的重量。当$w_i \leq b$时我们可以尝试在背包中加入$i$号物品，否则只能放弃添加它并使用前一个子问题的最大价值$K[i-1, b]$。因此子问题的递归形式为：

$$
K[i, b] =
\begin{cases}
\max (v_i + K[i-1, b-w_i], K[i-1, b]), & \text{if } w_i \leq b \\
K[i-1, b], &\text{otherwise}
\end{cases}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/hvBRfA4.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zllCzsN.png" width="80%">
</div>

### DP Algorithm

因此，使用DP来求解knapsack问题的伪代码如下：

```
KnapsackNoRepeat(w[], v[], B):
  for b=0:B
    K[0, b] = 0
  for i=1:n
    K[i, 0] = 0

  for i=1:n
    for b=1:B
      if w[i] <= b
        K[i, b] = max(v[i]+K[i-1, b-w[i]], K[i-1, b])
      else
        K[i, b] = K[i-1, b]

  return K[i, b]
```

此时算法的复杂度为$O(nB)$。这里需要说明的是$O(nB)$依赖于限制$B$的值，而要表示$B$则需要$O(\log B)$的空间。因此这个算法并不是一个非常高效的算法。实际上人们已经证明knapsack问题是[NP-complete](/2022/10/25/OMSCS-GA-NOTES-06.html#knapsack)，我们目前无法找到一个高效的解法。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/wETdVXr.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CHzHAfM.png" width="80%">
</div>

### Knapsack Repetition

knapsack问题的一个变体是假设每个物品都可以无限地进行添加。在这种情况下我们同样可以使用一个二维数组$K[i, b]$来进行递推，不过递推关系为：

$$
K[i, b] = \max ( K[i-1, b], v_i+K[i, b-w_i] ), \ \text{if } w_i \leq b
$$

上式表示我们可以尝试在当前的背包中添加一个物品$i$以记录此时背包中的最大价值，此时的算法复杂度为$O(nB)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/rqmYLCe.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/edTQSrR.png" width="80%">
</div>

#### Simpler Subproblem

实际上对于允许重复的情况我们可以设计更简洁的算法。记$K[b]$为使用所有物品在重量约束为$b$情况下背包中的最大价值，此时的递推关系为：

$$
K[b] = \max \{ v_i + K[b-w_i] \vert 1 \leq i \leq n, w_i \leq b \}
$$

它表示当重量约束为$b$时，我们尝试在背包中添加1个$i$号物品从而记录下当前条件下背包的最大价值。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zxxW6cI.png" width="80%">
</div>

因此对于允许重复的knapsack问题可以按照如下过程进行求解：

```
KnapsackRepeat(w[], v[], B):
  for b=0:B
    K[b] = 0

    for i=1:n
      if w[i] <= b & K[b] < v[i]+K[b-w[i]]
        K[b] = v[i] + K[b-w[i]]

  return K[B]
```

此时算法的复杂度为$O(nB)$，仍然不是一个高效的解法。

## Chain Matrix Multiply

### Motivation

动态规划还可以用来处理矩阵乘法。回忆矩阵乘法的运算规则，新矩阵的每个元素都是$A$和$B$矩阵对应行列的内积。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6uXt8oz.png" width="80%">
</div>

由于矩阵乘法的结合性，对于链式相乘的矩阵我们可以调整矩阵乘法的计算顺序从而改变整个乘法的计算复杂度。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/w1C3UOG.png" width="80%">
</div>

具体地，对于矩阵乘法$Z_{a \times c} = W_{a \times b} \cdot Y_{b \times c}$的计算复杂度为$O(abc)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ujk2c9q.png" width="80%">
</div>

### General Problem

因此，连续矩阵相乘的问题就可以使用动态规划的思路进行建模。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vXBBgca.png" width="80%">
</div>

### Graphical View

同时我们也可以使用二叉树来理解计算的过程，从这个角度来看我们的目标则是找到总体代价最小的树。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/s2l9PYS.png" width="80%">
</div>

### Substring

这里我们引入substring的概念，它是原始序列中一段连续的子列。记$C[i, j]$表示矩阵$A_i$到$A_j$进行联乘的最小计算代价，根据二叉树我们可以得到递推形式：

$$
C[i, j] = \min \{ C[i, l] + C[l+1, j] + m_{i-1} m_l m_j \vert 1 \leq l \leq j-1 \}
$$

其中$C[i, l]$和$C[l+1, j]$分别表示左子树和右子树的最小计算代价，而$m_{i-1} m_l m_j$则是合并两个子树的代价。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/w40ZPsG.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/k6jcry4.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/gMQp4mA.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/5PLssSE.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2IiCARh.png" width="80%">
</div>

### DP Algorithm

因此使用动态规划计算矩阵相乘最小复杂度的核心是从对角线开始逐步向上进行递推，它和上面介绍过的其它动态规划方法在递推形式上有着明显的区别。

```
ChainMultiply(m0, m1, ..., mn):
  for i=1:n
    C[i, i] = 0

  for s=1:n-1
    for i=1:n-s
      j=i+s
      C[i, j] = inf

      for l=1:j-1
        cur = m[i-1]*m[l]*m[j] + C[i, l] + C[l+1, j]
        if cur < C[i, j]
          C[i, j] = cur

  return C[1, n]
```

整个算法的复杂度为$O(n^3)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/m7cWQsf.png" width="80%">
</div>

## Shortest Path Algorithms

动态规划的一个重要应用是计算最短路径，实际上经典的Dijkstra算法就是基于动态规划来进行设计的。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/kdv3aLQ.png" width="80%">
</div>

### Negative Weight Cycles

Dijkstra算法的一个局限性在于它不能处理带负边的情况。对于更一般的图结构，我们希望能够找到图上权重和为负的环，同时计算出任意两个顶点之间的最短路径。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ruFCOBY.png" width="80%">
</div>

### Single Source

首先考虑单源最短路径问题。由于此时图上包含负边，我们不能直接使用Dijkstra算法进行求解。同时为了避免出现无限循环的问题，我们还要求每个节点最多被访问一次。对于包含$m$个节点和$n$条边的图，记$D[i, z]$为使用最多$i$条边从起点出发到达节点$z$的最小代价。此时$D[i, z]$的递推关系为：

$$
D[i, z] = \min \big\{ D[i-1, y], \min \{ D[i-1, y] + w[y, z] \vert yz \in E \} \big\}
$$

其中$w[y, z]$为节点$y$到节点$z$的代价，而$$\min \{ D[i-1, y] + w[y, z] \vert yz \in E \}$$则是添加一条边后起点到节点$z$的最小代价。因此上式意为当我们增加一条可用边时，起点到$z$的最小代价是前一步的代价和添加一条边后路径代价中较小的那个。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/utgmT6Z.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/AZhFsqV.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cEXxH1G.png" width="80%">
</div>

这种利用动态规划来解决带负边的单源最短路径问题的算法称为**Bellman-Ford算法**，它的复杂度为$O(mn)$：

```
Bellman-Ford(G, s, w):
  for z in V
    D[0, z] = inf

  D[0, s] = 0

  for i=1:n-1
    for z in V
      D[i, z] = D[i-1, z]

      for yz in E
        if D[i, z] > D[i-1, y] + w[y, z]:
          D[i, z] = D[i-1, y] + w[y, z]
    
  return D[n-1, :]
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ZlFiZrV.png" width="80%">
</div>

### Finding Negative Weight Cycle

当图上有权重为负的环时还需要找到这样的环，此时该环上的路径其代价会更小一些。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JoiX2hV.png" width="80%">
</div>

### All Pairs

除了单源最短路径问题之外，在很多情况下我们希望计算图上任意两个节点之间的最短路径。对于这样的问题同样可以使用动态规划来进行建模和处理。记三维数组$D[i,s,t]$为最多使用$$\{ 1, 2, \dots, i \}$$的子集作为中间节点从节点$s$出发到达节点$t$的最小代价，当$s$和$t$直接相连时有$D[0, s, t] = w[s, t]$，否则将代价初始化为无穷大。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/n9rQbHR.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qIrRCLy.png" width="80%">
</div>

$D[i,s,t]$的递推关系取决于节点$i$是否位于$s$到$t$的最短路径上。当节点$i$不在最短路径上时有：

$$
D[i, s, t] = D[i-1, s, t]
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/j4qdxpN.png" width="80%">
</div>

否则$D[i, s, t]$等于$s$到$i$与$i$到$t$两段路径最小代价之和：

$$
D[i, s, t] = D[i-1, s, i]+D[i-1, i, t]
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/G5wVlUi.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vv7ZyT4.png" width="80%">
</div>

整理后可以得到递推关系：

$$
D[i, s, t] = \min \big( D[i-1, s, t], D[i-1, s, i]+D[i-1, i, t] \big)
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/eTOb0IX.png" width="80%">
</div>

因此计算图上所有节点之间最短路径的**Floyd-Warshall算法**如下：

```
Floyd-Warshall(G, w):
  for i=1:n
    for t=1:n
      if st in E:
        D[0, s, t] = w[s, t]
      else:
        D[0, s, t] = inf

  for i=1:n
    for s=1:n
      for t=1:n
        D[i, s, t] = min(D[i-1, s, t], D[i-1, s, i]+D[i-1, i, t])

  return D[i, :, :]
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/K0ZTLpJ.png" width="80%">
</div>

Floyd-Warshall算法的计算复杂度为$O(n^3)$，其中$n$为图上节点数。需要注意的是Floyd-Warshall算法假设图上没有权重为负的环，因此在使用时需要首先对图进行检测。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CZ6c72t.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FvSIHcG.png" width="80%">
</div>

## Reference

- [Dynamic Programming](https://teapowered.dev/assets/ga-notes.pdf#page=8)