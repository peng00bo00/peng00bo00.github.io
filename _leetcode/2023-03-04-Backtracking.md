---
layout: post
title: 回溯
date: 2023-03-04
tags: leetcode
toc:
  sidebar: left
---

**回溯(backtracking)**的本质是暴力搜索，一般会通过函数的递归来实现。回溯一般可以解决如下问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后，解数独等

所有回溯可以解决的问题都可以抽象为树的结构。每一步问题子集的大小定义了树的宽度，而递归的次数定义了树的深度。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/OlMp0Bz.png" width="80%">
</div>

回溯的算法模板大致如下：

```
def backtracking():
    if 终止条件:
        存放结果
        return
    
    for (选择：本层集合中元素 (树中节点孩子的数量就是集合的大小) ):
        处理节点
        递归 backtracking()
        回溯(撤销对节点的处理)
```

## 组合

### 77. 组合

给定两个整数`n`和`k`，返回范围`[1, n]`中所有可能的`k`个数的组合。

你可以按**任何顺序**返回答案。

**示例1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例2：**

```
输入：n = 1, k = 1
输出：[[1]]
```

**提示：**

- 1 <= `n` <= 20。
- 1 <= `k` <= `n`。

#### Solution

组合问题是回溯算法的经典应用。由于组合是无序的，我们引入一个变量`startIdx`表示本次(递归)调用开始的数字，即本次递归中处理的子集为`[startIdx, startIdx+1, ..., n]`，这样就可以保证每次递归的结果不会和前面出现重复。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/LEDr8Cj.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/combinations/)：

python代码：

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        path= []

        def backtracking(startIdx: int) -> None:
            if len(path) == k:
                res.append(path[:])
                return
            
            for i in range(startIdx, n+1):
                path.append(i)
                backtracking(i+1)
                path.pop()
        
        backtracking(1)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        res.clear();
        path.clear();

        backtracking(n, k, 1);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(int n, int k, int start) {
        if (path.size() == k) {
            res.emplace_back(path);
            return;
        }

        for (int i=start; i<=n; ++i) {
            path.push_back(i);
            backtracking(n, k, i+1);
            path.pop_back();
        }
    }
};
```


更进一步，我们可以利用当前路径`path`的长度来控制每一层循环次数。假设当前搜索过程的路径为`path`而起始数字为`startIdx`，那么本层for循环中可能的最大数字为`n - (k - len(path)) + 1`，当`i`超过这个数字时集合中的剩余元素就小于`k`也就无法形成有效的`path`。利用这一性质我们可以实现对搜索树的剪枝，从而提升算法效率。剪枝过程可以参考下图。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/KGgRln1.png" width="90%">
</div>

使用剪枝来优化后的代码可参考如下。

[题目链接](https://leetcode.cn/problems/combinations/)：

python代码：

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res  = []
        path = []

        def backtracking(startIdx: int) -> None:
            if len(path) == k:
                res.append(path[:])
                return
            
            for i in range(startIdx, n-(k-len(path))+2):
                path.append(i)
                backtracking(i+1)
                path.pop()
        
        backtracking(1)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        res.clear();
        path.clear();

        backtracking(n, k, 1);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(int n, int k, int start) {
        if (path.size() == k) {
            res.emplace_back(path);
            return;
        }

        for (int i=start; i<=n; ++i) {
            path.push_back(i);
            backtracking(n, k, i+1);
            path.pop_back();
        }
    }
};
```


### 216. 组合总和 III

找出所有相加之和为`n`的`k`个数的组合，且满足下列条件：

- 只使用数字1到9
- 每个数字**最多使用一次**

返回**所有可能的有效组合的列表**。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

**示例1：**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。
```

**示例2：**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
解释:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
没有其他符合的组合了。
```

**示例3：**

```
输入: k = 4, n = 1
输出: []
解释: 不存在有效的组合。
在[1,9]范围内使用4个不同的数字，我们可以得到的最小和是1+2+3+4 = 10，因为10 > 1，没有有效的组合。
```

**提示：**

- 2 <= `k` <= 9。
- 1 <= `n` <= `60`。

#### Solution

本题的暴力解法类似于[组合](/leetcode/2023-03-04-Backtracking.html#77-组合)，我们只需要在收集结果时要求总和为`n`即可。

[题目链接](https://leetcode.cn/problems/combination-sum-iii/)：

python代码：

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        path= []

        def backtracking(startIdx: int) -> None:
            if len(path) == k and sum(path) == n:
                res.append(path[:])
                return
            
            for i in range(startIdx, 10):
                path.append(i)
                backtracking(i+1)
                path.pop()
        
        backtracking(1)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        res.clear();
        path.clear();

        backtracking(k, n, 0, 1);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(int k, int n, int sum, int start) {
        if (path.size() == k) {
            if (sum == n) res.emplace_back(path);

            return;
        }

        for (int i=start; i<=9; ++i) {
            path.push_back(i);
            backtracking(k, n, sum+i, i+1);
            path.pop_back();
        }
    }
};
```


本题的剪枝过程类似于[组合](/leetcode/2023-03-04-Backtracking.html#77-组合)，即每次for循环时数字的最大值为`10 - (k - len(path))`。除此之外我们还可以记录下当前`path`中数字的和`s = sum(path)`，当`s > n`时提前结束搜索。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/kM7Sby1.png">
</div>

[题目链接](https://leetcode.cn/problems/combination-sum-iii/)：

python代码：

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        path= []

        def backtracking(startIdx: int, s: int) -> None:
            if s > n:
                return
            
            if len(path) == k:
                if s == n:
                    res.append(path[:])
                return
            
            for i in range(startIdx, 10 - (k - len(path)) + 1):
                path.append(i)
                backtracking(i+1, s+i)
                path.pop()
        
        backtracking(1, 0)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        res.clear();
        path.clear();

        backtracking(k, n, 0, 1);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(int k, int n, int sum, int start) {
        if (sum > n) return;

        if (path.size() == k) {
            if (sum == n) res.emplace_back(path);

            return;
        }

        for (int i=start; i<=9-(k-path.size())+1; ++i) {
            path.push_back(i);
            backtracking(k, n, sum+i, i+1);
            path.pop_back();
        }
    }
};
```


### 17. 电话号码的字母组合

给定一个仅包含数字`2-9`的字符串，返回所有它能表示的字母组合。答案可以按**任意顺序**返回。

给出数字到字母的映射如下(与电话按键相同)。注意`1`不对应任何字母。

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png">
</div>

**示例1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例2：**

```
输入：digits = ""
输出：[]
```

**示例3：**

```
输入：digits = "2"
输出：["a","b","c"]
```

**提示：**

- 0 <= `digits.length` <= 4。
- `digits[i]`是范围`['2', '9']`的一个数字。

#### Solution

本题的暴力解法类似于[组合](/leetcode/2023-03-04-Backtracking.html#77-组合)，在处理时需要先建立数字到字母的映射`keyboard`然后对所有可能的字母组合进行暴力搜索。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/gee7c8t.png" width="90%">
</div>

[题目链接](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)：

python代码：

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        keyboard = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz"
        }

        res = []
        path= []

        def backtracking(startIdx: int) -> None:
            if startIdx == len(digits):
                if path:
                    res.append("".join(path[:]))
                return
            
            number = digits[startIdx]
            for letter in keyboard[number]:
                path.append(letter)
                backtracking(startIdx+1)
                path.pop()
        
        backtracking(0)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        res.clear();
        path.clear();

        if (digits.empty()) return res;

        backtracking(digits, 0);

        return res;
    }

private:
    const string keyboard[10] = {
        "",
        "",
        "abc",
        "def",
        "ghi",
        "jkl",
        "mno",
        "pqrs",
        "tuv",
        "wxyz"
    };

    vector<string> res;
    string path;

    void backtracking(string& digits, int startIdx) {
        if (startIdx == digits.size()) {
            res.emplace_back(path);
            return;
        }

        string letter = keyboard[digits[startIdx] - '0'];
        for (auto& ch: letter) {
            path.push_back(ch);
            backtracking(digits, startIdx+1);
            path.pop_back();
        }
    }
};
```


### 39. 组合总和

给你一个**无重复元素**的整数数组`candidates`和一个目标整数`target`，找出`candidates`中可以使数字和为目标数`target`的所有**不同组合**，并以列表形式返回。你可以按**任意顺序**返回这些组合。

`candidates`中的**同一个**数字可以**无限制重复被选取**。如果至少一个数字的被选数量不同，则两种组合是不同的。

对于给定的输入，保证和为`target`的不同组合数少于`150`个。

**示例1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例3：**

```
输入: candidates = [2], target = 1
输出: []
```

**提示：**

- 1 <= `candidates.length` <= 30。
- 2 <= `candidates[i]` <= 40。
- `candidates`的所有元素**互不相同**。
- 1 <= `target` <= 40。

#### Solution

本题解法类似于[组合总和 III](/leetcode/2023-03-04-Backtracking.html#216-组合总和-iii)，我们需要使用`startIdx`来记录当前搜索的起始数字索引，再使用`s`来记录搜索路径上的数字之和。和[组合总和 III](/leetcode/2023-03-04-Backtracking.html#216-组合总和-iii)不同的是本题中允许数字的重复使用，因此在进行回溯时不会修改当前的子集大小而是利用`s`和`target`的大小关系来控制递归是否终止。整个算法过程可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/JQ1lTUf.png" width="90%">
</div>

[题目链接](https://leetcode.cn/problems/combination-sum/)：

python代码：

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        path= []

        def backtracking(startIdx: int, s: int) -> None:
            if s > target:
                return
            elif s == target:
                res.append(path[:])
                return
            
            for i in range(startIdx, len(candidates)):
                num = candidates[i]

                path.append(num)
                backtracking(i, s+num)
                path.pop()
        
        backtracking(0, 0)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        res.clear();
        path.clear();

        backtracking(candidates, target, 0, 0);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& candidates, int target, int sum, int startIdx) {
        if (sum > target) return;
        else if (sum == target) {
            res.emplace_back(path);
            return;
        }

        for (int i=startIdx; i<candidates.size(); ++i) {
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum+candidates[i], i);
            path.pop_back();
        }
    }
};
```

而在进行剪枝优化时则需要先对`candidates`数组进行排序。然后在每一层的循环中如果发现`s+num > target`则表明之后的每一次循环`s+num`都会大于`target`，因此我们可以提前终止循环。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/S1aCkUV.png" width="90%">
</div>

python代码：

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        path= []
        
        candidates = sorted(candidates)

        def backtracking(startIdx: int, s: int) -> None:
            if s == target:
                res.append(path[:])
                return
            
            for i in range(startIdx, len(candidates)):
                num = candidates[i]

                if s+num > target:
                    return

                path.append(num)
                backtracking(i, s+num)
                path.pop()
        
        backtracking(0, 0)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        res.clear();
        path.clear();

        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& candidates, int target, int sum, int startIdx) {
        if (sum == target) {
            res.emplace_back(path);
            return;
        }

        for (int i=startIdx; i<candidates.size() && sum+candidates[i] <= target; ++i) {
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum+candidates[i], i);
            path.pop_back();
        }
    }
};
```

### 40. 组合总和 II

给定一个候选人编号的集合`candidates`和一个目标数`target`，找出`candidates`中所有可以使数字和为`target`的组合。

`candidates`中的每个数字在每个组合中只能使用**一次**。

**注意：**解集不能包含重复的组合。 

**示例1：**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

**示例2：**

```
输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```

**提示：**

- 1 <= `candidates.length` <= 100。
- 1 <= `candidates[i]` <= 50。
- 1 <= `target` <= 30。

#### Solution

本题和[组合总和](/leetcode/2023-03-04-Backtracking.html#39-组合总和)的区别在于`candidates`中的每个数字只能使用一次，而且`candidates`中可能会出现重复的数字。因此本题的关键在于如何对`candidates`进行去重，即保证使用过的元素不会被重复选取。这里可以首先对`candidates`进行**排序**，然后越过for循环中`startIdx`之后所有等于前一个数字的数字。

[题目链接](https://leetcode.cn/problems/combination-sum-ii/)：

python代码：

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []
        path= []
        candidates = sorted(candidates)

        def backtracking(startIdx: int, s: int) -> None:
            if s == target:
                res.append(path[:])
                return
            
            for i in range(startIdx, len(candidates)):
                num = candidates[i]

                if s+num > target:
                    return
                
                ## skip used num
                if i > startIdx and num == candidates[i-1]:
                    continue

                path.append(num)
                backtracking(i+1, s+num)
                path.pop()
        
        backtracking(0, 0)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        res.clear();
        path.clear();

        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& candidates, int target, int sum, int startIdx) {
        if (sum == target) {
            res.emplace_back(path);
            return;
        }

        for (int i=startIdx; i<candidates.size() && sum+candidates[i] <= target; ++i) {
            if (i>startIdx && candidates[i] == candidates[i-1]) continue;

            path.push_back(candidates[i]);
            backtracking(candidates, target, sum+candidates[i], i+1);
            path.pop_back();
        }
    }
};
```


## 分割

### 131. 分割回文串

给你一个字符串`s`，请你将`s`分割成一些子串，使每个子串都是**回文串**。返回`s`所有可能的分割方案。

**回文串**是正着读和反着读都一样的字符串。

**示例1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例2：**

```
输入：s = "a"
输出：[["a"]]
```

**提示：**

- 1 <= `s.length` <= 16。
- `s`仅由小写英文字母组成。

#### Solution

分割问题与[组合](/leetcode/2023-03-04-Backtracking.html#组合)问题是非常类似的。在分割问题中，分割位置的选取等价于组合问题中子集的选取。整个算法流程可以参考下图。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/tbiP98F.png" width="90%">
</div>

[题目链接](https://leetcode.cn/problems/palindrome-partitioning/)：

python代码：

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res = []
        path= []

        def backtracking(startIdx: int) -> None:
            if startIdx == len(s):
                res.append(path[:])
                return
            
            for i in range(startIdx, len(s)):
                ss = s[startIdx:i+1]
                
                if ss == ss[::-1]:
                    path.append(ss)
                    backtracking(i+1)
                    path.pop()
        
        backtracking(0)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        res.clear();
        path.clear();

        backtracking(s, 0);

        return res;
    }

private:
    vector<vector<string>> res;
    vector<string> path;

    void backtracking(string &s, int startIdx) {
        if (startIdx == s.size()) {
            res.emplace_back(path);
            return;
        }

        for (int i=startIdx; i<s.size(); ++i) {
            if (check(s, startIdx, i)) {
                string str = s.substr(startIdx, i-startIdx+1);
                path.push_back(str);
            } else continue;

            backtracking(s, i+1);
            path.pop_back();
        }
    }

    bool check(string &s, int left, int right) {
        while (left < right) {
            if (s[left] != s[right]) return false;

            ++left;
            --right;
        }

        return true;
    }
};
```


### 93. 复原IP地址

**有效IP地址**正好由四个整数(每个整数位于`0`到`255`之间组成，且不能含有前导`0`)，整数之间用`'.'`分隔。

- 例如：`"0.1.2.201"`和`"192.168.1.1"` **有效**IP地址，但是`"0.011.255.245"`、`"192.168.1.312"`和`"192.168@1.1"`是**无效**IP地址。

给定一个只包含数字的字符串`s`，用以表示一个IP地址，返回所有可能的有效`IP`地址，这些地址可以通过在`s`中插入'.'来形成。你**不能**重新排序或删除`s`中的任何数字。你可以按**任何**顺序返回答案。

**示例1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

**提示：**

- 1 <= `s.length` <= 20。
- `s`仅由数字组成。

#### Solution

本题解法类似于[分割回文串](/leetcode/2023-03-04-Backtracking.html#131-分割回文串)，但需要注意IP地址有自身的要求。因此我们可以利用IP地址的要求来进行剪枝：当路径`path`中存在4个整数时提前终止搜索，同时只把满足要求的数字所构成的IP地址添加到`path`中。整个算法流程可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/Z29ZrR1.png">
</div>

[题目链接](https://leetcode.cn/problems/restore-ip-addresses/)：

python代码：

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        res = []
        path= []

        def check(num: str) -> bool:
            if len(num) > 1 and num[0] == "0":
                return False
            elif int(num) > 255:
                return False

            return True 

        def backtracking(startIdx: int) -> None:
            if len(path) == 4:
                if startIdx == len(s):
                    res.append(".".join(path))
                return
            
            for i in range(startIdx, min(len(s), startIdx+3)):
                num = s[startIdx: i+1]
                
                if check(num):
                    path.append(num)
                    backtracking(i+1)
                    path.pop()
        
        backtracking(0)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        if (s.size() < 4 || s.size() > 12) return res;

        res.clear();

        backtracking(s, 0, 0);

        return res;
    }

private:
    vector<string> res;

    void backtracking(string &s, int startIdx, int pointNum) {
        if (pointNum == 3) {
            if (check(s, startIdx, s.size()-1)) res.emplace_back(s);

            return;
        }

        for (int i=startIdx; i<s.size(); ++i) {
            if (check(s, startIdx, i)) {
                s.insert(s.begin() + i + 1, '.');

                backtracking(s, i+2, pointNum+1);

                s.erase(s.begin() + i + 1);
            } else break;
        }
    }

    bool check(const string &s, int left, int right) {
        if (left > right) return false;
        else if (s[left] == '0' && left != right) return false;

        int num = 0;
        for (int i=left; i<=right; ++i) {
            if (s[i] > '9' || s[i] < '0') return false;

            num = num * 10 + (s[i] - '0');
            if (num > 255) return false;
        }

        return true;
    }
};
```


## 子集

### 78. 子集

给你一个整数数组`nums`，数组中的元素**互不相同**。返回该数组所有可能的子集(幂集)。

解集**不能**包含重复的子集。你可以按**任意顺序**返回解集。

**示例1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**提示：**

- 1 <= `nums.length` <= 10。
- -10 <= `nums[i]` <= 10。
- `nums`中的所有元素**互不相同**。

#### Solution

子集问题与[组合](/leetcode/2023-03-04-Backtracking.html#组合)以及[分割](/leetcode/2023-03-04-Backtracking.html#分割)问题之间的区别在于我们需要**收集搜索树的所有节点**，而不只是叶节点。因此在每次调用回溯函数时都需要把当前的搜索路径`path`添加到`res`中，整个算法流程可以参考下图。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/NjZaybT.png" width="90%">
</div>

[题目链接](https://leetcode.cn/problems/subsets/)：

python代码：

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        path= []

        def backtracking(startIdx: int) -> None:
            ## collect current path
            res.append(path[:])

            for i in range(startIdx, len(nums)):
                path.append(nums[i])
                backtracking(i+1)
                path.pop()
        
        backtracking(0)
        
        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        res.clear();
        path.clear();

        backtracking(nums, 0);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& nums, int startIdx) {
        res.emplace_back(path);

        if (startIdx == nums.size()) return;

        for (int i=startIdx; i<nums.size(); ++i) {
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
};
```


### 90. 子集 II

给你一个整数数组`nums`，其中可能包含重复元素，请你返回该数组所有可能的子集(幂集)。

解集**不能**包含重复的子集。返回的解集中，子集可以按**任意顺序**排列。

**示例1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**提示：**

- 1 <= `nums.length` <= 10。
- -10 <= `nums[i]` <= 10。

#### Solution

本题的解法可以参考[子集](/leetcode/2023-03-04-Backtracking.html#78-子集)和[组合总和 II](/leetcode/2023-03-04-Backtracking.html#40-组合总和-ii)，我们需要在子集问题的基础上加入去重逻辑来保证不会出现重复的路径。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/LWo8AEl.png" width="90%">
</div>

[题目链接](https://leetcode.cn/problems/subsets-ii/)：

python代码：

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res = []
        path= []

        ## sort nums
        nums = sorted(nums)

        def backtracking(startIdx: int) -> None:
            ## collect current path
            res.append(path[:])

            for i in range(startIdx, len(nums)):
                num = nums[i]

                ## skip used num
                if i > startIdx and num == nums[i-1]:
                    continue
                
                path.append(num)
                backtracking(i+1)
                path.pop()

        backtracking(0)
        
        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        res.clear();
        path.clear();

        sort(nums.begin(), nums.end());
        backtracking(nums, 0);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& nums, int startIdx) {
        res.emplace_back(path);

        for (int i=startIdx; i<nums.size(); ++i) {
            if (i > startIdx && nums[i] == nums[i-1]) continue;

            path.push_back(nums[i]);
            backtracking(nums, i+1);
            path.pop_back();
        }
    }
};
```


## 排列

### 46. 全排列

给定一个不含重复数字的数组`nums`，返回其**所有可能的全排列**。你可以**按任意顺序**返回答案。

**示例1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例3：**

```
输入：nums = [1]
输出：[[1]]
```

**提示：**

- 1 <= `nums.length` <= 6。
- -10 <= `nums[i]` <= 10。
- `nums`中的所有整数**互不相同**。

#### Solution

排列与组合的区别在于每一个排列都包含原始序列`nums`中的全部元素，只是顺序有所不同。因此在回溯函数中每条路径`path`的长度都是一样的，而且每次for循环中备选元素集合的大小会不断减少。本题可以把备选元素的集合作为参数传入回溯函数中，在for循环中去除当前元素来得到下一次调用的备选集合。整体算法流程可参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/W8tUciG.png">
</div>

[题目链接](https://leetcode.cn/problems/permutations/)：

python代码：

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        path= []

        def backtracking(nums: List[int]) -> None:
            if not nums:
                res.append(path[:])
                return
            
            for i, num in enumerate(nums):                
                path.append(num)
                backtracking(nums[:i] + nums[i+1:])
                path.pop()
        
        backtracking(nums)
        
        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        res.clear();
        path.clear();

        vector<bool> used = vector<bool>(nums.size(), false);
        backtracking(nums, used);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& nums, vector<bool>& used) {
        if (path.size() == nums.size()) {
            res.emplace_back(path);
            return;
        }

        for (int i=0; i<nums.size(); ++i) {
            if (used[i]) continue;

            used[i] = true;

            path.push_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();

            used[i] = false;
        }
    }
};
```


### 47. 全排列 II

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

**示例1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**提示：**

- 1 <= `nums.length` <= 8。
- -10 <= `nums[i]` <= 10。

#### Solution

本题需要在[全排列](/leetcode/2023-03-04-Backtracking.html#46-全排列)的基础上引入去重。具体来说我们需要对`nums`进行排序，然后在for循环中越过重复的元素。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/eQOzqhi.png">
</div>

[题目链接](https://leetcode.cn/problems/permutations-ii/)：

python代码：

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        res = []
        path= []

        N = len(nums)
        nums = sorted(nums)

        def backtracking(nums: List[int]) -> None:
            if len(path) == N:
                res.append(path[:])
                return

            for i, num in enumerate(nums):
                ## skip used num
                if i > 0 and nums[i] == nums[i-1]:
                    continue
                
                path.append(num)
                backtracking(nums[:i] + nums[i+1:])
                path.pop()
        
        backtracking(nums)

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        res.clear();
        path.clear();
        vector<bool> used(nums.size(), false);

        sort(nums.begin(), nums.end());
        backtracking(nums, used);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(vector<int>& nums, vector<bool> used) {
        if (path.size() == nums.size()) {
            res.emplace_back(path);
            return;
        }

        for (int i=0; i<nums.size(); ++i) {
            if (i>0 && nums[i] == nums[i-1] && used[i-1] == false) continue;

            if (used[i] == false) {
                used[i] = true;

                path.push_back(nums[i]);
                backtracking(nums, used);
                path.pop_back();

                used[i] = false;
            }
        }
    }
};
```


## 棋盘问题

### 51. N皇后

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

**n皇后问题**研究的是如何将`n`个皇后放置在`n×n`的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数`n`，返回所有不同的**n皇后问题**的解决方案。

每一种解法包含一个不同的**n皇后问题**的棋子放置方案，该方案中`'Q'`和`'.'`分别代表了皇后和空位。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/11/13/queens.jpg">
</div>

```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例2：**

```
输入：n = 1
输出：[["Q"]]
```

**提示：**

- 1 <= `nums.length` <= 8。
- -10 <= `nums[i]` <= 10。

#### Solution

N皇后是回溯的经典问题，它的解法是暴力搜索所有可能的棋子放置方式然后收集其中满足要求的解。因此我们需要额外定义一个`check()`函数检验棋盘上的棋子摆放是否满足要求，然后仅在它返回`True`时才继续向下进行搜索。整个算法流程可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/oKzf66e.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/n-queens/)：

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        res = []
        path= []

        ## attempt to place another queen
        def check(i: int) -> bool:
            for row, col in enumerate(path):
                if i == col or abs(i-col) == len(path)-row:
                    return False
            
            return True
        
        ## column index to chessboard
        def generateBoard() -> List[str]:
            board = []

            for idx in path:
                row = ["." for _ in range(n)]
                row[idx] = "Q"
                board.append("".join(row))

            return board

        def backtracking() -> None:
            if len(path) == n:
                res.append(generateBoard())
                return
            
            for i in range(n):
                if check(i):
                    path.append(i)
                    backtracking()
                    path.pop()
        
        backtracking()

        return res
```


### 52. N皇后 II

**n皇后问题** 研究的是如何将`n`个皇后放置在`n × n`的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数`n`，返回**n皇后问题**不同的解决方案的数量。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/11/13/queens.jpg">
</div>

```
输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

**示例2：**

```
输入：n = 1
输出：1
```

**提示：**

- 1 <= `n` <= 9。

#### Solution

本题解法与[N皇后](/leetcode/2023-03-04-Backtracking.html#51-n皇后)基本一致，只需在完成搜索令`res += 1`即可。

[题目链接](https://leetcode.cn/problems/n-queens-ii/)：

```python
class Solution:
    def totalNQueens(self, n: int) -> int:
        res = 0
        path= []

        def check(num: int) -> bool:
            for row, col in enumerate(path):
                if col == num or abs(col-num) == len(path) - row:
                    return False
            
            return True
        
        def backtracking() -> None:
            nonlocal res, path

            if len(path) == n:
                res += 1
                return
            
            for i in range(n):
                ## attempt to place a queen
                if check(i):
                    path.append(i)
                    backtracking()
                    path.pop()
        
        backtracking()

        return res
```


### 37. 解数独

编写一个程序，通过填充空格来解决数独问题。

数独的解法需**遵循如下规则**：

1. 数字`1-9`在每一行只能出现一次。
2. 数字`1-9`在每一列只能出现一次。
3. 数字`1-9`在每一个以粗实线分隔的`3x3`宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用`'.'`表示。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714svg.png">
</div>

```
输入：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
输出：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
解释：输入的数独如上图所示，唯一有效的解决方案如下所示：
```

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714_solutionsvg.png">
</div>

**提示：**

- `board.length` == 9。
- `board[i].length` == 9。
- `board[i][j]`是一位数字或者`'.'`。
- 题目数据**保证**输入数独仅有一个解。

#### Solution

数独问题可以说是回溯中最复杂的问题。在解数独中我们需要遍历整个棋盘上的所有位置，然后尝试去填充`[1, 9]`范围中的数字。如果能够找到满足要求的解直接返回，否则继续进行尝试。本题的一个技巧在于将回溯函数`backtracking()`的返回值设计为`bool`变量，如果棋盘完成了填充则返回`True`，而如果在任意空白处尝试了全部数字都无法满足要求则返回`False`。算法的整体流程可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/svJDIza.png">
</div>

[题目链接](https://leetcode.cn/problems/sudoku-solver/)：

```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """

        N = len(board)
        
        def check(i: int, j: int, num: str) -> bool:
            ## check row
            for col in range(9):
                if board[i][col] == num:
                    return False
            
            ## check column
            for row in range(9):
                if board[row][j] == num:
                    return False
            
            ## check block
            blockRow = (i // 3) * 3
            blockCol = (j // 3) * 3

            for x in range(blockRow, blockRow+3):
                for y in range(blockCol, blockCol+3):
                    if board[x][y] == num:
                        return False

            return True

        def backtracking() -> bool:
            for i in range(N):
                for j in range(N):
                    if board[i][j] != ".":
                        continue
                    
                    for num in range(1, 10):
                        num = str(num)
                        if check(i, j, num):
                            board[i][j] = num

                            if backtracking():
                                return True
                        
                            board[i][j] = "."
                    
                    return False
            
            return True
        
        backtracking()
```


## 其它

### 491. 递增子序列

给你一个整数数组`nums`，找出并返回所有该数组中不同的递增子序列，递增子序列中**至少有两个元素**。你可以按**任意顺序**返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

**示例1：**

```
输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

**示例2：**

```
输入：nums = [4,4,3,2,1]
输出：[[4,4]]
```

**提示：**

- 1 <= `nums.length` <= 15。
- -100 <= `nums[i]` <= 100。

#### Solution

本题解法类似于[子集 II](/leetcode/2023-03-04-Backtracking.html#90-子集-ii)，但二者的主要区别在于本题中序列元素的顺序不能随意更改，换句话说我们不能对`nums`进行排序来实现去重。因此本题的去重策略是使用一个集合`used`来存储当前循环中已经遍历过的元素，如果`nums[i]`在`used`中则跳过它并继续遍历。同时为了保证子序列是递增的，我们还需要检查`nums[i]`是否小于`path[-1]`，如果`nums[i] < path[-1]`同样需要跳过当前元素。整体算法流程可以参考如下。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/eDbFycf.png">
</div>

[题目链接](https://leetcode.cn/problems/non-decreasing-subsequences/)：

```python
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        res = []
        path= []

        def backtracking(startIdx: int) -> None:
            if len(path) > 1:
                res.append(path[:])
            
            used = set()
            for i in range(startIdx, len(nums)):
                ## skip used num
                if nums[i] in used:
                    continue
                
                ## skip decreasing path
                if path and path[-1] > nums[i]:
                    continue
                
                used.add(nums[i])
                path.append(nums[i])
                backtracking(i+1)
                path.pop()
        
        backtracking(0)

        return res
```


### 332. 重新安排行程

给你一份航线列表`tickets`，其中`tickets[i] = [fromi, toi]`表示飞机出发和降落的机场地点。请你对该行程进行重新规划排序。

所有这些机票都属于一个从`JFK(肯尼迪国际机场)`出发的先生，所以该行程必须从`JFK`开始。如果存在多种有效的行程，请你按字典排序返回最小的行程组合。

- 例如，行程`["JFK", "LGA"]`与`["JFK", "LGB"]`相比就更小，排序更靠前。

假定所有机票至少存在一种合理的行程。且所有的机票必须都用一次**且**只能用一次。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg">
</div>

```
输入：tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
输出：["JFK","MUC","LHR","SFO","SJC"]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg">
</div>

```
输入：tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
输出：["JFK","ATL","JFK","SFO","ATL","SFO"]
解释：另一种有效的行程是 ["JFK","SFO","ATL","JFK","ATL","SFO"] ，但是它字典排序更大更靠后。
```

#### Solution

本题中我们需要对所有可能的行程进行遍历，当找到一个符合要求的行程后直接退出即可。因此本题的技巧在于要对所有具有相同起点的机票进行排序，这样可以保证搜索过程中的路径`path`满足字典序；而另一个技巧在于设置终止条件，当`path`中的机场数恰为机票数加1时说明已经找到了满足要求的行程，此时直接退出即可。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/EbQmt6V.png" width="70%">
</div>

[题目链接](https://leetcode.cn/problems/reconstruct-itinerary/)：

```python
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        path= ["JFK"]

        tickets_dict = defaultdict(list)
        for start, end in tickets:
            tickets_dict[start].append(end)
        
        ## sort the arrival airports
        for start in tickets_dict:
            tickets_dict[start].sort()

        def backtracking(start: str) -> bool:
            if len(path) == len(tickets)+1:
                return True
            
            for _ in tickets_dict[start]:
                end = tickets_dict[start].pop(0)
                path.append(end)

                if backtracking(end):
                    return True

                path.pop()
                tickets_dict[start].append(end)
            
            return False

        backtracking("JFK")

        return path
```


## Reference

- [回溯算法理论基础](https://www.bilibili.com/video/BV1cy4y167mM/?vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：77.组合](https://www.bilibili.com/video/BV1ti4y1L7cv/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：77.组合(剪枝)](https://www.bilibili.com/video/BV1wi4y157er/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：216.组合总和III](https://www.bilibili.com/video/BV1wg411873x/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：17.电话号码的字母组合](https://www.bilibili.com/video/BV1yV4y1V7Ug/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：39.组合总和](https://www.bilibili.com/video/BV1KT4y1M7HJ/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：40.组合总和II](https://www.bilibili.com/video/BV12V4y1V73A/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：131.分割回文串](https://www.bilibili.com/video/BV1c54y1e7k6/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：93.复原IP地址](https://www.bilibili.com/video/BV1XP4y1U73i/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：78.子集](https://www.bilibili.com/video/BV1U84y1q7Ci/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：90.子集II](https://www.bilibili.com/video/BV1vm4y1F71J/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：491.递增子序列](https://www.bilibili.com/video/BV1EG4y1h78v/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：46.全排列](https://www.bilibili.com/video/BV19v4y1S79W/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：47.全排列II](https://www.bilibili.com/video/BV1R84y1i7Tm/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：51.N皇后](https://www.bilibili.com/video/BV1Rd4y1c7Bq/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：37.解数独](https://www.bilibili.com/video/BV1TW4y1471V/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)