---
title: Leetcode112. 路径总和
tags:
  - 递归
  - 树
categories:
  - 算法
abbrlink: '78212637'
date: 2020-08-26 08:51:01
---

**使用[递归分析模板](./分而治之和递归的区别.md)将会十分清晰**

**转载自：[Leetcode题解（sweetiee），略有增删](https://leetcode-cn.com/problems/path-sum/solution/jian-dan-di-gui-bi-xu-miao-dong-by-sweetiee-2/)**

<!-- more -->

1. 状态：树节点(变化量)，用root表示；recursion(root)表示从 `root` 到叶子节点是否存在路径和为 `sum` 的路径 `hasPathSum(root, sum)`

2. 递归终止条件：

   ```java
   // 若为空节点，不管sum为多少都是false
   if (root == null) {
     return false;
   }
   ```

   **用动态规划的角度表示最小子问题为：`dp[0][sum] = false`**

3. 状态转移方程(可能涉及到多次if条件判断)：

   ```java
   // 到达叶子节点时，递归终止，判断 sum 是否符合条件。
   if (root.left == null && root.right == null) {
     return root.val == sum;
   }
   // 递归地判断root节点的左孩子和右孩子。
   return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
   ```

   **用动态规划的角度表示转移方程为：**

   ```java
   if (root.left == null && root.right == null) {
     dp[root][sum] = root.val == sum;
   } else {
     dp[root][sum] = dp[root.left][sum - root.val] || dp[root.right][sum - root.val]
   }
   ```

4. 返回最终状态：return recursion(root, sum)；

   **用动态规划的角度表示最终返回值为：`return dp[n][sum]`**

**完整代码**

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        // 到达叶子节点时，递归终止，判断 sum 是否符合条件。
        if (root.left == null && root.right == null) {
            return root.val == sum;
        }
        // 递归地判断root节点的左孩子和右孩子。
        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
    }
}
```

