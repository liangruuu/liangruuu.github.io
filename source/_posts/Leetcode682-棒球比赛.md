---
title: Leetcode682. 棒球比赛
tags:
  - 栈
categories:
  - 算法
abbrlink: 278fc5d
date: 2020-08-24 22:40:33
---

**转载自：[Leetcode题解（程序员吴师兄），略有增删](https://leetcode-cn.com/problems/daily-temperatures/solution/leetcode-tu-jie-739mei-ri-wen-du-by-misterbooo/)**

**栈模板[使用栈解题思想](./使用栈解题思想.md)**

<!-- more -->

先从使用栈的几个前提条件出发：

1. 是否涉及到**比较**：没有
2. 是否涉及到**延迟比较**：涉及到保存分数

**注：该题是使用栈的一个经典题型，但不是使用单调栈，两者区别详见栈解题思想**

```java
class Solution {
    public int calPoints(String[] ops) {
        Stack<Integer> stack = new Stack();

        for(String op : ops) {
            if (op.equals("+")) {
                int top = stack.pop();
                int newtop = top + stack.peek();
                stack.push(top);
                stack.push(newtop);
            } else if (op.equals("C")) {
                stack.pop();
            } else if (op.equals("D")) {
                stack.push(2 * stack.peek());
            } else {
                stack.push(Integer.valueOf(op));
            }
        }

        int ans = 0;
        for(int score : stack) ans += score;
        return ans;
    }
}
```

