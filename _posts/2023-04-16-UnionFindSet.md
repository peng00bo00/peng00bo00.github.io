---
layout: post
title: 并查集
date: 2023-04-16 0:00:00-0400
tags: leetcode
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---

**并查集(union-find set)**是一种用于管理元素所属集合的数据结构。它的实现为一个森林，其中每棵树表示一个集合，树中的节点表示对应集合中的元素。

<div align=center>
<img src="https://images.weserv.nl/?url=https://he-s3.s3.amazonaws.com/media/uploads/4c11a99.jpg" width="60%">
<img src="https://images.weserv.nl/?url=https://he-s3.s3.amazonaws.com/media/uploads/7439d01.jpg" width="60%">
</div>

并查集需要实现**合并(union)**和**查询(find)**两个操作：

- 合并：合并两个元素所属集合(合并对应的树)
- 查询：查询某个元素所属的集合(树的根节点)，一般可以用来判断两个元素是否来自同一集合

并查集的经典应用在于处理**动态连接(dynamic connectivity)**问题，即判断两个对象是否相互联通或者图上有多少不连通的集合。因此在初始化并查集时我们需要初始化两个属性：`self.pa`用来记录每个节点的父节点，以及`self.count`用来记录图上的集合数。

```python
class UnionFindSet:
    def __init__(self, n: int):
        self.pa    = [i for i in range(n)]  ## 初始化每个节点的父节点为其自身
        self.count = n                      ## 初始化集合数为全部节点数
```

使用并查集进行查询时需要调用`find(x)`方法返回`x`节点的根节点，这里还可以结合**路径压缩**的技巧来加快查询速度。

```python
def find(self, x: int) -> int:
    if self.pa[x] != x:
        self.pa[x] = self.find(self.pa[x])  ## 路径压缩
    
    return self.pa[x]
```

在`find(x)`的基础上我们可以实现`union(x, y)`方法用来合并`x`和`y`两个节点所在集合。

```python
def union(self, x: int, y: int) -> None:
    root_x = self.find(x)                   ## 查询x的根节点
    root_y = self.find(y)                   ## 查询y的根节点

    if root_x != root_y:
        self.pa[root_y] = root_x            ## 当x和y来自不同集合时把y的根节点连到x根节点上
        self.count -= 1                     ## 同时集合数量减1
```

并查集的标准实现可以参考如下。

```python
class UnionFindSet:
    def __init__(self, n: int):
        self.pa    = [i for i in range(n)]
        self.count = n
    
    def union(self, x: int, y: int) -> None:
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x != root_y:
            self.pa[root_y] = root_x
            self.count -= 1
    
    def find(self, x: int) -> int:
        if self.pa[x] != x:
            self.pa[x] = self.find(self.pa[x])
        
        return self.pa[x]
```

## 冗余连接

### 684. 冗余连接

树可以看成是一个连通且**无环**的**无向**图。

给定往一棵`n`个节点(节点值`1`～`n`)的树中添加一条边后的图。添加的边的两个顶点包含在`1`到`n`中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为`n`的二维数组`edges`，`edges[i] = [ai, bi]`表示图中在`ai`和`bi`之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着`n`个节点的树。如果有多个答案，则返回数组`edges`中最后出现的边。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/1626676174-hOEVUL-image.png">
</div>

```
输入：edges = [[1,2], [1,3], [2,3]]
输出：[2,3]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/1626676179-kGxcmu-image.png">
</div>

```
输入：edges = [[1,2], [2,3], [3,4], [1,4], [1,5]]
输出：[1,4]
```

**提示：**

- `n` == `edges.length`
- 3 <= `n` <= 1000
- `edges[i].length` == 2
- 1 <= `ai` < `bi` <= `edges.length`
- `ai` != `bi`
- `edges`中无重复元素
- 给定的图是连通的

#### Solution

本题的解法在于对`edges`进行遍历，如果边的两个端点`u`和`v`位于不同集合则将它们合并，否则说明它们已经连接到一起了直接返回即可。

[题目链接](https://leetcode.cn/problems/redundant-connection/)：

python代码：

```python
class UnionFindSet:
    def __init__(self, n: int):
        self.pa    = [i for i in range(n)]
        self.count = n
    
    def union(self, x, y) -> None:
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x != root_y:
            self.pa[root_y] = root_x
            self.count -= 1
    
    def find(self, x: int) -> int:
        if self.pa[x] != x:
            self.pa[x] = self.find(self.pa[x])
        
        return self.pa[x]

class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)

        UF = UnionFindSet(n+1)

        for u, v in edges:
            if UF.find(u) == UF.find(v):
                return [u, v]
            else:
                UF.union(u, v)
        
        return []
```


C++代码：

```c++
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int N = edges.size();
        init(N+1);

        for (int i=0; i<N; ++i) {
            int u = edges[i][0], v = edges[i][1];

            if (find(u) == find(v)) return edges[i];
            else join(u, v);
        }

        return {};
    }

private:
    vector<int> pa;

    void init(int N) {
        pa.clear();
        pa.resize(N);

        for (int i=0; i<N; ++i) pa[i] = i;
    }

    void join(int u, int v) {
        u = find(u);
        v = find(v);

        if (u != v) pa[v] = u;
    }

    int find(int u) {
        if (pa[u] != u) pa[u] = find(pa[u]);

        return pa[u];
    }
};
```


### 685. 冗余连接 II

在本问题中，有根树指满足以下条件的**有向**图。该树只有一个根节点，所有其他节点都是该根节点的后继。该树除了根节点之外的每一个节点都有且只有一个父节点，而根节点没有父节点。

输入一个有向图，该图由一个有着`n`个节点(节点值不重复，从`1`到`n`)的树及一条附加的有向边构成。附加的边包含在`1`到`n`中的两个不同顶点间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组`edges`。每个元素是一对`[uᵢ, vᵢ]`，用以表示**有向**图中连接顶点`uᵢ`和顶点`vᵢ`的边，其中`uᵢ`是`vᵢ`的一个父节点。

返回一条能删除的边，使得剩下的图是有`n`个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/20/graph1.jpg">
</div>

```
输入：edges = [[1,2],[1,3],[2,3]]
输出：[2,3]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/20/graph2.jpg">
</div>

```
输入：edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
输出：[4,1]
```

**提示：**

- `n` == `edges.length`
- 3 <= `n` <= 1000
- `edges[i].length` == 2
- 1 <= `uᵢ`, `vᵢ` <= `n`

#### Solution

本题的整体思路类似于[冗余连接](/leetcode/2023-04-16-UnionFindSet.html#684-冗余连接)，我们需要对`edges`进行遍历并尝试构造出树结构。由于整个图上只有一条额外的边，这条边有三种可能性：

1. 这条边破坏了树结构，使得某个节点`v`有两个父节点(如示例1中的节点`3`)
2. 这条边导致图上出现了环(如示例2中的`[4, 1]`)
3. 这条边既破坏了树结构又产生了环

对于树结构，我们设置一个数组`parent`用来记录每个节点当前的父节点。我们把`parent`初始化为节点自身的编号，表示此时没有添加任何边，然后在对边进行遍历时检查`parent[v]`是否与`v`相等。如果发现`parent[v] != v`说明`v`节点已经存在一个父节点了，此时使用变量`conflict`来记录这条边的编号；而如果发现`parent[v] == v`则说明`v`节点还没有任何父节点，令`parent[v] = u`同时利用并查集检查`u`和`v`两个节点是否联通，如果联通则还需要利用变量`cycle`记录下这条边的编号，最后合并`u`和`v`这两个节点。

完成遍历后我们需要结合`conflict`和`cycle`两个变量进行讨论：

- 如果`conflict < 0`说明图上只存在环，此时可以直接返回`edges[cycle]`
- 如果`conflict >=0`说明图上有节点存在两个父节点，需要进一步讨论：
  - 如果`cycle < 0`说明图上没有环，直接返回`edges[conflict]`
  - 如果`cycle >=0`说明图上存在环，我们需要先提取具有两个父节点的子节点`u, v = edges[conflict]`然后返回`v`节点当前的边`[parent[v], v]`

整个算法流程可以参考如下。

[题目链接](https://leetcode.cn/problems/redundant-connection-ii/)：

python代码：

```python
class UnionFindSet:
    def __init__(self, n: int):
        self.pa    = [i for i in range(n)]
        self.count = n
    
    def find(self, x: int) -> int:
        if self.pa[x] != x:
            self.pa[x] = self.find(self.pa[x])
        return self.pa[x]
    
    def union(self, x: int, y: int) -> None:
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x != root_y:
            self.pa[root_y] = root_x
            self.count -= 1

class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)

        UF     = UnionFindSet(n+1)
        parent = [i for i in range(n+1)]

        conflict = -1
        cycle    = -1

        for i, (u, v) in enumerate(edges):
            if parent[v] == v:
                parent[v] = u

                if UF.find(u) == UF.find(v):
                    cycle = i
                
                UF.union(u, v)
            
            else:
                conflict = i
        
        if conflict < 0:
            return edges[cycle]
        else:
            if cycle < 0:
                return edges[conflict]
            else:
                u, v = edges[conflict]
                return [parent[v], v]
```


C++代码：

```c++
struct UnionFind {
    vector<int> pa;

    UnionFind(int N) {
        pa.clear();
        pa.resize(N);

        for (int i=0; i<N; ++i) pa[i] = i;
    }

    void join(int u, int v) {
        u = find(u);
        v = find(v);

        if (u != v) pa[v] = u;
    }

    int find(int u) {
        if (pa[u] != u) pa[u] = find(pa[u]);

        return pa[u];
    }
};

class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int N = edges.size();
        UnionFind UF(N+1);

        vector<int> parent(N+1);
        for (int i=0; i<N+1; ++i) parent[i] = i;

        int conflict = -1;
        int cycle    = -1;

        for (int i=0; i<N; ++i) {
            int u = edges[i][0];
            int v = edges[i][1];

            if (parent[v] != v) {
                conflict = i;
            } else {
                parent[v] = u;

                if (UF.find(u) == UF.find(v)) cycle = i;
                else UF.join(u, v);
            }
        }

        if (conflict < 0) return {edges[cycle][0], edges[cycle][1]};
        else {
            if (cycle < 0) return {edges[conflict][0], edges[conflict][1]};
            else return {parent[edges[conflict][1]], edges[conflict][1]};
        }
    }
};
```
