---
title: Leetcode226. 翻转二叉树
tags:
  - BFS
  - DFS
  - 树
categories:
  - 算法
abbrlink: 62dc7c8b
date: 2020-09-04 20:47:13
---

**典型的DFS和BFS都能做的题目**

[DFS模板参考](./分而治之和递归的区别.md)和[BFS模板参考](./BFS问题模板.md)

<!-- more -->

#### 方法一：DFS

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }
    TreeNode right = invertTree(root.right);
    TreeNode left = invertTree(root.left);
    root.left = right;
    root.right = left;
    return root;
}
```

#### 方法二：BFS

**翻转二叉树是从上到下，从左到右的翻转，这就是层序遍历的流程**

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) return null;
    Queue<TreeNode> queue = new LinkedList<TreeNode>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode current = queue.poll();
        TreeNode temp = current.left;
        current.left = current.right;
        current.right = temp;
        if (current.left != null) queue.add(current.left);
        if (current.right != null) queue.add(current.right);
    }
    return root;
}
```

