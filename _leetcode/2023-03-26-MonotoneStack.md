---
layout: post
title: 单调栈
date: 2023-03-26
tags: leetcode
toc:
  sidebar: left
---

**单调栈(monotone stack)**是一种特殊的栈。在栈「先进后出」的基础上，单调栈要求栈中的元素按照从「栈顶」到「栈底」的顺序是**单调递增**或是**单调递减**，因此单调栈适合求解"第一个大于"或"第一个小于"某个元素这样的问题。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/qYBAlZK.png" width="80%">
</div>

以单调递增栈为例，算法模板可参考如下：

```python
for i in range(len(nums))
    while stack and nums[i] > nums[stack[-1]]:
        idx = stack.pop()
        res[idx] = i - idx
    
    stack.append(i)
```

## 单调递增栈

### 739. 每日温度

给定一个整数数组`temperatures`，表示每天的温度，返回一个数组`answer`，其中`answer[i]`是指对于第`i`天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用`0`来代替。

**示例1：**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例2：**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例3：**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

**提示：**

- 1 <= `temperatures.length` <= 10⁵
- 30 <= `temperatures[i]` <= 100

#### Solution

本题是经典的单调递增栈问题，它的本质是求数组中大于当前元素的第一个元素编号。这里我们使用一个栈`stack`作为单调递增栈保存每一天的编号，使用数组`answer`记录结果。当我们对`temperatures`进行遍历时令当前编号`i`入栈，根据单调栈的性质所有小于`temperatures[i]`的编号`idx`会依次出栈。因此`i`即为`idx`后出现的第一个大于它的数字编号，它们之间的距离为`answer[idx] = i-idx`。由于`temperatures`中所有的元素只会入栈一次，使用单调栈的时间复杂度为`O(n)`。

[题目链接](https://leetcode.cn/problems/daily-temperatures/)：

python代码：

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        N = len(temperatures)
        answer = [0 for i in range(N)]
        stack = []

        for i in range(N):
            while stack and temperatures[i] > temperatures[stack[-1]]:
                idx = stack.pop()
                answer[idx] = i - idx
            
            stack.append(i)

        return answer
```


C++代码：

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int N = temperatures.size();
        vector<int> res = vector(N, 0);

        stack<int> stk;

        for (int i=0; i<N; ++i) {
            while (!stk.empty() && temperatures[stk.top()] < temperatures[i]) {
                int idx = stk.top(); stk.pop();
                res[idx] = i - idx;
            }

            stk.push(i);
        }

        return res;
    }
};
```



### 496. 下一个更大元素 I

`nums1`中数字`x`的**下一个更大元素**是指`x`在`nums2`中对应位置**右侧**的**第一个**比`x`大的元素。

给你两个**没有重复元素**的数组`nums1`和`nums2`，下标从`0`开始计数，其中`nums1`是`nums2`的子集。

对于每个`0 <= i < nums1.length`，找出满足`nums1[i] == nums2[j]`的下标`j`，并且在`nums2`确定`nums2[j]`的**下一个更大元素**。如果不存在下一个更大元素，那么本次查询的答案是`-1`。

返回一个长度为`nums1.length`的数组`ans`作为答案，满足`ans[i]`是如上所述的**下一个更大元素**。

**示例1：**

```
输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
```

**示例2：**

```
输入：nums1 = [2,4], nums2 = [1,2,3,4].
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
```

**提示：**

- 1 <= `nums1.length` <= `nums2.length` <= 1000
- 0 <= `nums1[i]`, `nums2[i]` <= 10⁴
- `nums1`和`nums2`中所有整数**互不相同**
- `nums1`中的所有整数同样出现在`nums2`中

#### Solution

由于`nums1`和`nums2`中所有整数互不相同且`nums1`是`nums2`的子集，本题可以理解为寻找`nums2`中每个元素右边第一个大于它的值。这里使用`stack`作为单调递增栈，`ans`作为字典(哈希表)记录`nums2`中元素的下一个更大元素。

使用单调栈时需要注意**从右向左**对`nums2`进行遍历，如果当前元素`num`大于栈顶元素`stack[-1]`则令其出栈。这样在`num`入栈前如果存在`stack[-1]`则其就是`num`对应的下一个更大元素，令`ans[num] = stack[-1]`；否则说明`num`右侧元素都小于它，令`ans[num] = -1`。完成对`nums2`的遍历后从`ans`中提取`nums1`中元素对应的下一个更大元素即可。

[题目链接](https://leetcode.cn/problems/next-greater-element-i/)：

python代码：

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        N = len(nums2)

        stack = []
        ans = {}

        for i in range(N-1, -1, -1):
            num = nums2[i]
            while stack and num > stack[-1]:
                stack.pop()
            
            ans[num] = stack[-1] if stack else -1
            stack.append(num)
        
        return [ans[num] for num in nums1]
```


C++代码：

```c++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        int N = nums2.size();

        stack<int> stk;
        unordered_map<int,int> table;

        for (int i=N-1; i>=0; --i) {
            while (!stk.empty() && stk.top() < nums2[i]) {
                stk.pop();
            }

            table[nums2[i]] = stk.empty() ? -1 : stk.top();
            stk.push(nums2[i]);
        }
        
        vector<int> res(nums1.size(), -1);
        for (int i=0; i<nums1.size(); ++i) {
            res[i] = table[nums1[i]];
        }

        return res;
    }
};
```


当然本题也可以使用[每日温度](/leetcode/2023-03-26-MonotoneStack.html#739-每日温度)中的单调递增栈模板来进行处理，代码可参考如下。

[题目链接](https://leetcode.cn/problems/next-greater-element-i/)：

python代码：

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        N = len(nums2)

        stack = []
        ans = {}

        for i in range(N):
            while stack and nums2[i] > nums2[stack[-1]]:
                idx = stack.pop()
                ans[nums2[idx]] = nums2[i]
            
            stack.append(i)
        
        return [ans.get(num, -1) for num in nums1]
```


### 503. 下一个更大元素 II

给定一个循环数组`nums`(`nums[nums.length - 1]`的下一个元素是`nums[0]`)，返回`nums`中每个元素的**下一个更大元素**。

数字`x`的**下一个更大的元素**是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出`-1`。

**示例1：**

```
输入: nums = [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```

**示例2：**

```
输入: nums = [1,2,3,4,3]
输出: [2,3,4,-1,4]
```

**提示：**

- 1 <= `nums.length` <= 10⁴
- -10⁹ <= nums[i] <= 10⁹

#### Solution

本题和[每日温度](/leetcode/2023-03-26-MonotoneStack.html#739-每日温度)以及[下一个更大元素 I](/leetcode/2023-03-26-MonotoneStack.html#496-下一个更大元素-i)的主要区别在于需要处理循环数组。而循环数组的处理方法非常简单，我们只需要对原始数组`nums`进行两次遍历即可。为了避免对数组进行复制，这里使用对编号`i`进行循环，而在索引元素时需要注意对`i`取余数`nums[i % N]`。

[题目链接](https://leetcode.cn/problems/next-greater-element-ii/)：

python代码：

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        N = len(nums)

        res = [-1 for i in range(N)]
        stack = []

        for i in range(N*2):
            while stack and nums[i % N] > nums[stack[-1] % N]:
                idx = stack.pop()
                res[idx % N] = nums[i % N]
            
            stack.append(i)
        
        return res
```


C++代码：

```c++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int N = nums.size();
        stack<int> stk;

        vector<int> res(N, -1);

        for (int i=0; i<2*N; ++i) {
            while (!stk.empty() && nums[stk.top()] < nums[i%N]) {
                int idx = stk.top(); stk.pop();
                res[idx] = nums[i%N];
            }

            stk.push(i%N);
        }

        return res;
    }
};
```


### 42. 接雨水

给定`n`个非负整数表示每个宽度为`1`的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png">
</div>

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

**提示：**

- `n` == `height.length`
- 1 <= `n` <= 2*10⁴
- 0 <= `height[i]` <= 10⁵

#### Solution

本题是单调栈中较为复杂的问题。我们可以从每一行出发，对于每一行上的槽`mid`分别找到它左边和右边第一个比它高的柱子记为`left`和`right`。这个槽的长度为`w = right - left - 1`，而高度为`h = min(height[left], height[right]) - height[mid]`，这样能够接收的雨水量为`h * w`。我们把每一行上每个槽的雨水量加起来就得到了最大雨水量。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/Wx6DB0g.png" width="70%">
</div>

因此本题的难点在于如何得到每个位置上左边和右边第一个比它高的柱子。对于右边第一个比它高的柱子，我们只需要按照[每日温度](/leetcode/2023-03-26-MonotoneStack.html#739-每日温度)中的模板使用单调递增栈就可以得到；而左边第一个比它高的柱子实际上就是在栈中当前柱子的下一个元素。因此当我们从栈中`pop()`出`mid`后只需要检查此时栈是否为空，如果非空则栈口的第一个元素就是左边第一个比它高的柱子`left`，而当前需要入栈的编号`i`即为`right`。然后按照上面的算法计算出`h`和`w`并对所有位置上的`h * w`进行累加即可。

[题目链接](https://leetcode.cn/problems/trapping-rain-water/)：

python代码：

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        N = len(height)

        stack = []
        res = 0

        for i in range(N):
            while stack and height[i] > height[stack[-1]]:
                mid = stack.pop()

                if stack:
                    left = stack[-1]
                    right= i

                    h = min(height[left], height[right]) - height[mid]
                    w = right - left - 1

                    res += h * w
            
            stack.append(i)

        return res
```


C++代码：

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int N = height.size();
        stack<int> stk;

        int res = 0;
        for (int i=0; i<N; ++i) {
            while (!stk.empty() && height[stk.top()] < height[i]) {
                int mid = stk.top(); stk.pop();
                
                if (!stk.empty()) {
                    int left  = stk.top();
                    int right = i;

                    int h = min(height[left], height[right]) - height[mid];
                    int w = right - left - 1;

                    res += h*w;
                }
            }
            stk.push(i);
        }

        return res;
    }
};
```


本题的另一种处理方式是使用动态规划。我们可以使用两个数组`leftMax`和`rightMax`分别记录下当前位置`i`处左右两边雨水能够到达的最大高度，这样存在递推关系：

- `leftMax[i]  = max(leftMax[i-1],  height[i])`
- `rightMax[i] = max(rightMax[i+1], height[i])`

因此可以正向遍历数组`height`得到`leftMax`的每个元素值，反向遍历得到`rightMax`的每个元素值。在每个位置`i`处能够得到的最大雨水量即为`min(leftMax[i], rightMax[i]) - height[i]`，最后对所有`i`进行求和即可。

<div align=center>
<img src="https://images.weserv.nl/?url=https://assets.leetcode-cn.com/solution-static/42/1.png">
</div>

[题目链接](https://leetcode.cn/problems/trapping-rain-water/)：

python代码：

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        N = len(height)
        if N == 0:
            return 0

        leftMax = [height[0] for i in range(N)]
        rightMax= [height[-1] for i in range(N)]

        for i in range(1, N):
            leftMax[i] = max(leftMax[i-1], height[i])
        
        for i in range(N-2, -1, -1):
            rightMax[i] = max(rightMax[i+1], height[i])
        
        res = 0
        for i in range(N):
            res += min(leftMax[i], rightMax[i]) - height[i]

        return res
```


C++代码：

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int N = height.size();
        if (N == 0) return 0;

        vector<int> leftMax(N, 0);
        leftMax[0] = height[0];
        for (int i=1; i<N; ++i) leftMax[i] = max(leftMax[i-1], height[i]);

        vector<int> rightMax(N, 0);
        rightMax[N-1] = height[N-1];
        for (int i=N-2; i>=0; --i) rightMax[i] = max(rightMax[i+1], height[i]);

        int res = 0;
        for (int i=0; i<N; ++i) res += min(leftMax[i], rightMax[i]) - height[i];

        return res;
    }
};
```


## 单调递减栈

### 84. 柱状图中最大的矩形

给定`n`个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为`1`。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/04/histogram.jpg">
</div>

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg">
</div>

```
输入： heights = [2,4]
输出： 4
```

**提示：**

- 1 <= `height.length` <= 10⁵
- 0 <= `height[i]` <= 10⁴

#### Solution

本题类似于[接雨水](/leetcode/2023-03-26-MonotoneStack.html#42-接雨水)。当我们让`heights`中的元素`mid`出栈时需要寻找到它左边和右边**第一个小于**`heights[mid]`的位置，分别记为`left`和`right`。因此本题中需要使用到**单调递减栈**，当元素`heights[i]`需要入栈时`i`即为`mid`右边第一个小于它的柱子编号`right`，而栈中`mid`后面的下一个元素即为左边第一个小于它的柱子编号`left`。这样包含`mid`最大矩形的高和宽分别为`h = heights[mid]`与`w = right - left - 1`，最大矩形面积为`h * w`，我们只需要对所有可能的最大矩形面积取最大值即可。

除此之外我们还需要在遍历前对`heights`数组的首尾添加一个`0`，这样可以保证`heights`单调递增或递减时仍然能够找到`left`和`right`。整个算法可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/YkniR1O.png" width="70%">
</div>

[题目链接](https://leetcode.cn/problems/largest-rectangle-in-histogram/)：

python代码：

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights = [0] + heights + [0]
        N = len(heights)

        stack = []
        res = 0

        for i in range(N):
            while stack and heights[i] < heights[stack[-1]]:
                mid = stack.pop()

                if stack:
                    left = stack[-1]
                    right= i

                    h = heights[mid]
                    w = right - left - 1

                    res = max(res, h*w)
            
            stack.append(i)

        return res
```


C++代码：

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        heights.insert(heights.begin(), 0);
        heights.push_back(0);

        int N = heights.size();
        stack<int> stk;
        stk.push(0);

        int res = 0;
        for (int i=1; i<N; ++i) {
            while (!stk.empty() && heights[stk.top()] > heights[i]) {
                int mid = stk.top(); stk.pop();

                if (!stk.empty()) {
                    int left = stk.top();
                    int right= i;

                    int h = heights[mid];
                    int w = right - left -1;

                    res = max(res, h*w);
                }
            }

            stk.push(i);
        }

        return res;
    }
};
```


## Reference

- [LeetCode：739.每日温度](https://www.bilibili.com/video/BV1my4y1Z7jj/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：496.下一个更大元素 I](https://www.bilibili.com/video/BV1jA411m7dX/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：503.下一个更大元素II](https://www.bilibili.com/video/BV15y4y1o7Dw/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：42.接雨水](https://www.bilibili.com/video/BV1uD4y1u75P/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：84.柱状图中最大的矩形](https://www.bilibili.com/video/BV1Ns4y1o7uB/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)