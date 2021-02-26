---
title: Leetcode257. 二叉树的所有路径
tags:
  - DFS
  - 树
categories:
  - 算法
abbrlink: 148a306b
date: 2020-09-04 21:50:58
---

**按照DFS模板分析**

<!-- more -->

```c++
class Solution {
		public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        dfs(root, "", res);
        return res;
    }

    private void dfs(TreeNode root, String path, List<String> res) {
        //如果为空，直接返回
        if (root == null)
            return;
        //如果是叶子节点，说明找到了一条路径，把它加入到res中
        if (root.left == null && root.right == null) {
            res.add(path + root.val);
            return;
        }
        //如果不是叶子节点，在分别遍历他的左右子节点
        dfs(root.left, path + root.val + "->", res);
        dfs(root.right, path + root.val + "->", res);
    }
}
```

