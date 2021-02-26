---
title: Leetcode129. 求根到叶子节点数字之和
tags:
  - 递归
  - 树
categories:
  - 算法
abbrlink: d6166674
date: 2020-08-26 12:47:49
---

**使用[递归分析模板](./分而治之和递归的区别.md)将会十分清晰**

**转载自：[Leetcode题解（力扣官方题解），略有增删](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/solution/er-cha-shu-zhong-de-zui-da-lu-jing-he-by-leetcode-/)**

<!-- more -->

1. 状态：树节点(变化量)，用root表示；recursion(root)表示以 `root` 为根节点的到叶子结点的数字之和。但是本题有点特殊，因为一个节点的值是和它的父亲节点是有关系的，所以还需要传递一个父亲节点的值，即递归函数的参数有2个`recursion(root, i)`。**但是一般来说是有几个状态函数参数就有几个的**

2. 递归终止条件：

   ```c++
   // 空节点的数字和为0
   if (root == null) return 0;
   ```

   **用动态规划的角度表示最小子问题为：`dp[null] = 0`**

3. 状态转移方程(可能涉及到多次if条件判断)：

   ```java
   // 一个节点的值是和它的父亲节点是有关系的
   int res = i * 10 + root.val;
   if (root.left == null && root.right == null)//2、节点为叶子节点
     return res;
   return DFS(root.left, res) + DFS(root.right, res);//3、节点为非叶子节点
   ```

   **用动态规划的角度表示转移方程为：**

   ```java
   int res = i * 10 + root.val;
   if (root.left == null && root.right == null){
   	dp[root] = res;  
   }
   dp[root] = dp[root.left] + dp[root.right];
   ```

4. 返回最终状态：return recursion(n, 0)；

   **用动态规划的角度表示最终返回值为：`return dp[root]`**

**完整代码**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int sumNumbers(TreeNode root) {
        return DFS(root, 0);
    }

    private int DFS(TreeNode root, int i) {
        if (root == null) return 0;//1、节点为空
        int res = i * 10 + root.val;
        if (root.left == null && root.right == null)//2、节点为叶子节点
            return res;
        return DFS(root.left, res) + DFS(root.right, res);//3、节点为非叶子节点
    }
}
```

