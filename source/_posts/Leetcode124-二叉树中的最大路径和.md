---
title: Leetcode124. 二叉树中的最大路径和
tags:
  - 递归
  - 树
categories:
  - 算法
abbrlink: 7545a421
date: 2020-08-26 10:53:22
---

**使用[递归分析模板](./分而治之和递归的区别.md)将会十分清晰**

**转载自：[Leetcode题解（力扣官方题解），略有增删](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/solution/er-cha-shu-zhong-de-zui-da-lu-jing-he-by-leetcode-/)**

<!-- more -->

1. 状态：树节点(变化量)，用root表示；recursion(root)表示以 `root` 为根节点的最大路径和

2. 递归终止条件：

   ```c++
   // 空节点是没有值的
   if (node == null) {
     return 0;
   }
   ```

   **用动态规划的角度表示最小子问题为：**`dp[null] = 0`

3. 状态转移方程(可能涉及到多次if条件判断)：

   非空节点的最大贡献值等于节点值与其子节点中的最大贡献值之和（对于叶节点而言，最大贡献值等于节点值）。	

   例如，考虑如下二叉树。

   ```c++
      -10
      / \
     9  20
       /  \
      15   7
   ```

   叶节点 9、15、7 的最大贡献值分别为 9、15、7。

   得到叶节点的最大贡献值之后，再计算非叶节点的最大贡献值。节点 2020 的最大贡献值等于 20 + max(15, 7) = 3520 + max(15,7) = 35，节点 -10−10 的最大贡献值等于 -10 + max(9, 35) = 25 − 10 + max(9, 35) = 25。

   上述计算过程是递归的过程，因此，对根节点调用函数 `maxGain`，即可得到每个节点的最大贡献值。

   根据函数 `maxGain` 得到每个节点的最大贡献值之后，如何得到二叉树的最大路径和？对于二叉树中的一个节点，该节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值，如果子节点的最大贡献值为正，则计入该节点的最大路径和，否则不计入该节点的最大路径和。维护一个全局变量 `maxSum `存储最大路径和，在递归过程中更新 `maxSum `的值，最后得到的 `maxSum `的值即为二叉树中的最大路径和。

   ```c++
   // 递归计算左右子节点的最大贡献值
   // 只有在最大贡献值大于 0 时，才会选取对应子节点
   int leftGain = max(maxGain(node->left), 0);
   int rightGain = max(maxGain(node->right), 0);
   
   
   // 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值
   int priceNewpath = node->val + leftGain + rightGain;
   
   // 更新答案
   maxSum = max(maxSum, priceNewpath);
   
   // 返回节点的最大贡献值
   return node->val + max(leftGain, rightGain);
   ```

   **用动态规划的角度表示转移方程为：**

   ```c++
   dp[root] = root.val + Math.max(dp[root.left], dp[root.right])
   ```

4. 返回最终状态：return recursion(root)；

   **用动态规划的角度表示最终返回值为：`return dp[root]`**

**完整代码**

```c++
class Solution {
private:
    int maxSum = INT_MIN;

public:
    int maxGain(TreeNode* node) {
        if (node == nullptr) {
            return 0;
        }
        
        // 递归计算左右子节点的最大贡献值
        // 只有在最大贡献值大于 0 时，才会选取对应子节点
        int leftGain = max(maxGain(node->left), 0);
        int rightGain = max(maxGain(node->right), 0);

        // 节点的最大路径和取决于该节点的值与该节点的左右子节点的最大贡献值
        int priceNewpath = node->val + leftGain + rightGain;

        // 更新答案
        maxSum = max(maxSum, priceNewpath);

        // 返回节点的最大贡献值
        return node->val + max(leftGain, rightGain);
    }

    int maxPathSum(TreeNode* root) {
        maxGain(root);
        return maxSum;
    }
};
```

