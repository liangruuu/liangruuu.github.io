---
title: Leetcode404. 左叶子之和
tags:
  - DFS
  - 树
categories:
  - 算法
abbrlink: 23310d0f
date: 2020-09-04 22:08:52
---

[DFS模板参考](./分而治之和递归的区别.md)

<!-- more -->

本题需要计算二叉树所有左叶子之和，只需在遍历的过程中判断是否到达了左叶子，在这里到达左叶子的判断标准为：

1. 是否是当前节点的左孩子
2. 当前节点的左孩子是不是叶子节点（叶子结点：没有左右孩子）

```c++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if(root==NULL)return 0;
        int ans=0;
        if(root->left!=NULL){
            if(root->left->left==NULL&&root->left->right==NULL)ans+=root->left->val;
            else ans+=sumOfLeftLeaves(root->left);
        }
        if(root->right!=NULL){
            ans+=sumOfLeftLeaves(root->right);
        }
        return ans;
    }
};
```



