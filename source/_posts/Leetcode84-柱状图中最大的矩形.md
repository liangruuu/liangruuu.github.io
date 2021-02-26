---
title: Leetcode84. 柱状图中最大的矩形
tags:
  - 栈
categories:
  - 算法
abbrlink: '25209534'
date: 2020-08-24 19:57:30
---

**转载自：[Leetcode题解（Sweetiee），略有增删](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/xiang-jie-dan-diao-zhan-bi-xu-miao-dong-by-sweetie/)**

**栈模板[使用栈解题思想](./使用栈解题思想.md)**

<!-- more -->

先从使用栈的几个前提条件出发：

1. 是否涉及到**比较**：比较长和宽乘积的大小
2. 是否涉及到**延迟比较**：对于当前柱子的高度，之后可能出现更高的柱子，导致长和宽都增大从而使可勾勒面积增大，所以需要**保存**当前柱子高度以便之后运算比较

思路分析
我们可以遍历每根柱子，以当前柱子 i 的高度作为矩形的高，那么矩形的宽度边界即为向左找到第一个高度小于当前柱体 i 的柱体，向右找到第一个高度小于当前柱体 i 的柱体。

对于每个柱子我们都如上计算一遍以当前柱子作为高的矩形面积，最终比较出最大的矩形面积即可。

```java
// 跟归纳总结过的使用栈当辅助结构的模思想板高度一致
class Solution {
    public int largestRectangleArea(int[] heights) {
        // 这里为了代码简便，在柱体数组的头和尾加了两个高度为 0 的柱体。
        int[] tmp = new int[heights.length + 2];
      	// 这里的技巧是给数组左右两端添值为0，以进行普遍操作，就像给链表添加头节点一样
        System.arraycopy(heights, 0, tmp, 1, heights.length); 
        
        Stack<Integer> stack = new Stack<>();
        int area = 0;
        for (int i = 0; i < tmp.length; i++) {
            // 如果当前数组元素值大于或等于栈顶元素值
          	// 说明柱子高度还在增加，从而勾勒的面积可能继续增大，所以不进入while处理
          	// 以此说明while循环的其中一个条件是当前处理元素小于栈顶元素
            while (!stack.isEmpty() && tmp[i] < tmp[stack.peek()]) {
                int h = tmp[stack.pop()];
                area = Math.max(area, (i - stack.peek() - 1) * h);   
            }
            stack.push(i);
        }

        return area;
    }
}
```

