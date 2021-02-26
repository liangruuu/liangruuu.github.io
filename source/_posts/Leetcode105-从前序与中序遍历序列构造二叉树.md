---
title: Leetcode105. 从前序与中序遍历序列构造二叉树
tags:
  - 分而治之
  - 树
categories:
  - 算法
abbrlink: 5df2e57e
date: 2020-08-26 07:56:28
---

**转载自：[Leetcode题解（王尼玛），略有增删](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/solution/dong-hua-yan-shi-105-cong-qian-xu-yu-zhong-xu-bian/)**

<!-- more -->

### 一、让我们尝试用递归模板来解题

[分而治之和递归的区别](./分而治之和递归的区别.md)

1. 状态：前序和中序数组的下标(变化量)，用pre_i和in_i表示，**recursion(pre_i, in_i)表示构造以in_i为根节点的二叉树**。只不过需要注意的是**分而治之**的思想一般用两个变量`left和right`来表示数组下标，并且递归函数需要有原数组作为参数，所以递归函数更改为**(int[] preorder, int p_start, int p_end, int[] inorder, int i_start, int i_end)**
2. 递归终止条件：

```java
if (p_start > p_end) {
  return null;
}
```

3. 状态转移方程：

```java
//在中序遍历中找到根节点的位置
int i_root_index = 0;
for (int i = i_start; i < i_end; i++) {
  if (root_val == inorder[i]) {
    i_root_index = i;
    break;
  }
}
int leftNum = i_root_index - i_start;
//==============上面的代码只是为了找到中序数组中的根节点下标，你需要明白用前序和中序构造二叉树的原理

//递归的构造左子树
root.left = buildTreeHelper(preorder, p_start + 1, p_start + leftNum + 1, inorder, i_start,
                            i_root_index);
//递归的构造右子树
root.right = buildTreeHelper(preorder, p_start + leftNum + 1, p_end, inorder, i_root_index + 1, i_end);
```

**如果按照动态规划角度来看**

```java
dp[root] = dp[root.left] + dp[root.right]
```

4. 返回最终状态：`return recursion(root);`

**完整代码**

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return buildTreeHelper(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
}

private TreeNode buildTreeHelper(int[] preorder, int p_start, int p_end, int[] inorder, int i_start, int i_end) {
    // preorder 为空，直接返回 null
    if (p_start > p_end) {
        return null;
    }
    int root_val = preorder[p_start];
    TreeNode root = new TreeNode(root_val);
    //在中序遍历中找到根节点的位置
    int i_root_index = 0;
    for (int i = i_start; i < i_end; i++) {
        if (root_val == inorder[i]) {
            i_root_index = i;
            break;
        }
    }
    int leftNum = i_root_index - i_start;
    //递归的构造左子树
    root.left = buildTreeHelper(preorder, p_start + 1, p_start + leftNum + 1, inorder, i_start, i_root_index);
    //递归的构造右子树
    root.right = buildTreeHelper(preorder, p_start + leftNum + 1, p_end, inorder, i_root_index + 1, i_end);
    return root;
}
```

