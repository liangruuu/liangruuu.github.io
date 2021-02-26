---
title: Leetcode108. 将有序数组转换为二叉搜索树
tags:
  - 分而治之
  - 树
categories:
  - 算法
abbrlink: 4372fe16
date: 2020-08-25 19:11:20
---

**转载自：[Leetcode题解（sweetiee），略有增删](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/solution/jian-dan-di-gui-bi-xu-miao-dong-by-sweetiee/)**

<!-- more -->

### 一、题目分析
**题意**：根据升序数组，恢复一棵高度平衡的BST🌲。

**分析**：BST的中序遍历是升序的，因此本题等同于根据中序遍历的序列恢复二叉搜索树。因此我们可以以升序序列中的任一个元素作为根节点，以该元素左边的升序序列构建左子树，以该元素右边的升序序列构建右子树，这样得到的树就是一棵二叉搜索树啦～ 又因为本题要求高度平衡，因此我们需要选择升序序列的中间元素作为根节点奥～

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return dfs(nums, 0, nums.length - 1);
    }

    private TreeNode dfs(int[] nums, int lo, int hi) {
        if (lo > hi) {
            return null;
        } 
        // 以升序数组的中间元素作为根节点 root。
        int mid = lo + (hi - lo) / 2;
      	// 治：root.left和root.right本身就是一种治，把分和总联系在一起
        TreeNode root = new TreeNode(nums[mid]);
        // 分
        root.left = dfs(nums, lo, mid - 1);
        root.right = dfs(nums, mid + 1, hi); 
        return root;
    }
}
```

### 二、让我们尝试用递归模板来解题

[分而治之和递归的区别](./分而治之和递归的区别.md)

1. 状态：有序数组的下标(变化量)，用`i`来表示。`recursion(i)`表示以nums[i]为根节点，以有序数组和平衡二叉树的关系，这个i处于数组的正中间。怎么表示出正中间这个性质呢？那么`i`就需要用`(left + right) / 2`来表示了，那么递归函数的参数就应该有两个，一个代表left，一个代表right，但是本质就只是表示一个数组下标。最后因为原数组也要写进函数参数里，所以最后的形式为`recursion(nums, left, right)`

2. 递归终止条件：

   ```c++
   // 这里着重说一下本题的递归终止条件
   // 依照”最容易想到“原则，也许会把left=right当成最容易想到的情况，则代码为
   if(left == right){
     return new TreeNode(nums[left]);
   }
   // 但是上面的情况违背了"最小子问题的值始终固定"原则
   // 因为随着left或者right的值不同，与之对应的nums[left]值也不同
   
   // 所以另一个容易想到的情况为left>right，这是一种不可能情况，则代码为
   if(left > right){
     return null;
   }
   // 这种情况依据"最小子问题的值始终固定"原则，无论left和right是什么值，返回的都是固定值null，所以符合条件
   ```

3. 状态转移方程(通过对状态的循环多次，有几个状态就有几层循环，对立相反的状态只需写两行代码即可，不需要循环)：

   **如果按照动态规划角度来看，可以看做recursion(root) = root.val + recursion(root.left) + recursion(root.right)**

   **是不是和dp[i] = 1 + dp[i - 1] + dp[i + 1]很类似呀**

   ```java
   // 以升序数组的中间元素作为根节点 root。
   int mid = left + (right - lift) / 2;
   TreeNode root = new TreeNode(nums[mid]);
   
   root.left = recursion(nums, left, mid - 1);
   root.right = recursion(nums, mid + 1, right); 
   ```

4. 返回最终状态：**return root;**

