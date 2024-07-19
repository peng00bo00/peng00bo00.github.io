---
layout: post
title: 动态规划
date: 2023-03-13
description: LeetCode动态规划习题
tags: code
categories: LeetCode
giscus_comments: false
related_posts: false
toc:
  sidebar: left
---


**动态规划(dynamic programming, DP)**是利用子问题之间的关系来递推出整个问题的解。一般来说DP问题可以分解为以下五步来求解：

1. 确定`dp`数组(dp table)以及下标的含义
2. 确定递推公式
3. `dp`数组如何初始化
4. 确定遍历顺序
5. 举例推导`dp`数组

## 基础题目

### 509. 斐波那契数

**斐波那契数**(通常用`F(n)`表示)形成的序列称为**斐波那契数列**。该数列由`0`和`1`开始，后面的每一项数字都是前面两项数字的和。也就是：

```
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
```

给定`n`，请计算`F(n)`。

**示例1：**

```
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

**示例2：**

```
输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
```

**示例3：**

```
输入：n = 4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
```

**提示：**

- 0 <= `n` <= 30

#### Solution

求斐波那契数是DP的经典问题。我们建立一个数组`dp[]`用来表示数列，其中`dp[i]`即为`F(i)`。此外，题目中已经给出了初始化条件和递推关系：

- `dp[0] = 0`，`dp[1] = 1`
- `dp[i] = dp[i-2] + dp[i-1]`

这样我们就只需要直接递推即可。

[题目链接](https://leetcode.cn/problems/fibonacci-number/)：

python代码：

```python
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        elif n == 1:
            return 1

        dp = [0 for _ in range(n+1)]
        dp[1] = 1

        for i in range(2, n+1):
            dp[i] = dp[i-2] + dp[i-1]
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int fib(int n) {
        if (n == 0) return 0;
        else if (n == 1) return 1;

        vector<int> dp(n+1, 0);
        dp[1] = 1;

        for (int i=2; i<n+1; ++i) {
            dp[i] = dp[i-1] + dp[i-2];
        }

        return dp[n];
    }
};
```


除了上述标准解法外，我们还可以只需要使用两个变量来进行递推而无需记录整个数组。这种解法的空间复杂度为`O(1)`。

[题目链接](https://leetcode.cn/problems/fibonacci-number/)：

python代码：

```python
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        elif n == 1:
            return 1

        pre, cur = 0, 1
        for i in range(n-1):
            pre, cur = cur, pre+cur
        
        return cur
```


C++代码：

```c++
class Solution {
public:
    int fib(int n) {
        if (n == 0) return 0;
        else if (n == 1) return 1;

        int pre = 0, cur = 1;
        for (int i=2; i<n+1; ++i) {
            swap(pre, cur);
            cur += pre;
        }

        return cur;
    }
};
```


### 70. 爬楼梯

假设你正在爬楼梯。需要`n`阶你才能到达楼顶。

每次你可以爬`1`或`2`个台阶。你有多少种不同的方法可以爬到楼顶呢？

**示例1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

**提示：**

- 1 <= `n` <= 45

#### Solution

爬楼梯同样是DP的经典问题。我们建立一个数组`dp[]`，其中`dp[i]`为爬到第`i-1`阶楼梯的最短方式。本题的初始化条件和递推关系为：

- `dp[0] = 1`，`dp[1] = 2`
- `dp[i] = dp[i-2] + dp[i-1]`

这样我们就只需要直接递推即可。

[题目链接](https://leetcode.cn/problems/climbing-stairs/)：

python代码：

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        elif n == 2:
            return 2
        
        dp = [0 for i in range(n)]
        dp[0] = 1
        dp[1] = 2

        for i in range(2, n):
            dp[i] = dp[i-2] + dp[i-1]
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1) return 1;

        vector<int> dp(n+1, 0);
        dp[0] = 1; dp[1] = 1;

        for (int i=2; i<=n; ++i) {
            dp[i] = dp[i-2] + dp[i-1];
        }

        return dp[n];
    }
};
```


类似于[斐波那契数](/leetcode/2023-03-13-DynamicProgramming.html#509-斐波那契数)，我们同样可以使用`O(1)`的空间复杂度来进行递推。

[题目链接](https://leetcode.cn/problems/climbing-stairs/)：

python代码：

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        elif n == 2:
            return 2
        
        pre, cur = 1, 2

        for i in range(n-2):
            pre, cur = cur, pre+cur
        
        return cur
```


C++代码：

```c++
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1) return 1;

        int pre = 1, cur = 1;

        for (int i=2; i<=n; ++i) {
            swap(pre, cur);
            cur += pre;
        }

        return cur;
    }
};
```


### 746. 使用最小花费爬楼梯

给你一个整数数组`cost`，其中`cost[i]`是从楼梯第`i`个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为`0`或下标为`1`的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

**示例1：**

```
输入：cost = [10,15,20]
输出：15
解释：你将从下标为 1 的台阶开始。
- 支付 15 ，向上爬两个台阶，到达楼梯顶部。
总花费为 15 。
```

**示例2：**

```
输入：cost = [1,100,1,1,1,100,1,1,100,1]
输出：6
解释：你将从下标为 0 的台阶开始。
- 支付 1 ，向上爬两个台阶，到达下标为 2 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 4 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 6 的台阶。
- 支付 1 ，向上爬一个台阶，到达下标为 7 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 9 的台阶。
- 支付 1 ，向上爬一个台阶，到达楼梯顶部。
总花费为 6 。
```

**提示：**

- 2 <= `cost.length` <= 1000
- 0 <= `cost[i]` <= 999

#### Solution

本题类似于[爬楼梯](/leetcode/2023-03-13-DynamicProgramming.html#70-爬楼梯)，但要稍微复杂一些。我们建立一个数组`dp[]`，其中`dp[i]`为爬到第`i`阶楼梯的最小代价。由于我们可以选择下标为`0`或下标为`1`的台阶开始爬楼梯，因此数组`dp[]`需要初始化如下：

- `dp[0] = 0`，`dp[1] = 0`

接下来考虑递推关系，我们可以从第`i-2`阶或是第`i-1`阶向上爬。根据题意，最小代价而二者中较小的那个：

- `dp[i] = min(dp[i-2] + cost[i-2], dp[i-1] + cost[i-1])`

得到递推关系后直接进行递推即可。

[题目链接](https://leetcode.cn/problems/min-cost-climbing-stairs/)：

python代码：

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        N = len(cost)

        dp = [0 for i in range(N+1)]

        for i in range(2, N+1):
            dp[i] = min(dp[i-2] + cost[i-2], dp[i-1] + cost[i-1])
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int N = cost.size();
        
        vector<int> dp(N+1, 0);

        for (int i=2; i<=N; ++i) {
            dp[i] = min(dp[i-2]+cost[i-2], dp[i-1]+cost[i-1]);
        }

        return dp[N];
    }
};
```


本题同样可以使用`O(1)`的空间复杂度来进行求解。

[题目链接](https://leetcode.cn/problems/min-cost-climbing-stairs/)：

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        N = len(cost)
        pre = cur = 0

        for i in range(2, N+1):
            pre, cur = cur, min(pre+cost[i-2], cur+cost[i-1])
        
        return cur
```


### 62. 不同路径

一个机器人位于一个`m x n`网格的左上角(起始点在下图中标记为“Start”)。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角(在下图中标记为“Finish”)。

问总共有多少条不同的路径？

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2018/10/22/robot_maze.png">
</div>

**示例1：**

```
输入：m = 3, n = 7
输出：28
```

**示例2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

**示例3：**

```
输入：m = 7, n = 3
输出：28
```

**示例4：**

```
输入：m = 3, n = 3
输出：6
```

**提示：**

- 1 <= `m`, `n` <= 100
- 题目数据保证答案小于等于`2 * 10⁹`

#### Solution

本题中我们需要使用一个二维数组`dp[][]`，其中每一个元素`dp[i][j]`表示从`[0][0]`到`[i][j]`位置有多少种路径。根据题意我们只能从左边`[i-1][j]`或者上边`[i][j-1]`前往`[i][j]`，因此动态规划的递推关系为：

- `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

接下来考虑初始条件。对于数组`dp[][]`的第一行和第一列，我们只能一直向右或是一直向下前进，即只有一条路径。因此我们可以把数组`dp[][]`的所有元素都初始化为`1`，然后从`dp[1][1]`开始向右和向下进行递推。

[题目链接](https://leetcode.cn/problems/unique-paths/)：

python代码：

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[1 for _ in range(n)] for _ in range(m)]

        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]

        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));

        for (int i=0; i<m; ++i) dp[i][0] = 1;
        for (int i=0; i<n; ++i) dp[0][i] = 1;

        for (int i=1; i<m; ++i) {
            for (int j=1; j<n; ++j) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }

        return dp[m-1][n-1];
    }
};
```


当然本题的空间复杂度也可以进行优化，实际上我们只需要维护`dp[][]`的一行或者一列即可。

[题目链接](https://leetcode.cn/problems/unique-paths/)：

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [1 for i in range(n)]

        for i in range(1, m):
            for j in range(1, n):
                dp[j] += dp[j-1]

        return dp[-1]
```


### 63. 不同路径 II

一个机器人位于一个`m x n`网格的左上角(起始点在下图中标记为“Start”)。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角(在下图中标记为“Finish”)。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用`1`和`0`来表示。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/11/04/robot1.jpg">
</div>

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/11/04/robot2.jpg">
</div>

```
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

**提示：**

- `m` == `obstacleGrid.length`
- `n` == `obstacleGrid[i].length`
- 1 <= `m, n` <= 100
- `obstacleGrid[i][j]`为`0`或`1`

#### Solution

本题类似于[不同路径](/leetcode/2023-03-13-DynamicProgramming.html#62-不同路径)，不过由于障碍的存在我们在初始化和递推时需要一些额外的注意。在初始化时我们不能直接将`dp[][]`的第一行和第一列初始化为`1`，而是要通过从左到右从上到下的顺序进行初始化。如果在某个位置遇到了障碍后面的位置会被挡住，因此我们需要结束初始化。完成初始化后需要按照递推公式进行递推：

- `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

需要注意的是我们同样需要越过有障碍的位置，让有障碍的位置满足`dp[i][j] == 0`。

[题目链接](https://leetcode.cn/problems/unique-paths-ii/)：

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if obstacleGrid[0][0]:
            return 0
        
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])

        dp = [[0 for _ in range(n)] for _ in range(m)]
        dp[0][0] = 1

        for i in range(m):
            if obstacleGrid[i][0]:
                break
            dp[i][0] = 1
        
        for j in range(n):
            if obstacleGrid[0][j]:
                break
            dp[0][j] = 1

        for i in range(1, m):
            for j in range(1, n):
                if obstacleGrid[i][j] == 0:
                    dp[i][j] = dp[i-1][j] + dp[i][j-1]
        
        return dp[-1][-1]
```


使用一维数组来进行递推的解法可参考如下。

[题目链接](https://leetcode.cn/problems/unique-paths-ii/)：

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if obstacleGrid[0][0]:
            return 0
        
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])

        dp = [0 for _ in range(n)]
        dp[0] = 1

        ## initialize first row
        for j in range(n):
            if obstacleGrid[0][j]:
                break
            dp[j] = 1

        for i in range(1, m):
            for j in range(n):
                if obstacleGrid[i][j]:
                    dp[j] = 0
                elif j > 0:
                    dp[j] += dp[j-1]
        
        return dp[-1]
```


### 343. 整数拆分

给定一个正整数`n`，将其拆分为`k`个**正整数**的和(`k` >= 2)，并使这些整数的乘积最大化。

返回你可以获得的**最大乘积**。

**示例1：**

```
输入: n = 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

**示例2：**

```
输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

**提示：**

- 2 <= `n` <= 58

#### Solution

本题的难点在于动态规划的递推关系。我们建立一个数组`dp[]`，其中`dp[i]`表示整数`i`可以拆分成的最大乘积。显然有初始条件：

- `dp[2]` = 1

接下来考虑递推关系。假设正整数`i`拆分出的第一个数为`j`，则`dp[i]`有以下两种可能：

1. 将`i`拆分为`j`和`i-j`，此时乘积为`j*(i-j)`
2. 继续对`i-j`进行拆分，这部分乘积的最大值为`dp[i-j]`，因此拆分后所有部分的乘积为`j*dp[i-j]`

因此我们需要对这两种情况的乘积取最大值，得到递推关系：

- `dp[i] = max([dp[i], j*(i-j), j*dp[i-j]])`

这样只需要在递推时对`j`进行遍历然后一直取`dp[i]`的最大值即可。

[题目链接](https://leetcode.cn/problems/integer-break/)：

python代码：

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        dp = [0 for _ in range(n+1)]
        dp[2] = 1

        for i in range(2, n+1):
            for j in range(1, i):
                dp[i] = max(dp[i], j*(i-j), j*dp[i-j])

        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1, 0);
        dp[2] = 1;

        for (int i=2; i<=n; ++i) {
            for (int j=1; j<i; ++j) {
                dp[i] = max(dp[i], max(dp[i-j]*j, j*(i-j)));
            }
        }

        return dp[n];
    }
};
```



### 96. 不同的二叉搜索树

给你一个整数`n`，求恰由`n`个节点组成且节点值从`1`到`n`互不相同的**二叉搜索树**有多少种？返回满足题意的二叉搜索树的种数。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg">
</div>

```
输入：n = 3
输出：5
```

**示例2：**

```
输入：n = 1
输出：1
```

**提示：**

- 1 <= `n` <= 19

#### Solution

本题的难点在于动态规划的递推关系。我们建立一个数组`dp[]`，其中`dp[i]`表示`[1, i]`所能构成的最多二叉搜索树的数目。假设某一个二叉搜索树的根节点为`1 <= j <= i`，则其左子树的可能数量为`dp[j-1]`，而右子树的数量则为`dp[i-j]`。这样以`j`为根节点的二叉搜索树的数目为`dp[j-1] * dp[i-j]`，我们对`j`进行遍历可以得到递推关系：

- `dp[i] += dp[j-1] * dp[i-j]`

接下来考虑初始条件。显然由`1`只能构成一棵二叉搜索树，而空节点也只能对应一棵二叉搜索树，即：

- `dp[0] = 1`，`dp[1] = 1`

这样只需要对`i`进行递推，同时对`j`进行遍历即可。

[题目链接](https://leetcode.cn/problems/unique-binary-search-trees/)：

python代码：

```python
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [0 for _ in range(n+1)]
        dp[0] = 1
        dp[1] = 1

        for i in range(2, n+1):
            for j in range(1, i+1):
                dp[i] += dp[j-1] * dp[i-j]
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n+1, 0);
        dp[0] = 1; dp[1] = 1;

        for (int i=2; i<=n; ++i) {
            for (int j=1; j<=i; ++j) {
                dp[i] += dp[j-1] * dp[i-j];
            }
        }

        return dp[n];
    }
};
```


## 背包问题

### 01背包

**01背包问题(01 knapsack problem)**是背包问题中最基础的问题，它可以表述为一共有`N`件物品其中每个物品的重量和价值分别为`w[i]`和`v[i]`。每件物品**只能用一次**，我们的目标是在保证物品的总重量不超过背包上限`W`的情况下计算包内物品的最大价值。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/Lr5L3HI.png" width="50%">
</div>

使用动态规划求解01背包问题时我们需要建立一个二维数组`dp[][]`，它的行数等于物品数、列数等于背包的容量、每个元素`dp[i][j]`表示背包容量为`j`且选择了`[0:i]`号物品情况下背包中物品的最大价值。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/zeQnAmr.png" width="70%">
</div>

接下来考虑递推关系。`dp[i][j]`对于物品`i`存在两种情况：

1. 物品`i`不在背包中，此时放不放物品`i`都不会影响包内的最大价值，即`dp[i][j] = dp[i-1][j]`
2. 物品`i`在背包中，此时包内的价值等于选择了`[0:i-1]`号物品且包内容量为`j-w[i]`情况下的最大价值加上物品`i`的价值，即`dp[i][j] = dp[i-1][j-w[i]] + v[i]`

以上两种情况取最大值，进而得到递推公式：

- `dp[i][j] = max(dp[i-1][j], dp[i-1][j-w[i]] + v[i])`

然后考虑`dp[][]`数组的初始化。显然当背包容量为`0`时无法放入任何物品，即最大价值为`0`。因此我们需要将`dp[][]`数组的第一列初始化为`0`，当然也可以直接把所有元素都初始化为`0`。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/1ONEPyC.png" width="70%">
</div>

而第一行要根据`0`号物品的重量来考虑，当`j >= w[0]`时令`dp[0][j] = v[0]`。

```python
for j in range(W):
    if j >= w[0]:
        dp[0][j] = v[0]
```

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/HcT1DTY.png" width="70%">
</div>

最后从左上角向右向下进行递推即可，`dp[][]`数组的最后一个元素即为所求。

```python
for i in range(1, N):
    for j in range(1, W+1):
        if j >= w[i]:
            dp[i][j] = max(dp[i-1][j], dp[i-1][j-w[i]] + v[i])
        else:
            dp[i][j] = dp[i-1][j]
```

当然上述递推过程可以优化为只使用一维`dp[]`数组，实际上我们只需要维护原来二维数组的一行即可。此时的递推公式为：

- `dp[j] = max(dp[j], dp[j - w[i]] + v[i])`

不过需要注意的是一维的情况必须要**倒序遍历容量`j`**，这样才能保证物品只会被添加一次。

```python
for i in range(N):
    for j in range(W, w[i] - 1, -1):
        dp[j] = max(dp[j], dp[j - w[i]] + v[i])
```

### 416. 分割等和子集

给你一个**只包含正整数**的**非空**数组`nums`。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**示例1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

**提示：**

- 1 <= `nums.length` <= 200
- 1 <= `nums[i]` <= 100

#### Solution

本题可以转换为在`nums`中寻找一个子集，使其和为`nums`总和的一半`target = sum(nums) // 2`。我们可以把这样的问题抽象为一个01背包问题，它的背包容量为`target`且每个物品的价值和重量都是`nums[i]`。如果背包恰好装满、最大价值恰好为`target`则说明可以把`nums`分割为等和的子集。

[题目链接](https://leetcode.cn/problems/partition-equal-subset-sum/)：

python代码：

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        N = len(nums)
        if N < 2:
            return False

        if sum(nums) % 2 :
            return False
        
        target = sum(nums) // 2

        dp = [[0 for j in range(target+1)] for i in range(N)]

        for j in range(target+1):
            if j >= nums[0]:
                dp[0][j] = nums[0]

        for i in range(1, N):
            for j in range(1, target+1):
                if j >= nums[i]:
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j - nums[i]]+nums[i])
                else:
                    dp[i][j] = dp[i-1][j]
        
        return dp[-1][-1] == target
```


C++代码：

```c++
int sum(vector<int>& nums) {
    int res = 0;
    for (int x : nums) res += x;

    return res;
}

class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int N = nums.size();
        if (N == 1) return false;

        int s = sum(nums);
        if (s % 2 == 1) return false;

        int target = s / 2;
        vector<vector<int>> dp(N, vector<int>(target+1, 0));

        for (int i=0; i<=target; ++i) {
            if (i >= nums[0]) dp[0][i] = nums[0];
        }

        for (int i=1; i<N; ++i) {
            for (int j=1; j<=target; ++j) {
                if (j >= nums[i]) {
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-nums[i]]+nums[i]);
                } else dp[i][j] = dp[i-1][j];
            }
        }

        return dp[N-1][target] == target;
    }
};
```


本题的一维数组递推代码可参考如下。

[题目链接](https://leetcode.cn/problems/partition-equal-subset-sum/)：

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        N = len(nums)
        if N < 2:
            return False

        if sum(nums) % 2 :
            return False
        
        target = sum(nums) // 2

        dp = [0 for j in range(target+1)]

        for i in range(N):
            for j in range(target, nums[i]-1, -1):
                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])
        
        return dp[-1] == target
```


### 1049. 最后一块石头的重量 II

有一堆石头，用整数数组`stones`表示。其中`stones[i]`表示第`i`块石头的重量。

每一回合，从中选出任意两块石头，然后将它们一起粉碎。假设石头的重量分别为`x`和`y`，且`x <= y`。那么粉碎的可能结果如下：

- 如果`x == y`，那么两块石头都会被完全粉碎；
- 如果`x != y`，那么重量为`x`的石头将会完全粉碎，而重量为`y`的石头新重量为`y-x`。

最后，**最多只会剩下一块**石头。返回此石头**最小的可能重量**。如果没有石头剩下，就返回`0`。

**示例1：**

```
输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
```

**示例2：**

```
输入：stones = [31,26,33,21,40]
输出：5
```

**提示：**

- 1 <= `stones.length` <= 30
- 1 <= `stones[i]` <= 100

#### Solution

本题可以理解为将`stones`分成两个和尽可能相等的子集，这样就可以使用的[分割等和子集](/leetcode/2023-03-13-DynamicProgramming.html#416-分割等和子集)思路来进行求解。此时背包的最大容量为`target = sum(stones) // 2`，每件物品的价值和重量都是`stones[i]`，背包容量为`j`时使用前`i`个物品的最大价值记录在二维数组`dp[i][j]`中。这样结束递推后背包中的最大价值为`dp[-1][-1]`，它表示将`stones`分成两个和尽可能相等的子集后其中较小的子集和。此时的返回值为`sum(stones) - 2*dp[-1][-1]`。

[题目链接](https://leetcode.cn/problems/last-stone-weight-ii/)：

python代码：

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        N = len(stones)
        target = sum(stones) // 2

        dp = [[0 for j in range(target+1)] for i in range(N)]

        for j in range(target+1):
            if j >= stones[0]:
                dp[0][j] = stones[0]

        for i in range(1, N):
            for j in range(1, target+1):
                if j >= stones[i]:
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j - stones[i]]+stones[i])
                else:
                    dp[i][j] = dp[i-1][j]
        
        return sum(stones) - 2*dp[-1][-1]
```


C++代码：

```c++
int sum(vector<int>& nums) {
    int res = 0;
    for (int x : nums) res += x;

    return res;
}

class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int N = stones.size();
        int s = sum(stones);
        int target = s / 2;

        vector<vector<int>> dp(N, vector<int>(target+1, 0));
        for (int j=0; j<=target; ++j) {
            if (j >= stones[0]) dp[0][j] = stones[0];
        }

        for (int i=1; i<N; ++i) {
            for (int j=1; j<=target; ++j) {
                if (j >= stones[i]) 
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-stones[i]]+stones[i]);
                else dp[i][j] = dp[i-1][j];
            }
        }

        return s-2*dp[N-1][target];
    }
};
```


本题的一维数组递推代码可参考如下。

[题目链接](https://leetcode.cn/problems/last-stone-weight-ii/)：

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        N = len(stones)
        target = sum(stones) // 2

        dp = [0 for j in range(target+1)]

        for i in range(N):
            for j in range(target, stones[i]-1,-1):
                dp[j] = max(dp[j], dp[j - stones[i]] + stones[i])
        
        return sum(stones) - 2*dp[-1]
```


### 494. 目标和

给你一个整数数组`nums`和一个整数`target`。

向数组中的每个整数前添加`'+'`或`'-'`，然后串联起所有整数，可以构造一个**表达式**：

- 例如，`nums = [2, 1]`，可以在`2`之前添加`'+'`，在`1`之前添加`'-'`，然后串联起来得到表达式`"+2-1"`。

返回可以通过上述方法构造的、运算结果等于`target`的不同**表达式**的数目。

**示例1：**

```
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**示例2：**

```
输入：nums = [1], target = 1
输出：1
```

**提示：**

- 1 <= `nums.length` <= 20
- 0 <= `nums[i]` <= 1000
- 0 <= `sum(nums[i])` <= 1000
- -1000 <= `target` <= 1000

#### Solution

本题中我们需要把`nums`中的元素分为两组，其中正数部分的和为`pos`而负数部分的和为`neg`，且它们满足：

- `pos + neg = sum(nums)`
- `pos - neg = target`

这样我们就可以求出`pos = (target + sum(nums)) // 2`。此时本题就转换为在`nums`中选择一个子集使其和为`pos`，而我们的目标则是找到这样的子集的数目。

实际上这样的问题可以看成是01背包问题的一种变体。我们建立一个二维数组`dp[][]`，它有`N+1`行`pos+1`列。其中元素`dp[i+1][j]`表示数组中前`i`个元素和为`j`的子集数目，这样可以得到递推关系：

- `dp[i+1][j] = dp[i][j] + dp[i][j - nums[i]]`

在进行初始化时我们需要令`dp[0][0] = 1`，它表示没有任何元素选中时空集的和恰为`0`，因此只存在这一种子集满足要求。接下来就只需要完成递推并返回`dp[-1][-1]`即可。

[题目链接](https://leetcode.cn/problems/target-sum/)：

python代码：

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        sumNums = sum(nums)

        if abs(target) > sumNums or (sumNums + target) % 2:
            return 0
        
        pos = (sumNums + target) // 2

        N = len(nums)
        dp = [[0 for j in range(pos+1)] for i in range(N+1)]
        dp[0][0] = 1

        for i in range(1, N+1):
            num = nums[i-1]
            for j in range(pos+1):
                dp[i][j] = dp[i-1][j]

                if j >= num:
                    dp[i][j] += dp[i-1][j-num]

        return dp[-1][-1]
```


C++代码：

```c++
int sum(vector<int>& nums) {
    int res = 0;
    for (int x : nums) res += x;

    return res;
}

class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int N = nums.size();
        int s = sum(nums);

        if (abs(target) > s || (s+target) % 2 == 1) return 0;

        int pos = (s + target) / 2;

        vector<vector<int>> dp(N+1, vector<int>(pos+1, 0));
        dp[0][0] = 1;

        for (int i=1; i<=N; ++i) {
            int num = nums[i-1];

            for (int j=0; j<=pos; ++j) {
                dp[i][j] = dp[i-1][j];

                if (j >= num) dp[i][j] += dp[i-1][j-num];
            }
        }

        return dp[N][pos];
    }
};
```


本题的一维数组递推代码可参考如下。

[题目链接](https://leetcode.cn/problems/target-sum/)：

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        sumNums = sum(nums)

        if abs(target) > sumNums or (sumNums + target) % 2:
            return 0
        
        pos = (sumNums + target) // 2

        N = len(nums)
        dp = [0 for j in range(pos+1)]
        dp[0] = 1

        for i in range(N):
            for j in range(pos, nums[i] - 1, -1):
                dp[j] += dp[j - nums[i]]

        return dp[-1]
```


### 474. 一和零

给你一个二进制字符串数组`strs`和两个整数`m`和`n`。

请你找出并返回`strs`的最大子集的长度，该子集中**最多**有`m`个`0`和`n`个`1`。

如果`x`的所有元素也是`y`的元素，集合`x`是集合`y`的**子集**。

**示例1：**

```
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```

**示例2：**

```
输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
```

**提示：**

- 1 <= `strs.length` <= 600
- 1 <= `strs[i].length` <= 100
- `strs[i]`仅由`'0'`和`'1'`组成
- 1 <= `m`, `n` <= 100

#### Solution

本题中我们需要建立一个三维`dp[][][]`数组来进行递推。`dp[i+1][j][k]`表示使用前`i`个字符串能够得到的最多包含`j`个`0`和`k`个`1`的子集大小，显然这样的`dp[][][]`数组可以把每个元素都初始化为`0`。接下来考虑递推关系，对于第`i`个字符串有两种可能性：

1. 第`i`个字符串不在满足要求的子集中，此时`dp[i+1][j][k] = dp[i][j][k]`
2. 第`i`个字符串在满足要求的子集中，此时`dp[i+1][j][k] = dp[i][j-zeros][k-ones]+1`，其中`zeros`和`ones`分别为第`i`个字符串中的`0`和`1`数量

这样我们就需要对以上两种情况取最大值，得到递推关系：

- `dp[i+1][j][k] = max(dp[i][j][k], dp[i][j-zeros][k-ones]+1)`

然后通过三重循环进行递推即可。

[题目链接](https://leetcode.cn/problems/ones-and-zeroes/)：

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        N = len(strs)

        dp = [[[0 for k in range(n+1)] for j in range(m+1)]for i in range(N+1)]

        for i in range(1, N+1):
            zeros= strs[i-1].count("0")
            ones = strs[i-1].count("1")

            for j in range(m+1):
                for k in range(n+1):
                    dp[i][j][k] = dp[i-1][j][k]

                    if j >= zeros and k >= ones:
                        dp[i][j][k] = max(dp[i][j][k], dp[i-1][j-zeros][k-ones]+1)
        
        return dp[-1][-1][-1]
```


本题的二维数组递推代码可参考如下。

[题目链接](https://leetcode.cn/problems/ones-and-zeroes/)：

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        N = len(strs)

        dp = [[0 for k in range(n+1)] for j in range(m+1)]

        for i in range(N):
            zeros= strs[i].count("0")
            ones = strs[i].count("1")

            for j in range(m, zeros-1, -1):
                for k in range(n, ones-1, -1):
                    dp[j][k] = max(dp[j][k], dp[j-zeros][k-ones]+1)
        
        return dp[-1][-1]
```


### 完全背包

完全背包问题与[01背包问题](/leetcode/2023-03-13-DynamicProgramming.html#01背包)是类似的，唯一的区别在于完全背包问题中我们可以无限次地向背包中添加任意物品，只要背包的重量满足要求即可。这一区别需要我们修改递推关系如下：

- `dp[i][j] = max(dp[i-1][j], dp[i][j - w[i]] + v[i])`

注意上式中我们在取最大值时使用了**同一行**的递推结果`dp[i][j - w[i]]`。更常见的形式是只使用一维`dp[]`数组来进行递推：

- `dp[j] = max(dp[j], dp[j - w[i]] + v[i])`

而在进行递推时也无需逆序，按照从左到右的顺序进行递推即可。

```python
for i in range(N):
    for j in range(w[i], W+1):
        dp[j] = max(dp[j], dp[j-w[i]] + v[i])
```

### 518. 零钱兑换 II

给你一个整数数组`coins`表示不同面额的硬币，另给一个整数`amount`表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回`0`。

假设每一种面额的硬币有无限个。

题目数据保证结果符合32位带符号整数。

**示例1：**

```
输入：amount = 5, coins = [1, 2, 5]
输出：4
解释：有四种方式可以凑成总金额：
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**示例2：**

```
输入：amount = 3, coins = [2]
输出：0
解释：只用面额 2 的硬币不能凑成总金额 3 。
```

**示例3：**

```
输入：amount = 10, coins = [10] 
输出：1
```

**提示：**

- 1 <= `coins.length` <= 300
- 1 <= `coins[i]` <= 5000
- `coins`中的所有值**互不相同**
- 0 <= `amount` <= 5000

#### Solution

本题是完全背包问题的一个变体，当题干中出现**无限个**这样的字眼时就可以考虑使用完全背包的框架来处理。本题中的`dp[j]`表示使用当前的硬币集合凑成`j`金额的所有可能组合数，递推公式类似于[目标和](/leetcode/2023-03-13-DynamicProgramming.html#494-目标和)：

- `dp[j] += dp[j - coins[i]]`

它表示凑出`j`金额包括只使用前`i-1`个硬币，以及一定使用第`i`个硬币两种情况的所有组合方式之和。

还需要注意的是初始化，我们需要把`dp[]`数组的第一个元素初始化为`dp[0] = 1`，它表示目标金额为`0`且不能使用任何硬币时我们只有`1`种组合方式。最后按照完全背包的框架进行递推即可。

[题目链接](https://leetcode.cn/problems/coin-change-ii/)：

python代码：

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        N  = len(coins)

        dp = [0 for j in range(amount+1)]
        dp[0] = 1

        for i in range(N):
            for j in range(coins[i], amount+1):
                dp[j] += dp[j - coins[i]]

        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int N = coins.size();
        vector<int> dp(amount+1);
        dp[0] = 1;

        for (int i=0; i<N; ++i) {
            for (int j=coins[i]; j<=amount; ++j) {
                dp[j] += dp[j-coins[i]];
            }
        }

        return dp[amount];
    }
};
```


### 377. 组合总和 IV

给你一个由**不同**整数组成的数组`nums`，和一个目标整数`target`。请你从`nums`中找出并返回总和为`target`的元素组合的个数。

**示例1：**

```
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```

**示例2：**

```
输入：nums = [9], target = 3
输出：0
```

**提示：**

- 1 <= `nums.length` <= 200
- 1 <= `nums[i]` <= 1000
- `nums`中的所有元素**互不相同**
- 1 <= `target` <= 1000

#### Solution

本题类似于[零钱兑换 II](/leetcode/2023-03-13-DynamicProgramming.html#518-零钱兑换-ii)但这里需要考虑不同数字的顺序，因此本题实际上需要计算的是满足和为`target`子集的**排列**。

计算排列与组合的唯一区别在于递推时遍历的顺序：

- 计算**组合**需要先对背包中的**物品(零钱、数字)**进行遍历，然后再对背包的**容量(金额、数字总和)**进行遍历
- 计算**排列**需要先对背包的**容量(金额、数字总和)**进行遍历，然后再对背包中的**物品(零钱、数字)**进行遍历

[题目链接](https://leetcode.cn/problems/combination-sum-iv/)：

python代码：

```python
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        if target < min(nums):
            return 0
        
        dp = [0 for j in range(target+1)]
        dp[0] = 1

        for i in range(target+1):
            for num in nums:
                if i >= num:
                    dp[i] += dp[i - num]
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        int N = nums.size();

        vector<int> dp(target+1, 0);
        dp[0] = 1;

        for (int i=0; i<=target; ++i) {
            for (int num : nums) {
                if (i >= num && dp[i] < INT_MAX - dp[i - num])
                    dp[i] += dp[i-num];
            }
        }

        return dp[target];
    }
};
```


### 322. 零钱兑换

给你一个整数数组`coins`，表示不同面额的硬币；以及一个整数`amount`，表示总金额。

计算并返回可以凑成总金额所需的**最少的硬币个数**。如果没有任何一种硬币组合能组成总金额，返回`-1`。

你可以认为每种硬币的数量是无限的。

**示例1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例3：**

```
输入：coins = [1], amount = 0
输出：0
```

**提示：**

- 1 <= `coins.length` <= 12
- 1 <= `coins[i]` <= 2³¹ - 1
- 0 <= `amount` <= 10⁴

#### Solution

本题是[零钱兑换 II](/leetcode/2023-03-13-DynamicProgramming.html#518-零钱兑换-ii)的变体，我们的目标是计算和为`amount`的最小子集。由于是要计算出最小子集的大小，我们需要在递推时取最小值：

- `dp[j] = min(dp[j], dp[j - coins[i]])`

同时在初始化时将`dp[]`数组初始化为无穷大`float("inf")`。除此之外还需要注意的是`amount = 0`时不需要使用任何硬币，即`dp[0] = 0`。完成准备工作后直接进行递推即可。

[题目链接](https://leetcode.cn/problems/coin-change/)：

python代码：

```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        if amount == 0:
            return 0
        
        dp = [float("inf") for j in range(amount+1)]
        dp[0] = 0

        for coin in coins:
            for j in range(coin, amount+1):
                dp[j] = min(dp[j], dp[j-coin]+1)
        
        if dp[-1] == float("inf"):
            return -1
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int N = coins.size();
        vector<int> dp(amount+1, INT_MAX);
        dp[0] = 0;

        for (int coin : coins) {
            for (int j=coin; j<=amount; ++j) {
                if (dp[j-coin] < INT_MAX)
                    dp[j] = min(dp[j], dp[j-coin]+1);
            }
        }

        if (dp[amount] == INT_MAX) return -1;

        return dp[amount];
    }
};
```


### 279. 完全平方数

给你一个整数`n`，返回**和为`n`**的完全平方数的最少数量。

**完全平方数**是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9和16都是完全平方数，而3和11不是。

**示例1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

**提示：**

- 1 <= `n` <= 10⁴

#### Solution

本题类似于[零钱兑换](/leetcode/2023-03-13-DynamicProgramming.html#322-零钱兑换)，不过在进行递推时我们需要先确定完全平方数的范围。这里我们使用一个列表`nums[]`来记录所有平方小于等于`n`的正整数作为背包中的元素，然后按照[零钱兑换](/leetcode/2023-03-13-DynamicProgramming.html#322-零钱兑换)的思路进行递推即可。

[题目链接](https://leetcode.cn/problems/perfect-squares/)：

python代码：

```python
class Solution:
    def numSquares(self, n: int) -> int:
        nums = [i*i for i in range(1, n+1) if i*i <= n]

        dp = [float("inf") for j in range(n+1)]
        dp[0] = 0

        for num in nums:
            for j in range(num, n+1):
                dp[j] = min(dp[j], dp[j-num]+1)
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1, INT_MAX);
        dp[0] = 0;

        for (int i=1; i*i <= n; ++i) {
            for (int j = i*i; j <= n; ++j) {
                dp[j] = min(dp[j], dp[j-i*i]+1);
            }
        }

        return dp[n];
    }
};
```


### 139. 单词拆分

给你一个字符串`s`和一个字符串列表`wordDict`作为字典。请你判断是否可以利用字典中出现的单词拼接出`s`。

**注意：**不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

**示例1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```

**示例2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

**示例3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

**提示：**

- 1 <= `s.length` <= 300
- 1 <= `wordDict.length` <= 1000
- 1 <= `wordDict[i].length` <= 20
- `s`和`wordDict[i]`仅有小写英文字母组成
- `wordDict`中的所有字符串**互不相同**

#### Solution

本题可以看做是使用`wordDict`的一个子集通过**排列**来得到`s`，因此我们可以参考的[组合总和 IV](/leetcode/2023-03-13-DynamicProgramming.html#377-组合总和-iv)思路来进行处理。首先明确`dp[]`数组的意义，`dp[j]`表示使用`wordDict`的一个子集能否排列出`s`的前`j`个字符。同时由于本题是**排列**问题，我们需要先对字符串`s`进行遍历，然后再对单词`word`进行遍历。

接下来考虑递推关系，当我们要使用一个新的单词`word`时会有两种情况：

1. 不使用`word`作为末尾字符，此时`dp[j] = dp[j]`
2. 使用`word`作为末尾字符，此时`dp[j] = dp[j - len(word)] and word == s[j - len(word):j]`

两种情况中只要有一种可以构成排列即可，因此有：

- `dp[j] = dp[j] or (dp[j - len(word)] and word == s[j - len(word):j])`

`dp[]`数组的初始化与[组合总和 IV](/leetcode/2023-03-13-DynamicProgramming.html#377-组合总和-iv)类似，我们把整个数组都初始化为`False`然后令`dp[0] = True`，它表示空字符串`""`可以不用任何单词来构成。完成准备工作后进行递推即可。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/6K9QJQY.png" width="70%">
</div>

[题目链接](https://leetcode.cn/problems/word-break/)：

python代码：

```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        N = len(s)

        dp = [False for i in range(N+1)]
        dp[0] = True

        for j in range(1, N+1):
            for word in wordDict:
                if j >= len(word):
                    dp[j] = dp[j] or (dp[j - len(word)] and word == s[j - len(word):j])
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        vector<bool> dp(s.size() + 1, false);

        dp[0] = true;

        for (int i=1; i<=s.size(); ++i) {
            for (int j=0; j<i; ++j) {
                string word = s.substr(j, i-j);

                if (wordSet.find(word) != wordSet.end() && dp[j])
                    dp[i] = true;
            }
        }

        return dp[s.size()];
    }
};
```


## 打家劫舍

### 198. 打家劫舍

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你**不触动警报装置的情况下**，一夜之内能够偷窃到的最高金额。

**示例1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**提示：**

- 1 <= `nums.length` <= 100
- 0 <= `nums[i]` <= 400

#### Solution

本题的实质是在`nums`中选择一个子集并最大化子集的和，同时要求子集中的元素互不相邻。在使用动态规划进行求解时我们首先要明确`dp[]`数组的意义，`dp[i]`表示打劫前`i`个房间所能得到的最高金额。那么`dp[i]`在进行递推时需要考虑以下两种情况：

1. 跳过当前房屋，此时`dp[i] = dp[i-1]`
2. 打劫当前房屋，在这种情况下无法打劫前一间房屋，此时`dp[i] = dp[i-2] + nums[i]`

显然最大化金额需要对这两种情况取最大值，从而得到递推关系：

- `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

接下来考虑初始化，显然`dp[]`数组可以先都初始化为`0`。其第一个元素表示打劫第一间房屋，因此有`dp[0] = nums[0]`；而第二个元素表示前两间房屋所能构成的最大金额，即`dp[1] = max(nums[0], nums[1])`。完成准备工作后进行递推即可。

[题目链接](https://leetcode.cn/problems/house-robber/)：

python代码：

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        N = len(nums)
        
        if N == 1:
            return nums[0]
        elif N == 2:
            return max(nums)

        dp = [0 for i in range(N)]
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])

        for i in range(2, N):
            dp[i] = max(dp[i-1], dp[i-2] + nums[i])
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        else if (nums.size() == 1) return nums[0];
        else if (nums.size() == 2) return max(nums[0], nums[1]);

        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);

        for (int i=2; i<nums.size(); ++i) {
            dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
        }

        return dp[nums.size()-1];
    }
};
```


### 213. 打家劫舍 II

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都**围成一圈**，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你**在不触动警报装置的情况下**，今晚能够偷窃到的最高金额。

**示例1：**

```
输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**示例2：**

```
输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例3：**

```
输入：nums = [1,2,3]
输出：3
```

**提示：**

- 1 <= `nums.length` <= 100
- 0 <= `nums[i]` <= 1000

#### Solution

本题与[打家劫舍](/leetcode/2023-03-13-DynamicProgramming.html#198-打家劫舍)几乎完全一致，唯一的区别在于本题中`nums`是一个首尾相连的环形数组。实际上本题的关键在于是否需要选择第一个房间，如果选择了第一个房间则不可以选择最后一个房间；类似地，如果选择了最后一个房间则不可以选择第一个房间。在这种观察下我们可以把这个问题分解为两个独立的[打家劫舍](/leetcode/2023-03-13-DynamicProgramming.html#198-打家劫舍)进行处理，分别考虑排除首末元素的子列上的动态规划并取最大值即可。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/GbPTGw0.png" width="60%">
<img src="https://images.weserv.nl/?url=i.imgur.com/HVB9HGi.png" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/house-robber-ii/)：

python代码：

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        N = len(nums)

        if N == 1:
            return nums[0]
        elif N == 2:
            return max(nums)
        
        def _rob(nums: List[int]) -> int:
            N = len(nums)

            if N == 1:
                return nums[0]
            elif N == 2:
                return max(nums)
            
            dp = [0 for i in range(N)]
            dp[0] = nums[0]
            dp[1] = max(nums[0], nums[1])

            for i in range(2, N):
                dp[i] = max(dp[i-1], dp[i-2]+nums[i])

            return dp[-1]

        max1 = _rob(nums[:-1])
        max2 = _rob(nums[1:])

        return max(max1, max2)
```


C++代码：

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        else if (nums.size() == 1) return nums[0];
        else if (nums.size() == 2) return max(nums[0], nums[1]);
        
        return max(_rob(nums, 0, nums.size()-2), _rob(nums, 1, nums.size()-1));
    }

private:
    int _rob(vector<int>& nums, int start, int end) {
        int N = end - start + 1;
        vector<int> dp(N, 0);
        dp[0] = nums[start];
        dp[1] = max(nums[start], nums[start+1]);

        for (int i=2; i<N; ++i) {
            dp[i] = max(dp[i-1], dp[i-2] + nums[start+i]);
        }

        return dp[N-1];
    }
};
```


### 337. 打家劫舍 III

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为`root`。

除了`root`之外，每栋房子有且只有一个"父"房子与之相连。一番侦察之后，聪明的小偷意识到"这个地方的所有房屋的排列类似于一棵二叉树"。 如果**两个直接相连的房子在同一天晚上被打劫**，房屋将自动报警。

给定二叉树的`root`。返回**在不触动警报的情况下**，小偷能够盗取的最高金额。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg">
</div>

```
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg">
</div>

```
输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

**提示：**

- 树的节点数在`[1, 10⁴]`范围内
- 0 <= `Node.val` <= 10⁴

#### Solution

本题是打家劫舍系列最复杂的问题，需要我们把动态规划推广到二叉树上。这里我们首先定义每个节点都对应一个`dp[]`数组，`dp[0]`表示跳过当前节点所能盗取的最大金额，`dp[1]`则是打劫当前节点所能得到的最大金额。记左右子树的`dp[]`数组分别为`left`和`right`，当前节点`root`上`dp[]`数组存在如下递推关系：

- 选择跳过当前节点`root`时，最大金额为`left`和`right`两棵子树所能盗取的最大金额之和，即`dp[0] = max(left) + max(right)`
- 选择使用当前节点`root`时，最大金额为当前节点上的金额`root.val`与不使用左右子节点情况下的最大金额之和，即`dp[1] = root.val + left[0] + right[0]`

因此我们可以使用二叉树的[后序遍历](/leetcode/2023-02-03-BinaryTree.html#145-二叉树的后序遍历)来递推出整棵树的最大金额，然后返回`dp[]`数组中的较大值即可。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/urc43gP.png" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/house-robber-iii/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        def traversal(root):
            if not root:
                return (0, 0)
            
            left = traversal(root.left)
            right= traversal(root.right)

            ## skip root
            val1 = max(left) + max(right)
            ## use root
            val2 = root.val + left[0] + right[0]

            return (val1, val2)
        
        dp = traversal(root)
        return max(dp)
```


C++代码：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        vector<int> res = robTree(root);
        return max(res[0], res[1]);
    }

private:
    vector<int> robTree(TreeNode* root) {
        if (root == nullptr) return vector<int>{0, 0};

        vector<int> left  = robTree(root->left);
        vector<int> right = robTree(root->right);

        int val1 = max(left[0], left[1]) + max(right[0], right[1]);
        int val2 = root->val + left[0] + right[0];

        return {val1, val2};
    }
};
```



## 股票问题

### 121. 买卖股票的最佳时机

给定一个数组`prices`，它的第`i`个元素`prices[i]`表示一支给定股票第`i`天的价格。

你只能选择**某一天**买入这只股票，并选择在**未来的某一个不同的日子**卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回`0`。

**示例1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**提示：**

- 1 <= prices.length <= 10⁵
- 0 <= prices[i] <= 10⁴

#### Solution

股票问题中我们在每一天都有持有股票以及不持有股票两种状态，因此在使用动态规划进行求解时需要建立一个二维`dp[][]`数组，`dp[i][0]`和`dp[i][1]`分别表示在第`i`天不持有和持有股票两种情况的最大收益。接下来考虑递推关系：

- 如果第`i`天不持有股票，则在第`i-1`天存在存在持有股票和不持有股票两种情况：
  - 如果第`i-1`天不持有股票，则两天的收益相同`dp[i][0] = dp[i-1][0]`
  - 如果第`i-1`天持有股票，则需要在第`i`天将股票卖出`dp[i][0] = dp[i-1][1] + prices[i]`

- 如果第`i`天持有股票，同样需要考虑在第`i-1`天持有股票和不持有股票两种情况：
  - 如果第`i-1`天不持有股票，则需要在第`i`天将股票买入`dp[i][1] = -prices[i]`
  - 如果第`i-1`天持有股票，则两天的收益相同`dp[i][1] = dp[i-1][1]`

对这两种情况分别取最大值可以得到递推公式：

- `dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])`
- `dp[i][1] = max(-prices[i], dp[i-1][1])`

然后考虑初始化。我们可以先把整个`dp[][]`数组初始化为`0`，然后令第一天的收益为`dp[0] = [0, -prices[0]]`分别对应第一天不持有股票和持有股票两种情况。最后执行递推并且返回最后一天不持有股票的最大收益`dp[-1][0]`即可。

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)：

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        N = len(prices)
        dp = [[0, 0] for i in range(N)]
        dp[0] = [0, -prices[0]]

        for i in range(1, N):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
            dp[i][1] = max(-prices[i], dp[i-1][1])
        
        return dp[-1][0]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int N = prices.size();
        vector<vector<int>> dp(N, vector<int>(2, 0));
        dp[0][1] = -prices[0];

        for (int i=1; i<N; ++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = max(-prices[i], dp[i-1][1]);
        }

        return dp[N-1][0];
    }
};
```


当然上面的算法也可以进行优化，实际上我们只需维护一个一维数组进行递推即可。

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        N = len(prices)
        dp = [0, -prices[0]]

        for i in range(1, N):
            dp[0] = max(dp[0], dp[1] + prices[i])
            dp[1] = max(-prices[i], dp[1])
        
        return dp[0]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int N = prices.size();
        int dp[2] = {0, -prices[0]};

        for (int i=1; i<N; ++i) {
            dp[0] = max(dp[0], dp[1] + prices[i]);
            dp[1] = max(-prices[i], dp[1]);
        }

        return dp[0];
    }
};
```


### 122. 买卖股票的最佳时机 II

给你一个整数数组`prices`，其中`prices[i]`表示某支股票第`i`天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候**最多**只能持有**一股**股票。你也可以先购买，然后在**同一天**出售。

返回 你能获得的**最大**利润。

**示例1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```

**示例2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     总利润为 4 。
```

**示例3：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0 。
```

**提示：**

- 1 <= `prices.length` <= 3*10⁴
- 0 <= `prices[i]` <= 10⁴

#### Solution

本题和[买卖股票的最佳时机](/leetcode/2023-03-13-DynamicProgramming.html#121-买卖股票的最佳时机)的区别在于可以买卖多次股票，因此我们需要稍微修改一下递推关系：

- 如果第`i`天不持有股票，则在第`i-1`天存在存在持有股票和不持有股票两种情况：
  - 如果第`i-1`天不持有股票，则两天的收益相同`dp[i][0] = dp[i-1][0]`
  - 如果第`i-1`天持有股票，则需要在第`i`天将股票卖出`dp[i][0] = dp[i-1][1] + prices[i]`

- 如果第`i`天持有股票，同样需要考虑在第`i-1`天持有股票和不持有股票两种情况：
  - 如果第`i-1`天不持有股票，则需要在第`i`天将股票买入`dp[i][0] = dp[i-1][0] - prices[i]`
  - 如果第`i-1`天持有股票，则两天的收益相同`dp[i][1] = dp[i-1][1]`

注意在第`i-1`天不持有股票而第`i`天持有股票的情况，我们在第`i`天的收益是第`i-1`天不持有股票的收益`dp[i-1][0]`减去第`i`天的股价`prices[i]`。修改递推公式后按照[买卖股票的最佳时机](/leetcode/2023-03-13-DynamicProgramming.html#121-买卖股票的最佳时机)的流程进行递推即可。

- `dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])`
- `dp[i][1] = max(dp[i-1][0] - prices[i], dp[i-1][1])`

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)：

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        N = len(prices)

        dp = [[0, 0] for i in range(N)]
        dp[0] = [0, -prices[0]]

        for i in range(1, N):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i])
            dp[i][1] = max(dp[i-1][0] - prices[i], dp[i-1][1])
        
        return dp[-1][0]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int N = prices.size();
        vector<vector<int>> dp(N, vector<int>(2, 0));
        dp[0][1] = -prices[0];

        for (int i=1; i<N; ++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = max(dp[i-1][0] - prices[i], dp[i-1][1]);
        }

        return dp[N-1][0];
    }
};
```


本题的一维递推代码可以参考如下。

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)：

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        N = len(prices)

        dp = [0, -prices[0]]

        for i in range(1, N):
            dp = [max(dp[0], dp[1] + prices[i]), max(dp[0] - prices[i], dp[1])]
        
        return dp[0]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int N = prices.size();
        vector<int> dp(N, 0);

        for (int i=1; i<N; ++i) {
            dp[i] = dp[i-1] + max(0, prices[i] - prices[i-1]);
        }

        return dp[N-1];
    }
};
```


### 123. 买卖股票的最佳时机 III

给定一个数组，它的第`i`个元素是一支给定的股票在第`i`天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成**两笔**交易。

**注意：**你不能同时参与多笔交易(你必须在再次购买前出售掉之前的股票)。

**示例1：**

```
输入：prices = [3,3,5,0,0,3,1,4]
输出：6
解释：在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```

**示例2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。   
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例3：**

```
输入：prices = [7,6,4,3,1] 
输出：0 
解释：在这个情况下, 没有交易完成, 所以最大利润为 0。
```

**提示：**

- 1 <= `prices.length` <= 10⁵
- 0 <= `prices[i]` <= 10⁵

#### Solution

本题在[买卖股票的最佳时机 II](/leetcode/2023-03-13-DynamicProgramming.html#122-买卖股票的最佳时机-ii)的基础上添加了**最多进行两次交易**的限制，因此我们需要修改`dp[][]`数组的定义。这里`dp[i][]`需要包含未持有过任何股票、第一次持有股票、卖掉(过)第一次持有的股票且没有持有股票、第二次持有股票以及卖掉(过)第二次持有的股票五种情况：

- 如果第`i`天未持有过任何股票，则收益与第`i-1`天未持有过任何股票相同，即`dp[i][0] = dp[i-1][0]`

- 如果第`i`天第一次持有股票，则需要考虑在第`i-1`天第一次持有股票和未持有过股票两种情况：
  - 如果第`i-1`天不持有股票，则需要在第`i`天第一次购买股票`dp[i][0] = dp[i-1][0] - prices[i]`
  - 如果第`i-1`天持有股票，则两天的收益相同`dp[i][1] = dp[i-1][1]`

- 如果第`i`天卖掉(过)第一次持有的股票且没有持有股票，则需要考虑在第`i-1`天存在第一次持有股票和卖掉第一次持有的股票和两种情况：
  - 如果第`i-1`天第一次持有股票，则需要在第`i`天将股票卖出`dp[i][2] = dp[i-1][1] + prices[i]`
  - 如果第`i-1`天已经卖掉(过)第一次持有的股票，则两天的收益相同`dp[i][2] = dp[i-1][2]`

- 如果第`i`天第二次持有股票，则需要考虑在第`i-1`天卖掉第一次持有的股票和第二次持有股票两种情况：
  - 如果第`i-1`天卖掉第一次持有的股票，则需要在第`i`天第二次购买股票`dp[i][3] = dp[i-1][2] - prices[i]`
  - 如果第`i-1`天第二次持有股票，则两天的收益相同`dp[i][3] = dp[i-1][3]`

- 如果第`i`天卖掉第二次持有的股票，则需要考虑第`i-1`天第二次持有股票和卖掉第二次持有股票两种情况：
  - 如果第`i-1`天第二次持有股票，则需要在第`i`天第二次卖出股票`dp[i][4] = dp[i-1][3] + prices[i]`
  - 如果第`i-1`天卖掉(过)第二次持有股票，则两天的收益相同`dp[i][4] = dp[i-1][4]`

接下来考虑初始化的问题。显然在一开始没有持有任何股票的时候收益为`0`，第一次买入股票收益为`-prices[0]`，第一次卖掉(过)持有股票收益为`0`，第二次卖掉(过)持有股票收益为`0`，而难点在于如何初始化**第二次持有股票**这种情况。第二次买入依赖于第一次卖出的状态，其实相当于第0天第一次买入了，第一次卖出了，然后再买入一次，此时的收益为`-prices[0]`。这样可以得到`dp[][]`数组的初始化：

- `dp[0] = [0, -prices[0], 0, -prices[0], 0]`

完成所有准备工作后进行递推即可。

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)：

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        N = len(prices)

        dp = [[0 for _ in range(5)] for i in range(N)]
        dp[0] = [0, -prices[0], 0, -prices[0], 0]

        for i in range(1, N):
            dp[i][0] = dp[i-1][0]
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
            dp[i][2] = max(dp[i-1][2], dp[i-1][1] + prices[i])
            dp[i][3] = max(dp[i-1][3], dp[i-1][2] - prices[i])
            dp[i][4] = max(dp[i-1][4], dp[i-1][3] + prices[i])
        
        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int N = prices.size();
        vector<vector<int>> dp(N, vector<int>(5, 0));
        dp[0][1] = -prices[0];
        dp[0][3] = -prices[0];

        for (int i=1; i< N; ++i) {
            dp[i][0] = dp[i-1][0];
            dp[i][1] = max(dp[i-1][0] - prices[i], dp[i-1][1]);
            dp[i][2] = max(dp[i-1][1] + prices[i], dp[i-1][2]);
            dp[i][3] = max(dp[i-1][2] - prices[i], dp[i-1][3]);
            dp[i][4] = max(dp[i-1][3] + prices[i], dp[i-1][4]);
        }

        return dp[N-1][4];
    }
};
```


本题的一维递推代码可以参考如下。

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)：

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        N = len(prices)

        dp = [0, -prices[0], 0, -prices[0], 0]

        for i in range(1, N):
            dp = [
                    dp[0],
                    max(dp[1], dp[0] - prices[i]), 
                    max(dp[2], dp[1] + prices[i]),
                    max(dp[3], dp[2] - prices[i]),
                    max(dp[4], dp[3] + prices[i])
                  ]
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int N = prices.size();
        int dp[5] = {0, -prices[0], 0, -prices[0], 0};

        for (int i=1; i< N; ++i) {
            dp[1] = max(dp[0] - prices[i], dp[1]);
            dp[2] = max(dp[1] + prices[i], dp[2]);
            dp[3] = max(dp[2] - prices[i], dp[3]);
            dp[4] = max(dp[3] + prices[i], dp[4]);
        }

        return dp[4];
    }
};
```


### 188. 买卖股票的最佳时机 IV

给定一个整数数组`prices`，它的第`i`个元素`prices[i]`是一支给定的股票在第`i`天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成`k`笔交易。

**注意：**你不能同时参与多笔交易(你必须在再次购买前出售掉之前的股票)。

**示例1：**

```
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```

**示例2：**

```
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```

**提示：**

- 0 <= `k` <= 100
- 1 <= `prices.length` <= 1000
- 0 <= `prices[i]` <= 1000

#### Solution

本题是对[买卖股票的最佳时机 III](/leetcode/2023-03-13-DynamicProgramming.html#123-买卖股票的最佳时机-iii)的推广。不难发现当交易次数小于等于`k`时，`dp[][]`数组的列数为`2*k+1`，其第一列表示未进行任何交易可以获得的最大利润，而后一次表示第一次买入股票、第一次卖出(过)股票、第二次买入股票、第二次卖出(过)股票...后的最大利润。因此我们可以归纳出递推公式：

- `dp[i][0] = dp[i-1][0]`
- `dp[i][j] = max(dp[i-1][j], dp[i-1][j-1] - prices[i])`，`j`列对应买入股票
- `dp[i][j] = max(dp[i-1][j], dp[i-1][j-1] + prices[i])`，`j`列对应卖出股票

类似地，`dp[][]`数组需要初始化为：

- `dp[0][0] = 0`
- `dp[0][j] = -prices[0]`，`j`列对应买入股票
- `dp[0][j] = 0`，`j`列对应卖出股票

完成所有准备工作后进行递推即可。

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)：

python代码：

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        N = len(prices)
        dp = [[0 for _ in range(2*k+1)] for i in range(N)]
        dp[0] = [-prices[0] if (i % 2) else 0 for i in range(2*k+1)]

        for i in range(1, N):
            dp[i][0] = dp[i-1][0]
            for j in range(1, 2*k+1):
                dp[i][j] = max(dp[i-1][j], dp[i-1][j-1] + (-1)**j * prices[i])
        
        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int N = prices.size();
        vector<vector<int>> dp(N, vector<int>(2*k + 1, 0));

        for (int i=1; i<2*k; i+=2) {
            dp[0][i] = -prices[0];
        }

        for (int i=1; i<N; ++i) {
            for (int j=0; j<2*k; j+=2) {
                dp[i][j+1] = max(dp[i-1][j+1], dp[i-1][j] - prices[i]);
                dp[i][j+2] = max(dp[i-1][j+1] + prices[i], dp[i-1][j+2]);
            }
        }

        return dp[N-1][2*k];
    }
};
```


### 309. 最佳买卖股票时机含冷冻期

给定一个整数数组`prices`，其中第`prices[i]`表示第`i`天的股票价格。​

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易(多次买卖一支股票):

- 卖出股票后，你无法在第二天买入股票(即冷冻期为1天)。

**注意：**你不能同时参与多笔交易(你必须在再次购买前出售掉之前的股票)。

**示例1：**

```
输入: prices = [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

**示例2：**

```
输入: prices = [1]
输出: 0
```

**提示：**

- 1 <= `prices.length` <= 5000
- 1 <= `prices[i]` <= 1000

#### Solution

本题中需要考虑三种状态：持有股票、不持有股票但位于冷冻期中以及不持有股票且不位于冷冻期中。因此我们建立一个二维数组`dp[][]`，它的三列分别表示这三种状态下的最大收益：

- 如果第`i`天持有股票，需要考虑第`i-1`天持有股票或是不持有股票且不位于冷冻期两种情况：
  - 如果第`i-1`天持有股票，则两天的收益相同`dp[i][0] = dp[i-1][0]`
  - 如果第`i-1`天不持有股票且不位于冷冻期，则第`i`天需要买入股票`dp[i][0] = dp[i-1][2] - prices[i]`

- 如果第`i`天不持有股票但位于冷冻期中，则第`i-1`天需要持有股票且第`i`天一定卖出了股票`dp[i][1] = dp[i-1][0] + prices[i]`

- 如果第`i`天不持有股票且不位于冷冻期中，则需要考虑第`i-1`天不持有股票但位于冷冻期中或是不持有股票且不位于冷冻期两种情况：
  - 如果第`i-1`天不持有股票但位于冷冻期中，则两天收益相同`dp[i][1] = dp[i-1][1]`
  - 如果第`i-1`天不持有股票且不位于冷冻期中，则两天收益相同`dp[i][1] = dp[i-1][2]`

这样可以得到递推公式：

- `dp[i][0] = max(dp[i-1][0], dp[i-1][2] - prices[i])`
- `dp[i][1] = dp[i-1][0] + prices[i]`
- `dp[i][2] = max(dp[i-1][1], dp[i-1][2])`

而对于初始化，我们只需要把第一天持有股票状态初始化为`-prices[0]`即可。

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)：

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        N = len(prices)

        dp = [[0 for _ in range(3)] for i in range(N)]
        dp[0] = [-prices[0], 0, 0]

        for i in range(1, N):
            dp[i][0] = max(dp[i-1][0], dp[i-1][2] - prices[i])
            dp[i][1] = dp[i-1][0] + prices[i]
            dp[i][2] = max(dp[i-1][1], dp[i-1][2])

        return max(dp[-1])
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int N = prices.size();

        vector<vector<int>> dp(N, vector<int>(3, 0));
        dp[0][0] = -prices[0];

        for (int i=1; i<N; ++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][2]-prices[i]);
            dp[i][1] = dp[i-1][0] + prices[i];
            dp[i][2] = max(dp[i-1][1], dp[i-1][2]);
        }

        return max(dp[N-1][1], dp[N-1][2]);
    }
};
```


### 714. 买卖股票的最佳时机含手续费

给定一个整数数组`prices`，其中`prices[i]`表示第`i`天的股票价格；整数`fee`代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

**注意：**这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

**示例1：**

```
输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8
解释：能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8
```

**示例2：**

```
输入：prices = [1,3,7,5,10,3], fee = 3
输出：6
```

**提示：**

- 1 <= `prices.length` <= 5 * 10⁴
- 1 <= `prices[i]` < 5 * 10⁴
- 0 <= `fee` < 5 * 10⁴

#### Solution

本题类似于[买卖股票的最佳时机 II](/leetcode/2023-03-13-DynamicProgramming.html#122-买卖股票的最佳时机-ii)。不过需要注意的是由于手续费的存在卖出股票时需要支付`fee`，因此递推关系相应修改为：

- `dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i] - fee)`
- `dp[i][1] = max(dp[i-1][0] - prices[i], dp[i-1][1])`

[题目链接](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)：

python代码：

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        N = len(prices)
        
        dp = [[0, 0] for i in range(N)]
        dp[0] = [0, -prices[0]]

        for i in range(1, N):
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i] - fee)
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i])
        
        return dp[-1][0]
```


C++代码：

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int N = prices.size();

        vector<vector<int>> dp(N, vector<int>(2, 0));
        dp[0][1] = -prices[0];

        for (int i=1; i<N; ++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i] - fee);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]);
        }

        return dp[N-1][0];
    }
};
```


## 子序列问题

### 300. 最长递增子序列

给你一个整数数组`nums`，找到其中最长严格递增子序列的长度。

**子序列**是由数组派生而来的序列，删除(或不删除)数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]`是数组`[0,3,1,6,2,2,7]`的子序列。

**示例1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

**提示：**

- 1 <= `nums.length` <= 2500
- -10⁴ <= `nums[i]` <= 10⁴

#### Solution

本题中我们定义一个一维数组`dp[]`，`dp[i]`表示以`nums[i]`结尾的最大递增子序列长度。接下来考虑递推关系，任何在`nums[i]`前面的末尾小于`nums[i]`的子序列都可以作为当前子序列的前驱，因此我们需要对这些子序列长度取最大值：

- `dp[i] = max(dp[i], dp[j]+1)`，`0 <= j < i`且`nums[i] > nums[j]`

然后考虑初始化。显然`nums`中的任意元素自身都可以作为一个递增子序列，因此我们需要把`dp[]`数组中的元素都初始化为`1`。完成准备工作后进行递推并返回`dp[]`数组中的最大值即可。

[题目链接](https://leetcode.cn/problems/longest-increasing-subsequence/)：

python代码：

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        N = len(nums)
        if N == 1:
            return 1

        dp = [1 for i in range(N)]
        res = 0

        for i in range(1, N):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j]+1)

            res = max(res, dp[i])

        return res
```


C++代码：

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int N = nums.size();
        vector<int> dp(N, 1);

        int res = 0;
        for (int i=0; i<N; ++i) {
            for (int j=0; j<i; ++j) {
                if (nums[j] < nums[i]) 
                    dp[i] = max(dp[i], dp[j] + 1);
            }

            res = max(res, dp[i]);
        }

        return res;
    }
};
```


### 674. 最长连续递增序列

给定一个未经排序的整数数组，找到最长且**连续递增的子序列**，并返回该序列的长度。

**连续递增的子序列**可以由两个下标`l`和`r`(`l < r`)确定，如果对于每个`l <= i < r`，都有`nums[i] < nums[i + 1]`，那么子序列`[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]`就是连续递增子序列。

**示例1：**

```
输入：nums = [1,3,5,4,7]
输出：3
解释：最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为 5 和 7 在原数组里被 4 隔开。
```

**示例2：**

```
输入：nums = [2,2,2,2,2]
输出：1
解释：最长连续递增序列是 [2], 长度为1。
```

**提示：**

- 1 <= `nums.length` <= 10⁴
- -10⁹ <= `nums[i]` <= 10⁹

#### Solution

本题类似于[最长递增子序列](/leetcode/2023-03-13-DynamicProgramming.html#300-最长递增子序列)。不过由于题目要求连续递增的子序列，我们需要修改一下递推关系。对于`nums[i]`有两种情况：

1. 如果`nums[i] > nums[i-1]`，则`nums[i]`可以加入以`nums[i-1]`结尾的连续递增子序列，此时`dp[i] = dp[i-1] + 1`
2. 如果`nums[i] <= nums[i-1]`，则`nums[i]`必须重头开启一个新的连续递增子序列，此时`dp[i] = 1`

这样就得到递推公式：

- `dp[i] = dp[i-1] + 1`，`nums[i-1] < nums[i]`

最后进行递推即可。

[题目链接](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)：

python代码：

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        N = len(nums)

        if N == 1:
            return 1
        
        dp = [1 for i in range(N)]
        res = 0

        for i in range(1, N):
            if nums[i-1] < nums[i]:
                dp[i] = dp[i-1] + 1
            
            res = max(res, dp[i])

        return res
```


C++代码：

```c++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int N = nums.size();
        vector<int> dp(N, 1);

        int res = 1;
        for (int i=1; i<N; ++i) {
            if (nums[i-1] < nums[i]) {
                dp[i] = dp[i-1] + 1;
            }

            res = max(res, dp[i]);
        }

        return res;
    }
};
```


### 718. 最长重复子数组

给两个整数数组`nums1`和`nums2`，返回两个数组中**公共的 、长度最长的子数组的长度**。

**示例1：**

```
输入：nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
输出：3
解释：长度最长的公共子数组是 [3,2,1] 。
```

**示例2：**

```
输入：nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
输出：5
```

**提示：**

- 1 <= `nums1.length`, `nums2.length` <= 1000
- 0 <= `nums1[i]`, `nums2[i]` <= 100

#### Solution

对于两个数组的情况我们需要使用一个二维数组`dp[][]`来进行递推，`dp[i+1][j+1]`表示以`nums1[i]`和`nums2[j]`结尾的最长公共子串长度。当`nums1[i] == nums2[j]`时，最长公共子串长度等于以`nums1[i-1]`和`nums2[j-1]`结尾的最长公共子串长度加1。这样可以得到递推关系：

- `dp[i+1][j+1] = dp[i][j] + 1`，`nums1[i] == nums2[j]`

接下来考虑初始化。为了方便起见我们把`dp[][]`数组初始化为`len(nums1)+1`行`len(nums2)+1`列的数组，其中每个元素为`0`。这样`dp[][]`数组的第一行和第一列表示没有使用`nums1`或`nums2`数组元素情况下构成的最长公共子串为空，其长度为`0`。完成准备工作后直接进行递推即可。

[题目链接](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)：

python代码：

```python
class Solution:
    def findLength(self, nums1: List[int], nums2: List[int]) -> int:
        N1 = len(nums1)
        N2 = len(nums2)

        dp = [[0 for j in range(N2+1)] for i in range(N1+1)]
        res = 0

        for i in range(1, N1+1):
            for j in range(1, N2+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1

                res = max(res, dp[i][j])
        
        return res
```


C++代码：

```c++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int N1 = nums1.size();
        int N2 = nums2.size();

        vector<vector<int>> dp(N1+1, vector<int>(N2+1, 0));
        
        int res = 0;
        for (int i=1; i<N1+1; ++i) {
            for (int j=1; j<N2+1; ++j) {
                if (nums1[i-1] == nums2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                }

                res = max(res, dp[i][j]);
            }
        }

        return res;
    }
};
```


### 1143. 最长公共子序列

给定两个字符串`text1`和`text2`，返回这两个字符串的最长**公共子序列**的长度。如果不存在**公共子序列**，返回`0`。

一个字符串的`子序列`是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符(也可以不删除任何字符)后组成的新字符串。

- 例如，`"ace"`是`"abcde"`的子序列，但`"aec"`不是`"abcde"`的子序列。

两个字符串的**公共子序列**是这两个字符串所共同拥有的子序列。

**示例1：**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

**示例2：**

```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。
```

**示例3：**

```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。
```

**提示：**

- 1 <= `text1.length`, `text2.length` <= 1000
- `text1`和`text2`仅由小写英文字符组成

#### Solution

本题类似于[最长重复子数组](/leetcode/2023-03-13-DynamicProgramming.html#718-最长重复子数组)，不过子序列不要求数字是连续的，因此我们需要相应地修改递推关系。这里我们建立一个二维数组`dp[][]`来进行递推，`dp[i+1][j+1]`表示使用`nums1[:i]`和`nums2[:j]`两个子数组所能构成的**最长公共子序列**。此时有如下递推关系：

- `text1[i] == text2[j]`时，只需要把公共元素添加到已有的最长公共子序列中，即`dp[i+1][j+1] = dp[i][j] + 1`
- `text1[i] != text2[j]`时，最长公共子序列不变，此时`dp[i+1][j+1] = max(dp[i][j-1], dp[i-1][j])`

更新递推公式后直接进行递推并返回`dp[-1][-1]`即可。

[题目链接](https://leetcode.cn/problems/longest-common-subsequence/)：

python代码：

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        N1 = len(text1)
        N2 = len(text2)

        dp = [[0 for j in range(N2+1)] for i in range(N1+1)]

        for i in range(1, N1+1):
            for j in range(1, N2+1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        
        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int N1 = text1.size();
        int N2 = text2.size();

        vector<vector<int>> dp(N1+1, vector<int>(N2+1, 0));

        for (int i=1; i<N1+1; ++i) {
            for (int j=1; j<N2+1; ++j) {
                if (text1[i-1] == text2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }

        return dp[N1][N2];
    }
};
```


### 1035. 不相交的线

在两条独立的水平线上按给定的顺序写下`nums1`和`nums2`中的整数。

现在，可以绘制一些连接两个数字`nums1[i]`和`nums2[j]`的直线，这些直线需要同时满足满足：

- `nums1[i] == nums2[j]`
- 且绘制的直线不与任何其他连线(非水平线)相交

请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

以这种方法绘制线条，并返回可以绘制的最大连线数。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2019/04/26/142.png" width="30%">
</div>

```
输入：nums1 = [1,4,2], nums2 = [1,2,4]
输出：2
解释：可以画出两条不交叉的线，如上图所示。 
但无法画出第三条不相交的直线，因为从 nums1[1]=4 到 nums2[2]=4 的直线将与从 nums1[2]=2 到 nums2[1]=2 的直线相交。
```

**示例2：**

```
输入：nums1 = [2,5,1,2,5], nums2 = [10,5,2,1,5,2]
输出：3
```

**示例3：**

```
输入：nums1 = [1,3,7,1,7,5], nums2 = [1,9,2,5,1]
输出：2
```

**提示：**

- 1 <= `nums1.length`, `nums2.length` <= 500
- 1 <= `nums1[i]`, `nums2[i]` <= 2000

#### Solution

本题的描述比较复杂，但实际上题目的意思是在`nums1`和`nums2`中寻找一个**最大的公共子序列**且它们保持一致的相对顺序。在这种观察下本题与[最长公共子序列](/leetcode/2023-03-13-DynamicProgramming.html#1143-最长公共子序列)是完全相同的，直接套用[最长公共子序列](/leetcode/2023-03-13-DynamicProgramming.html#1143-最长公共子序列)的代码即可。

[题目链接](https://leetcode.cn/problems/uncrossed-lines/)：

python代码：

```python
class Solution:
    def maxUncrossedLines(self, nums1: List[int], nums2: List[int]) -> int:
        N1 = len(nums1)
        N2 = len(nums2)

        dp = [[0 for j in range(N2+1)] for i in range(N1+1)]

        for i in range(1, N1+1):
            for j in range(1, N2+1):
                if nums1[i-1] == nums2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        
        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        int N1 = nums1.size();
        int N2 = nums2.size();

        vector<vector<int>> dp(N1+1, vector<int>(N2+1, 0));

        for (int i=1; i<N1+1; ++i) {
            for (int j=1; j<N2+1; ++j) {
                if (nums1[i-1] == nums2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }

        return dp[N1][N2];
    }
};
```


### 53. 最大子数组和

给你一个整数数组`nums`，请你找出一个具有最大和的连续子数组(子数组最少包含一个元素)，返回其最大和。

**子数组**是数组中的一个连续部分。

**示例1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例2：**

```
输入：nums = [1]
输出：1
```

**示例3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

**提示：**

- 1 <= `nums.length` <= 10⁵
- -10⁴ <= `nums[i]` <= 10⁴

#### Solution

同[最大子数组和](/leetcode/2023-03-10-Greedy.html#53-最大子数组和)。

[题目链接](https://leetcode.cn/problems/maximum-subarray/)：

python代码：

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        N = len(nums)
        dp = [nums[0] for _ in range(N)]
        
        for i in range(1, N):
            dp[i] = max(dp[i-1]+nums[i], nums[i])
        
        return max(dp)
```


C++代码：

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 0) return 0;

        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        
        int res = nums[0];

        for (int i=1; i<nums.size(); ++i) {
            dp[i] = max(dp[i-1]+nums[i], nums[i]);
            res = max(res, dp[i]);
        }

        return res;
    }
};
```


### 392. 判断子序列

给定字符串`s`和`t`，判断`s`是否为`t`的子序列。

字符串的一个子序列是原始字符串删除一些(也可以不删除)字符而不改变剩余字符相对位置形成的新字符串。(例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是)。

**示例1：**

```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例2：**

```
输入：s = "axc", t = "ahbgdc"
输出：false
```

**提示：**

- 0 <= `s.length` <= 100
- 0 <= `t.length` <= 10⁴
- 两个字符串都只由小写字符组成

#### Solution

本题类似于[最长公共子序列](/leetcode/2023-03-13-DynamicProgramming.html#1143-最长公共子序列)。实际上我们只需要判断`s`和`t`的最长公共子序列的长度是否等于`s`的长度即可，不过在递推时我们需要稍微修改一下递推公式。假设`dp[i+1][j+1]`表示子串`s[:i+1]`和`t[:j+1]`所能构成的相同子序列长度，根据`s[i]`和`t[j]`的关系有两种可能情况：

- `s[i] == t[j]`时我们可以把相同的字符添加到子序列末尾，这样子序列的长度需要加1，即`dp[i+1][j+1] = dp[i][j] + 1`
- `s[i] != t[j]`时子序列只能由子串`s[:i+1]`和`t[:j]`构成，其长度与前一个结果相同，即`dp[i+1][j+1] = dp[i+1][j]`

整个递推过程可以参考下图。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/mzw426s.png" width="70%">
</div>

[题目链接](https://leetcode.cn/problems/is-subsequence/)：

python代码：

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        N1 = len(s)
        N2 = len(t)

        dp = [[0 for j in range(N2+1)] for i in range(N1+1)]

        for i in range(1, N1+1):
            for j in range(1, N2+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = dp[i][j-1]
        
        return dp[-1][-1] == N1
```


C++代码：

```c++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int N1 = s.size();
        int N2 = t.size();

        vector<vector<int>> dp(N1+1, vector<int>(N2+1, 0));
        
        for (int i=1; i<N1+1; ++i) {
            for (int j=1; j<N2+1; ++j) {
                if (s[i-1] == t[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = dp[i][j-1];
                }
            }
        }

        return dp[N1][N2] == N1;
    }
};
```


### 115. 不同的子序列

给定一个字符串`s`和一个字符串`t`，计算在`s`的子序列中`t`出现的个数。

字符串的一个**子序列**是指，通过删除一些(也可以不删除)字符且不干扰剩余字符相对位置所组成的新字符串。(例如，`"ACE"`是`"ABCDE"`的一个子序列，而`"AEC"`不是)

**示例1：**

```
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下图所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
rabbbit
rabbbit
rabbbit
```

**示例2：**

```
输入：s = "babgbag", t = "bag"
输出：5
解释：
如下图所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
babgbag
babgbag
babgbag
babgbag
babgbag
```

**提示：**

- 0 <= `s.length`, `t.length` <= 1000
- `s`和`t`由英文字母组成

#### Solution

本题中我们使用一个二维数组`dp[][]`进行递推，`dp[i+1][j+1]`表示使用`s[:i+1]`和`t[:j+1]`构造的子序列个数。根据`s[i]`与`t[j]`的关系有两种可能情况：

- `s[i] == t[j]`时，子序列有两种构成形式，我们需要把这两种可能形式加起来：
   1. 使用末尾`s[i]`和`t[j]`去匹配，此时问题规模缩小为`s[:i-1]`和`t[:j-1]`构造的子序列个数，这种形式的子序列一共有`dp[i][j]`个
   2. 不使用末尾`s[i]`去匹配，这种形式的子序列一共有`dp[i][j+1]`个

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/GkjX13X.png" width="80%">
</div>

- `s[i] != t[j]`时无法使用字符`s[i]`，因此需要使用字符串`s[:i]`进行匹配，这样子序列有`dp[i][j+1]`个

接下来考虑初始化问题。本题中需要把`dp[][]`数组的第一列初始化为1，它表示`t`为空字符串时可以与任意的`s`字符串构造出一个子序列。完成准备工作后直接进行递推并返回`dp[][]`数组的末尾元素即可。

[题目链接](https://leetcode.cn/problems/distinct-subsequences/)：

python代码：

```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        N1 = len(s)
        N2 = len(t)

        dp = [[0 for j in range(N2+1)] for i in range(N1+1)]
        for i in range(N1+1):
            dp[i][0] = 1

        for i in range(1, N1+1):
            for j in range(1, N2+1):
                if s[i-1] == t[j-1]:
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j]
                else:
                    dp[i][j] = dp[i-1][j]
        
        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int numDistinct(string s, string t) {
        int N1 = s.size();
        int N2 = t.size();

        vector<vector<uint64_t>> dp(N1+1, vector<uint64_t>(N2+1, 0));
        for (int i=0; i<N1+1; ++i) {
            dp[i][0] = 1;
        }

        for (int i=1; i<N1+1; ++i) {
            for (int j=1; j<N2+1; ++j) {
                if (s[i-1] == t[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + dp[i-1][j];
                } else {
                    dp[i][j] = dp[i-1][j];
                }
            }
        }

        return dp[N1][N2];
    }
};
```


### 583. 两个字符串的删除操作

给定两个单词`word1`和`word2`，返回使得`word1`和`word2`**相同**所需的**最小步数**。

**每步**可以删除任意一个字符串中的一个字符。

**示例1：**

```
输入: word1 = "sea", word2 = "eat"
输出: 2
解释: 第一步将 "sea" 变为 "ea" ，第二步将 "eat "变为 "ea"
```

**示例2：**

```
输入：word1 = "leetcode", word2 = "etco"
输出：4
```

**提示：**

- 1 <= `word1.length`, `word2.length` <= 500
- `word1`和`word2`只包含小写英文字母

#### Solution

本题中我们使用一个二维数组`dp[][]`进行递推，`dp[i+1][j+1]`表示保证`word1[:i]`和`word2[:j]`相同所需删除操作的最小次数。根据`word1[i]`与`word2[j]`的关系有两种可能情况：

- `word1[i] == word2[j]`时由于末尾已经相等我们只需要考虑前面的字符串即可，因此`dp[i+1][j+1] = dp[i][j]`
- `word1[i] != word2[j]`时有三种可能情况：
   1. 删除`word1[i]`，此时的最小删除操作为`dp[i+1][j+1] = dp[i][j+1] + 1`
   2. 删除`word2[j]`，此时的最小删除操作为`dp[i+1][j+1] = dp[i+1][j] + 1`
   3. 同时删除`word1[i]`和`word2[j]`，此时的最小删除操作为`dp[i+1][j+1] = dp[i][j] + 2`

总结一下可以得到递推公式：

- `dp[i+1][j+1] = dp[i][j]`，`word1[i] == word2[j]`
- `dp[i+1][j+1] = min(dp[i][j+1] + 1, dp[i+1][j] + 1, dp[i][j] + 2)`，`word1[i] != word2[j]`

接下来考虑初始化。对于`dp[][]`数组的第一行和第一列我们需要分别将其初始化为对应的行号和列号，表示当其中一个字符串为空时需要把另一个字符串的全部元素都删除才能满足要求。完成准备工作后直接进行递推并返回`dp[][]`的最后一个元素即可，整个递推过程可以参考下图。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/OxYAXdY.png" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/delete-operation-for-two-strings/)：

python代码：

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        N1 = len(word1)
        N2 = len(word2)

        dp = [[0 for j in range(N2+1)] for i in range(N1+1)]
        for i in range(N1+1):
            dp[i][0] = i
        
        for j in range(N2+1):
            dp[0][j] = j
        
        for i in range(1, N1+1):
            for j in range(1, N2+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1]+2)

        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int N1 = word1.size();
        int N2 = word2.size();

        vector<vector<int>> dp(N1+1, vector<int>(N2+1, 0));
        for (int i=0; i<N1+1; ++i) dp[i][0] = i;
        for (int j=0; j<N2+1; ++j) dp[0][j] = j;

        for (int i=1; i<N1+1; ++i) {
            for (int j=1; j<N2+1; ++j) {
                if (word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                } else {
                    dp[i][j] = min({dp[i-1][j-1] + 2, dp[i-1][j] + 1, dp[i][j-1] + 1});
                }
            }
        }

        return dp[N1][N2];
    }
};
```


### 72. 编辑距离

给你两个单词`word1`和`word2`，请返回将`word1`转换成`word2`所使用的**最少操作数**。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

**示例1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

**提示：**

- 1 <= `word1.length`, `word2.length` <= 500
- `word1`和`word2`只包含小写英文字母

#### Solution

编辑距离是动态规划中的经典问题。本题的整体思路类似于[两个字符串的删除操作](/leetcode/2023-03-13-DynamicProgramming.html#583-两个字符串的删除操作)，我们首先建立一个二维数组`dp[][]`进行递推，其中`dp[i+1][j+1]`表示将`word1[:i]`转换为`word2[:j]`所需的最小操作数。根据字符`word1[i]`和`word[j]`的关系有两种情况：

- `word1[i] == word2[j]`时无需再进行任何修改操作，因此`dp[i+1][j+1] = dp[i][j]`
- `word1[i] != word2[j]`时有三种可能情况：
   1. 删除`word1[i]`，此时的最小操作数为`dp[i+1][j+1] = dp[i][j+1] + 1`
   2. 在`word1[i]`后添加一个字符，这相当于删除`word2[j]`，此时的最小操作数为`dp[i+1][j+1] = dp[i+1][j] + 1`
   3. 替换`word1[i]`为`word2[j]`，此时的最小删除操作为`dp[i+1][j+1] = dp[i][j] + 1`

总结一下可以得到递推公式：

- `dp[i+1][j+1] = dp[i][j]`，`word1[i] == word2[j]`
- `dp[i+1][j+1] = min(dp[i][j+1], dp[i+1][j], dp[i][j]) + 1`，`word1[i] != word2[j]`

而初始化则与[两个字符串的删除操作](/leetcode/2023-03-13-DynamicProgramming.html#583-两个字符串的删除操作)完全相同。整个递推算法流程可参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/s6Mq8d4.png" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/edit-distance/)：

python代码：

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        N1 = len(word1)
        N2 = len(word2)

        dp = [[0 for j in range(N2+1)] for i in range(N1+1)]

        for i in range(N1+1):
            dp[i][0] = i
        
        for j in range(N2+1):
            dp[0][j] = j

        for i in range(1, N1+1):
            for j in range(1, N2+1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
        
        return dp[-1][-1]
```


C++代码：

```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int N1 = word1.size();
        int N2 = word2.size();

        vector<vector<int>> dp(N1+1, vector<int>(N2+1, 0));
        for (int i=0; i<N1+1; ++i) dp[i][0] = i;
        for (int j=0; j<N2+1; ++j) dp[0][j] = j;

        for (int i=1; i<N1+1; ++i) {
            for (int j=1; j<N2+1; ++j) {
                if (word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                } else {
                    dp[i][j] = min({dp[i-1][j-1], dp[i-1][j], dp[i][j-1]}) + 1;
                }
            }
        }

        return dp[N1][N2];
    }
};
```


### 647. 回文子串

给你一个字符串`s`，请你统计并返回这个字符串中**回文子串**的数目。

**回文字符串**是正着读和倒过来读一样的字符串。

**子字符串**是字符串中的由连续字符组成的一个序列。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

**示例1：**

```
输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

**示例2：**

```
输入：s = "aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

**提示：**

- 1 <= `s.length` <= 1000
- `s`由小写英文字母组成

#### Solution

本题中我们首先要考虑回文字符串的结构。显然任何回文字符串最左边和最右边的字符都是相等的，而且去掉它们后得到的字符串同样是一个回文字符串。因此我们可以从一个已知的回文字符串出发，通过向两边添加字符的方式来递推出新的回文字符串。

这样我们可以建立一个二维数组`dp[][]`，`dp[i][j]`表示以`s[i]`开始`s[j]`结尾的字符串是否是一个回文字符串。根据`s[i]`和`s[j]`的关系有两种情况：

- `s[i] == s[j]`时需要考虑`i`和`j`的关系：
   1. `i == j`时字符串只包含一个字符，此时一定是一个回文字符串，`dp[i][j] = 1`
   2. `i+1 == j`时字符串包含两个字符，此时一定是一个回文字符串，`dp[i][j] = 1`
   3. `i+1 < j`时字符串包含至少三个字符，此时需要进一步判断里侧字符串是否是一个回文字符串，`dp[i][j] = dp[i+1][j-1]`
- `s[i] != s[j]`时无法组成回文字符串，跳过

总结一下可以得到递推公式：

- `dp[i][j] = 0`，`s[i] != s[j]`
- `dp[i][j] = 1`，`s[i] == s[j]`且`j - i <= 1`
- `dp[i][j] = dp[i+1][j-1]`，`s[i] == s[j]`且`j - i > 1`

接下来考虑初始化。本题中我们只需要把`dp[][]`数组初始化为`0`即可，它表示开始时我们不知道`s`中是否存在回文字符串。

本题的另一个难点在于遍历顺序。根据递推关系我们知道`dp[i][j]`会依赖于`dp[i+1][j-1]`，因此我们不能像通常情况那样从左到右从上到下进行递推，而是需要从左下向右上进行递推。这样就需要对行号`i`进行逆序遍历，而对列号`j`进行顺序遍历，如下图所示。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/7TGQF2x.png" width="50%">
</div>

[题目链接](https://leetcode.cn/problems/palindromic-substrings/)：

python代码：

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        N = len(s)
        res = 0

        dp = [[0 for j in range(N)] for i in range(N)]

        for i in range(N-1, -1, -1):
            for j in range(i, N):
                if s[i] == s[j]:
                    if j - i <= 1:
                        dp[i][j] = 1
                        res += 1
                    elif dp[i+1][j-1]:
                        dp[i][j] = 1
                        res += 1
        
        return res
```


C++代码：

```c++
class Solution {
public:
    int countSubstrings(string s) {
        int N = s.size();
        int res = 0;

        vector<vector<int>> dp(N, vector<int>(N, 0));

        for (int i=N-1; i>=0; --i){
            for (int j=i; j<N; ++j) {
                if (s[i] == s[j]) {
                    if (j-i <= 1) {
                        dp[i][j] = 1;
                        ++res;
                    } 
                    else if (dp[i+1][j-1]) {
                        dp[i][j] = 1;
                        ++res;
                    }
                }
            }
        }

        return res;
    }
};
```


### 5. 最长回文子串

给你一个字符串`s`，找到`s`中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

**示例1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例2：**

```
输入：s = "cbbd"
输出："bb"
```

**提示：**

- 1 <= `s.length` <= 1000
- `s`仅由数字和英文字母组成

#### Solution

本题的整体思路类似于[回文子串](/leetcode/2023-03-13-DynamicProgramming.html#647-回文子串)，我们只需要在遍历中记录下最长回文子串的长度及起点终点位置即可。

[题目链接](https://leetcode.cn/problems/longest-palindromic-substring/)：

python代码：

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        N = len(s)

        dp = [[0 for j in range(N)] for i in range(N)]
        res = 0
        ii = jj = 0

        for i in range(N-1, -1, -1):
            for j in range(i, N):
                if s[i] == s[j]:
                    if j - i <= 1:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = dp[i+1][j-1]
                
                    if dp[i][j] and j-i+1 > res:
                        res = j-i+1
                        ii = i
                        jj = j

        return s[ii:jj+1]
```


C++代码：

```c++
class Solution {
public:
    string longestPalindrome(string s) {
        int N = s.size();

        int res = 0;
        int start = 0, end = 0;

        vector<vector<bool>> dp(N, vector<bool>(N, false));

        for (int i=N-1; i>=0; --i) {
            for (int j=i; j<N; ++j) {
                if (s[i] == s[j]) {
                    if (j-i <= 1) {
                        dp[i][j] = true;
                    } else if (dp[i+1][j-1]) {
                        dp[i][j] = true;
                    }
                }

                if (dp[i][j] && res < j-i+1) {
                    start = i; end = j;
                    res = j-i+1;
                }
            }
        }

        return s.substr(start, end-start+1);
    }
};
```


### 516. 最长回文子序列

给你一个字符串`s`，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。

**示例1：**

```
输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb" 。
```

**示例2：**

```
输入：s = "cbbd"
输出：2
解释：一个可能的最长回文子序列为 "bb" 。
```

**提示：**

- 1 <= `s.length` <= 1000
- `s`仅由小写英文字母组成

#### Solution

本题的整体思路类似于[回文子串](/leetcode/2023-03-13-DynamicProgramming.html#647-回文子串)，不过我们需要修改一下`dp[][]`数组的定义：`dp[i][j]`表示以`s[i]`开始`s[j]`结尾的字符串中最长回文子序列的长度。根据`s[i]`和`s[j]`的关系有两种情况：

- `s[i] == s[j]`时子序列长度为`s[i+1]`开始`s[j-1]`结尾的字符串中最长回文子序列的长度加2，即`dp[i][j] = dp[i+1][j-1] + 2`
- `s[i] != s[j]`时无法构成新的回文子序列，需要考虑已知的回文子序列长度，即`dp[i][j] = max(dp[i+1][j], dp[i][j-1])`

总结一下可以得到递推公式：

- `dp[i][j] = dp[i+1][j-1] + 2`，`s[i] == s[j]`
- `dp[i][j] = max(dp[i+1][j], dp[i][j-1])`，`s[i] != s[j]`

接下来考虑初始化。本题中我们可以把`dp[][]`数组都初始化为`0`并且把对角线元素都初始化为`1`，这表示每个单独的字符都可以组成一个回文子序列。

而在遍历时则需要注意对`j`从`i+1`开始遍历。整个递推过程可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/OHvxOsk.png" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/longest-palindromic-subsequence/)：

python代码：

```python
class Solution:
    def longestPalindromeSubseq(self, s: str) -> int:
        N = len(s)

        dp = [[0 for j in range(N)] for i in range(N)]
        for i in range(N):
            dp[i][i] = 1

        for i in range(N-1, -1, -1):
            for j in range(i+1, N):
                if s[i] == s[j]:
                    dp[i][j] = dp[i+1][j-1] + 2
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])

        return dp[0][-1]
```


C++代码：

```c++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int N = s.size();

        vector<vector<int>> dp(N, vector<int>(N, 0));
        for (int i=0; i<N; ++i) dp[i][i] = 1;

        for (int i=N-1; i>=0; --i) {
            for (int j=i+1; j<N; ++j) {
                if (s[i] == s[j]) dp[i][j] = dp[i+1][j-1] + 2;
                else dp[i][j] = max(dp[i+1][j], dp[i][j-1]);
            }
        }

        return dp[0][N-1];
    }
};
```


### 132. 分割回文串 II

给你一个字符串`s`，请你将`s`分割成一些子串，使每个子串都是回文。

返回符合要求的**最少分割次数**。

**示例1：**

```
输入：s = "aab"
输出：1
解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
```

**示例2：**

```
输入：s = "a"
输出：0
```

**示例3：**

```
输入：s = "ab"
输出：1
```

**提示：**

- 1 <= `s.length` <= 2000
- `s`仅由小写英文字母组成

#### Solution

本题需要使用一个一维数组`dp[]`进行递推，`dp[i]`表示将字符串`s[:i+1]`分割为回文子串的最小分割次数。根据字符串`s[:i+1]`的形式有两种可能情况：

- `s[:i+1]`本身是回文字符串则无需进行分割，`dp[i] = 0`
- `s[:i+1]`不是回文字符串则至少需要额外进行一次分割。此时需要对`s[i]`前面的字符进行遍历，如果`s[j]`后面的字符串`s[j:i+1]`是回文字符串则是一个可行的分割方案，此时的分割次数为`dp[j-1]+1`。这表示我们利用`s[j]`将字符串分割为两部分，前一部分需要`dp[j-1]`次分割来获得回文字符串，而后一部分本身就是一个回文字符串。

为了方便查询`s`的各个子串是否是回文字符串，我们可以使用[回文子串](/leetcode/2023-03-13-DynamicProgramming.html#647-回文子串)中介绍的方法，把子串是否是回文字符串的结果记录在一个`check[][]`数组中。整个算法流程可以参考如下。

[题目链接](https://leetcode.cn/problems/palindrome-partitioning-ii/)：

python代码：

```python
class Solution:
    def minCut(self, s: str) -> int:
        N = len(s)

        ## check if substring is palindromic
        check = [[0 for j in range(N)] for i in range(N)]

        for i in range(N-1, -1, -1):
            for j in range(i, N):
                if s[i] == s[j]:
                    if j-i <= 1:
                        check[i][j] = 1
                    elif check[i+1][j-1]:
                        check[i][j] = 1

        dp = [float("inf") for i in range(N)]

        for i in range(N):
            if check[0][i]:
                dp[i] = 0
            else:
                for j in range(i):
                    if check[j+1][i]:
                        dp[i] = min(dp[i], dp[j]+1)
        
        return dp[-1]
```


C++代码：

```c++
class Solution {
public:
    int minCut(string s) {
        int N = s.size();

        vector<vector<bool>> check(N, vector<bool>(N, false));
        for (int i=N-1; i>=0; --i) {
            for (int j=i; j<N; ++j) {
                if (s[i] == s[j] && (j-i <= 1 || check[i+1][j-1]))
                    check[i][j] = true;
            }
        }

        vector<int> dp(N, N);

        for (int i=0; i<N; ++i) {
            if (check[0][i]) dp[i] = 0;
            else {
                for (int j=0; j<i; ++j) {
                    if (check[j+1][i]) dp[i] = min(dp[i], dp[j]+1);
                }
            }
        }

        return dp[N-1];
    }
};
```


### 673. 最长递增子序列的个数

给定一个未排序的整数数组`nums`，返回最长递增子序列的个数。

**注意**这个数列必须是**严格**递增的。

**示例1：**

```
输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
```

**示例2：**

```
输入: [2,2,2,2,2]
输出: 5
解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5。
```

**提示：**

- 1 <= `nums.length` <= 2000
- -10⁶ <= `nums[i]` <= 10⁶

#### Solution

本题是[最长递增子序列](/leetcode/2023-03-13-DynamicProgramming.html#300-最长递增子序列)的进阶版本，我们需要在寻找最长递增子序列长度的基础上记录这样子序列的个数。因此我们需要使用到两个数组`dp[]`和`count[]`，其中`dp[i]`表示以`nums[i]`结尾的递增子序列长度而`count[i]`则表示这样子序列的个数。除此之外，我们还需要一个变量`maxLen`来记录这些递增子序列中最大的序列长度。

`dp[]`数组的递推公式与[最长递增子序列](/leetcode/2023-03-13-DynamicProgramming.html#300-最长递增子序列)中一致：对于`nums[i]`前面的数字`nums[j]`，如果`nums[i] > nums[j]`则令`dp[i] = dp[j] + 1`表示把`nums[i]`接在以`nums[j]`结尾的递增子序列上会使子序列长度加1。而`count[]`数组的递推关系与之类似：

- `dp[j] + 1 > dp[i]`说明找到了更长的递增子序列，此时需要更新`count[i] = count[j]`
- `dp[j] + 1 == dp[i]`说明找到长度相同的其它最大递增子序列，此时需要进行累加`count[i] += count[j]`

这样可以得到`count[]`数组的递推公式：

- `count[i] = count[j]`，如果`nums[i] > nums[j]`且`dp[j] + 1 > dp[i]`
- `count[i]+= count[j]`，如果`nums[i] > nums[j]`且`dp[j] + 1 == dp[i]`

接下来考虑初始化，显然我们可以把`dp[]`和`count[]`两个数组都初始化为`1`，这表示任意数字`nums[i]`自己都可以组成一个只包含自身的递增序列。

完成递推后我们只需要统计`dp[]`数组中等于最大长度`maxLen`的部分，并把相应的序列数`count[]`累加即可。整个算法流程可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/XWiBTJp.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/number-of-longest-increasing-subsequence/)：

python代码：

```python
class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        N = len(nums)

        if N <= 1:
            return N
        
        dp = [1 for i in range(N)]
        count = [1 for i in range(N)]
        maxLen = 0

        for i in range(N):
            for j in range(i):
                if nums[i] > nums[j]:
                    if dp[j] + 1 > dp[i]:
                        dp[i] = dp[j] + 1
                        count[i] = count[j]
                    elif dp[j] + 1 == dp[i]:
                        count[i] += count[j]
                
                if dp[i] > maxLen:
                    maxLen = dp[i]
        
        res = 0
        for i in range(N):
            if dp[i] == maxLen:
                res += count[i]

        return res
```


C++代码：

```c++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int N = nums.size();
        if (N == 1) return 1;

        vector<int> dp(N, 1);
        vector<int> count(N, 1);
        int maxLen = 0;

        for (int i=1; i<N; ++i) {
            for (int j=0; j<i; ++j) {
                if (nums[j] < nums[i]) {
                    if (dp[j]+1 > dp[i]) {
                        dp[i] = dp[j] + 1;
                        count[i] = count[j];
                    } else if (dp[j]+1 == dp[i]) {
                        count[i] += count[j];
                    }
                }
            }

            maxLen = max(maxLen, dp[i]);
        }

        int res = 0;
        for (int i=0; i<N; ++i) {
            if (dp[i] == maxLen) res += count[i];
        }

        return res;
    }
};
```


## Reference

- [动态规划理论基础](https://www.bilibili.com/video/BV13Q4y197Wg/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：509.斐波那契数](https://www.bilibili.com/video/BV1f5411K7mo/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：70.爬楼梯](https://www.bilibili.com/video/BV17h411h7UH/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：746.使用最小花费爬楼梯](https://www.bilibili.com/video/BV16G411c7yZ/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：62.不同路径](https://www.bilibili.com/video/BV1ve4y1x7Eu/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：63.不同路径 II](https://www.bilibili.com/video/BV1Ld4y1k7c6/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：343.整数拆分](https://www.bilibili.com/video/BV1Mg411q7YJ/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：96.不同的二叉搜索树](https://www.bilibili.com/video/BV1eK411o7QA/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [01背包理论基础](https://www.bilibili.com/video/BV1cg411g7Y6/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [01背包理论基础(滚动数组)](https://www.bilibili.com/video/BV1BU4y177kY/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：416.分割等和子集](https://www.bilibili.com/video/BV1rt4y1N7jE/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：1049.最后一块石头的重量II](https://www.bilibili.com/video/BV14M411C7oV/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：494.目标和](https://www.bilibili.com/video/BV1o8411j73x/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：474.一和零](https://www.bilibili.com/video/BV1rW4y1x7ZQ/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [完全背包理论基础](https://www.bilibili.com/video/BV1uK411o7c9/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：518.零钱兑换II](https://www.bilibili.com/video/BV1KM411k75j/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：377.组合总和IV](https://www.bilibili.com/video/BV1V14y1n7B6/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：322.零钱兑换](https://www.bilibili.com/video/BV14K411R7yv/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：279.完全平方数](https://www.bilibili.com/video/BV12P411T7Br/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：139.单词拆分](https://www.bilibili.com/video/BV1pd4y147Rh/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：198.打家劫舍](https://www.bilibili.com/video/BV1Te411N7SX/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：213.打家劫舍II](https://www.bilibili.com/video/BV1oM411B7xq/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：337.打家劫舍III](https://www.bilibili.com/video/BV1H24y1Q7sY/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：121.买卖股票的最佳时机](https://www.bilibili.com/video/BV1Xe4y1u77q/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：122.买卖股票的最佳时机II](https://www.bilibili.com/video/BV1D24y1Q7Ls/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：123.买卖股票最佳时机III](https://www.bilibili.com/video/BV1WG411K7AR/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：124.买卖股票最佳时机IV](https://www.bilibili.com/video/BV16M411U7XJ/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：309.买卖股票的最佳时机含冷冻期](https://www.bilibili.com/video/BV1rP4y1D7ku/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：714.买卖股票的最佳时机含手续费](https://www.bilibili.com/video/BV1z44y1Z7UR/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：300.最长递增子序列](https://www.bilibili.com/video/BV1ng411J7xP/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：674.最长连续递增序列](https://www.bilibili.com/video/BV1bD4y1778v/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：718.最长重复子数组](https://www.bilibili.com/video/BV178411H7hV/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：1143.最长公共子序列](https://www.bilibili.com/video/BV1ye4y1L7CQ/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：1035.不相交的线](https://www.bilibili.com/video/BV1h84y1x7MP/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：53.最大子序和](https://www.bilibili.com/video/BV19V4y1F7b5/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：392.判断子序列](https://www.bilibili.com/video/BV1tv4y1B7ym/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：115.不同的子序列](https://www.bilibili.com/video/BV1fG4y1m75Q/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：583.两个字符串的删除操作](https://www.bilibili.com/video/BV1we4y157wB/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：72.编辑距离](https://www.bilibili.com/video/BV1qv4y1q78f/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：647.回文子串](https://www.bilibili.com/video/BV17G4y1y7z9/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：516.最长回文子序列](https://www.bilibili.com/video/BV1d8411K7W6/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)