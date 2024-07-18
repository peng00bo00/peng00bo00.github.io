---
layout: post
title: 哈希表
date: 2023-01-21
tags: leetcode
toc:
  sidebar: left
---


## 比较数组/字符串

### 242. 有效的字母异位词

给定两个字符串`s`和`t`，编写一个函数来判断`t`是否是`s`的字母异位词。

**注意**：若`s`和`t`中每个字符出现的次数都相同，则称`s`和`t`互为字母异位词。

**示例1：**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例2：**

```
输入: s = "rat", t = "car"
输出: false
```

**提示：**

- 1 <= `s.length, t.length` <= 5 * 10⁴
- `s`和`t`仅包含小写字母

#### Solution

本题的解法在于使用哈希表来统计`s`和`t`中字符出现的个数。如果两个哈希表完全相等则返回`True`，否则返回`False`。

[题目链接](https://leetcode.cn/problems/valid-anagram/)：

python代码：

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        
        s_dict = {}
        t_dict = {}

        for char in s:
            s_dict[char] = s_dict.get(char, 0) + 1
        
        for char in t:
            t_dict[char] = t_dict.get(char, 0) + 1
        
        return s_dict == t_dict
```


C++代码：

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) return false;

        int table[26] = {0};

        for (auto& ch : s) {
            ++table[ch - 'a'];
        }

        for (auto& ch : t) {
            --table[ch - 'a'];

            if (table[ch - 'a'] < 0) return false;
        }

        return true;
    }
};
```


当然本题也可以使用python内置的[Counter](https://docs.python.org/3/library/collections.html#collections.Counter)类来进行处理。很多记数问题直接使用`Counter`类会更容易一些。

```python
class Solution(object):
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import Counter

        a_count = Counter(s)
        b_count = Counter(t)

        return a_count == b_count
```


### 205. 同构字符串

给定两个字符串`s`和`t`，判断它们是否是同构的。

如果`s`中的字符可以按某种映射关系替换得到`t`，那么这两个字符串是同构的。

每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

**示例1：**

```
输入：s = "egg", t = "add"
输出：true
```

**示例2：**

```
输入：s = "foo", t = "bar"
输出：false
```

**示例3：**

```
输入：s = "paper", t = "title"
输出：true
```

**提示：**

- 1 <= `s.length` <= 5 * 10⁴
- `t.length` == `s.length`
- `s`和`t`由任意有效的ASCII字符组成

#### Solution

本题的解法在于使用两个哈希表`s2t`和`t2s`分别表示`s`到`t`和`t`到`s`之间的映射。如果这两个映射都是一一映射(双射)则返回`True`，否则返回`False`。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/3sf3khi.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/isomorphic-strings/)：

python代码：

```python
class Solution:
    def isIsomorphic(self, s: str, t: str) -> bool:
        s2t, t2s = {}, {}
        for ss, tt in zip(s, t):
            if (ss in s2t and s2t[ss] != tt) or \
               (tt in t2s and t2s[tt] != ss):
                return False
            
            s2t[ss], t2s[tt] = tt, ss

        return True
```


C++代码：

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char, char> s2t;
        unordered_map<char, char> t2s;

        for (int i=0; i < s.length(); ++i) {
            char ss = s[i];
            char tt = t[i];

            if ((s2t.count(ss) && s2t[ss] != tt) || (t2s.count(tt) && t2s[tt] != ss)) {
                return false;
            }

            s2t[ss] = tt;
            t2s[tt] = ss;
        }

        return true;
    }
};
```


## 寻找指定元素

### 202. 快乐数

编写一个算法来判断一个数`n`是不是快乐数。

**「快乐数」**定义为：

- 对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
- 然后重复这个过程直到这个数变为`1`，也可能是**无限循环**但始终变不到`1`。
- 如果这个过程**结果**为`1`，那么这个数就是快乐数。

如果`n`是**「快乐数」**就返回`true`；不是，则返回`false`。

**示例1：**

```
输入：n = 19
输出：true
解释：
1² + 9² = 82
8² + 2² = 68
6² + 8² = 100
1² + 0² + 0² = 1
```

**示例2：**

```
输入：n = 2
输出：false
```

**提示：**

- 1 <= `n` <= 2³¹ - 1

#### Solution

本题的难点在于如何考虑无限循环的问题。为了方便判断我们需要使用一个[集合](https://docs.python.org/3.9/library/stdtypes.html#set-types-set-frozenset)`nums`来存储已经收集到的结果，如果数字`n`出现在`nums`中则说明发生了循环直接返回`False`；否则按照题目公式生成下一个数字直到它等于`1`并返回`True`。

[题目链接](https://leetcode.cn/problems/happy-number/)：

python代码：

```python
class Solution:
    def isHappy(self, n: int) -> bool:

        def nextHappy(n: int) -> int:
            res = 0

            while n:
                res += (n % 10) * (n % 10)
                n = n // 10

            return res
        
        nums = set()

        while n != 1:
            if n in nums:
                return False
            
            nums.add(n)
            n = nextHappy(n)

        return True
```


C++代码：

```c++
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> nums;

        while (nums.count(n) == 0) {
            nums.emplace(n);

            if (n == 1) return true;

            n = nextHappy(n);
        }

        return false;
    }


    int nextHappy(int n) {
        int res = 0;

        while (n) {
            int x = n % 10;
            res += x*x;
            n = n / 10;
        }

        return res;
    }
};
```


### 1. 两数之和

给定一个整数数组`nums`和一个整数目标值`target`，请你在该数组中找出**和为目标值**`target`的那**两个**整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示：**

- 2 <= `nums.length` <= 10⁴
- -10⁹ <= `nums[i]` <= 10⁹
- -10⁹ <= `target` <= 10⁹
- **只会存在一个有效答案**

#### Solution

两数之和是哈希表的经典问题。本题中我们需要使用一个哈希表`records`对`nums`进行遍历，`records`的键值分别为`nums[idx]`和`idx`。这样在遍历时如果发现`target-nums[idx]`在`records`中则说明找到了和为`target`的一对数字，返回`[idx, records[target-nums[idx]]]`即可；否则继续遍历直至结束并返回`[]`。

[题目链接](https://leetcode.cn/problems/two-sum/)：

python代码：

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        records = {}
        for idx, num in enumerate(nums):
            if target-num in records:
                return [idx, records[target-num]]
            
            records[num] = idx
        
        return []
```


C++代码：

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> table;

        for (int i=0; i < nums.size(); ++i) {
            int x = nums[i];

            if (table.count(target - x)) {
                return vector<int> {i, table[target - x]};
            } else {
                table[x] = i;
            }
        }

        return {};
    }
};
```


### 454. 四数相加 II

给你四个整数数组`nums1`、`nums2`、`nums3`和`nums4`，数组长度都是`n`，请你计算有多少个元组`(i, j, k, l)`能满足：

- 0 <= `i, j, k, l` < `n`
- `nums1[i] + nums2[j] + nums3[k] + nums4[l]` == 0

**示例1：**

```
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

**示例2：**

```
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

**提示：**

- `n` == `nums1.length`
- `n` == `nums2.length`
- `n` == `nums3.length`
- `n` == `nums4.length`
- 1 <= `n` <= 200
- -2²⁸ <= `nums1[i], nums2[i], nums3[i], nums4[i]` <= 2²⁸

#### Solution

本题中我们可以把四个数组分为`nums1`和`nums2`以及`nums3`和`nums4`两组。首先对`nums1`和`nums2`进行遍历，使用一个哈希表`ret`记录所有可能的数字之和`nums1[i]+nums2[j]`及其出现的次数。然后对`nums3`和`nums4`进行遍历，如果`-(nums3[k]+nums4[l])`出现在`ret`中则说明出现了满足和为0的元组，令`count += ret[-(nums3[k]+nums4[l])]`，最后返回`res`即可。

[题目链接](https://leetcode.cn/problems/4sum-ii/)：

python代码：

```python
class Solution:
    def fourSumCount(self, nums1: List[int], nums2: List[int], nums3: List[int], nums4: List[int]) -> int:

        ret = {}
        for a in nums1:
            for b in nums2:
                ret[a+b] = ret.get(a+b, 0) + 1
        
        count = 0
        for c in nums3:
            for d in nums4:
                if -(c+d) in ret:
                    count += ret[-(c+d)]
        
        return count
```


C++代码：

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> ret;

        for (int x : nums1) {
            for (int y: nums2) {
                ++ret[x+y];
            }
        }

        int count = 0;

        for (int x : nums3) {
            for (int y: nums4) {
                if (ret.count(-x-y)) {
                    count += ret[-x-y];
                }
            }
        }

        return count;
    }
};
```


## 公共元素

### 1002. 查找共用字符

给你一个字符串数组`words`，请你找出所有在`words`的每个字符串中都出现的共用字符(**包括重复字符**)，并以数组形式返回。你可以按**任意顺序**返回答案。

**示例1：**

```
输入：words = ["bella","label","roller"]
输出：["e","l","l"]
```

**示例2：**

```
输入：words = ["cool","lock","cook"]
输出：["c","o"]
```

**提示：**

- 1 <= `words.length` <= 100
- 1 <= `words[i].length` <= 100
- `words[i]`由小写英文字母组成

#### Solution

本题的解法在于使用哈希表来统计`words`中每个单词里每个字母出现的频率，然后对每个字母的频率取最小值来得到公共字符。得到字符后将它们按频率输出到`res`中即可。

[题目链接](https://leetcode.cn/problems/find-common-characters/)：

python代码：

```python
class Solution:
    def commonChars(self, words: List[str]) -> List[str]:
        minFreq = [float("inf") for i in range(26)]

        for word in words:
            freq = [0 for i in range(26)]

            for char in word:
                freq[ord(char) - ord('a')] += 1
            
            for i in range(26):
                minFreq[i] = min(minFreq[i], freq[i])
        
        res = []
        for i in range(26):
            for _ in range(minFreq[i]):
                res.append(chr(ord('a')+i))

        return res
```


C++代码：

```c++
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        int minFreq[26];
        for (int i = 0; i < 26; ++i) {
            minFreq[i] = INT_MAX;
        }

        for (auto &word : words) {
            int freq[26] = {0};

            for (auto &ch : word) {
                ++freq[ch - 'a'];
            }

            for (int i = 0; i < 26; ++i) {
                minFreq[i] = min(minFreq[i], freq[i]);
            }
        }

        vector<string> res;
        for (int i = 0; i < 26; ++i) {
            for (int j = 0; j < minFreq[i]; ++j) {
                string s(1, 'a' + i);   // convert 'a'+i to string
                res.emplace_back(s);
            }
        }

        return res;
    }
};
```


本题同样可以使用python内置的[Counter](https://docs.python.org/3/library/collections.html#collections.Counter)类来进行处理。使用字符串`word`来初始化时会统计`word`中字符出现的个数，然后需要使用`Counter`类的**交集**运算符`&`对每个单词进行遍历从而得到公共字符。最后按照公共字符的频率进行输出即可。

```python
class Solution:
    def commonChars(self, words: List[str]) -> List[str]:
        from collections import Counter

        freq = Counter(words[0])
        for word in words:
            freq = freq & Counter(word)
        
        res = []
        for k, v in freq.items():
            for _ in range(v):
                res.append(k)

        return res
```


### 349. 两个数组的交集

给定两个数组`nums1`和`nums2`，返回**它们的交集**。输出结果中的每个元素一定是**唯一**的。我们可以**不考虑输出结果的顺序**。

**示例1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

**提示：**

- 1 <= `nums1.length, nums2.length` <= 1000
- 0 <= `nums1[i], nums2[i]` <= 1000

#### Solution

本题只需要将两个数组转换为集合并计算它们的交集即可。

[题目链接](https://leetcode.cn/problems/intersection-of-two-arrays/)：

python代码：

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        set1 = {num: 1 for num in nums1}
        set2 = {num: 1 for num in nums2}

        return [num for num in set1 if num in set2]
```


```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        set1 = set(nums1)
        set2 = set(nums2)

        return list(set1 & set2)
```


C++代码：

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set1, set2;

        for (auto &x : nums1) set1.emplace(x);
        for (auto &x : nums2) set2.emplace(x);

        return setIntersection(set1, set2);
    }

    vector<int> setIntersection(unordered_set<int>& set1, unordered_set<int>& set2) {
        if (set1.size() > set2.size()) return setIntersection(set2, set1);

        vector<int> res;
        for (auto &x : set1) {
            if (set2.count(x)) res.emplace_back(x);
        }

        return res;
    }
};
```


### 383. 赎金信

给你两个字符串：`ransomNote`和`magazine`，判断`ransomNote`能不能由`magazine`里面的字符构成。

如果可以，返回`true`；否则返回`false`。

`magazine`中的每个字符只能在`ransomNote`中使用一次。

**示例1：**

```
输入：ransomNote = "a", magazine = "b"
输出：false
```

**示例2：**

```
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

**示例3：**

```
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

**提示：**

- 1 <= `ransomNote.length, magazine.length` <= 10⁵。
- `ransomNote`和`magazine`由小写英文字母组成。

#### Solution

本题只需要分别统计`ransomNote`和`magazine`中字符出现的频率，然后判断`ransomNote`中的频率是否都大于等于`magazine`中字符的频率即可。

[题目链接](https://leetcode.cn/problems/ransom-note/)：

python代码：

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        chars = {}

        for char in magazine:
            chars[char] = chars.get(char, 0) + 1

        for char in ransomNote:
            chars[char] = chars.get(char, 0) - 1
        
        for char, count in chars.items():
            if count < 0:
                return False
        
        return True
```


C++代码：

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        if (ransomNote.size() > magazine.size()) return false;

        int freq[26] = {0};
        for (auto &ch : magazine) {
            ++freq[ch - 'a'];
        }

        for (auto &ch : ransomNote) {
            --freq[ch - 'a'];

            if (freq[ch - 'a'] < 0) return false;
        }

        return true;
    }
};
```


除此之外本题同样可以使用python内置的[Counter](https://docs.python.org/3/library/collections.html#collections.Counter)类来进行处理。这里需要使用到`Counter`类的**差集**运算符`-`，它会对两个`Counter`对象具有相同键的值作差并只保留正数部分。因此我们只需要判断`rCount - mCount`的长度是否为`0`即可。

[题目链接](https://leetcode.cn/problems/ransom-note/)：

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        from collections import Counter

        rCount = Counter(ransomNote)
        mCount = Counter(magazine)
        
        return len(rCount - mCount) == 0
```


## Reference

- [哈希表理论基础](https://programmercarl.com/%E5%93%88%E5%B8%8C%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
- [Leetcode：242.有效的字母异位词](https://www.bilibili.com/video/BV1YG411p7BA/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [Leetcode：349.两个数组的交集](https://www.bilibili.com/video/BV1ba411S7wu/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [Leetcode：1.两数之和](https://www.bilibili.com/video/BV1aT41177mK/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：454.四数相加II](https://www.bilibili.com/video/BV1Md4y1Q7Yh/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)