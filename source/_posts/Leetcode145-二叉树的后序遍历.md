---
title: Leetcode145. 二叉树的后序遍历
tags:
  - 栈
categories:
  - 算法
abbrlink: 6ac94452
date: 2020-08-24 19:01:15
---

**转载自：[Leetcode题解（力扣 (LeetCode)），略有增删](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-by-leetcode/)**

**栈模板[使用栈解题思想](./使用栈解题思想.md)**

<!-- more -->

先从使用栈的几个前提条件出发：

1. 是否涉及到**比较**：没涉及到比较
2. 是否涉及到**延迟比较**：后序遍历的核心就是**左右中**，先遍历左节点——>再遍历右节点——>再遍历根节点，那么根节点就需要被保存，这就是**延迟**。虽然题目中没有涉及到比较操作，但是涉及到延迟，也可以使用栈来辅助实现中序遍历

**我们根据栈模板中提到的`必写代码`来分析如下答案**

```java
class Solution {
  public List<Integer> postorderTraversal(TreeNode root) {
    LinkedList<TreeNode> stack = new LinkedList<>();
    LinkedList<Integer> output = new LinkedList<>();
    if (root == null) {
      return output;
    }

    stack.add(root);
    while (!stack.isEmpty()) {
      TreeNode node = stack.pollLast();
      // 注意这里是addFirst，即在数组首部插入元素
      output.addFirst(node.val);
      if (node.left != null) {
        stack.add(node.left);
      }
      if (node.right != null) {
        stack.add(node.right);
      }
    }
    return output;
  }
}
```

