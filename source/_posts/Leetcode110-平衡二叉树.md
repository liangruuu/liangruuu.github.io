---
title: Leetcode110. 平衡二叉树
tags:
  - 递归
  - 树
categories:
  - 算法
abbrlink: 3b0aff9b
date: 2020-08-25 21:45:19
---

**使用[递归分析模板](./分而治之和递归的区别.md)将会十分清晰**

<!-- more -->

1. 状态：树节点(变化量)，用root表示；recursion(root)表示以该root节点为根节点的树是否是平衡二叉树
2. 递归终止条件：空节点就是一个平衡二叉树

```c++
if(root == null){
  return true;
}
```

3. 状态转移方程

```java
Math.abs(depth(root.left) - depth(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
```

**如果用动态规划的转移方程来写，大概是**

```java
dp[root] = Math.abs(depth(root.left) - depth(root.right)) <= 1 && dp[root.left] && dp[root.right]
```

4. 返回最终状态：return recursion(root);

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        return Math.abs(depth(root.left) - depth(root.right)) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }

    private int depth(TreeNode root) {
        if (root == null) return 0;
        return Math.max(depth(root.left), depth(root.right)) + 1;
    }
}
```

**计算树高度的depth()函数其实也按照这种模板**

1. 状态：树节点(变化量)，用root表示；recursion(root)表示以该root节点为根节点的树的高度
2. 递归终止条件：空节点的树高为0

```java
if(root == null){
  return 0;
}
```

3. 状态转移方程

```java
Math.max(depth(root.left), depth(root.right)) + 1;
```

**如果用动态规划的转移方程来写，大概是**

```java
dp[root] = max(dp[root.left] + dp[root.right]) + 1 
```

4. 返回最终状态：return depth(root);