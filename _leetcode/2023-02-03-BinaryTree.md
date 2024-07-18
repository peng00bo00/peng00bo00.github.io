---
layout: post
title: 二叉树
date: 2023-02-03
tags: leetcode
toc:
  sidebar: left
---

**二叉树(binary tree)**是一种基础数据结构。在二叉树中，每个节点除了自身保存的数据外还包含两个指针`left`和`right`分别指向左右两个子节点。因此python中二叉树(节点)的定义可参考如下：

```python
class TreeNode: 
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```


## 遍历

二叉树的经典问题是对树中的节点进行遍历。根据惯例，我们在访问子节点时会按照先左后右的顺序进行遍历。因此根据对当前节点的访问顺序二叉树的遍历可以分为**前序遍历(中左右)**、**中序遍历(左中右)**和**后序遍历(左右中)**三种。

### 144. 二叉树的前序遍历

给你二叉树的根节点`root`，返回它节点值的**前序**遍历。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg">
</div>

```
输入：root = [1,null,2,3]
输出：[1,2,3]
```

**示例2：**

```
输入：root = []
输出：[]
```

**示例3：**

```
输入：root = [1]
输出：[1]
```

**示例4：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg">
</div>

```
输入：root = [1,2]
输出：[1,2]
```

**示例5：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/15/inorder_4.jpg">
</div>

```
输入：root = [1,null,2]
输出：[1,2]
```

**提示：**

- 树中节点数目在范围`[0, 100]`内。
- -100 <= `Node.val` <= 100。

#### Solution

二叉树的遍历有着天然的递归结构，我们只需要按照访问顺序进行递归调用即可。

[题目链接](https://leetcode.cn/problems/binary-tree-preorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        return [root.val] + self.preorderTraversal(root.left) + self.preorderTraversal(root.right)
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
    void traversal(TreeNode* root, vector<int> &res) {
        if (root == nullptr) return;

        res.push_back(root->val);
        traversal(root->left,  res);
        traversal(root->right, res);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);

        return res;
    }

};
```


除了递归解法之外，我们还可以使用迭代来进行遍历。和递归相比，通过迭代进行的遍历可以减少函数调用的开销，因此在程序运行时有着更好的性能。二叉树遍历的迭代解法思路是利用栈来模拟程序的递归调用，其模板如下：

```
while stack or cur:
    while cur:
        stack.append(cur)
        cur 向下遍历
    
    cur = stack.pop()
    使用 cur.left 或 cur.right 来更新 cur
```

前序遍历的特点是从当前节点沿左节点自上而下访问沿途节点，然后再自下而上访问沿途节点的右子树。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/VAD6jmh.png" width="80%">
</div>

因此利用循环模板可以得到如下代码。

[题目链接](https://leetcode.cn/problems/binary-tree-preorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        stack = []
        cur = root

        while stack or cur:
            while cur:
                res.append(cur.val)
                stack.append(cur)
                cur = cur.left

            cur = stack.pop()
            cur = cur.right 
        
        return res
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
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> res;

        TreeNode* cur = root;

        while (!stk.empty() || cur != nullptr) {
            while (cur != nullptr) {
                res.push_back(cur->val);
                stk.push(cur);

                cur = cur->left;
            }

            cur = stk.top(); stk.pop();
            cur = cur->right;
        }

        return res;
    }
};
```


### 94. 二叉树的中序遍历

给定一个二叉树的根节点`root`，返回它的**中序**遍历。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg">
</div>

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例2：**

```
输入：root = []
输出：[]
```

**示例3：**

```
输入：root = [1]
输出：[1]
```

**提示：**

- 树中节点数目在范围`[0, 100]`内。
- -100 <= `Node.val` <= 100。

#### Solution

中序遍历的递归解法与[前序遍历](/leetcode/2023-02-03-BinaryTree.html#144-二叉树的前序遍历)类似，只需调整节点上的相对访问顺序即可。

[题目链接](https://leetcode.cn/problems/binary-tree-inorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        return self.inorderTraversal(root.left) + [root.val] + self.inorderTraversal(root.right)
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
    void traversal(TreeNode* root, vector<int> &res) {
        if (root == nullptr) return;

        res.push_back(root->val);
        traversal(root->left,  res);
        traversal(root->right, res);
    }

    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);

        return res;
    }

};
```


中序遍历的节点访问顺序是沿左节点，自下而上访问沿途节点及其右子树。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/9u57wmH.png" width="80%">
</div>

因此在迭代解法中只需要调整向`res`添加节点值的顺序即可，此时我们只在出栈时才将节点值添加到`res`中。

[题目链接](https://leetcode.cn/problems/binary-tree-inorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        res = []
        stack = []
        cur = root

        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.left
            
            cur = stack.pop()
            res.append(cur.val)
            cur = cur.right

        return res
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
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> res;

        TreeNode* cur = root;

        while (!stk.empty() || cur != nullptr) {
            while (cur != nullptr) {
                stk.push(cur);
                cur = cur->left;
            }

            cur = stk.top(); stk.pop();
            res.push_back(cur->val);
            cur = cur->right;
        }

        return res;
    }
};
```


### 145. 二叉树的后序遍历

给你一棵二叉树的根节点`root`，返回其节点值的**后序**遍历。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/08/28/pre1.jpg">
</div>

```
输入：root = [1,null,2,3]
输出：[3,2,1]
```

**示例2：**

```
输入：root = []
输出：[]
```

**示例3：**

```
输入：root = [1]
输出：[1]
```

**提示：**

- 树中节点数目在范围`[0, 100]`内。
- -100 <= `Node.val` <= 100。

#### Solution

后序遍历的递归解法同样非常简单。

[题目链接](https://leetcode.cn/problems/binary-tree-postorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        return self.postorderTraversal(root.left) + self.postorderTraversal(root.right) + [root.val]
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
    void traversal(TreeNode* root, vector<int> &res) {
        if (root == nullptr) return;

        traversal(root->left,  res);
        traversal(root->right, res);
        res.push_back(root->val);
    }

    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        traversal(root, res);

        return res;
    }
};
```


后序遍历的迭代解法与前序遍历基本一致，只不过需要先不断向下访问右节点然后再自下而上访问左子树。同时需要注意的是最后要把结果反序输出。

[题目链接](https://leetcode.cn/problems/binary-tree-postorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:        
        res = []
        stack = []
        cur = root

        while stack or cur:
            while cur:
                res.append(cur.val)
                
                stack.append(cur)
                cur = cur.right
            
            cur = stack.pop()
            cur = cur.left

        return res[::-1]
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
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> stk;
        vector<int> res;

        TreeNode* cur = root;

        while (!stk.empty() || cur != nullptr) {
            while (cur != nullptr) {
                res.push_back(cur->val);
                stk.push(cur);
                
                cur = cur->right;
            }

            cur = stk.top(); stk.pop();
            cur = cur->left;
        }

        reverse(res.begin(), res.end());

        return res;
    }
};
```


### 102. 二叉树的层序遍历

给你二叉树的根节点`root`，返回其节点值的**层序遍历**。(即逐层地，从左到右访问所有节点)。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/tree1.jpg">
</div>

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

**示例2：**

```
输入：root = [1]
输出：[[1]]
```

**示例3：**

```
输入：root = []
输出：[]
```

**提示：**

- 树中节点数目在范围`[0, 2000]`内。
- -1000 <= `Node.val` <= 1000。

#### Solution

二叉树的另一种遍历方式是层序遍历。顾名思义，层序遍历会按照节点所在的层级从左到右依次进行遍历，它的本质是**广度优先搜索(breadth-first search, BFS)**。因此我们可以使用一个队列来存储位于同一层级的节点，然后依次进行访问。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UM13bu7.gif" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/binary-tree-level-order-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        from collections import deque

        res = []
        queue = deque([root])

        while queue:
            N = len(queue)
            level = []

            for i in range(N):
                node = queue.popleft()
                level.append(node.val)

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            res.append(level)

        return res
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        queue<TreeNode*> que;

        if (root != nullptr) que.push(root);

        while (!que.empty()) {
            int N = que.size();
            vector<int> level;

            for (int i = 0; i < N; ++i) {
                TreeNode* node = que.front();
                que.pop();

                level.push_back(node->val);

                if (node->left != nullptr) que.push(node->left);
                if (node->right != nullptr) que.push(node->right);
            }

            res.emplace_back(level);
        }

        return res;
    }
};
```

## 属性

### 101. 对称二叉树

给你一个二叉树的根节点`root`，检查它是否轴对称。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/symtree1.jpg">
</div>

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/symtree2.jpg">
</div>

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

**提示：**

- 树中节点数目在范围`[0, 1000]`内。
- -100 <= `Node.val` <= 100。

#### Solution

判断一棵树是否对称时我们需要考虑它的`left`和`right`两棵子树是否对称：

- 如果`left`和`right`均为空则**对称**；
- 如果`left`和`right`中一个为空另一个非空则**非对称**；
- 如果`left`和`right`均非空但`val`不相等则**非对称**；
- 如果`left`和`right`均非空且`val`相等则需要继续判断。

因此判断树是否对称具有天然的递归结构：当`left`和`right`非空且`val`相等时则需要进一步考虑树的内外两侧是否分别对称，只有当两侧都是对称时`left`和`right`才是对称的。

<div align=center>
<img src="https://images.weserv.nl/?url=camo.githubusercontent.com/c6b7e44d2d4a15a2ab7bf066addab1380aab6e9ce15491a4aaca04388abebc7b/68747470733a2f2f696d672d626c6f672e6373646e696d672e636e2f32303231303230333134343632343431342e706e67" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/symmetric-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        def compare(T1: Optional[TreeNode], T2: Optional[TreeNode]) -> bool:
            if (not T1) and (not T2):
                return True
            elif (not T1) or (not T2) or (T1.val != T2.val):
                return False
            
            return compare(T1.left, T2.right) and compare(T1.right, T2.left)
        
        return compare(root.left, root.right)
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
    bool compare(TreeNode* T1, TreeNode* T2) {
        if (T1 == nullptr && T2 == nullptr) return true;
        else if (T1 == nullptr && T2 != nullptr) return false;
        else if (T1 != nullptr && T2 == nullptr) return false;
        else if (T1->val != T2->val) return false;

        return compare(T1->left, T2->right) && compare(T1->right, T2->left);
    }

    bool isSymmetric(TreeNode* root) {
        if (root == nullptr) return true;
        
        return compare(root->left, root->right);
    }
};
```


我们同样可以使用广度优先搜索的思想来处理这样的问题，此时队列中按照由外向内的顺序存储了每一层的一对节点。进行遍历时每次从队列取一对节点进行比较，并且按照由外向内的顺序把它们的子节点加入到队列中。整个比较过程可以参考如下：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/UO7P6jh.gif">
</div>

[题目链接](https://leetcode.cn/problems/symmetric-tree/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        from collections import deque
        
        queue = deque([root.left, root.right])

        while queue:
            node1 = queue.popleft()
            node2 = queue.popleft()

            if (not node1) and (not node2):
                continue
            elif (not node1) or (not node2) or (node1.val != node2.val):
                return False
            
            queue.append(node1.left)
            queue.append(node2.right)

            queue.append(node1.right)
            queue.append(node2.left)
        
        return True
```


除此之外也可以使用[层序遍历](/leetcode/2023-02-03-BinaryTree.html#102-二叉树的层序遍历)来进行处理，此时只需要考虑每一层是否对称即可。

[题目链接](https://leetcode.cn/problems/symmetric-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        
        from collections import deque

        queue = deque([root.left, root.right])

        while queue:
            N = len(queue)
            for i in range(N//2):
                node1 = queue[i]
                node2 = queue[N-1-i]
                
                if not node1 and not node2:
                    continue
                elif node1 and not node2:
                    return False
                elif not node1 and node2:
                    return False
                elif node1.val != node2.val:
                    return False

            for i in range(N):
                node = queue.popleft()
                
                if node:
                    queue.append(node.left)
                    queue.append(node.right)
        
        return True
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
    bool isSymmetric(TreeNode* root) {
        if (root == nullptr) return true;

        queue<TreeNode*> que;
        que.push(root);

        while (!que.empty()) {
            int N = que.size();
            vector<TreeNode*> level;

            for (int i=0; i<N; ++i) {
                level.push_back(que.front());
                que.pop();
            }

            for (int i=0; i<N/2; ++i) {
                TreeNode* n1 = level[i];
                TreeNode* n2 = level[N-1-i];

                if (n1 == nullptr && n2 == nullptr) continue;
                else if (n1 == nullptr && n2 != nullptr) return false;
                else if (n1 != nullptr && n2 == nullptr) return false;
                else if (n1->val != n2->val) return false;
            }

            for (auto n : level) {
                if (n != nullptr) {
                    que.push(n->left);
                    que.push(n->right);
                }
            }
        }

        return true;
    }
};
```


### 100. 相同的树

给你两棵二叉树的根节点`p`和`q`，编写一个函数来检验这两棵树是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/20/ex1.jpg">
</div>

```
输入：p = [1,2,3], q = [1,2,3]
输出：true
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/20/ex2.jpg">
</div>

```
输入：p = [1,2], q = [1,null,2]
输出：false
```

**示例3：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/20/ex3.jpg">
</div>

```
输入：p = [1,2,1], q = [1,1,2]
输出：false
```

**提示：**

- 两棵树上的节点数目都在范围`[0, 100]`内。
- -10⁴ <= `Node.val` <= 10⁴。

#### Solution

本题解法与[对称二叉树](/leetcode/2023-02-03-BinaryTree.html#101-对称二叉树)基本一致，只需分别判断两棵树的相应节点即可。

[题目链接](https://leetcode.cn/problems/same-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        elif (not p) or (not q) or (p.val != q.val):
            return False
        
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```


python代码：

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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (p == nullptr && q == nullptr) return true;
        else if (p != nullptr && q == nullptr) return false;
        else if (p == nullptr && q != nullptr) return false;
        else if (p->val != q->val) return false;

        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
};
```


### 572. 另一棵树的子树

给你两棵二叉树`root`和`subRoot`。检验`root`中是否包含和`subRoot`具有相同结构和节点值的子树。如果存在，返回`true`；否则，返回`false`。

二叉树`tree`的一棵子树包括`tree`的某个节点和这个节点的所有后代节点。`tree`也可以看做它自身的一棵子树。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg">
</div>

```
输入：root = [3,4,5,1,2], subRoot = [4,1,2]
输出：true
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg">
</div>

```
输入：root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
输出：false
```

**提示：**

- `root`树上的节点数量范围是`[1, 2000]`。
- `subRoot`树上的节点数量范围是`[1, 1000]`。
- -10⁴ <= `root.val` <= 10⁴。
- -10⁴ <= `subRoot.val` <= 10⁴。

#### Solution

本题解法可参考[相同的树](/leetcode/2023-02-03-BinaryTree.html#100-相同的树)。我们首先定义一个比较函数`compare(T1, T2)`用来判断两棵树是否相同，然后分别测试`root`与`subRoot`是否相同、`root.left`或者`root.right`是否包含`subRoot`即可。

[题目链接](https://leetcode.cn/problems/subtree-of-another-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        def compare(T1: Optional[TreeNode], T2: Optional[TreeNode]) -> bool:
            if (not T1) and (not T2):
                return True
            elif (not T1) or (not T2) or T1.val != T2.val:
                return False

            return compare(T1.left, T2.left) and compare(T1.right, T2.right)
        
        if not root and not subRoot:
            return True
        elif not root and subRoot:
            return False
        
        return compare(root, subRoot) or self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)
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
    bool compare(TreeNode* T1, TreeNode* T2) {
        if (T1 == nullptr && T2 == nullptr) return true;
        else if (T1 == nullptr && T2 != nullptr) return false;
        else if (T1 != nullptr && T2 == nullptr) return false;
        else if (T1->val != T2->val) return false;

        return compare(T1->left, T2->left) && compare(T1->right, T2->right);
    }

    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        if (root == nullptr) return false;
        
        return compare(root, subRoot) || isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);
    }
};
```


本题的迭代解法则需要使用广度优先或者深度优先对`root`进行遍历：如果找到相同的树则返回`True`，否则继续向下直到`root`中所有节点都进行过比较并最终返回`False`。

[题目链接](https://leetcode.cn/problems/subtree-of-another-tree/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        def compare(T1: Optional[TreeNode], T2: Optional[TreeNode]) -> bool:
            if (not T1) and (not T2):
                return True
            elif (not T1) or (not T2) or T1.val != T2.val:
                return False

            return compare(T1.left, T2.left) and compare(T1.right, T2.right)
        
        from collections import deque

        queue = deque([root])

        while queue:
            node = queue.popleft()

            if compare(node, subRoot):
                return True
            elif node:
                queue.append(node.left)
                queue.append(node.right)
        
        return False
```


### 104. 二叉树的最大深度

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明: **叶子节点是指没有子节点的节点。

**示例：**
给定二叉树`[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度`3`。

#### Solution

二叉树最大深度的基本解法是使用递归：我们先分别计算左子树和右子树的最大深度`d1`和`d2`，然后当前节点的最大深度等于`max(d1, d2)+1`。

[题目链接](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        d1 = self.maxDepth(root.left)
        d2 = self.maxDepth(root.right)

        return max(d1, d2)+1
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
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;

        int leftDepth = maxDepth(root->left);
        int rightDepth= maxDepth(root->right);

        return max(leftDepth, rightDepth) + 1;
    }
};
```


而最大深度的迭代解法则类似于[层序遍历](/leetcode/2023-02-03-BinaryTree.html#102-二叉树的层序遍历)。我们使用一个变量`depth`来记录当前的深度，每深入一层就令`depth`加一。当队列为空时`depth`即为最大深度。

[题目链接](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        from collections import deque

        queue = deque([root])
        depth = 0

        while queue:
            N = len(queue)
            depth += 1

            for i in range(N):
                node = queue.popleft()

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return depth
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
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;

        queue<TreeNode*> que;
        int depth = 0;

        que.push(root);

        while (!que.empty()) {
            int N = que.size();

            for (int i=0; i<N; ++i) {
                TreeNode* n = que.front(); que.pop();

                if (n->left != nullptr) que.push(n->left);
                if (n->right != nullptr) que.push(n->right);
            }

            ++depth;
        }

        return depth;
    }
};
```


### 559. N叉树的最大深度

给定一个N叉树，找到其最大深度。

最大深度是指从根节点到最远叶子节点的最长路径上的节点总数。

N叉树输入按层序遍历序列化表示，每组子节点由空值分隔（请参见示例）。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2018/10/12/narytreeexample.png" width="50%">
</div>

```
输入：root = [1,null,3,2,4,null,5,6]
输出：3
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2019/11/08/sample_4_964.png" width="55%">
</div>

```
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：5
```
#### Solution

本题解法与[二叉树最大深度](/leetcode/2023-02-03-BinaryTree.html#104-二叉树的最大深度)基本一致。

[题目链接](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)：

python代码：

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        elif len(root.children) == 0:
            return 1
        
        return 1 + max([self.maxDepth(child) for child in root.children])
```


C++代码：

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    int maxDepth(Node* root) {
        if (root == nullptr) return 0;

        int depth = 0;
        for (auto child : root->children) {
            depth = max(depth, maxDepth(child));
        }

        return depth+1;
    }
};
```


python代码：

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        
        from collections import deque

        queue = deque([root])
        depth = 0

        while queue:
            N = len(queue)
            depth += 1

            for i in range(N):
                node = queue.popleft()
                
                for child in node.children:
                    queue.append(child)
        
        return depth
```


C++代码：

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val) {
        val = _val;
    }

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
*/

class Solution {
public:
    int maxDepth(Node* root) {
        if (root == nullptr) return 0;

        queue<Node*> que;
        que.push(root);

        int depth = 0;

        while (!que.empty()) {
            int N = que.size();

            for (int i=0; i<N; ++i) {
                Node* n = que.front(); que.pop();

                for (auto child : n->children) {
                    que.push(child);
                }
            }

            ++depth;
        }

        return depth;
    }
};
```


### 111. 二叉树的最小深度

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

**说明：**叶子节点是指没有子节点的节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg">
</div>

```
输入：root = [3,9,20,null,null,15,7]
输出：2
```

**示例2：**

```
输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```

**提示：**

- 树中节点数的范围在`[0, 10⁵]`内。
- -1000 <= `Node.val` <= 1000。

#### Solution

本题解法与[二叉树最大深度](/leetcode/2023-02-03-BinaryTree.html#104-二叉树的最大深度)类似，但需要注意的是节点的最小深度并不是左右子树深度的最小值加1，而是需要考虑两棵子树是否存在：

- 如果左右两棵子树都存在，则当前节点的最小深度是两棵子树深度的最小值加1。
- 如果只有一棵子树存在，则当前节点的最小深度是存在子树的深度加1。
- 如果两棵子树都不存在，则当前节点的深度为1。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/6G1rJ9c.png" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        if root.left and root.right:
            d1 = self.minDepth(root.left)
            d2 = self.minDepth(root.right)

            return 1 + min(d1, d2)
        elif root.left:
            return 1 + self.minDepth(root.left)
        elif root.right:
            return 1 + self.minDepth(root.right)
        else:
            return 1
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
    int minDepth(TreeNode* root) {
        if (root == nullptr) return 0;

        int leftDepth = minDepth(root->left);
        int rightDepth= minDepth(root->right);

        if (root->left != nullptr && root->right != nullptr) return min(leftDepth, rightDepth) + 1;
        else if (root->left == nullptr) return rightDepth+1;
        else return leftDepth+1;
    }
};
```


基于层序遍历的迭代解法思路更加清晰：我们只需要在遍历节点时额外检查当前节点是否为叶节点，如果是叶节点就直接返回深度。

[题目链接](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        from collections import deque

        queue = deque([root])
        depth = 0

        while queue:
            N = len(queue)
            depth += 1

            for i in range(N):
                node = queue.popleft()

                if not node.left and not node.right:
                    return depth
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

        return depth
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
    int minDepth(TreeNode* root) {
        if (root == nullptr) return 0;

        queue<TreeNode*> que;
        que.push(root);

        int depth = 0;

        while (!que.empty()) {
            int N = que.size();
            ++depth;

            for (int i=0; i<N; ++i) {
                TreeNode* n = que.front(); que.pop();

                if (n->left == nullptr && n->right == nullptr) return depth;

                if (n->left  != nullptr) que.push(n->left);
                if (n->right != nullptr) que.push(n->right);
            }
        }

        return depth;
    }
};
```


### 222. 完全二叉树的节点个数

给你一棵**完全二叉树**的根节点`root`，求出该树的节点个数。

**完全二叉树**的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第`h`层，则该层包含`1~ 2ʰ`个节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/14/complete.jpg">
</div>

```
输入：root = [1,2,3,4,5,6]
输出：6
```

**示例2：**

```
输入：root = []
输出：0
```

**示例3：**

```
输入：root = [1]
输出：1
```

**提示：**

- 树中节点的数目范围是`[0, 5 * 10⁴]`。
- 0 <= `Node.val` <= 5*10⁴。
- 题目数据保证输入的树是**完全二叉树**。

#### Solution

首先按照一般的二叉树来进行求解，我们可以通过深度优先或是广度优先来对树进行遍历。深度优先的时间复杂度为`O(n)`，而空间复杂度为`O(log n)`。

[题目链接](https://leetcode.cn/problems/count-complete-tree-nodes/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        cnt = 1

        if root.left:
            cnt += self.countNodes(root.left)
        if root.right:
            cnt += self.countNodes(root.right)

        return cnt
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
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;

        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
```


广度优先的时间复杂度为`O(n)`，而空间复杂度为`O(n)`。

[题目链接](https://leetcode.cn/problems/count-complete-tree-nodes/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        from collections import deque

        queue = deque([root])
        cnt = 0

        while queue:
            N = len(queue)
            cnt += N

            for i in range(N):
                node = queue.popleft()

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return cnt
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
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;

        queue<TreeNode*> que;
        que.push(root);

        int num = 0;

        while (!que.empty()) {
            int N = que.size();
            num += N;

            for (int i=0; i<N; ++i) {
                TreeNode* node = que.front(); que.pop();

                if (node->left  != nullptr) que.push(node->left);
                if (node->right != nullptr) que.push(node->right);
            }
        }

        return num;
    }
};
```


本题的最优解法需要使用到**完全二叉树**的性质。根据定义，完全二叉树要么每一层都被填满，要么只有最下层没被填满。

<div align=center>
<img src="https://images.weserv.nl/?url=camo.githubusercontent.com/5d85c9f8df419ce8db22f668ff22f7a3ae55660b8e9bead3b98e2d8cdd69ac9c/68747470733a2f2f696d672d626c6f672e6373646e696d672e636e2f32303230313132343039323534333636322e706e67" width="70%">
</div>

当二叉树每一层都填满时称为**满二叉树**，此时树中节点数量为`2ʰ-1`。在这种情况下我们只需要遍历树的深度就能够得到节点的总数量。而如果完全二叉树不是满二叉树，则只能分别统计两棵子树中节点的数量，然后树中节点总数等于两棵子树节点数量之和加1。这样可以得到递归代码如下，其时间复杂度为`O(log n × log n)`，而空间复杂度为`O(log n)`。

[题目链接](https://leetcode.cn/problems/count-complete-tree-nodes/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        left = root.left
        right= root.right

        leftDepth = 0
        rightDepth= 0

        while left:
            leftDepth += 1
            left = left.left
        
        while right:
            rightDepth += 1
            right = right.right
        
        if leftDepth == rightDepth:
            return (2 << leftDepth) - 1
        
        return self.countNodes(root.left) + self.countNodes(root.right) + 1
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
    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;

        TreeNode* left = root->left;
        TreeNode* right= root->right;

        int leftDepth = 0, rightDepth = 0;

        while (left != nullptr) {
            left = left->left;
            ++leftDepth;
        }

        while (right != nullptr) {
            right = right->right;
            ++rightDepth;
        }

        if (leftDepth == rightDepth) return (2 << leftDepth) - 1;

        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
```


### 110. 平衡二叉树

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

- 一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过`1`。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/06/balance_1.jpg">
</div>

```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/06/balance_2.jpg">
</div>

```
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

**示例3：**

```
输入：root = []
输出：true
```

**提示：**

- 树中的节点数在范围`[0, 5000]`内。
- -10⁴ <= `Node.val` <= 10⁴。

#### Solution

本题需要递归计算左右两棵子树的高度，因此我们需要定义一个辅助函数`getHeight(node)`：

- 首先计算左右两棵子树的高度，分别记录到`leftHeight`和`rightHeight`中；
- 当左子树或右子树不是高度平衡二叉树时返回`-1`，表示当前树不可能是高度平衡二叉树；
- 继续比较左右两棵子树的高度之差，如果大于`1`则返回`-1`表示表示当前树不是高度平衡二叉树；
- 最后返回当前树的高度`1 + max(leftHeight, rightHeight)`。

这样判断`root`是不是高度平衡二叉树只需要看`getHeight(root)`是否等于`-1`即可。

[题目链接](https://leetcode.cn/problems/balanced-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def getHeight(node: Optional[TreeNode]) -> int:
            if not node:
                return 0
            
            if (leftHeight := getHeight(node.left)) == -1:
                return -1
            if (rightHeight := getHeight(node.right)) == -1:
                return -1
            if abs(leftHeight - rightHeight) > 1:
                return -1
            return 1 + max(leftHeight, rightHeight)
        
        return getHeight(root) != -1
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
    bool isBalanced(TreeNode* root) {
        return getHeight(root) != -1;
    }

    int getHeight(TreeNode* root) {
        if (root == nullptr) return 0;

        int leftHeight = getHeight(root->left);
        if (leftHeight == -1) return -1;

        int rightHeight = getHeight(root->right);
        if (rightHeight == -1) return -1;

        if (abs(leftHeight - rightHeight) > 1) return -1;

        return max(leftHeight, rightHeight) + 1;
    }
};
```


### 257. 二叉树的所有路径

给你一个二叉树的根节点`root`，按**任意顺序**，返回所有从根节点到叶子节点的路径。

**叶子节点**是指没有子节点的节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg">
</div>

```
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

**示例2：**

```
输入：root = [1]
输出：["1"]
```

**提示：**

- 树中的节点数在范围`[0, 100]`内。
- -100 <= `Node.val` <= 100。

#### Solution

本题的基本解法是深度优先搜索：我们自上而下对`root`进行遍历，每当遇到叶节点就构造一条路径添加到结果中。因此我们构造一个辅助函数`traverse()`，它需要3个输入参数：

- `node`表示当前节点；
- `path`记录了从`root`到`node`节点的路径；
- `res`用来保存所有的路径结果。

不难发现，整个遍历过程类似于[前序遍历](/leetcode/2023-02-03-BinaryTree.html#144-二叉树的前序遍历)。

[题目链接](https://leetcode.cn/problems/binary-tree-paths/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        def traverse(node: Optional[TreeNode], path: str, res: List[str]) -> List[str]:

            if not node.left and not node.right:
                res.append(path)
                return res
            
            if node.left:
                res = traverse(node.left, path+"->"+str(node.left.val), res)
            if node.right:
                res = traverse(node.right, path+"->"+str(node.right.val), res)
            
            return res
        
        return traverse(root, str(root.val), [])
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
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        string path;

        traversal(root, path, res);

        return res;
    }

    void traversal(TreeNode* node, string path, vector<string> &res) {
        path += to_string(node->val);

        if (node->left == nullptr && node->right == nullptr) {
            res.push_back(path);
            return;
        }

        if (node->left != nullptr) {
            path += "->";

            traversal(node->left, path, res);
            path.pop_back();
            path.pop_back();
        }

        if (node->right != nullptr) {
            path += "->";

            traversal(node->right, path, res);
            path.pop_back();
            path.pop_back();
        }
    }
};
```


本题的另一种理解方式是**回溯**：当我们访问完叶节点后需要通过回溯的方式来回到上一层的父节点，然后再进入下一个路径。这样的过程可以使用栈来实现。

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/1662887575-zcfIKH-image.png"  width="70%">
</div>

[题目链接](https://leetcode.cn/problems/binary-tree-paths/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        stack = [root]
        path_st = [str(root.val)]
        res = []

        while stack:
            node = stack.pop()
            path = path_st.pop()

            if (not node.left) and (not node.right):
                res.append(path)

                continue
            
            if node.right:
                stack.append(node.right)
                path_st.append(path + "->" + str(node.right.val))
            
            if node.left:
                stack.append(node.left)
                path_st.append(path + "->" + str(node.left.val))

        return res
```


本题还可以使用回溯模板来进行处理。

[题目链接](https://leetcode.cn/problems/binary-tree-paths/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        res = []
        path= [root]

        def backtracking(root: Optional[TreeNode]) -> None:
            nonlocal res, path

            if (not root.left) and (not root.right):
                res.append("->".join([str(node.val) for node in path]))
                return
            
            for child in [root.left, root.right]:
                if child:
                    path.append(child)
                    backtracking(child)
                    path.pop()
            
        backtracking(root)

        return res
```


### 404. 左叶子之和

给定二叉树的根节点`root`，返回所有左叶子之和。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg">
</div>

```
输入: root = [3,9,20,null,null,15,7] 
输出: 24 
解释: 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
```

**示例2：**

```
输入: root = [1]
输出: 0
```

**提示：**

- 树中的节点数在范围`[0, 1000]`内。
- -1000 <= `Node.val` <= 1000。

#### Solution

本题的难点在于**左叶子**的定义，这里需要先明确它：

- 一个节点为**左叶子**节点，当且仅当它是某个节点的左子节点，并且它是一个叶子结点。

因此我们无法直接判断当前节点是否是一个左叶子节点，而必须借助它的父节点才能判断。明确定义后我们可以得到递归结构：

- 计算`root`左子树和右子树的左叶子之和，把它们加起来记录到`total`中；
- 如果`root.left`是左叶子节点，`total`还要再加上`root.left.val`；
- 返回`total`作为当前树的左叶子之和。

不难发现上面的结构类似于[后序遍历](/leetcode/2023-02-03-BinaryTree.html#145-二叉树的后序遍历)。

[题目链接](https://leetcode.cn/problems/sum-of-left-leaves/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        leftSum = self.sumOfLeftLeaves(root.left)
        rightSum= self.sumOfLeftLeaves(root.right)

        total = leftSum + rightSum
        if isLeaf(root.left):
            total += root.left.val

        return total

def isLeaf(root: Optional[TreeNode]) -> bool:
    if not root:
        return False
    
    return (not root.left) and (not root.right)
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
    int sumOfLeftLeaves(TreeNode* root) {
        if (root == nullptr) return 0;

        int leftSum = sumOfLeftLeaves(root->left);
        int rightSum= sumOfLeftLeaves(root->right);

        int total = leftSum + rightSum;

        TreeNode* left = root->left;
        if (left != nullptr && left->left == nullptr && left->right == nullptr) {
            total += left->val;
        }

        return total;
    }
};
```


本题同样可以使用迭代来进行处理。此时只需要不断检查当前节点的左节点是否是左叶子节点即可，代码可参考如下：

[题目链接](https://leetcode.cn/problems/sum-of-left-leaves/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        stack = [root]
        res = 0

        while stack:
            node = stack.pop()

            if isLeaf(node.left):
                res += node.left.val
            
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        
        return res

def isLeaf(root: Optional[TreeNode]) -> bool:
    if not root:
        return False
    
    return (not root.left) and (not root.right)
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
    int sumOfLeftLeaves(TreeNode* root) {
        if (root == nullptr) return 0;

        int res = 0;
        queue<TreeNode*> que;

        que.push(root);
        while (!que.empty()) {
            TreeNode* node = que.front(); que.pop();

            if (isLeaf(node->left)) res += node->left->val;

            if (node->left  != nullptr) que.push(node->left);
            if (node->right != nullptr) que.push(node->right);
        }

        return res;
    }

    bool isLeaf(TreeNode* node) {
        if (node == nullptr) return false;

        if (node->left == nullptr && node->right == nullptr) return true;

        return false;
    }
};
```


### 513. 找树左下角的值

给定一个二叉树的 **根节点** `root`，请找出该二叉树的 **最底层** **最左边** 节点的值。

假设二叉树中至少有一个节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/14/tree1.jpg">
</div>

```
输入: root = [2,1,3]
输出: 1
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/14/tree2.jpg">
</div>

```
输入: [1,2,3,4,null,5,6,null,null,7]
输出: 7
```

**提示：**

- 树中的节点数在范围`[1, 10⁴]`内。
- -2³² <= `Node.val` <= 2³²-1。

#### Solution

本题的基本解法是使用[层序遍历](/leetcode/2023-02-03-BinaryTree.html#102-二叉树的层序遍历)。根据题意，我们只需要用每一层最左边节点的值来更新`res`即可。完成遍历后`res`的值即为二叉树最底层最左边节点的值。

[题目链接](https://leetcode.cn/problems/find-bottom-left-tree-value/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        from collections import deque

        queue = deque([root])
        res = 0

        while queue:
            N = len(queue)
            res = queue[0].val

            for i in range(N):
                node = queue.popleft()

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return res
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
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);

        int res;

        while (!que.empty()) {
            int N = que.size();

            for (int i=0; i<N; ++i) {
                TreeNode* node = que.front(); que.pop();

                if (i == 0) res = node->val;

                if (node->left  != nullptr) que.push(node->left);
                if (node->right != nullptr) que.push(node->right);
            }
        }

        return res;
    }
};
```


本题的另一种解法是使用深度优先搜索。我们需要分别遍历左节点和右节点，然后返回两棵子树最底层最左边节点的值以及对应的深度。这样当前树最底层最左边的值即为两棵子树中拥有最大深度树的返回节点值，相应代码可以参考如下。

[题目链接](https://leetcode.cn/problems/find-bottom-left-tree-value/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        def traversal(node: Optional[TreeNode], depth: int):
            if not node:
                return 0, 0
            elif (not node.left) and (not node.right):
                return node.val, depth
            
            leftVal, leftDepth = traversal(node.left, depth+1)
            rightVal, rightDepth = traversal(node.right, depth+1)

            if leftDepth >= rightDepth:
                return leftVal, leftDepth
            else:
                return rightVal, rightDepth
        
        res, _ = traversal(root, 0)

        return res
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
    int findBottomLeftValue(TreeNode* root) {
        res = 0;
        maxDepth = 0;

        traversal(root, 0);

        return res;
    }

private:
    int res;
    int maxDepth;

    void traversal(TreeNode* node, int depth) {
        if (node->left == nullptr && node->right == nullptr) {
            if (depth > maxDepth) {
                maxDepth = depth;
                res = node->val;
            }

            return;
        }

        if (node->left  != nullptr) traversal(node->left,  depth + 1);
        if (node->right != nullptr) traversal(node->right, depth + 1);
    }
};
```



### 112. 路径总和

给你二叉树的根节点`root`和一个表示目标和的整数`targetSum`。判断该树中是否存在**根节点到叶子节点**的路径，这条路径上所有节点值相加等于目标和`targetSum`。如果存在，返回`true`；否则，返回`false`。

**叶子节点**是指没有子节点的节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg">
</div>

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
解释：等于目标和的根节点到叶节点路径如上图所示。
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg">
</div>

```
输入：root = [1,2,3], targetSum = 5
输出：false
解释：树中存在两条根节点到叶子节点的路径：
(1 --> 2): 和为 3
(1 --> 3): 和为 4
不存在 sum = 5 的根节点到叶子节点的路径。
```

**示例3：**

```
输入：root = [], targetSum = 0
输出：false
解释：由于树是空的，所以不存在根节点到叶子节点的路径。
```

**提示：**

- 树中的节点数在范围`[0, 5000]`内。
- -1000 <= `Node.val` <= 1000。
- -1000 <= `targetSum` <= 1000。

#### Solution

本题可以使用深度优先搜索来求解。我们只需要分别递归遍历左子树和右子树，只要其中存在满足要求的路径即可。

[题目链接](https://leetcode.cn/problems/path-sum/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        
        if (not root.left) and (not root.right):
            return root.val == targetSum
        else:
            return self.hasPathSum(root.left, targetSum-root.val) or self.hasPathSum(root.right, targetSum-root.val)
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
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == nullptr) return false;

        if (root->left == nullptr && root->right == nullptr) return root->val == targetSum;

        return hasPathSum(root->left, targetSum-root->val) || hasPathSum(root->right, targetSum-root->val);
    }
};
```


本题的另一种求解思路是使用回溯。这里我们需要两个栈来分别存储当前遍历到的节点以及路径上节点值之和，这样就可以利用出栈来模拟回溯的过程。具体代码可参考如下。

[题目链接](https://leetcode.cn/problems/path-sum/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        
        stack = [root]
        val_st= [root.val]

        while stack:
            node = stack.pop()
            val  = val_st.pop()

            if (not node.left) and (not node.right) and val == targetSum:
                return True
            
            if node.right:
                stack.append(node.right)
                val_st.append(val+node.right.val)
            if node.left:
                stack.append(node.left)
                val_st.append(val+node.left.val)

        return False
```


### 113. 路径总和II

给你二叉树的根节点`root`和一个整数目标和`targetSum`，找出所有**从根节点到叶子节点**路径总和等于给定目标和的路径。

**叶子节点**是指没有子节点的节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg">
</div>

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg">
</div>

```
输入：root = [1,2,3], targetSum = 5
输出：[]
```

**示例3：**

```
输入：root = [1,2], targetSum = 0
输出：[]
```

**提示：**

- 树中的节点数在范围`[0, 5000]`内。
- -1000 <= `Node.val` <= 1000。
- -1000 <= `targetSum` <= 1000。

#### Solution

本题解法与[路径总和](/leetcode/2023-02-03-BinaryTree.html#112-路径总和)基本一致，不过需要注意的是在进行递归时要保存中间结果。

[题目链接](https://leetcode.cn/problems/path-sum-ii/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        def traversal(root, target, path, res) -> List[List[int]]:
            if not root:
                return res
            
            if (not root.left) and (not root.right):
                if root.val == target:
                    res.append(path)
                return res
            
            if root.left:
                res = traversal(root.left, target-root.val, path+[root.left.val], res)
            if root.right:
                res = traversal(root.right, target-root.val, path+[root.right.val], res)
            
            return res
        
        if not root:
            return []
        
        return traversal(root, targetSum, [root.val], [])
```


本题的回溯解法相对比较简单，代码可参考如下。

[题目链接](https://leetcode.cn/problems/path-sum-ii/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root:
            return []
        
        stack   = [root]
        val_st  = [root.val]
        path_st = [[root.val]]
        res = []

        while stack:
            node = stack.pop()
            val  = val_st.pop()
            path = path_st.pop()

            if (not node.left) and (not node.right) and val == targetSum:
                res.append(path)
            
            if node.right:
                stack.append(node.right)
                val_st.append(val + node.right.val)
                path_st.append(path + [node.right.val])
            
            if node.left:
                stack.append(node.left)
                val_st.append(val + node.left.val)
                path_st.append(path + [node.left.val])
        
        return res
```


本题还可以使用回溯模板来进行处理。

[题目链接](https://leetcode.cn/problems/path-sum-ii/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> List[List[int]]:
        if not root:
            return []

        res = []
        path= [root]

        def backtracking(root: Optional[TreeNode], targetSum: int) -> None:
            nonlocal res, path

            if (not root.left) and (not root.right):
                if root.val == targetSum:
                    res.append([node.val for node in path])
                return
            
            for child in [root.left, root.right]:
                if child:
                    path.append(child)
                    backtracking(child, targetSum-root.val)
                    path.pop()
        
        backtracking(root, targetSum)

        return res
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
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        res.clear();
        if (root == nullptr) return res;


        path.clear();
        path.push_back(root->val);

        backtracking(root, targetSum);

        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(TreeNode* root, int target) {
        if (root->left == nullptr && root->right == nullptr) {
            if (target == root->val) {
                res.push_back(path);
            }

            return;
        }

        if (root->left != nullptr) {
            path.push_back(root->left->val);
            backtracking(root->left, target-root->val);
            path.pop_back();
        }

        if (root->right != nullptr) {
            path.push_back(root->right->val);
            backtracking(root->right, target-root->val);
            path.pop_back();
        }
    }
};
```


### 129. 求根节点到叶节点数字之和

给你一个二叉树的根节点`root`，树中每个节点都存放有一个`0`到`9`之间的数字。

每条从根节点到叶节点的路径都代表一个数字：

- 例如，从根节点到叶节点的路径`1 -> 2 -> 3`表示数字`123`。

计算从根节点到叶节点生成的**所有数字之和**。

**叶节点**是指没有子节点的节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/num1tree.jpg">
</div>

```
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/num2tree.jpg">
</div>

```
输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026
```

#### Solution

本题解法类似于[路径总和](/leetcode/2023-02-03-BinaryTree.html#112-路径总和)和[路径总和II](/leetcode/2023-02-03-BinaryTree.html#113-路径总和ii)，整体思路是自上而下遍历整棵树然后把路径上的数字进行相加。

[题目链接](https://leetcode.cn/problems/sum-root-to-leaf-numbers/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        sumPath = 0

        def traversal(root: Optional[TreeNode], path: int) -> None:
            nonlocal sumPath

            if (not root.left) and (not root.right):
                sumPath += path
                return
            
            if root.left:
                traversal(root.left, path*10+root.left.val)
            if root.right:
                traversal(root.right, path*10+root.right.val)
        
        traversal(root, root.val)
        
        return sumPath
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        sumPath = 0

        stack   = [root]
        path_st = [root.val]

        while stack:
            node = stack.pop()
            path = path_st.pop()

            if (not node.left) and (not node.right):
                sumPath += path
            
            if node.left:
                stack.append(node.left)
                path_st.append(path*10+node.left.val)
            
            if node.right:
                stack.append(node.right)
                path_st.append(path*10+node.right.val)
        
        return sumPath

```


本题还可以使用回溯模板来进行处理。

[题目链接](https://leetcode.cn/problems/sum-root-to-leaf-numbers/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        res = 0

        def backtracking(root: Optional[TreeNode], val: int) -> None:
            nonlocal res

            if (not root.left) and (not root.right):
                res += val
                return
            
            for child in [root.left, root.right]:
                if child:
                    backtracking(child, val*10+child.val)
        
        backtracking(root, root.val)

        return res
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
    int sumNumbers(TreeNode* root) {
        res = 0;

        backtracking(root, root->val);
        
        return res;
    }

private:
    int res;

    void backtracking(TreeNode* root, int curSum) {
        if (root->left == nullptr && root->right == nullptr) {
            res += curSum;
            return;
        }

        if (root->left != nullptr) {
            backtracking(root->left, curSum*10 + root->left->val);
        }

        if (root->right != nullptr) {
            backtracking(root->right, curSum*10 + root->right->val);
        }
    }
};
```


## 修改与构造

### 226. 翻转二叉树

给你一棵二叉树的根节点`root`，翻转这棵二叉树，并返回其根节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg">
</div>

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg">
</div>

```
输入：root = [2,1,3]
输出：[2,3,1]
```

**示例3：**

```
输入：root = []
输出：[]
```

**提示：**

- 树中节点数目在范围`[0, 100]`内。
- -100 <= `Node.val` <= 100。

#### Solution

翻转二叉树是二叉树的经典问题，利用问题自身的递归结构我们可以很容易地得到递归版本的代码。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/RHTOTSo.gif" width="60%">
</div>

[题目链接](https://leetcode.cn/problems/invert-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return root

        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        
        return root
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
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return root;

        swap(root->left, root->right);
        root->left = invertTree(root->left);
        root->right = invertTree(root->right);

        return root;
    }
};
```


当然我们也可以基于迭代来实现翻转，此时只需要利用栈或队列来保存中间节点即可。

[题目链接](https://leetcode.cn/problems/invert-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return root

        stack = [root]

        while stack:
            node = stack.pop()
            node.left, node.right = node.right, node.left

            if node.left:
                stack.append(node.left)
            
            if node.right:
                stack.append(node.right)
        
        return root
```


C++代码

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
    TreeNode* invertTree(TreeNode* root) {
        if (root == nullptr) return root;

        stack<TreeNode*> stk;
        stk.push(root);

        while (!stk.empty()) {
            TreeNode* node = stk.top(); stk.pop();

            swap(node->left, node->right);

            if (node->left  != nullptr) stk.push(node->left);
            if (node->right != nullptr) stk.push(node->right);
        }

        return root;
    }
};
```



### 106. 从中序与后序遍历序列构造二叉树

给定两个整数数组`inorder`和`postorder`，其中`inorder`是二叉树的中序遍历，`postorder`是同一棵树的后序遍历，请你构造并返回这棵**二叉树**。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/tree.jpg">
</div>

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

**示例2：**

```
输入：inorder = [-1], postorder = [-1]
输出：[-1]
```

**提示：**

- 1 <= `inorder.length` <= 3000。
- `postorder.length` == `inorder.length`。
- -3000 <= `inorder[i]`, `postorder[i]` <= 3000。
- `inorder`和`postorder`都由**不同**的值组成。
- `postorder`中每一个值都在`inorder`中。
- `inorder`**保证**是树的中序遍历。
- `postorder`**保证**是树的后序遍历。

#### Solution

本题需要结合[中序遍历](/leetcode/2023-02-03-BinaryTree.html#94-二叉树的中序遍历)和[后序遍历](/leetcode/2023-02-03-BinaryTree.html#145-二叉树的后序遍历)的顺序进行求解。回忆中序遍历的顺序为**左-中-右**，而后序遍历为**左-右-中**，因此`postorder`的末尾一定是当前的`root`。接下来利用`root`把`inorder`拆成`inorderLeft`和`inorderRight`两部分，分别对应`root.left`和`root.right`的中序遍历。类似地，我们还需要再把`postorder`同样拆成`postorderLeft`和`postorderRight`对应后序遍历的左右子树。由于`inorderLeft`和`postorderLeft`一定具有相同的长度，我们可以直接对`postorder`进行拆分。加下来只需要分别递归构造`root.left`和`root.right`即可。整个算法过程可以参考下图。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/gG6Z7vv.png">
</div>

[题目链接](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> Optional[TreeNode]:
        if not postorder:
            return None
        
        ## the last one in postorder is the root
        val = postorder[-1]
        root= TreeNode(val)

        ## split idx
        idx = inorder.index(val)

        inorderLeft = inorder[:idx]
        inorderRight= inorder[idx+1:]

        postorderLeft = postorder[:idx]
        postorderRight= postorder[idx:-1]

        ## recursively build root.left and root.right
        root.left = self.buildTree(inorderLeft, postorderLeft)
        root.right= self.buildTree(inorderRight, postorderRight)

        return root
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
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.empty() || postorder.empty()) return nullptr;

        return traversal(inorder, postorder);
    }

private:
    TreeNode* traversal(vector<int>& inorder, vector<int>& postorder) {
        if (postorder.empty()) return nullptr;

        int N = postorder.size();
        TreeNode* root = new TreeNode(postorder[N-1]);

        int midIdx;
        for (midIdx=0; midIdx < N; ++midIdx) {
            if (inorder[midIdx] == postorder[N-1]) break;
        }

        vector<int> leftInorder(inorder.begin(), inorder.begin()+midIdx);
        vector<int> rightInorder(inorder.begin()+midIdx+1, inorder.end());

        postorder.resize(N-1);
        vector<int> leftPostorder(postorder.begin(), postorder.begin() + leftInorder.size());
        vector<int> rightPostorder(postorder.begin() + leftInorder.size(), postorder.end());

        root->left  = traversal(leftInorder, leftPostorder);
        root->right = traversal(rightInorder, rightPostorder);

        return root;
    }
};
```


### 105. 从前序与中序遍历序列构造二叉树

给定两个整数数组`preorder`和`inorder`，其中`preorder`是二叉树的**先序遍历**，`inorder`是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/tree.jpg">
</div>

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

**示例2：**

```
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

**提示：**

- 1 <= `preorder.length` <= 3000。
- `inorder.length` == `preorder.length`。
- -3000 <= `preorder[i]`, `inorder[i]` <= 3000。
- `preorder`和`inorder`均**无重复**元素。
- `inorder`中每一个值都在`preorder`中。
- `preorder`**保证**为二叉树的前序遍历序列。
- `inorder`**保证**为二叉树的中序遍历序列。

#### Solution

本题解法与[从中序与后序遍历序列构造二叉树](/leetcode/2023-02-03-BinaryTree.html#106-从中序与后序遍历序列构造二叉树)基本一致，唯一需要注意的是`preorder`的第一个元素对应了`root`节点。需要额外说明的是如果给定前序和后序遍历是**无法**确定唯一的二叉树的，这是因为没有中序遍历无法确定`root`左右部分，也就是无法完成分割。

[题目链接](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        if not preorder:
            return None
        
        ## the first one in preorder is the root
        val = preorder[0]
        root= TreeNode(val)

        ## split idx
        idx = inorder.index(val)

        inorderLeft = inorder[:idx] 
        inorderRight= inorder[idx+1:]

        preorderLeft = preorder[1:idx+1]
        preorderRight= preorder[1+idx:]

        ## recursively build root.left and root.right
        root.left = self.buildTree(preorderLeft, inorderLeft)
        root.right= self.buildTree(preorderRight, inorderRight)

        return root
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
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.empty() || inorder.empty()) return nullptr;

        return traversal(preorder, inorder);
    }

private:
    TreeNode* traversal(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.empty()) return nullptr;

        int N = inorder.size();
        TreeNode* root = new TreeNode(preorder[0]);

        int midIdx;
        for (midIdx=0; midIdx < N; ++midIdx) {
            if (inorder[midIdx] == preorder[0]) break;
        }

        vector<int> leftPreorder(preorder.begin()+1, preorder.begin()+midIdx+1);
        vector<int> rightPreorder(preorder.begin()+midIdx+1, preorder.end());

        vector<int> leftInorder(inorder.begin(), inorder.begin() + midIdx);
        vector<int> rightInorder(inorder.begin()+midIdx+1, inorder.end());

        root->left  = traversal(leftPreorder, leftInorder);
        root->right = traversal(rightPreorder, rightInorder);

        return root;
    }
};
```


### 654. 最大二叉树

给定一个不重复的整数数组`nums`。**最大二叉树**可以用下面的算法从`nums`递归地构建:

- 创建一个根节点，其值为`nums`中的最大值。
- 递归地在最大值**左边**的**子数组前缀上**构建左子树。
- 递归地在最大值**右边**的**子数组后缀上**构建右子树。

返回`nums`构建的**最大二叉树**。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/24/tree1.jpg">
</div>

```
输入：nums = [3,2,1,6,0,5]
输出：[6,3,5,null,2,0,null,null,1]
解释：递归调用如下所示：
- [3,2,1,6,0,5] 中的最大值是 6 ，左边部分是 [3,2,1] ，右边部分是 [0,5] 。
    - [3,2,1] 中的最大值是 3 ，左边部分是 [] ，右边部分是 [2,1] 。
        - 空数组，无子节点。
        - [2,1] 中的最大值是 2 ，左边部分是 [] ，右边部分是 [1] 。
            - 空数组，无子节点。
            - 只有一个元素，所以子节点是一个值为 1 的节点。
    - [0,5] 中的最大值是 5 ，左边部分是 [0] ，右边部分是 [] 。
        - 只有一个元素，所以子节点是一个值为 0 的节点。
        - 空数组，无子节点。
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/24/tree2.jpg">
</div>

```
输入：nums = [3,2,1]
输出：[3,null,2,null,1]
```

**提示：**

- 1 <= `nums.length` <= 1000。
- 0 <= `nums[i]` <= 1000。
- `nums`中的所有整数**互不相同**。

#### Solution

本题只需要按照题干给出的递归算法进行实现即可。

[题目链接](https://leetcode.cn/problems/maximum-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def constructMaximumBinaryTree(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None
        
        val = nums[0]
        idx = 0

        for i, num in enumerate(nums):
            if num > val:
                val = num
                idx = i
        
        root = TreeNode(val)

        ## recursively build root.left and root.right
        root.left = self.constructMaximumBinaryTree(nums[:idx])
        root.right= self.constructMaximumBinaryTree(nums[idx+1:])

        return root
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
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return traversal(nums, 0, nums.size());
    }

private:
    TreeNode* traversal(vector<int>& nums, int left, int right) {
        if (left >= right) return nullptr;
        
        int rootIdx = left;

        for (int i=left+1; i<right; ++i) {
            if (nums[rootIdx] < nums[i]) rootIdx = i;
        }

        TreeNode* root = new TreeNode(nums[rootIdx]);

        root->left  = traversal(nums, left, rootIdx);
        root->right = traversal(nums, rootIdx+1, right);

        return root;
    }
};
```


### 617. 合并二叉树

给你两棵二叉树：`root1`和`root2`。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为`null`的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意**: 合并过程必须从两个树的根节点开始。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/05/merge.jpg">
</div>

```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

**示例2：**

```
输入：root1 = [1], root2 = [1,2]
输出：[2,2]
```

**提示：**

- 两棵树中的节点数目在范围`[0, 2000]`内。
- -10⁴ <= `Node.val` <= 10⁴。

#### Solution

本题的递归解法非常直观，我们只需要融合根节点`root1`和`root2`然后递归地融合左右节点即可。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/OrMVBCh.gif">
</div>

[题目链接](https://leetcode.cn/problems/merge-two-binary-trees/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1:
            return root2
        elif not root2:
            return root1
        
        root = TreeNode(root1.val+root2.val)
        root.left = self.mergeTrees(root1.left, root2.left)
        root.right= self.mergeTrees(root1.right, root2.right)

        return root
```


C++代码

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
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (root1 == nullptr && root2 == nullptr) return nullptr;
        else if (root1 != nullptr && root2 == nullptr) return root1;
        else if (root1 == nullptr && root2 != nullptr) return root2;

        root1->val += root2->val;
        root1->left  = mergeTrees(root1->left, root2->left);
        root1->right = mergeTrees(root1->right, root2->right);

        return root1;
    }
};
```


本题的迭代解法要相对麻烦一些。我们的整体思路是基于[层序遍历](/leetcode/2023-02-03-BinaryTree.html#102-二叉树的层序遍历)，每次向队列中添加两棵树的对应节点并进行更新：

- 如果两个节点都有左节点，则将两个左节点添加到队列中；
- 如果两个节点都有右节点，则将两个右节点添加到队列中。

这样可以保证队列中的前两个节点都是有效的节点，也因此当前节点的`val`等于两个节点`val`之和。接下来考虑两个节点中只有一个节点有左节点或右节点的情况，此时只要把该节点的左节点或右节点直接连接到当前节点上即可。整个算法流程可以参考如下。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/4UTn2Mj.gif" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/merge-two-binary-trees/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1:
            return root2
        elif not root2:
            return root1

        from collections import deque
        
        queue = deque([root1, root2])

        while queue:
            node1 = queue.popleft()
            node2 = queue.popleft()

            if node1.left and node2.left:
                queue.append(node1.left)
                queue.append(node2.left)
            
            if node1.right and node2.right:
                queue.append(node1.right)
                queue.append(node2.right)
            
            node1.val += node2.val

            if not node1.left and node2.left: 
                node1.left = node2.left
            if not node1.right and node2.right: 
                node1.right = node2.right

        return root1
```


## 二叉搜索树

### 700. 二叉搜索树中的搜索

给定二叉搜索树(BST)的根节点`root`和一个整数值`val`。

你需要在BST中找到节点值等于`val`的节点。返回以该节点为根的子树。如果节点不存在，则返回`null`。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/12/tree1.jpg">
</div>

```
输入：root = [4,2,7,1,3], val = 2
输出：[2,1,3]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/01/12/tree2.jpg">
</div>

```
输入：root = [4,2,7,1,3], val = 5
输出：[]
```

**提示：**

- 树中节点数在`[1, 5000]`范围内。
- 1 <= `Node.val` <= 10⁷。
- `root`是二叉搜索树。
- 1 <= `val` <= 10⁷。

#### Solution

二叉搜索树的递归遍历比较容易。借助节点的顺序我们可以得到递归关系：

- 当`root.val == val`时返回`root`；
- 当`root.val < val`时向下对右节点继续遍历；
- 当`root.val > val`时向下对左节点继续遍历。

[题目链接](https://leetcode.cn/problems/search-in-a-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return None
        elif root.val == val:
            return root
        elif root.val < val:
            return self.searchBST(root.right, val)
        else:
            return self.searchBST(root.left, val)
```


C++代码

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
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == nullptr || root->val == val) return root;

        if (root->val < val) return searchBST(root->right, val);
        else return searchBST(root->left, val);
    }
};
```


而二叉搜索树的迭代遍历也比二叉树的迭代要容易一些。此时我们不再需要借助栈来进行回溯，直接向下遍历即可。

[题目链接](https://leetcode.cn/problems/search-in-a-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        node = root

        while node:
            if node.val == val:
                return node
            elif node.val < val:
                node = node.right
            else:
                node = node.left
        
        return None
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
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == nullptr || root->val == val) return root;

        TreeNode* cur = root;
        while (cur != nullptr) {
            if (cur->val == val) return cur;
            else if (cur->val < val) cur = cur->right;
            else cur = cur->left;
        }

        return nullptr;
    }
};
```


### 98. 验证二叉搜索树

给你一个二叉树的根节点`root`，判断其是否是一个有效的二叉搜索树。

**有效**二叉搜索树定义如下：

- 节点的左子树只包含**小于**当前节点的数。
- 节点的右子树只包含**大于**当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/01/tree1.jpg">
</div>

```
输入：root = [2,1,3]
输出：true
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/12/01/tree2.jpg">
</div>

```
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

**提示：**

- 树中节点数目范围在`[1, 10⁴]`范围内。
- -2³¹ <= `Node.val` <= 2³¹ - 1。

#### Solution

本题的技巧在于对二叉搜索树使用[中序遍历](/leetcode/2023-02-03-BinaryTree.html#94-二叉树的中序遍历)进行展开可以得到一个单调递增的序列。因此我们可以按照递归或是迭代的方式来展开`root`，然后再验证得到的序列是否满足单调递增条件，如果满足则`root`是一个有效的二叉搜索树。对应代码可参考如下。

[题目链接](https://leetcode.cn/problems/validate-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:

        def traversal(root: Optional[TreeNode]) -> List[int]:
            if not root:
                return []
            
            return traversal(root.left) + [root.val] + traversal(root.right)
        
        vals = traversal(root)

        for i in range(len(vals)-1):
            if vals[i] >= vals[i+1]:
                return False

        return True
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        cur = root
        stack = []
        vals = []

        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.left
            
            cur = stack.pop()
            vals.append(cur.val)
            cur = cur.right
        
        for i in range(len(vals)-1):
            if vals[i] >= vals[i+1]:
                return False

        return True
```


上面的代码还可以进一步简化。实际上我们并不需要完全将`root`展开，只需要记录当前节点的前一个节点`pre`并且比较它们是否单调递增即可。

[题目链接](https://leetcode.cn/problems/validate-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        pre = None

        def traversal(root: Optional[TreeNode]) -> bool:
            nonlocal pre

            if not root:
                return True
            
            validLeft = traversal(root.left)
            
            if pre and pre.val >= root.val:
                return False
            
            ## update pre
            pre = root

            validRight = traversal(root.right)
            
            return validLeft and validRight
        
        return traversal(root)
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        stack = []
        cur = root
        pre = None

        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.left
            
            cur = stack.pop()
            
            if pre and pre.val >= cur.val:
                return False
            
            ## update pre and cur
            pre = cur
            cur = cur.right
        
        return True
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
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> stk;

        TreeNode* pre = nullptr;
        TreeNode* cur = root;

        while (cur != nullptr || !stk.empty()) {
            while (cur != nullptr) {
                stk.push(cur);
                cur = cur->left;
            }
            
            cur = stk.top(); stk.pop();

            if (pre != nullptr && pre->val >= cur->val) return false;

            pre = cur;
            cur = cur->right;
        }

        return true;
    }
};
```


### 530. 二叉搜索树的最小绝对差

给你一个二叉搜索树的根节点`root`，返回**树中任意两不同节点值之间的最小差值**。

差值是一个正数，其数值等于两值之差的绝对值。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/05/bst1.jpg">
</div>

```
输入：root = [4,2,6,1,3]
输出：1
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/05/bst2.jpg">
</div>

```
输入：root = [1,0,48,null,null,12,49]
输出：1
```

**提示：**

- 树中节点数目范围在`[2, 10⁴]`范围内。
- 0 <= `Node.val` <= 10⁵。

#### Solution

本题解法与[验证二叉搜索树](/leetcode/2023-02-03-BinaryTree.html#98-验证二叉搜索树)类似，都是利用[中序遍历](/leetcode/2023-02-03-BinaryTree.html#94-二叉树的中序遍历)对二叉搜索树进行展开。展开后序列相邻元素的最小绝对值之差即为所求。

[题目链接](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:

        def traversal(root: Optional[TreeNode]) -> List[int]:
            if not root:
                return []
            
            return traversal(root.left) + [root.val] + traversal(root.right)
        
        vals = traversal(root)
        minDiff = vals[1] - vals[0]

        for i in range(len(vals)-1):
            minDiff = min(minDiff, vals[i+1] - vals[i])
        
        return minDiff
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        stack = []
        vals  = []
        cur   = root

        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.left
            
            cur = stack.pop()
            vals.append(cur.val)
            cur = cur.right
        
        minDiff = vals[1] - vals[0]

        for i in range(len(vals)-1):
            minDiff = min(minDiff, vals[i+1] - vals[i])
        
        return minDiff
```


我们同样可以简化二叉搜索树的展开过程。这里使用`pre`来记录当前节点的前一个节点，`minDiff`来记录当前遍历过程中的相邻节点最小绝对值之差。这样只需要在遍历过程中不断对它们进行更新即可，递归和迭代版本的代码可以参考如下。

[题目链接](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        pre = None
        minDiff = float("inf")

        def traversal(root: Optional[TreeNode]) -> None:
            nonlocal minDiff, pre

            if not root:
                return
            
            traversal(root.left)

            ## update minDiff
            if pre:
                minDiff = min(minDiff, root.val-pre.val)
            
            ## update pre
            pre = root

            traversal(root.right)
        
        traversal(root)
        
        return minDiff
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
        stack = []
        pre   = None
        cur   = root
        minDiff = float("inf")

        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.left
            
            cur = stack.pop()
            
            ## update minDiff
            if pre:
                minDiff = min(minDiff, cur.val-pre.val)

            ## update pre and minDiff
            pre = cur
            cur = cur.right
        
        return minDiff
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
    int getMinimumDifference(TreeNode* root) {
        stack<TreeNode*> stk;
        TreeNode* pre = nullptr;
        TreeNode* cur = root;

        int minDiff = INT_MAX;

        while (cur != nullptr || !stk.empty()) {
            while (cur != nullptr) {
                stk.push(cur);
                cur = cur->left;
            }

            cur = stk.top(); stk.pop();

            if (pre != nullptr) minDiff = min(minDiff, cur->val - pre->val);

            pre = cur;
            cur = cur->right;
        }

        return minDiff;
    }
};
```


### 501. 二叉搜索树中的众数

给你一个含重复值的二叉搜索树(BST)的根节点`root`，找出并返回BST中的所有**众数**(即，出现频率最高的元素)。

如果树中有不止一个众数，可以按**任意顺序**返回。

假定BST满足如下定义：

- 结点左子树中所含节点的值**小于等于**当前节点的值
- 结点右子树中所含节点的值**大于等于**当前节点的值
- 左子树和右子树都是二叉搜索树

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/11/mode-tree.jpg">
</div>

```
输入：root = [1,null,2,2]
输出：[2]
```

**示例2：**

```
输入：root = [0]
输出：[0]
```

**提示：**

- 树中节点数目范围在`[1, 10⁴]`范围内。
- -10⁵ <= `Node.val` <= 10⁵。

#### Solution

本题的技巧同样是二叉搜索树[中序遍历](/leetcode/2023-02-03-BinaryTree.html#94-二叉树的中序遍历)可以展开为单调序列。由于序列是单调不减的，我们只需要一次遍历就可以得到众数。具体来说我们使用`count`来记录当前节点出现的次数，`maxCount`来记录最大出现次数。如果`count == maxCount`则把当前值加入到`res`中，而如果`count > maxCount`则清空`res`并使其只包含当前节点值。递归和迭代版本的代码可参考如下。

[题目链接](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        pre = None
        res = []

        count = 0
        maxCount = 0

        def traversal(root: Optional[TreeNode]) -> None:
            nonlocal pre, res, count, maxCount

            if not root:
                return

            traversal(root.left)

            ## update count
            if not pre:
                count = 1
            elif pre.val == root.val:
                count+= 1
            else:
                count = 1
            
            ## update maxCount and res
            if count == maxCount:
                res.append(root.val)
            elif count > maxCount:
                maxCount = count
                res = [root.val]
            
            ## update pre
            pre = root
            
            traversal(root.right)
        
        traversal(root)

        return res
```


```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        stack = []
        pre   = None
        cur   = root

        res = []
        count = 0
        maxCount = 0

        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.left
            
            cur = stack.pop()

            ## update count
            if not pre:
                count = 1
            elif pre.val == cur.val:
                count += 1
            else:
                count = 1

            ## update maxCount and res
            if count == maxCount:
                res.append(cur.val)
            elif count > maxCount:
                maxCount = count
                res = [cur.val]

            ## update pre and cur
            pre = cur
            cur = cur.right

        return res
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
    vector<int> findMode(TreeNode* root) {
        stack<TreeNode*> stk;

        TreeNode* pre = nullptr;
        TreeNode* cur = root;

        vector<int> res;

        int count    = 0;
        int maxCount = 0;

        while (cur != nullptr || !stk.empty()) {
            while (cur != nullptr) {
                stk.push(cur);
                cur = cur->left;
            }

            cur = stk.top(); stk.pop();

            if (pre == nullptr) count = 1;
            else if (pre->val == cur->val) ++count;
            else count = 1;

            if (count == maxCount) res.push_back(cur->val);
            else if (count > maxCount) {
                maxCount = count;

                res.clear();
                res.push_back(cur->val);
            }

            pre = cur;
            cur = cur->right;
        }

        return res;
    }
};
```


### 538. 把二叉搜索树转换为累加树

给出二叉**搜索**树的根节点，该树的节点值各不相同，请你将其转换为累加树(Greater Sum Tree)，使每个节点`node`的新值等于原树中大于或等于`node.val`的值之和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键**小于**节点键的节点。
- 节点的右子树仅包含键**大于**节点键的节点。
- 左右子树也必须是二叉搜索树。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png">
</div>

```
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**示例2：**

```
输入：root = [0,null,1]
输出：[1,null,1]
```

**示例3：**

```
输入：root = [1,0,2]
输出：[3,3,2]
```

**示例4：**

```
输入：root = [3,2,4,1]
输出：[7,9,4,10]
```

**提示：**

- 树中的节点数介于0和10⁴之间。
- 每个节点的值介于10⁴和10⁴之间。
- 树中的所有值**互不相同**。
- 给定的树为二叉搜索树。

#### Solution

本题解法与[中序遍历](/leetcode/2023-02-03-BinaryTree.html#94-二叉树的中序遍历)类似，不过需要按照**右中左**的顺序进行遍历。利用二叉搜索树的性质把上一个节点更新后的值加到当前节点上即可。

[题目链接](https://leetcode.cn/problems/convert-bst-to-greater-tree/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        pre = None

        def traversal(root: Optional[TreeNode]) -> Optional[TreeNode]:
            if not root:
                return None
            
            nonlocal pre
            
            root.right = traversal(root.right)
            
            if pre:
                root.val += pre.val
            
            pre = root
            
            root.left = traversal(root.left)

            return root

        return traversal(root)
```


迭代解法可参考如下。

[题目链接](https://leetcode.cn/problems/convert-bst-to-greater-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def convertBST(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        stack = []
        pre = None
        cur = root
        
        while stack or cur:
            while cur:
                stack.append(cur)
                cur = cur.right
            
            cur = stack.pop()

            if pre:
                cur.val += pre.val

            pre = cur
            cur = cur.left
        
        return root
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
    TreeNode* convertBST(TreeNode* root) {
        stack<TreeNode*> stk;
        TreeNode* pre = nullptr;
        TreeNode* cur = root;

        while (cur != nullptr || !stk.empty()) {
            while (cur != nullptr) {
                stk.push(cur);
                cur = cur->right;
            }

            cur = stk.top(); stk.pop();
            if (pre != nullptr) cur->val += pre->val;

            pre = cur;
            cur = cur->left;
        }

        return root;
    }
};
```


## 公共祖先

### 236. 二叉树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先的定义为：对于有根树`T`的两个节点`p`、`q`，最近公共祖先表示为一个节点`x`，满足`x`是`p`、`q`的祖先且`x`的深度尽可能大(一个节点也可以是它自己的祖先)。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2018/12/14/binarytree.png">
</div>

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2018/12/14/binarytree.png">
</div>

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

**提示：**

- 树中节点数目范围在`[2, 10⁵]`范围内。
- -10⁹ <= `Node.val` <= 10⁹。
- 所有`Node.val`互不相同。
- `p` != `q`。
- `p`和`q`均存在于给定的二叉树中。

#### Solution

本题的解法在于自下而上对二叉树进行遍历，这相当于[后序遍历](/leetcode/2023-02-03-BinaryTree.html#145-二叉树的后序遍历)的过程。如果当前节点`root`为空或是等于`p`和`q`则直接返回`root`，然后再对左子树以及右子树进行递归遍历并将结果分别记录到`left`和`right`中：

- 如果`left`和`right`均非空则说明当前节点`root`是最近公共祖先，返回`root`；
- 如果`left`和`right`只有一个非空则返回非空的节点。

整个递归过程可参考下图。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/IUQ9Lem.png" width="70%">
</div>

[题目链接](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root or root == p or root == q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right= self.lowestCommonAncestor(root.right, p, q)
        
        if left and right:
            return root
        if left:
            return left
        return right
```


C++代码：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr || root == p || root == q) return root;

        TreeNode* left  = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        if (left != nullptr && right != nullptr) return root;
        else if (left == nullptr && right != nullptr) return right;

        return left;
    }
};
```


### 235. 二叉搜索树的最近公共祖先

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先的定义为：对于有根树`T`的两个节点`p`、`q`，最近公共祖先表示为一个节点`x`，满足`x`是`p`、`q`的祖先且`x`的深度尽可能大(一个节点也可以是它自己的祖先)。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png">
</div>

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

**示例2：**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

**提示：**

- 所有节点的值都是唯一的。
- `p`、`q`为不同节点且均存在于给定的二叉搜索树中。

#### Solution

由于二叉搜索树是有序的，本题在代码层面要比[二叉搜索树的最近公共祖先](/leetcode/2023-02-03-BinaryTree.html#236-二叉树的最近公共祖先)简单一些。如果当前节点`root`是`p`和`q`的公共祖先，那么它一定位于区间`[p, q]`上。因此我们只需要从根节点开始进行遍历，当`root`位于区间`[p, q]`时直接返回即可。

[题目链接](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        
        return root
```


C++代码：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == nullptr) return root;

        if (root->val < p->val && root->val < q->val) return lowestCommonAncestor(root->right, p, q);
        else if (root->val > p->val && root->val > q->val) return lowestCommonAncestor(root->left, p, q);

        return root;
    }
};
```


迭代版本的代码可参考如下。

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        vmin = min(p.val, q.val)
        vmax = max(p.val, q.val)
        node = root

        while True:
            if node.val < vmin:
                node = node.right
            elif node.val > vmax:
                node = node.left
            else:
                return node
```


C++代码：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (p->val > q->val) return lowestCommonAncestor(root, q, p);

        TreeNode* cur = root;

        while (cur != nullptr) {
            if (p->val <= cur->val && cur->val <= q->val) return cur;
            else if (cur->val < p->val) cur = cur->right;
            else if (cur->val > q->val) cur = cur->left;
        }

        return cur;
    }
};
```

## 修改与构造

### 701. 二叉搜索树中的插入操作

给定二叉搜索树(BST)的根节点`root`和要插入树中的值`value`，将值插入二叉搜索树。返回插入后二叉搜索树的根节点。输入数据**保证**新值和原始二叉搜索树中的任意节点值都不同。

**注意**，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。你可以返回**任意有效的结果**。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/05/insertbst.jpg">
</div>

```
输入：root = [4,2,7,1,3], val = 5
输出：[4,2,7,1,3,5]
解释：另一个满足题目要求可以通过的树是：
```

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/05/bst.jpg">
</div>

**示例2：**

```
输入：root = [40,20,60,10,30,50,70], val = 25
输出：[40,20,60,10,30,50,70,null,null,25]
```

**示例3：**

```
输入：root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
输出：[4,2,7,1,3,5]
```

**提示：**

- 树中的节点数将在`[0, 10⁴]`的范围内。
- -10⁸ <= `Node.val` <= 10⁸。
- 所有值`Node.val`是**独一无二**的。
- -10⁸ <= `val` <= 10⁸。
- 保证`val`在原始BST中不存在。

#### Solution

本题的技巧在于把新节点作为叶节点插入到树本来的叶节点上，这样就只需要向下遍历二叉树即可。插入节点的条件如下：

- 如果当前节点的左节点`root.left`为空且`root.val > val`，则把`TreeNode(val)`插入到左节点中；
- 如果当前节点的右节点`root.right`为空且`root.val < val`，则把`TreeNode(val)`插入到右节点中；
- 否则根据`root.val`与`val`的大小关系继续向下遍历。

递归算法流程可以参考如下。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/Bw4vpGs.gif">
</div>

[题目链接](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val)
        
        if root.val > val and not root.left:
            root.left = TreeNode(val)
            return root
        elif root.val < val and not root.right:
            root.right= TreeNode(val)
            return root
        
        if root.val < val:
            self.insertIntoBST(root.right, val)
        else:
            self.insertIntoBST(root.left, val)
        
        return root
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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == nullptr) return new TreeNode(val);

        if (root->val > val && root->left == nullptr) {
            root->left = new TreeNode(val);
            return root;
        } else if (root->val < val && root->right == nullptr) {
            root->right = new TreeNode(val);
            return root;
        }

        if (root->val < val) insertIntoBST(root->right, val);
        else if (root->val > val) insertIntoBST(root->left, val);

        return root;
    }
};
```


迭代解法的思路与递归基本一致，代码可参考如下。

[题目链接](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return TreeNode(val)

        node = root

        while node:
            if node.val > val and not node.left:
                node.left = TreeNode(val)
                break
            elif node.val < val and not node.right:
                node.right= TreeNode(val)
                break
            elif node.val < val:
                node = node.right
            elif node.val > val:
                node = node.left
        
        return root
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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == nullptr) return new TreeNode(val);

        TreeNode* cur = root;
        TreeNode* node= new TreeNode(val);

        while (cur != nullptr) {
            if (cur->left == nullptr && cur->val > val) {
                cur->left = node;
                break;
            } else if (cur->right == nullptr && cur->val < val) {
                cur->right = node;
                break;
            }

            if (cur->val < val) cur = cur->right;
            else if (cur->val > val) cur = cur->left;
        }

        return root;
    }
};
```


### 450. 删除二叉搜索树中的节点

给定一个二叉搜索树的根节点`root`和一个值`key`，删除二叉搜索树中的`key`对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树(有可能被更新)的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg">
</div>

```
输入：root = [5,3,6,2,4,null,7], key = 3
输出：[5,4,6,2,null,null,7]
解释：给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。
另一个正确答案是 [5,2,6,null,4,null,7]。
```

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg">
</div>

**示例2：**

```
输入: root = [5,3,6,2,4,null,7], key = 0
输出: [5,3,6,2,4,null,7]
解释: 二叉树不包含值为 0 的节点
```

**示例3：**

```
输入: root = [], key = 0
输出: []
```

#### Solution

删除二叉搜索树中的节点比较复杂，这里我们首先明确函数的返回值为删除节点后该位置上的节点。根据当前节点值`root.val`我们可以分情况讨论：

- 如果`root`为空说明`key`不在二叉树中，返回`None`；
- 如果`key < root.val`说明`key`可能位于左子树上，继续向`root.left`递归；
- 如果`key > root.val`说明`key`可能位于右子树上，继续向`root.right`递归；
- 此时`root.val == key`说明`root`即为待删除节点，根据`root`是否是叶节点继续分情况讨论：
    - 如果`root.left`为空且`root.right`非空，返回`root.right`作为新节点；
    - 如果`root.right`为空且`root.left`非空，返回`root.left`作为新节点；
    - 否则`root`不是叶节点，我们需要找到右子树的最左下节点`node`并把`root.left`连接到`node`上，最后用`root.right`来更新`root`。

整个删除节点的过程可以参考下图。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6OyQVgL.gif">
</div>

[题目链接](https://leetcode.cn/problems/delete-node-in-a-bst/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if not root:
            return None
        elif key < root.val:
            root.left = self.deleteNode(root.left, key)
        elif key > root.val:
            root.right = self.deleteNode(root.right, key)
        else:
            if not root.left:
                root = root.right
            elif not root.right:
                root = root.left
            else:
                node = root.right
                ## leftmost node on the right child
                while node.left:
                    node = node.left
                
                node.left = root.left
                root = root.right
        
        return root
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
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) return root;

        if (root->val == key) {
            if (root->left == nullptr && root->right == nullptr) {
                delete root;
                return nullptr;
            }
            else if (root->left == nullptr) {
                auto node = root->right;
                delete root;
                return node;
            }
            else if (root->right == nullptr) {
                auto node = root->left;
                delete root;
                return node;
            } else {
                TreeNode* cur = root->right;
                while (cur->left != nullptr) cur = cur->left;

                cur->left = root->left;
                
                cur = root;
                root = root->right;
                delete cur;

                return root;
            }
        }

        else if (root->val > key) root->left = deleteNode(root->left, key);
        else root->right = deleteNode(root->right, key);

        return root;
    }
};
```


### 669. 修剪二叉搜索树

给你二叉搜索树的根节点`root`，同时给定最小边界`low`和最大边界`high`。通过修剪二叉搜索树，使得所有节点的值在`[low, high]`中。修剪树**不应该**改变保留在树中的元素的相对结构(即，如果没有被移除，原有的父代子代关系都应当保留)。可以证明，存在**唯一的答案**。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/09/trim1.jpg">
</div>

```
输入：root = [1,0,2], low = 1, high = 2
输出：[1,null,2]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/09/trim2.jpg">
</div>

```
输入：root = [3,0,4,null,2,null,null,1], low = 1, high = 3
输出：[3,2,null,1]
```

**提示：**

- 树中的节点数将在`[1, 10⁴]`的范围内。
- 0 <= `Node.val` <= 10⁴。
- 所树中每个节点的值都是**唯一**的。
- 题目数据保证输入是一棵有效的二叉搜索树。
- 0 <= `low` <= `high` <= 10⁴。

#### Solution

本题要利用二叉搜索树的性质进行求解。首先明确递归解法的返回值为当前树修剪后的根节点；当`root.val < low`时说明`root`及其左子树`root.left`都在区间`[low, high]`之外，因此只需返回修剪后的右子树即可；类似地，当`root.val < high`时只需返回修剪后的左子树；而当`root.val`在区间`[low, high]`中时则需要进一步递归处理左子树以及右子树。

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/1662768804-EhxyIe-1.png">
</div>

[题目链接](https://leetcode.cn/problems/trim-a-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return None
        
        if root.val < low:
            return self.trimBST(root.right, low, high)
        elif root.val > high:
            return self.trimBST(root.left, low, high)
        else:
            root.left = self.trimBST(root.left, low, high)
            root.right= self.trimBST(root.right, low, high)

            return root
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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (root == nullptr) return root;

        if (root->val < low) return trimBST(root->right, low, high);
        else if (root->val > high) return trimBST(root->left, low, high);
        else {
            root->left  = trimBST(root->left, low, high);
            root->right = trimBST(root->right, low, high);

            return root;
        }
    }
};
```


本题的迭代解法要相对复杂一些，大体流程可以分为三步：

- 寻找到位于区间`[low, high]`的节点作为`root`；
- 处理左子树`root.left`使其都位于区间`[low, high]`内；
- 处理右子树`root.right`使其都位于区间`[low, high]`内。

在处理左右子树时需要结合二叉搜索树的性质：如果左节点的值小于`low`则整个左子树都小于`low`，此时需要把`node.left`设置为`node.left.right`并继续进行修剪；类似地，对于右节点则需要考虑右节点的值是否大于`high`，如果`node.right.val > high`则需要向左移动`node.right = node.right.left`。

[题目链接](https://leetcode.cn/problems/trim-a-binary-search-tree/)：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: Optional[TreeNode], low: int, high: int) -> Optional[TreeNode]:
        if not root:
            return None
        
        ## find a valid root
        while root and (root.val < low or root.val > high):
            if root.val < low:
                root = root.right
            else:
                root = root.left
        
        ## process left child
        node = root
        while node:
            while node.left and node.left.val < low:
                node.left = node.left.right
            node = node.left

        ## process right child
        node = root
        while node:
            while node.right and node.right.val > high:
                node.right = node.right.left
            node = node.right
        
        return root
```


### 108. 将有序数组转换为二叉搜索树

给你一个整数数组`nums`，其中元素已经按**升序**排列，请你将其转换为一棵**高度平衡**二叉搜索树。

**高度平衡**二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过1」的二叉树。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/18/btree1.jpg">
</div>

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/18/btree2.jpg">
</div>

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/18/btree.jpg">
</div>

```
输入：nums = [1,3]
输出：[3,1]
解释：[1,null,3] 和 [3,1] 都是高度平衡二叉搜索树。
```

**提示：**

- 1 <= `nums.length` <= 10⁴。
- -10⁴ <= nums[i] <= 10⁴。
- `nums`按**严格递增**顺序排列。

#### Solution

本题的递归解法比较容易。我们只需要选择`nums`的中点作为`root`，然后分别对`nums`的左右部分递归构造构造左右子树即可。

[题目链接](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        if not nums:
            return None

        mid = len(nums) // 2

        root = TreeNode(nums[mid])
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right= self.sortedArrayToBST(nums[mid+1:])

        return root
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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return buildTree(nums, 0, nums.size());
    }

    TreeNode* buildTree(vector<int>& nums, int left, int right) {
        if (left >= right) return nullptr;

        int mid = left + (right - left) / 2;

        TreeNode* root = new TreeNode(nums[mid]);
        root->left  = buildTree(nums, left, mid);
        root->right = buildTree(nums, mid+1, right);

        return root;
    }
};
```


### 1382. 将二叉搜索树变平衡

给你一棵二叉搜索树，请你返回一棵**平衡后**的二叉搜索树，新生成的树应该与原来的树有着相同的节点值。如果有多种构造方法，请你返回任意一种。

如果一棵二叉搜索树中，每个节点的两棵子树高度差不超过`1`，我们就称这棵二叉搜索树是**平衡的**。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg">
</div>

```
输入：root = [1,null,2,null,3,null,4,null,null]
输出：[2,1,3,null,null,null,4]
解释：这不是唯一的正确答案，[3,1,4,null,2,null,null] 也是一个可行的构造方案。
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg">
</div>

```
输入: root = [2,1,3]
输出: [2,1,3]
```

**提示：**

- 树节点的数目在`[1, 10⁴]`范围内。
- 1 <= `Node.val` <= 10⁵。

#### Solution

本题解法可参考[验证二叉搜索树](/leetcode/2023-02-03-BinaryTree.html#98-验证二叉搜索树)和[将有序数组转换为二叉搜索树](/leetcode/2023-02-03-BinaryTree.html#108-将有序数组转换为二叉搜索树)，我们先利用[中序遍历](/leetcode/2023-02-03-BinaryTree.html#94-二叉树的中序遍历)将二叉搜索树转换为有序数组`nums`，再从`nums`来构造平衡的二叉搜索树即可。

[题目链接](https://leetcode.cn/problems/balance-a-binary-search-tree/)：

python代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def balanceBST(self, root: TreeNode) -> TreeNode:
        def traversal(root: TreeNode) -> List[int]:
            stack = []
            res   = []
            cur   = root

            while stack or cur:
                while cur:
                    stack.append(cur)
                    cur = cur.left
                
                cur = stack.pop()
                res.append(cur.val)
                cur = cur.right
            
            return res
        
        def buildTree(nums: List[int]) -> TreeNode:
            if not nums:
                return None
            
            mid = len(nums) // 2
            root= TreeNode(nums[mid])

            root.left = buildTree(nums[:mid])
            root.right= buildTree(nums[mid+1:])

            return root

        nums = traversal(root)

        return buildTree(nums)
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
    TreeNode* balanceBST(TreeNode* root) {
        vector<int> nums;
        traversal(root, nums);

        return buildTree(nums, 0, nums.size());
    }

    void traversal(TreeNode* root, vector<int> &nums) {
        stack<TreeNode*> stk;
        TreeNode* cur = root;

        while (cur != nullptr || !stk.empty()) {
            while (cur != nullptr) {
                stk.push(cur);
                cur = cur->left;
            }

            cur = stk.top(); stk.pop();
            nums.push_back(cur->val);
            cur = cur->right;
        }
    }

    TreeNode* buildTree(vector<int> &nums, int left, int right) {
        if (left >= right) return nullptr;

        int mid = left + (right - left) / 2;
        TreeNode* root = new TreeNode(nums[mid]);

        root->left  = buildTree(nums, left, mid);
        root->right = buildTree(nums, mid+1, right);

        return root;
    }
};
```



### 116. 填充每个节点的下一个右侧节点指针

给定一个**完美二叉树**，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```
struct Node {
    int val;
    Node *left;
    Node *right;
    Node *next;
}
```

填充它的每个`next`指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将`next`指针设置为`NULL`。

初始状态下，所有`next`指针都被设置为`NULL`。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2019/02/14/116_sample.png">
</div>

```
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
```

**示例2：**

```
输入：root = []
输出：[]
```

**提示：**

- 树中节点的数量在`[0, 2¹² - 1]`范围内。
- -1000 <= node.val <= 1000。

#### Solution

本题的基本解法是利用[层序遍历](/leetcode/2023-02-03-BinaryTree.html#102-二叉树的层序遍历)对每一层上的节点进行展开，然后令`next`指针指向同一层的下一个节点。

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/c657e60fe795868e754741d7019055879d3fd9a5152c965391312f00779d6121-2.jpg">
</div>

[题目链接](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)：

python代码：

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if not root:
            return None

        from collections import deque

        queue = deque([root])

        while queue:
            N = len(queue)

            for i in range(N):
                node = queue.popleft()
                
                if i == N-1:
                    node.next = None
                else:
                    node.next = queue[0]

                if node.left:
                    queue.append(node.left)
                    queue.append(node.right)
        
        return root
```


C++代码：

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (root == nullptr) return nullptr;

        queue<Node*> que;
        que.push(root);

        while (!que.empty()) {
            int N = que.size();

            for (int i=0; i<N; ++i) {
                Node* node = que.front(); que.pop();

                if (i < N-1) node->next = que.front();

                if (node->left != nullptr)  que.push(node->left);
                if (node->right != nullptr) que.push(node->right);
            }

        }

        return root;
    }
};
```


本题的另一种解法是把每一层的节点看做是一个链表。对于同一层的节点，如果它们来自相同的父节点则可以通过父节点将它们连接起来。

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/b7317ab6466b08ae5c47abafd266f919b09a67d733a5a4d471c76da19737fb44-3.jpg">
</div>

如果它们来自不同的父节点，则可以通过前一个节点父节点的`next`指针来串联起来。

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/fff80ea5177cb58e6860d920d98d2df9a2d7bb1745015aff048dc4a2d1c6ad50-4.jpg">
</div>

这样我们只需要从第一层开始将下一层的节点连接起来即可。这种算法的空间复杂度为`O(1)`，其流程可以参考如下。

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/54tbhID.gif" width="70%">
</div>

[题目链接](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)：

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if not root:
            return None

        leftmost = root
        
        while leftmost.left:
            head = leftmost

            while head:
                head.left.next = head.right
                
                if head.next:
                    head.right.next = head.next.left
                
                head = head.next
            
            leftmost = leftmost.left
        
        return root
```


## Reference

- [二叉树理论基础](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
- [关于二叉树，你该了解这些！](https://www.bilibili.com/video/BV1Hy4y1t7ij/?vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：144.二叉树的前序遍历 145.二叉树的后序遍历 94.二叉树的中序遍历](https://www.bilibili.com/video/BV1Wh411S7xt/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [二叉树的非递归遍历](https://www.bilibili.com/video/BV15f4y1W7i2/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [二叉树的非递归遍历-中序](https://www.bilibili.com/video/BV1Zf4y1a77g/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：6.翻转二叉树](https://www.bilibili.com/video/BV1sP4y1f7q7/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：101.对称二叉树](https://www.bilibili.com/video/BV1ue4y1Y7Mf/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：104.二叉树的最大深度](https://www.bilibili.com/video/BV1Gd4y1V75u/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：111.二叉树的最小深度](https://www.bilibili.com/video/BV1QD4y1B7e2/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：222.完全二叉树节点的数量](https://www.bilibili.com/video/BV1eW4y1B7pD/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：110.平衡二叉树](https://www.bilibili.com/video/BV1Ug411S7my/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：257.二叉树的所有路径](https://www.bilibili.com/video/BV1ZG411G7Dh/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：404.左叶子之和](https://www.bilibili.com/video/BV1GY4y1K7z8/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：513.找二叉树左下角的值](https://www.bilibili.com/video/BV1424y1Z7pn/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：112.路径总和](https://www.bilibili.com/video/BV19t4y1L7CR/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：106.从中序与后序遍历序列构造二叉树](https://www.bilibili.com/video/BV1vW4y1i7dn/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：654.最大二叉树](https://www.bilibili.com/video/BV1MG411G7ox/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：617.合并二叉树](https://www.bilibili.com/video/BV1m14y1Y7JK/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：700.二叉搜索树中的搜索](https://www.bilibili.com/video/BV1wG411g7sF/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：98.验证二叉搜索树](https://www.bilibili.com/video/BV18P411n7Q4/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：530.二叉搜索树的最小绝对差](https://www.bilibili.com/video/BV1DD4y11779/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：501.二叉搜索树中的众数](https://www.bilibili.com/video/BV1fD4y117gp/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：236.二叉树的最近公共祖先](https://www.bilibili.com/video/BV1jd4y1B7E2/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：235.二叉搜索树的最近公共祖先](https://www.bilibili.com/video/BV1Zt4y1F7ww/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：701.二叉搜索树中的插入操作](https://www.bilibili.com/video/BV1Et4y1c78Y/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：450.删除二叉搜索树中的节点](https://www.bilibili.com/video/BV1tP41177us/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：669.修剪二叉搜索树](https://www.bilibili.com/video/BV17P41177ud/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：108.将有序数组转换为二叉搜索树](https://www.bilibili.com/video/BV1uR4y1X7qL/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：538.把二叉搜索树转换为累加树](https://www.bilibili.com/video/BV1d44y1f7wP/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)