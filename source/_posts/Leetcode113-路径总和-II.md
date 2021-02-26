---
title: Leetcode113. 路径总和 II
tags:
  - 递归
  - 树
categories:
  - 算法
abbrlink: a876447c
date: 2020-08-26 09:24:50
---

**使用[递归分析模板](./分而治之和递归的区别.md)将会十分清晰**

**与[Leetcode112-路径总和](./Leetcode113-路径总和-II.md)的思想一致**

**转载自：[Leetcode题解（liweiwei1419），略有增删](https://leetcode-cn.com/problems/path-sum-ii/solution/hui-su-suan-fa-shen-du-you-xian-bian-li-zhuang-tai/)**

<!-- more -->

1. 状态：树节点(变化量)，用root表示；recursion(root)表示从 `root` 到叶子节点是否存在路径和为 `sum` 的路径 ，并且把路径打印出来

2. 递归终止条件：

   ```c++
   // 若为空节点，不管sum为多少，表示无路可走，自然什么也不干
   if (root == null) {
     return ; 
   }
   ```

3. 状态转移方程(可能涉及到多次if条件判断)：

   ```c++
   path.push_back(root->val);
   if(root->left == NULL && root->right == NULL && root->val == sum){
     res.push_back(path);
   }
   dfs(root->left, sum - root->val, path);
   dfs(root->right, sum - root->val, path);
   ```

   **用动态规划的角度表示转移方程为：**

   ```c++
   path.push_back(root->val);
   if(root->left == NULL && root->right == NULL && root->val == sum){
     res.push_back(path);
   }
   dp[root] = dp[root.left][sum - root.val] = 对path进行操作 
   					+ dp[root.right][sum - root.val] = 对path进行操作
   ```

4. 返回最终状态：return res；

**完整代码**

```c++
class Solution {
private:
    vector<vector<int>> res;
    void dfs(TreeNode* root, int sum, vector<int> path){
        if(root == NULL){
            return;
        }
        path.push_back(root->val);
        if(root->left == NULL && root->right == NULL && root->val == sum){
            res.push_back(path);
        }
        dfs(root->left, sum - root->val, path);
        dfs(root->right, sum - root->val, path);
    }
public:
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<int> path;
        dfs(root, sum, path);
        return res;
    }
};
```

