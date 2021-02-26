---
title: Leetcode445. 两数相加 II
tags:
  - 链表
categories:
  - 算法
abbrlink: 49af10aa
date: 2020-09-03 20:54:34
---

**转载自：[Leetcode题解（Sweetiee），略有增删](https://leetcode-cn.com/problems/add-two-numbers-ii/solution/javakai-fa-by-sweetiee/)**

[请结合该文处理链表题目的方法分析链表解题思路](./链表解题思路.md)，逆序处理，似乎一般都会用到栈，一个小tips

<!-- more -->

用 stack 保存链表，再从 stack 中取出来，就是数字从低位到高位访问了。

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) { 
        Stack<Integer> stack1 = new Stack<>();
        Stack<Integer> stack2 = new Stack<>();
        while (l1 != null) {
            stack1.push(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            stack2.push(l2.val);
            l2 = l2.next;
        }
        
        int carry = 0;
        ListNode dummyHead = new ListNode(-1);
        while (!stack1.isEmpty() || !stack2.isEmpty() || carry > 0) {
            int sum = carry;
            sum += stack1.isEmpty()? 0: stack1.pop();
            sum += stack2.isEmpty()? 0: stack2.pop();
            ListNode node = new ListNode(sum % 10);
            node.next = dummyHead.next;
            dummyHead.next = node;
            carry = sum / 10;
        }
        return dummyHead.next;
    }
}
```

