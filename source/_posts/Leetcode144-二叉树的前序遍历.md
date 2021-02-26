---
title: Leetcode144. 二叉树的前序遍历
tags:
  - 栈
categories:
  - 算法
abbrlink: 3ac91e5e
date: 2020-08-24 18:55:35
---

**转载自：[Leetcode题解（力扣 (LeetCode)），略有增删](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/er-cha-shu-de-qian-xu-bian-li-by-leetcode/)**

**栈模板[使用栈解题思想](./使用栈解题思想.md)**

<!-- more -->

先从使用栈的几个前提条件出发：

1. 是否涉及到**比较**：没涉及到比较
2. 是否涉及到**延迟比较**：前序遍历的核心就是**中左右**，先遍历根节点——>再遍历左节点——>再遍历右节点，那么右节点就需要被保存，这就是**延迟**。虽然题目中没有涉及到比较操作，但是涉及到延迟，也可以使用栈来辅助实现中序遍历

**我们根据栈模板中提到的`必写代码`来分析如下答案**

```java
class Solution {
  public List<Integer> preorderTraversal(TreeNode root) {
    LinkedList<TreeNode> stack = new LinkedList<>();
    LinkedList<Integer> output = new LinkedList<>();
    if (root == null) {
      return output;
    }

    stack.add(root);
    // 1. 必写代码之二：对栈的判空，不管是用while还是if
    while (!stack.isEmpty()) {
      // 2. 必写代码之四：出栈
      TreeNode node = stack.pollLast();
      // 3. 必写代码之三：取出栈顶元素
      output.add(node.val);
      if (node.right != null) {
        // 4. 必写代码之五：入栈
        stack.add(node.right);
      }
      // 5. 必写代码之一：对原始数据的循环遍历，对于树这种数据结构来说访问左右子节点就是在循环
      if (node.left != null) {
        stack.add(node.left);
      }
    }
    return output;
  }
}
```

