---
layout: post
title: OMSCS-GA课程笔记05-Randomized Algorithms
date: 2022-10-06
description: 密码学与随机算法
tags: CS6515-GA
categories: OMSCS
pretty_table: true
toc:
  sidebar: left
---


> 这个系列是Gatech OMSCS 高级算法课程([CS 6515: Intro to Grad Algorithms](https://omscs.gatech.edu/cs-6515-intro-graduate-algorithms))的同步课程笔记。课程内容涉及动态规划、随机算法、分治、图算法、最大流、线性规划以及NP完备等高级算法的设计思想和相关应用，本节主要介绍随机算法。
{: .block-preface }


随机算法在密码学中有着非常重要的应用。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/kmkXxWQ.png" width="80%">
</div>

## Modular Arithmetic

在介绍随机算法与密码学的关系前我们需要先来引入**模算数(modular arithmetic)**的相关概念。对于整数$x$我们定义取模运算mod来获得它关于$N$的余数，如果两个整数$x$和$y$具有相同的余数则称它们关于$N$**同余(congruence)**：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/iW67pbi.png" width="80%">
</div>

以除数3为例，通过取模运算可以定义3个同余等价类。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/3t2bEYZ.png" width="80%">
</div>

### Basic Fact

同余的一个基本性质是如果$x$和$y$以及$a$和$b$分别关于$N$同余等价，则$x+a$和$y+b$同余等价且$xa$和$yb$同余等价。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tyHZnld.png" width="80%">
</div>

### Modular Exp

模算数的一个重要应用是计算指数的余数。对于n-bit整数$x$和$y$，我们可以通过递推的方法来计算$x^y$的余数：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XqsNI54.png" width="80%">
</div>

上述算法的复杂度依赖于$y$的大小。更高效的算法是利用平方运算来减少迭代步骤：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/868cAeY.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/U0Ke678.png" width="80%">
</div>

### Multiplicative Inverse

我们定义mod运算的逆为满足$x z \equiv 1 \ \text{mod} \ N$的整数，记为$x \equiv z^{-1} \ \text{mod} \ N$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IRRD5SB.png" width="80%">
</div>

需要注意的是并不是所有的整数都存在逆。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6NXFAxN.png" width="80%">
</div>

实际上当且仅当$x$和$N$的**最大公约数(greatest common divisor, GCD)**为1时才存在逆。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/xz4S72s.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/2qcnxS8.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/pb7nmZX.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FKeZrQs.png" width="80%">
</div>

### GCD

因此计算mod的逆就归结于计算$x$和$N$的最大公约数。关于最大公约数的研究可以追溯到古希腊的欧几里得，他指出$x$和$y$的最大公约数等于$(x \ \text{mod} \ y)$和$y$的最大公约数：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/XxbUICn.png" width="80%">
</div>

基于欧几里得定理可以得到计算最大公约数的算法，其复杂度为$O(n^3)$：

```
Euclid(x, y):
  if y == 0:
    return x

  else:
    return Euclid(y, x mod y)
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/G7LIc8L.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/jz8iOIA.png" width="80%">
</div>

这样就可以计算mod的逆，其复杂度为$O(n^3)$：

```
Ext-Euclid(x, y):
  if y == 0:
    return (x, 1, 0)

  else:
    d, alpha', beta' = Ext-Euclid(y, x mod y)
    return (d, beta', alpha' - x//y * beta')
```

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/hDVaWBu.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/B5jfvC0.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/rLqq2wk.png" width="80%">
</div>

## RSA

### Fermat's Little Theorem

在模算数的基础上就可以介绍密码学中重要的RSA算法了，不过在此之前我们还需要引入**费马小定理(Fermat's little theorem)**。它指出对于任意质数$p$和小于$p$的整数$z$，$z^{p-1}$与1关于$p$同余：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/B4n9HY4.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/k2cdrfL.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/BleDVXl.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/QeWnBr4.png" width="80%">
</div>

### Euler's Theorem

**欧拉定理(Euler's theorem)**是对费马小定理的推广，它指出互质的整数$N$和$z$，$z^{\phi(N)}$与1关于$N$同余，其中$\phi(N)$为小于$N$且与$N$互质的整数数量。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/o5rJuW0.png" width="80%">
</div>

如果我们进一步令$N$为两个质数$p$和$q$的积$N=pq$，可以证明$\phi(N)=(p-1)(q-1)$。这样有$z^{(p-1)(q-1)}$与1关于$N$同余。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/0uTjBuG.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ay1HGWH.png" width="80%">
</div>

### RSA Idea

RSA算法的基本思想是利用欧拉定理来对$z$进行加密。我们取两个质数$p$和$q$并计算它们的积$N=pq$，接下来取关于$(p-1)(q-1)$互逆的一对整数$d$和$e$。进行加密时加密方已知$e$和$N$，这样可以计算$z^e$；而在解密时解密方知道$e$，利用$z^{de}$就可以恢复$z$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/TnrHrn4.png" width="80%">
</div>

### Cryptography Setting

在密码学中最常见的一类场景是**公开密钥加密(public-key cryptography)**。此时数据发送方通过加密函数`e()`对信息进行加密，而接收方通过解密函数`d()`进行解密，它们之间的直接通信都是加密后的消息`e(m)`。同时接收方会公开**公钥(public key)**，这样任何拥有公钥的人都可以加密，但只有数据接收方能够利用**私钥(private key)**进行解密。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/zFEjFeV.png" width="80%">
</div>

### RSA Protocols

因此数据接收方的任务在于计算公钥和私钥，然后公开公钥并保留私钥用来解密。为此它需要首先选择两个质数$p$和$q$，然后计算与$(p-1)(q-1)$互质的$e$用来加密，这样就得到了公钥$N=pq$和$e$，最后计算并保存私钥$d$即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/vETQhFj.png" width="80%">
</div>

对于数据发送方来说，它只需要先获取公钥$N$和$e$，然后对消息$m$进行加密$y \equiv m^e \ \text{mod} \ N$，最后发送加密后的消息$y$即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yIhMZXk.png" width="80%">
</div>

而当接收方需要进行解密时只需要利用私钥计算$y^d \ \text{mod} \ N = m$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/b2tkQab.png" width="80%">
</div>

### RSA Pitfalls

使用RSA进行加密时需要注意的是保证原始数据$m$和$N=pq$是互质的，而如果它们存在公约数则容易导致一些问题。假设$m$和$N$存在公约数$p$，对于数据接收方仍然可以正确地进行解密，但是任何截获加密数据$m^e$的人都可以利用`gcd(y, N)`来破解$p$进而获得私钥。因此在进行加密时需要注意验证$m$和$N$是否是互质。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7QG9xgr.png" width="80%">
</div>

另一方面我们还希望原始数据$m$既不是特别大也不是特别小的数字，当$m$特别大或者特别小时都容易导致加密失效。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/9cvB5ys.png" width="80%">
</div>

### RSA Recap

总结一下目前为止的内容，整个RSA算法的流程如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IexyKKP.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/JcmiC8Z.png" width="80%">
</div>

### Random Prime Numbers

整个RSA算法依赖于选取质数$p$和$q$，因此我们需要设计随机质数的生成算法。要生成随机质数我们首先需要生成随机n-bit整数，这个过程比较简单只需要对n-bit随机赋予0或者1即可。然后我们验证这个随机整数是否是一个质数，如果它是质数就得到了一个随机质数，否则重新生成一个新的随机整数再进行判断即可。可以证明n-bit随机整数恰为质数的概率约为$\frac{1}{n}$，因此理论上我们只需要重复生成n次随机整数就可以得到一个随机质数。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/lIzdCbC.png" width="80%">
</div>

要验证一个整数是否是质数则需要**费马素性检验(Fermat primality test)**。根据费马小定理，任意小于质数$r$的整数$z$一定有$z^{r-1} \equiv 1 \ \text{mod} \ r$。因此如果我们能够找到一个小于$r$的整数$z$使得$z^{r-1} \not\equiv 1 \ \text{mod} \ r$，那么$r$一定是一个合数，这样的$z$称为**Fermat witness**。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/IvRm9Ed.png" width="80%">
</div>

可以证明任意合数都至少存在一个Fermat witness。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/W4A63e7.png" width="80%">
</div>

因此合数的因数一定是Fermat witness，称为**trivial Fermat witness**。而实际上Fermat witness不一定都是因数，此时则称其为**non-trivial Fermat witness**。只有trivial Fermat witness的合数称为**Carmichael数(Carmichael number)**，在使用费马素性检验时它们的性质非常接近于质数，因此也被称为**伪质数(pseudo primes)**。不过Carmichael数非常少见，在简单情况下我们可以忽略它们的存在。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/fTluKV6.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/YRTJGdb.png" width="80%">
</div>

如果我们忽略Carmichael数的存在可以得到质数的检验算法：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Ag3NzB5.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DXZTyD2.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/S8jbbyT.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/qS0ILFx.png" width="80%">
</div>

## Bloom Filters

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/us8ZWmt.png" width="80%">
</div>

### Ball into Bins

假设我们有$n$个完全相同的小球与$n$个盒子，每次随机将一个小球置入其中一个盒子中，此时第$i$个盒子中的小球数量是一个随机变量$\text{load}(i)$。记盒子中数目最多的小球数为$\max \ \text{load}$，它同样是一个随机变量且最大值为$n$，即所有小球同时落入一个盒子中。不过这种情况的概率很低，为$(\frac{1}{n})^n$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/7R8DpEU.png" width="80%">
</div>

要分析$\max \ \text{load}$的分布并不容易。首先，前$\log (n)$个小球都落在第$i$个盒子的概率为$(\frac{1}{n})^{\log n}$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Ib7baKz.png" width="80%">
</div>

在此基础上可以证明第$i$个盒子中小球数量至少为$\log (n)$的概率小于等于${n \choose \log n} (\frac{1}{n})^{\log n}$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/dmzPr1A.png" width="80%">
</div>

当$n$比较大时组合数具有上界：

$$
{n \choose k} \approx \bigg( \frac{n}{k} \bigg)^k \leq \bigg( \frac{n \ e}{k} \bigg)^k
$$

带入概率上界可以得到：

$$
P \leq \bigg( \frac{n \ e}{\log n} \bigg)^{\log n} \bigg( \frac{1}{n} \bigg)^{\log n} = 
\bigg( \frac{e}{\log n} \bigg)^{\log n}
$$

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/V4rOfhr.png" width="80%">
</div>

由于$\frac{e}{\log n}$随$n$的增加而减小，我们可以进一步使用常数$\frac{1}{4}$进行放缩。进一步假设$n > 2^{11}$可以得到最终的概率上界$\frac{1}{n^2}$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/FrYFXRh.png" width="80%">
</div>

利用上面的结论可以推导出$\max \ \text{load}$至少为$\log n$的概率上界为$\frac{1}{n}$，而它的下界有大概率为$\frac{\log n}{\log \log n}$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/nsyB18H.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Nny03fK.png" width="80%">
</div>

除了随机放置小球外，另一种放置小球的方式是每次随机选择两个盒子然后把小球放到$\text{load}$较小的那个盒子中。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/N72Aown.png" width="80%">
</div>

可以证明按照这种方式进行放置时$\max \ \text{load}$的上界有大概率为$\log \log n$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RbqRJsj.png" width="80%">
</div>

### Chain Hashing

**Hashing**在现实生活中有着非常多的应用，比如说可以使用hashing作为容器来保存数据，此时我们需要回答的问题是容器中是否存在某个给定的元素$x$。最常见的实现方法是构造一个数组，其中每个位置对应一个链表。然后设计一个函数$h(x)$将可能的元素$x$映射到链表的编号。这样的实现方式称为**chain hashing**。假设$h(x)$是一个随机函数，我们可以把链表想象成小球问题中的盒子，而元素$x$则为小球，这样就可以使用上一节的结论来进行分析。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/33MChih.png" width="80%">
</div>

对于chain hashing这种数据结构，检测$x$是否在容器中需要两步：

1. 使用$h(x)$计算链表的编号$h(x) = i$。
2. 检测$x$是否在链表$H[i]$中。

记$x$的所有可能取值数量为$m$，链表的数目为$n$。利用上一小节的结论可以得到每个链表的大小有大概率为$\log n$，即查询的复杂度为$O(\log n)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/OcGKb4M.png" width="80%">
</div>

要提升查询的效率可以使用两个$h(x)$函数，每次添加元素时选择$\text{load}$较少的那个链表来加入。需要注意的是在进行查询时需要同时检查两个链表中的元素。可以证明此时的查询复杂度为$O(\log \log n)$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/CyyqmyM.png" width="80%">
</div>

### Bloom Filter

**Bloom滤波器(Bloom filter)**是对chain hashing的一种改进，它可以实现$O(1)$的查询效率而且实现起来非常简单。不过Bloom滤波器仅是概率正确的，存在假阳性的可能性。对于精度没有特别严格要求的场景，Bloom滤波器是一个非常强大的工具。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/DumbOT6.png" width="80%">
</div>

Bloom滤波器包括两个基本操作，插入`insert(x)`以及查询`query(x)`。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/cDnmuIZ.png" width="80%">
</div>

在实现时Bloom滤波器使用了一个大小为n的二进制数组。首先把数组的每一位都初始化为0，当我们需要插入$x$时将数组的第$h(x)$位设置为1，而在查询时只需要检测$H[h(x)]$是否为1即可。显然此时两种操作都只需要$O(1)$的复杂度，但查询操作可能会出现假阳性的情况。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/67RdQQO.png" width="80%">
</div>

要提升系统的鲁棒性可以同时使用k个hashing函数。此时要进行插入和查询则需要同时检查数组的k个可能位置，当数组的k个位置上都为1时说明$x$在容器中。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/yIinet2.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/mN2sbFv.png" width="80%">
</div>

显然k值对于假阳性的概率有着重要的影响，可以证明出现假阳性的概率约为$(1-e^{-\frac{k}{c}})^k$，这样最优的k值为$c \ln 2$而对应的假阳性概率约为$0.6185^c$。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/AzgdxDr.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/ixUvWVM.png" width="80%">
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/G3utfMJ.png" width="80%">
</div>

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/tyc58QP.png" width="80%">
</div>

## Reference

- [Cryptography](https://teapowered.dev/assets/ga-notes.pdf#page=60)