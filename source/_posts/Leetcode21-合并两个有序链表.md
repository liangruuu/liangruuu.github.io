---
title: Leetcode21. 合并两个有序链表
tags:
  - 链表
  - 递归
categories:
  - 算法
abbrlink: f818133b
date: 2020-09-03 13:18:48
---

**转载自：[Leetcode题解（力扣官方题解），略有增删](https://leetcode-cn.com/problems/merge-two-sorted-lists/solution/he-bing-liang-ge-you-xu-lian-biao-by-leetcode-solu/)**

[请结合该文处理链表题目的方法分析链表解题思路](./链表解题思路.md)，**该题也是合并K个有序链表的题根**

<!-- more -->

#### 一、递归

如果 l1 或者 l2 一开始就是空链表 ，那么没有任何操作需要合并，所以我们只需要返回非空链表。否则，我们要判断 l1 和 l2 哪一个链表的头节点的值更小，然后递归地决定下一个添加到结果里的节点。如果两个链表有一个为空，递归结束。

1. 状态：两个原始列表的节点，用l1和l2表示，recursion(l1, l2)表示两个链表前l1和l2个元素能够组成的递增链表

2. 递归终止条件：

   ```c++
   if (l1 == nullptr) {
     return l2;
   } else if (l2 == nullptr) {
     return l1;
   ```

3. 状态转移方程(可能涉及到多次if条件判断)：

   ```c++
   if (l1->val < l2->val) {
     l1->next = mergeTwoLists(l1->next, l2);
     return l1;
   } else {
     l2->next = mergeTwoLists(l1, l2->next);
     return l2;
   }
   ```

4. 返回最终状态：return recursion(l1, l2)；

**完整代码**

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == nullptr) {
            return l2;
        } else if (l2 == nullptr) {
            return l1;
        } else if (l1->val < l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        } else {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```

#### 二、迭代

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummyHead = new ListNode(-1);

        ListNode* prev = dummyHead;
        while (l1 != nullptr && l2 != nullptr) {
            if (l1->val < l2->val) {
                prev->next = l1;
                l1 = l1->next;
            } else {
                prev->next = l2;
                l2 = l2->next;
            }
            prev = prev->next;
        }

        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev->next = l1 == nullptr ? l2 : l1;

        return preHead->next;
    }
};
```

