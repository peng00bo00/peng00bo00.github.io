---
layout: post
title: 链表
date: 2023-01-09
tags: leetcode
toc:
  sidebar: left
---

**链表(linked list)**是一种通过指针串联在一起的线性结构，其中的每一个元素称为一个**节点(node)**。以单向链表为例，它的每个节点包含一个数据域用来存放数据，同时每个节点还包含一个指针域指向它的下一个节点。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/RKgLwHR.png" width="80%">
</div>

## 删除节点

### 203. 移除链表元素

给你一个链表的头节点`head`和一个整数`val`，请你删除链表中所有满足`Node.val == val`的节点，并返回**新的头节点**。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg" width="80%">
</div>

```
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

**示例2：**

```
输入：head = [], val = 1
输出：[]
```

**示例3：**

```
输入：head = [7,7,7,7], val = 7
输出：[]
```

**提示：**

- 列表中的节点数目在范围`[0, 10⁴]`内
- 1 <= `Node.val` <= 50
- 0 <= `val` <= 50

#### Solution

删除元素是链表的基本操作之一。由于我们不能像数组那样直接通过索引来获取节点，在删除元素时需要通过循环的方式来对整个链表进行遍历。在进行循环时我们首先为链表添加一个**虚拟头节点(哨兵节点)**`sentinel`，它指向链表的头节点`head`。然后从`sentinel`出发依次检验它的下一个节点是否包含`val`，如果包含则另当前节点指向再下一个节点。最后返回`sentinel`的后继节点即可。

[题目链接](https://leetcode.cn/problems/remove-linked-list-elements/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        sentinel = ListNode(next=head)
        node = sentinel

        while node.next is not None:
            if node.next.val == val:
                node.next = node.next.next
            else:
                node = node.next
        
        return sentinel.next
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode dummy;
        dummy.next = head;

        ListNode* cur = &dummy;
        while (cur) {
            if (cur->next && cur->next->val == val) {
                cur->next = cur->next->next;
            } else {
                cur = cur->next;
            }
        }

        return dummy.next;
    }
};
```


除了循环之外，我们也可以使用递归来处理删除节点的问题。每次调用函数时我们只检查当前的节点是否包含`val`，如果包含则把后继节点作为函数输入。

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        if head is None:
            return head
        
        if head.val != val:
            head.next = self.removeElements(head.next, val)
            return head
        else:
            return self.removeElements(head.next, val)
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        if (!head) return head;

        if (head->val == val) {
            return removeElements(head->next, val);
        } else{
            head->next = removeElements(head->next, val);
            return head;
        }
    }
};
```

### 19. 删除链表的倒数第N个结点

给你一个链表，删除链表的倒数第`n`个结点，并且返回链表的头结点。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg" width="80%">
</div>

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

**提示：**

- 链表中结点的数目为`sz`
- 1 <= `sz` <= 30
- 0 <= `Node.val` <= 100
- 1 <= `n` <= `sz`

#### Solution

本题的暴力解法是先对链表进行遍历从而得到它的大小`sz`然后删除第`sz-n`个节点，这种做法需要对链表进行两次遍历。实际上利用双指针只要一次遍历就可以实现相同的效果：我们首先为链表设置一个哨兵节点`sentinel`并且令`fast`和`slow`两个指针都指向它；然后令快指针`fast`先前进`n+1`步；最后让两个指针同时前进，当`fast`指向`None`时`slow`会指向倒数第`n`个节点的前一个节点，此时只要删除`slow`的下一个节点即可。

使用双指针的算法流程可以参考下图：

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/UG0xBSu.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        sentinel = ListNode(next=head)
        fast, slow = sentinel, sentinel

        for _ in range(n+1):
            fast = fast.next
        
        while fast is not None:
            fast = fast.next
            slow = slow.next
        
        slow.next = slow.next.next
        
        return sentinel.next
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);

        ListNode* fast = head;
        for(int i=0; i<n; ++i) {
            fast = fast->next;
        }

        ListNode* slow = dummy;
        while (fast) {
            fast = fast->next;
            slow = slow->next;
        }

        slow->next = slow->next->next;
        
        ListNode* res = dummy->next;
        delete dummy;

        return res;
    }
};
```


### 237. 删除链表中的节点

有一个单链表的`head`，我们想删除它其中的一个节点`node`。

给你一个需要删除的节点`node`。你将**无法访问**第一个节点`head`。

链表的所有值都是**唯一的**，并且保证给定的节点`node`不是链表中的最后一个节点。

删除给定的节点。注意，删除节点并不是指从内存中删除它。这里的意思是：

- 给定节点的值不应该存在于链表中。
- 链表中的节点数应该减少 1。
- `node`前面的所有值顺序相同。
- `node`后面的所有值顺序相同。

**自定义测试：**

- 对于输入，你应该提供整个链表`head`和要给出的节点`node`。`node`不应该是链表的最后一个节点，而应该是链表中的一个实际节点。
- 我们将构建链表，并将节点传递给你的函数。
- 输出将是调用你函数后的整个链表。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/01/node1.jpg">
</div>

```
输入：head = [4,5,1,9], node = 5
输出：[4,1,9]
解释：指定链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/09/01/node2.jpg">
</div>

```
输入：head = [4,5,1,9], node = 1
输出：[4,5,9]
解释：指定链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9
```

**提示：**

- 链表中节点的数目范围是`[2, 1000]`
- -1000 <= `Node.val` <= 1000
- 链表中每个节点的值都是**唯一**的
- 需要删除的节点`node`是**链表中的节点**，且**不是末尾节点**

#### Solution

本题的解法非常简单。我们只需要将`node.next`的值赋给`node`，然后再令`node`指向`node.next.next`即可。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/mw3r37R.png" width="70%">
<img src="https://images.weserv.nl/?url=i.imgur.com/WqWSwHI.png" width="70%">
<img src="https://images.weserv.nl/?url=i.imgur.com/8BSpSWr.png" width="70%">
</div>

[题目链接](https://leetcode.cn/problems/delete-node-in-a-linked-list/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """

        node.val = node.next.val
        node.next = node.next.next
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val  = node->next->val;
        node->next = node->next->next;
    }
};
```


## 合并链表

### 21. 合并两个有序链表

将两个升序链表合并为一个新的**升序**链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg">
</div>

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

**示例2：**

```
输入：l1 = [], l2 = []
输出：[]
```

**示例3：**

```
输入：l1 = [], l2 = [0]
输出：[0]
```

**提示：**

- 两个链表的节点数目范围是`[0, 50]`
- -100 <= `Node.val` <= 100
- `l1`和`l2`均按**非递减顺序**排列

#### Solution

本题的递归解法比较简单。我们只需要选择`list1`和`list2`中较小的那个头结点，然后将该节点连接到递归合并其它节点后的头结点即可。

[题目链接](https://leetcode.cn/problems/merge-two-sorted-lists/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        if not list1:
            return list2
        elif not list2:
            return list1
        
        if list1.val < list2.val:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
        else:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
```


```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (!list1) {
            return list2;
        } else if (!list2) {
            return list1;
        } else if (list1->val < list2->val) {
            list1->next = mergeTwoLists(list1->next, list2);
            return list1;
        } else {
            list2->next = mergeTwoLists(list1, list2->next);
            return list2;
        }

    }
};
```


本题的迭代解法与递归类似可参考如下。

[题目链接](https://leetcode.cn/problems/merge-two-sorted-lists/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        virtual = ListNode()

        head1 = list1
        head2 = list2
        node  = virtual

        while head1 and head2:
            if head1.val < head2.val:
                node.next = head1
                head1 = head1.next
            else:
                node.next = head2
                head2 = head2.next
            
            node = node.next
        
        if head1:
            node.next = head1
        else:
            node.next = head2

        return virtual.next
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* dummy = new ListNode();

        ListNode* n1   = list1;
        ListNode* n2   = list2;
        ListNode* node = dummy;

        while (n1 and n2) {
            if (n1->val < n2->val) {
                node->next = n1;
                n1 = n1->next;
            } else {
                node->next = n2;
                n2 = n2->next;
            }

            node = node->next;
        }

        if (!n1) {
            node->next = n2;
        } else if (!n2) {
            node->next = n1;
        }

        ListNode* res = dummy->next;
        delete dummy;

        return res;
    }
};
```


## 设计链表

### 707. 设计链表

设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：`val`和`next`。`val`是当前节点的值，`next`是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性`prev`以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

- get(index)：获取链表中第`index`个节点的值。如果索引无效，则返回`-1`。
- addAtHead(val)：在链表的第一个元素之前添加一个值为`val`的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为`val`的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第`index`个节点之前添加值为`val` 的节点。如果`index`等于链表的长度，则该节点将附加到链表的末尾。如果`index`大于链表长度，则不会插入节点。如果`index`小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引`index`有效，则删除链表中的第`index`个节点。

**示例：**

```
MyLinkedList linkedList = new MyLinkedList();
linkedList.addAtHead(1);
linkedList.addAtTail(3);
linkedList.addAtIndex(1,2);   //链表变为1-> 2-> 3
linkedList.get(1);            //返回2
linkedList.deleteAtIndex(1);  //现在链表是1-> 3
linkedList.get(1);            //返回3
```

**提示：**

- 0 <= `index`, `val` <= 1000
- 请不要使用内置的LinkedList库
- `get`, `addAtHead`, `addAtTail`, `addAtIndex`和`deleteAtIndex`的操作次数不超过`2000`

#### Solution

本题综合考量了对链表的各种基本操作。在实现各个方法时首先要定义链表节点类`Node`如下：

```python
class Node:
    def __init__(self, val, next=None):
        self.val = val
        self.next= next
```

然后在初始化中定义两个属性`self.virtual`和`self.size`分别作为虚拟头节点以及链表大小的记数：

```python
class MyLinkedList:
    def __init__(self):
        self.virtual = Node(0)
        self.size = 0
```

接下来开始实现所需的各个方法，首先是`get()`用来获取编号为`index`的节点值。当`index`是有效的编号时我们用指针`node`指向头结点`self.virtual.next`，然后令其前进`index`步就指向了编号为`index`的节点，最后返回`node.val`即可。

```python
    def get(self, index: int) -> int:
        if index >= self.size:
            return -1
        
        node = self.virtual.next

        for i in range(index):
            node = node.next
        
        return node.val
```

然后是`addAtHead()`方法。我们只需要在虚拟头节点`self.virtual`后添加一个新节点，并把原来的头结点`self.virtual.next`连接到新节点上即可，当然最后还要令记数`self.size += 1`。

```python
    def addAtHead(self, val: int) -> None:
        node = Node(val, next=self.virtual.next)
        self.virtual.next = node

        self.size += 1
```

`addAtTail()`方法与`addAtHead()`类似。我们首先需要通过遍历来找到尾结点，然后把新节点连到尾结点并令记数`self.size += 1`即可。

```python
    def addAtTail(self, val: int) -> None:
        node = self.virtual

        for i in range(self.size):
            node = node.next
        
        node.next = Node(val)

        self.size += 1
```

`addAtIndex()`要相对复杂一些。我们首先需要判断`index`的合法性：如果`index`不合法则直接返回，否则需要通过遍历找到编号为`index`的节点位置。这里使用指针`pre`指向虚拟头节点`self.virtual`，然后令其移动`index`步。此时`pre`指向了编号为`index`的节点的前一个节点，然后添加新节点并把`pre.next`连接到新节点上即可。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/fcUoVO0.png" width="70%">
</div>

```python
    def addAtIndex(self, index: int, val: int) -> None:
        if index > self.size:
            return
        
        pre = self.virtual

        for i in range(index):
            pre = pre.next
        
        node = Node(val, pre.next)
        pre.next = node

        self.size += 1
```

最后来考虑`deleteAtIndex()`，它与`addAtIndex()`基本一致。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/9oQYybE.png" width="70%">
</div>

```python
    def deleteAtIndex(self, index: int) -> None:
        if index >= self.size:
            return
        
        pre = self.virtual

        for i in range(index):
            pre = pre.next
        
        pre.next = pre.next.next
        
        self.size -= 1
```

[题目链接](https://leetcode.cn/problems/design-linked-list/)：

python代码：

```python
class Node:
    def __init__(self, val, next=None):
        self.val = val
        self.next= next

class MyLinkedList:
    def __init__(self):
        self.virtual = Node(0)
        self.size = 0

    def get(self, index: int) -> int:
        if index >= self.size:
            return -1
        
        node = self.virtual.next

        for i in range(index):
            node = node.next
        
        return node.val

    def addAtHead(self, val: int) -> None:
        node = Node(val, next=self.virtual.next)
        self.virtual.next = node

        self.size += 1

    def addAtTail(self, val: int) -> None:
        node = self.virtual

        for i in range(self.size):
            node = node.next
        
        node.next = Node(val)

        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index > self.size:
            return
        
        pre = self.virtual

        for i in range(index):
            pre = pre.next
        
        node = Node(val, pre.next)
        pre.next = node

        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index >= self.size:
            return
        
        pre = self.virtual

        for i in range(index):
            pre = pre.next
        
        pre.next = pre.next.next
        
        self.size -= 1
```


C++代码：

```c++
struct LinkedNode {
    int val;
    LinkedNode *next;

    LinkedNode() : val(0), next(nullptr) {}
    LinkedNode(int x) : val(x), next(nullptr) {}
};

class MyLinkedList {
public:
    MyLinkedList() {
        dummy = new LinkedNode();
        size  = 0;
    }
    
    int get(int index) {
        if (index < 0 || index > size-1) return -1;

        LinkedNode *cur = dummy->next;
        while (index) {
            cur = cur->next;
            --index;
        }

        return cur->val;
    }
    
    void addAtHead(int val) {
        LinkedNode* node = new LinkedNode(val);

        node->next = dummy->next;
        dummy->next = node;

        ++size;
    }
    
    void addAtTail(int val) {
        LinkedNode* node = new LinkedNode(val);

        auto cur = dummy;
        while (cur->next) {
            cur = cur->next;
        }

        cur->next = node;

        ++size;
    }
    
    void addAtIndex(int index, int val) {
        if (index > size) return;

        LinkedNode* node = new LinkedNode(val);

        auto cur = dummy;
        while (index) {
            cur = cur->next;
            --index;
        }

        node->next = cur->next;
        cur->next  = node;

        ++size;
    }
    
    void deleteAtIndex(int index) {
        if (index >= size || index < 0) return;

        auto cur = dummy;
        while (index) {
            cur = cur->next;
            --index;
        }

        auto node = cur->next;
        cur->next = node->next;

        delete node;

        --size;
    }

private:
    LinkedNode *dummy;
    int size;
};
```


除了上面介绍过的实现方法外，添加节点的相关函数中我们可以先实现`addAtIndex()`方法然后在它的基础上来实现`addAtHead()`和`addAtTail()`。这样的形式可以更好地复用已有代码，适合大型项目的开发。

```python
    def addAtHead(self, val: int) -> None:
        return self.addAtIndex(0, val)

    def addAtTail(self, val: int) -> None:
        return self.addAtIndex(self.size, val)
```

[题目链接](https://leetcode.cn/problems/design-linked-list/)：

python代码：

```python
class Node:
    def __init__(self, val, next=None):
        self.val = val
        self.next= next

class MyLinkedList:
    def __init__(self):
        self.virtual = Node(0)
        self.size = 0

    def get(self, index: int) -> int:
        if index >= self.size:
            return -1
        
        node = self.virtual.next

        for i in range(index):
            node = node.next
        
        return node.val

    def addAtHead(self, val: int) -> None:
        return self.addAtIndex(0, val)

    def addAtTail(self, val: int) -> None:
        return self.addAtIndex(self.size, val)

    def addAtIndex(self, index: int, val: int) -> None:
        if index > self.size:
            return
        
        pre = self.virtual

        for i in range(index):
            pre = pre.next
        
        node = Node(val, pre.next)
        pre.next = node

        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index >= self.size:
            return
        
        pre = self.virtual

        for i in range(index):
            pre = pre.next
        
        pre.next = pre.next.next
        
        self.size -= 1
```


C++代码：

```c++
struct LinkedNode {
    int val;
    LinkedNode *next;

    LinkedNode() : val(0), next(nullptr) {}
    LinkedNode(int x) : val(x), next(nullptr) {}
};

class MyLinkedList {
public:
    MyLinkedList() {
        dummy = new LinkedNode();
        size  = 0;
    }
    
    int get(int index) {
        if (index < 0 || index > size-1) return -1;

        LinkedNode *cur = dummy->next;
        while (index) {
            cur = cur->next;
            --index;
        }

        return cur->val;
    }
    
    void addAtHead(int val) {
        addAtIndex(0, val);
    }
    
    void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    void addAtIndex(int index, int val) {
        if (index > size) return;

        LinkedNode* node = new LinkedNode(val);

        auto cur = dummy;
        while (index) {
            cur = cur->next;
            --index;
        }

        node->next = cur->next;
        cur->next  = node;

        ++size;
    }
    
    void deleteAtIndex(int index) {
        if (index >= size || index < 0) return;

        auto cur = dummy;
        while (index) {
            cur = cur->next;
            --index;
        }

        auto node = cur->next;
        cur->next = node->next;

        delete node;

        --size;
    }

private:
    LinkedNode *dummy;
    int size;
};
```


## 翻转链表

### 206.反转链表

给你单链表的头节点`head`，请你反转链表，并返回反转后的链表。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg">
</div>

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg">
</div>

```
输入：head = [1,2]
输出：[2,1]
```

**示例3：**

```
输入：head = []
输出：[]
```

**提示：**

- 链表中节点的数目范围是`[0, 5000]`
- -5000 <= `Node.val` <= 5000

#### Solution

翻转链表是链表中的经典问题，我们可以通过修改节点指向来实现：记`cur`为指向当前节点的指针而`pre`为指向`cur`前一个节点的指针，我们只需要利用一个临时指针`tmp`来将当前节点的指向修改为`pre`即可。

整个链表翻转过程可参考下图：

<div align=center>
<img src="https://search.pstatic.net/common?src=https://i.imgur.com/6xzxAaV.gif" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/reverse-linked-list/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre, cur = None, head

        while cur is not None:
            tmp = cur
            cur = cur.next

            tmp.next = pre
            pre = tmp
        
        return pre
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = head;
        ListNode* pre = nullptr;
        
        ListNode* tmp;

        while (cur) {
            tmp = cur;
            cur = cur->next;

            tmp->next = pre;
            pre = tmp;
        }

        return pre;
    }
};
```


除了对链表进行遍历外我们还可以使用递归来进行处理：首先对当前头节点`head`的下一个节点`head.next`调用翻转函数，得到翻转的后继链表头节点，记为`newhead`；接着修改后继翻转链表末尾节点的指向，使它指向`head`；然后修改`head`的指向使其指向`None`，这样`head`就称为了翻转后链表的末尾节点；最后返回`newhead`即可。

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head
        
        newhead = self.reverseList(head.next)
        head.next.next = head
        head.next = None

        return newhead
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !(head->next)) return head;

        ListNode* newHead = reverseList(head->next);

        head->next->next = head;
        head->next       = nullptr;

        return newHead;
    }
};
```


## 两两交换链表中的节点

### 24. 两两交换链表中的节点

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题(即，只能进行节点交换)。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg">
</div>

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例2：**

```
输入：head = []
输出：[]
```

**示例3：**

```
输入：head = [1]
输出：[1]
```

**提示：**

- 链表中节点的数目在范围`[0, 100]`内。
- 0 <= `Node.val` <= 100。

#### Solution

要实现链表中元素两两交换需要每次获取两个节点`first`和`second`然后修改它们的指向。相应地，在更新当前头指针`cur`时需要向后移动2位。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/qN1kx4b.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/swap-nodes-in-pairs/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        sentinel = ListNode(next=head)
        cur = sentinel

        while cur.next and cur.next.next:
            first, second = cur.next, cur.next.next

            first.next = second.next
            second.next = first
            cur.next = second

            cur = cur.next.next

        return sentinel.next
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummy = new ListNode();
        dummy->next = head;

        ListNode* cur = dummy;

        while (cur->next && cur->next->next) {
            ListNode* first  = cur->next;
            ListNode* second = cur->next->next;

            first->next  = second->next;
            second->next = first;
            cur->next    = second;
            
            cur = cur->next->next;
        }

        auto res = dummy->next;
        delete dummy;

        return res;
    }
};
```


递归解法在思维上要更简洁一些：我们同样要获取当前链表的前2个节点`first`和`second`；接着令`first`指向交换后的后续链表；然后令`second`指向`first`，这样就完成了前两个节点的交换；最后返回`second`节点作为当前链表的头节点即可。

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head
        
        first, second = head, head.next

        first.next = self.swapPairs(second.next)
        second.next = first

        return second
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head || !(head->next)) return head;

        ListNode* first  = head;
        ListNode* second = head->next;

        first->next  = swapPairs(second->next);
        second->next = first;

        return second;
    }
};
```


## 双指针

### 面试题 02.07. 链表相交

给你两个单链表的头节点`headA`和`headB`，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回`null`。

图示两个链表在节点`c1`开始相交：

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/GvrrbR0.png" width="60%">
</div>

题目数据**保证**整个链式结构中不存在环。

注意，函数返回结果后，链表必须**保持其原始结构**。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/cyYQjd7.png" width="60%">
</div>

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/YbhMkxu.png" width="55%">
</div>

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例3：**

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/nfcXQEI.png" width="35%">
</div>

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

**提示：**

- `listA`中节点数目为`m`
- `listB`中节点数目为`n`
- 0 <= `m`, `n` <= 3 * 10⁴
- 1 <= `Node.val` <= 10⁵
- 0 <= `skipA` <= `m`
- 0 <= `skipB` <= `n`
- 如果`listA`和 `listB`没有交点，`intersectVal`为`0`
- 如果`listA`和 `listB`有交点，`intersectVal` == `listA[skipA + 1]` == `listB[skipB + 1]`

#### Solution

我们假设两个链表的长度分别为`a`和`b`，而公共部分的长度为`c`。这样A和B两个链表在交点前的长度就分别是`a-c`和`b-c`。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/IeaGrxu.png" width="80%">
</div>

我们用两个指针分别对A和B两个链表进行遍历。当A链表的指针走到终点时把它移动到B链表的表头；类似地，当A链表的指针走到终点时把它移动到A链表的表头。当它们都走过`a+b-c`步时恰好会在两个链表的交点处相遇。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/CiwIhrU.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        A, B = headA, headB

        while A != B:
            A = A.next if A else headB
            B = B.next if B else headA
        
        return A
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *A = headA, *B = headB;

        while (A != B) {
            A = (A == nullptr ? headB : A->next);
            B = (B == nullptr ? headA : B->next);
        }

        return A;
    }
};
```


### 142. 环形链表 II

给定一个链表的头节点`head`，返回链表开始入环的第一个节点。如果链表无环，则返回`null`。

如果链表中有某个节点，可以通过连续跟踪`next`指针再次到达，则链表中存在环。为了表示给定链表中的环，评测系统内部使用整数`pos`来表示链表尾连接到链表中的位置（索引从`0`开始）。如果`pos`是`-1`，则在该链表中没有环。注意：`pos`不作为参数进行传递，仅仅是为了标识链表的实际情况。

**不允许修改**链表。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/R70sKfT.png" width="45%">
</div>

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/8GdM8bK.png" width="22%">
</div>

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例3：**

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/iUX1OVd.png" width="8%">
</div>

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

**提示：**

- 链表中节点的数目范围在范围`[0, 10⁴]`内
- -10⁵ <= `Node.val` <= 10⁵
- `pos`的值为`-1`或者链表中的一个有效索引

#### Solution

本题使用双指针时需要让快指针`fast`每次前进2步而慢指针`slow`前进1步，这样当两个指针相遇时快指针`fast`恰好比慢指针`slow`多走了`n`圈，即两个指针重合时`fast`比`slow`多走**环的长度整数倍**。更进一步可以证明此时慢指针`slow`前进的步数为`nb`，而快指针`fast`则前进了`2nb`步，其中`b`为环的长度。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/barkckZ.png" width="80%">
</div>

接下来我们把快指针`fast`移动到链表头，再次让两个指针同时前进，每次都只前进1步。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/Ju6T4IM.png" width="80%">
</div>

当快指针`fast`再次前进了`a`步时，慢指针恰好前进了`a+nb`步，这说明当两个指针再次相遇时的位置就是环的入口。

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/DkORLmU.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/linked-list-cycle-ii/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return head

        fast, slow = head, head

        while True:
            if fast.next is None or fast.next.next is None:
                return None

            fast = fast.next.next
            slow = slow.next

            if fast == slow:
                break
        
        fast = head

        while fast != slow:
            fast = fast.next
            slow = slow.next
        
        return fast
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode *fast = head, *slow = head;

        while (1) {
            if (fast == nullptr || fast->next == nullptr) return nullptr;

            fast = fast->next->next;
            slow = slow->next;

            if (fast == slow) break;
        }

        fast = head;
        while (fast != slow) {
            fast = fast->next;
            slow = slow->next;
        }

        return fast;
    }
};
```


### 234. 回文链表

给你一个单链表的头节点`head`，请你判断该链表是否为回文链表。如果是，返回`true`；否则，返回`false`。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg">
</div>

```
输入：head = [1,2,2,1]
输出：true
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg">
</div>

```
输入：head = [1,2]
输出：false
```

**提示：**

- 链表中节点数目在范围`[1, 10⁵]`内
- 0 <= `Node.val` <= 9

#### Solution

本题的直接解法是将链表的所有值记录到一个列表`vals`中，然后判断`vals`是否是回文。这种解法的时间复杂度和空间复杂度都是`O(n)`。

[题目链接](https://leetcode.cn/problems/palindrome-linked-list/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        vals = []
        node = head

        while node:
            vals.append(node.val)
            node = node.next
        
        i = 0
        j = len(vals) - 1

        while i < j:
            if vals[i] != vals[j]:
                return False
            
            i += 1
            j -= 1
        
        return True
```


C++代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        vector<int> vals;

        auto ptr = head;
        while (ptr != nullptr) {
            vals.emplace_back(ptr->val);
            ptr = ptr->next;
        }

        int i = 0, j = vals.size()-1;
        while (i < j) {
            if (vals[i] != vals[j]) return false;

            ++i;
            --j;
        }

        return true;
    }
};
```


如果要求只能用`O(1)`的空间复杂度来处理则需要使用到双指针。具体来说算法包括以下3个步骤：

1. 找到前半部分链表的尾节点
2. 反转后半部分链表
3. 判断链表是否回文

<div align=center>
<img src="https://images.weserv.nl/?url=i.imgur.com/UcgnaT5.png" width="80%">
</div>

[题目链接](https://leetcode.cn/problems/palindrome-linked-list/)：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if not head.next:
            return True

        fast = slow = head
        pre = None

        while fast and fast.next:
            fast = fast.next.next
            pre = slow
            slow = slow.next
        
        mid = slow
        pre.next = None

        def reverse(head: Optional[ListNode])-> Optional[ListNode]:
            if not head or not head.next:
                return head
            
            pre = None
            cur = head

            while cur:
                tmp = cur
                cur = cur.next

                tmp.next = pre
                pre = tmp
            
            return pre
        
        head1 = head
        head2 = reverse(mid)

        while head1 and head2:
            if head1.val != head2.val:
                return False
            
            head1 = head1.next
            head2 = head2.next
        
        return True
```


### 143. 重排链表

给定一个单链表`L`的头节点`head`，单链表`L`表示为：

```
L₀ → L₁ → … → Lₙ₋₁ → Lₙ
```

请将其重新排列后变为：

```
L₀ → Lₙ → L₁ → Lₙ₋₁ → L₂ → Lₙ₋₂ → …
```

不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例1：**

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/1626420311-PkUiGI-image.png">
</div>

```
输入：head = [1,2,3,4]
输出：[1,4,2,3]
```

**示例2：**

<div align=center>
<img src="https://images.weserv.nl/?url=pic.leetcode-cn.com/1626420320-YUiulT-image.png">
</div>

```
输入：head = [1,2,3,4,5]
输出：[1,5,2,4,3]
```

**提示：**

- 链表的长度范围为`[1, 5 * 10⁴]`
- 1 <= `node.val` <= 1000

#### Solution

本题的整体思路类似于[回文链表](/leetcode/2023-01-09-LinkedList.html#234-回文链表)。如果允许使用额外的存储空间则可以利用一个双向队列来记录链表中的所有节点，然后重新组装即可。

[题目链接](https://leetcode.cn/problems/reorder-list/)：

python代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """

        from collections import deque
        queue = deque([])
        node = head

        while node:
            queue.append(node)
            node = node.next
        
        node = queue.popleft()

        while queue:
            node.next = queue.pop()
            node = node.next

            if queue:
                node.next = queue.popleft()
                node = node.next
        
        node.next = None
```


C++代码

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    void reorderList(ListNode* head) {
        if (head == nullptr) return;

        vector<ListNode*> nodes;
        ListNode* ptr = head;

        while (ptr) {
            nodes.emplace_back(ptr);
            ptr = ptr->next;
        }

        int i = 0, j = nodes.size()-1;
        while (i < j) {
            nodes[i]->next = nodes[j];
            ++i;

            nodes[j]->next = nodes[i];
            --j;
        }

        nodes[i]->next = nullptr;
    }
};
```


如果要求只能用`O(1)`的存储空间则需要使用类似于[回文链表](/leetcode/2023-01-09-LinkedList.html#234-回文链表)的处理方法将链表的后半部分翻转再重新组装。整个算法包括以下3个步骤：

1. 找到前半部分链表的尾节点
2. 反转后半部分链表
3. 重新组装两个链表

[题目链接](https://leetcode.cn/problems/reorder-list/)：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        if not head.next:
            return

        fast = slow = head
        pre = None

        while fast and fast.next:
            fast = fast.next.next
            pre  = slow
            slow = slow.next
        
        def reverse(head: Optional[ListNode]) -> Optional[ListNode]:
            if not head or not head.next:
                return head
            
            pre = None
            cur = head

            while cur:
                tmp = cur
                cur = cur.next

                tmp.next = pre
                pre = tmp

            return pre
        
        pre.next = None
        right = reverse(slow)
        left = head

        while left:
            node1 = left
            node2 = right

            left = left.next
            right= right.next

            node1.next = node2
            if left:
                node2.next = left
```


## Reference

- [链表理论基础](https://programmercarl.com/%E9%93%BE%E8%A1%A8%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html)
- [LeetCode：203.移除链表元素](https://www.bilibili.com/video/BV18B4y1s7R9/?vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：707.设计链表](https://www.bilibili.com/video/BV1FU4y1X7WD/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：206.反转链表](https://www.bilibili.com/video/BV1nB4y1i7eL/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：24.两两交换链表中的节点](https://www.bilibili.com/video/BV1YT411g7br/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：19.删除链表倒数第N个节点](https://www.bilibili.com/video/BV1vW4y1U7Gf/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)
- [LeetCode：142.环形链表II](https://www.bilibili.com/video/BV1if4y1d7ob/?spm_id_from=333.788&vd_source=7a2542c6c909b3ee1fab551277360826)