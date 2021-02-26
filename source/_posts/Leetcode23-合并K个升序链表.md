---
title: Leetcode23. 合并K个升序链表
tags:
  - 链表
categories:
  - 算法
abbrlink: f6c5761a
date: 2020-09-03 13:46:10
---

**转载自：[Leetcode题解（力扣官方题解），略有增删](https://leetcode-cn.com/problems/merge-k-sorted-lists/solution/he-bing-kge-pai-xu-lian-biao-by-leetcode-solutio-2/)**

[请结合该文处理链表题目的方法分析链表解题思路](./链表解题思路.md)，**该题也是[合并两个有序链表](./Leetcode21-合并两个有序链表.md)的拓展**

<!-- more -->

#### 一、顺序合并

先考虑合并三个有序链表，假设有这样的三个链表[L1, L2, L3]，如果以合并两个有序链表为原始问题的话，那么先把前两个链表两两合并生成L12，再把生成的新链表和L3链表合并生成L123。发现规律了吗？**其实合并K个链表就是进行K - 1次两两合并**，那么我么只需要对原始链表数组进行遍历合并即可，两两合并操作参考[合并两个有序链表](./Leetcode21-合并两个有序链表.md)

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

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode *ans = nullptr;
        for (size_t i = 0; i < lists.size(); ++i) {
            ans = mergeTwoLists(ans, lists[i]);
        }
        return ans;
    }
};
```

#### 二、分治合并

连续进行K - 1次两两合并很容易能联想到**分治算法**

![img](./Leetcode23-合并K个升序链表/6f70a6649d2192cf32af68500915d84b476aa34ec899f98766c038fc9cc54662-image.png)

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

        prev->next = l1 == nullptr ? l2 : l1;

        return preHead->next;
    }

    ListNode* merge(vector <ListNode*> &lists, int l, int r) {
        if (l == r) return lists[l];
        if (l > r) return nullptr;
        int mid = (l + r) / 2;
        return mergeTwoLists(merge(lists, l, mid), merge(lists, mid + 1, r));
    }

    ListNode* mergeKLists(vector<ListNode*>& lists) {
        return merge(lists, 0, lists.size() - 1);
    }
};
```

