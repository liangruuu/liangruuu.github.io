---
title: Leetcode94. 二叉树的中序遍历
tags:
  - 栈
  - 树
  - DFS
categories:
  - 算法
abbrlink: 6bf51b4b
date: 2020-08-24 11:24:57
---

**转载自：[Leetcode题解（力扣 (LeetCode)），略有增删](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/er-cha-shu-de-zhong-xu-bian-li-by-leetcode/)**

**栈模板[使用栈解题思想](./使用栈解题思想.md)**

<!-- more -->

#### 方法一：递归

```java
class Solution {
    public List < Integer > inorderTraversal(TreeNode root) {
        List < Integer > res = new ArrayList < > ();
        helper(root, res);
        return res;
    }

    public void helper(TreeNode root, List < Integer > res) {
        if (root != null) {
            if (root.left != null) {
                helper(root.left, res);
            }
            res.add(root.val);
            if (root.right != null) {
                helper(root.right, res);
            }
        }
    }
}
```

#### 方法二：基于栈的遍历

先从使用栈的几个前提条件出发：

1. 是否涉及到**比较**：没涉及到比较
2. 是否涉及到**延迟比较**：中序遍历的核心就是**左中右**，先遍历左节点——>再遍历根节点——>再遍历右节点，那么中间的根节点就需要被保存，这就是**延迟**。虽然题目中没有涉及到比较操作，但是涉及到延迟，也可以使用栈来辅助实现中序遍历

**我们根据栈模板中提到的`必写代码`来分析如下答案**

```java
public class Solution {
    public List < Integer > inorderTraversal(TreeNode root) {
        List < Integer > res = new ArrayList < > ();
        Stack < TreeNode > stack = new Stack < > ();
        TreeNode curr = root;
      	// 1. 必写代码之一：对原始数据的循环遍历
      	// 2. 必写代码之二：对栈的判空，不管是用while还是if
        while (curr != null || !stack.isEmpty()) {
          	// 外层如果是"或"循环，在内层需要分别对各个"或条件"进行单独判断
            while (curr != null) {
              	// 3. 必写代码之五：入栈
                stack.push(curr);
                curr = curr.left;
            }
          	// 4. 必写代码之三：出栈
            curr = stack.pop();
          	// 其实这行代码本质上就是获取栈顶元素
            res.add(curr.val);
            curr = curr.right;
        }
        return res;
    }
}
```

