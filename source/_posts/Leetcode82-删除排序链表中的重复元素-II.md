---
title: Leetcode82. 删除排序链表中的重复元素 II
tags:
  - 链表
  - 快慢指针
categories:
  - 算法
abbrlink: 49aa0bfb
date: 2020-09-03 15:38:13
---

**转载自：[Leetcode题解（keithy），略有增删](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/solution/java-ya-jie-dian-fei-di-gui-rong-yi-li-jie-yong-sh/)**，请结合该文处理链表题目的[方法分析链表解题思路](./链表解题思路.md)

<!-- more -->

1. 与链表的其他题目类似，为了防止删除头结点的极端情况发生，先创建空结点dummy，使dummy指向传入的head结点。然后创建cur的指针，指向链表的头部(即dummy)，这都是模板里提供的方法。
2. 为了删除值相同的节点，需要在遍历链表的循环里再嵌套一层循环
3. 注意循环条件

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
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = dummy;
        while (cur.next != null && cur.next.next != null) {
            if (cur.next.val == cur.next.next.val) {
                ListNode temp = cur.next;
                while (temp != null && temp.next != null && temp.val == temp.next.val ) {
                    temp = temp.next;
                }
                cur.next = temp.next;
            } 
            else
                cur = cur.next;
        }
        return dummy.next;
    }
}
```

