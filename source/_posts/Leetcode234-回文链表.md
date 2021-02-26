---
title: Leetcode234. 回文链表
tags:
  - 链表
categories:
  - 算法
abbrlink: 18779b66
date: 2020-09-03 19:21:58
---

**转载自：[Leetcode题解（hello_pretty），略有增删](https://leetcode-cn.com/problems/palindrome-linked-list/solution/hui-wen-lian-biao-1zhan-2kuai-man-zhi-zhen-fan-zhu/)**

[请结合该文处理链表题目的方法分析链表解题思路](./链表解题思路.md)，**此题是快慢指针题的一种，即使用快慢指针平分链表**

<!-- more -->

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(!head || !head->next)
            return 1;
        ListNode *fast = head, *slow = head;
        ListNode *p, *pre = NULL;
        while(fast && fast->next){
            p = slow;
            slow = slow->next;    //快慢遍历
            fast = fast->next->next;

            p->next = pre;  //翻转
            pre = p;
        }
        if(fast)  //奇数个节点时跳过中间节点
            slow = slow->next;

        while(p){       //前半部分和后半部分比较
            if(p->val != slow->val)
                return 0;
            p = p->next;
            slow = slow->next;
        }
        return 1;
    }
};
```

