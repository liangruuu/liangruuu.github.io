---
title: Leetcode143. 重排链表
tags:
  - 链表
categories:
  - 算法
abbrlink: dad28f1c
date: 2020-09-03 20:12:11
---

**转载自：[Leetcode题解（用力 扣不动），略有增删](https://leetcode-cn.com/problems/reorder-list/solution/2019tong-kao-408zhen-ti-jiao-ke-shu-shi-da-an-by-m/)**

[请结合该文处理链表题目的方法分析链表解题思路](./链表解题思路.md)，**此题是快慢指针题的一种，即使用快慢指针平分链表**

```c++
class Solution {
public:
    void reorderList(ListNode* head) {
        ListNode* p=head,*q=head,*r,*s=head;
        if(!head)            //head为空，则直接退出
            return ;         
        while(q->next){      //寻找中间结点
            q=q->next;       //p走一步
            p=p->next;
            if(q->next)
              q=q->next;     //q走两步
        }
      
      	// 根据链表模板里的说法我只需要后半部分的第一个节点
      	// 并且循环结束后，若链表节点个数为奇数，则slow为前后半之间的那个中间元素，若个数时偶数，则slow为前半部分最后一个节点
        q=p->next;           //p所指结点为中间结点，q为后半段链表的首结点
        p->next=nullptr;
        while(q){            //将链表后半段逆置
            r=q->next;
            q->next=p->next;
            p->next=q;
            q=r;
        }
        q=p->next;            //q指向后半段的第一个数据结点
        p->next=nullptr;
        while(q){             //将链表后半段的结点插入到指定位置
            r=q->next;        //r指向后半段的下一个结点
            q->next=s->next;  //将q所指结点插入到s所指结点（head结点）之后
            s->next=q;        
            s=q->next;        //s指向前半段的下一个插入点
            q=r;
        }
    }
};
```

