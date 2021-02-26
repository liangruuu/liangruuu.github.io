---
title: Leetcode95. 不同的二叉搜索树 II
tags:
  - 树
  - DFS
categories:
  - 算法
abbrlink: 3b6ad9e9
date: 2020-09-04 18:41:04
---

**转载自：[Leetcode题解（力扣官方题解），略有增删](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/solution/bu-tong-de-er-cha-sou-suo-shu-ii-by-leetcode-solut/)**

<!-- more -->

#### 一、递归

**递归解法详见：[分而治之和递归的区别](./分而治之和递归的区别.md)**

所以如果求 1...n 的所有可能。

我们只需要把 1 作为根节点，[ ] 空作为左子树，[ 2 ... n ] 的所有可能作为右子树。

2 作为根节点，[ 1 ] 作为左子树，[ 3...n ] 的所有可能作为右子树。

3 作为根节点，[ 1 2 ] 的所有可能作为左子树，[ 4 ... n ] 的所有可能作为右子树，然后左子树和右子树两两组合。

4 作为根节点，[ 1 2 3 ] 的所有可能作为左子树，[ 5 ... n ] 的所有可能作为右子树，然后左子树和右子树两两组合。

...

n 作为根节点，[ 1... n ] 的所有可能作为左子树，[ ] 作为右子树。

至于，[ 2 ... n ] 的所有可能以及 [ 4 ... n ] 以及其他情况的所有可能，可以利用上边的方法，把每个数字作为根节点，然后把所有可能的左子树和右子树组合起来即可。

如果只有一个数字，那么所有可能就是一种情况，把该数字作为一棵树。而如果是 [ ]，那就返回 null。

***

1. 状态：数组下标(变化量)，用i表示；`recursion(i)`表示以i为根节点构造符合条件的树，**而它返回的应该是一个树节点数组**。根据上面的分析，这个i是通过循环遍历整个序列所得，而序列是由左右边界的，即用(start, end)表示，同时把递归函数参数修改成`recursion(start, end)`

2. 递归终止条件：

   ```java
   // 如果用strat和end来限制边界的话，那么结束递归的条件就是start > end
   if (start > end) {
     allTrees.add(null);
     return allTrees;
   }
   ```

3. 状态转移方程(可能涉及到多次if条件判断)：

   ```java
   // 枚举可行根节点
   for (int i = start; i <= end; i++) {
     // 获得所有可行的左子树集合
     List<TreeNode> leftTrees = generateTrees(start, i - 1);
   
     // 获得所有可行的右子树集合
     List<TreeNode> rightTrees = generateTrees(i + 1, end);
   
     // 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点上
     for (TreeNode left : leftTrees) {
       for (TreeNode right : rightTrees) {
         TreeNode currTree = new TreeNode(i);
         currTree.left = left;
         currTree.right = right;
         allTrees.add(currTree);
       }
     }
   }
   ```

4. 返回最终状态：return recursion(1， n)；

```java
class Solution {
    public List<TreeNode> generateTrees(int n) {
        if (n == 0) {
            return new LinkedList<TreeNode>();
        }
        return generateTrees(1, n);
    }

    public List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> allTrees = new LinkedList<TreeNode>();
        if (start > end) {
            allTrees.add(null);
            return allTrees;
        }

        // 枚举可行根节点，这里采用循环是因为本题题意的特殊性
        for (int i = start; i <= end; i++) {
            // 获得所有可行的左子树集合
            List<TreeNode> leftTrees = generateTrees(start, i - 1);

            // 获得所有可行的右子树集合
            List<TreeNode> rightTrees = generateTrees(i + 1, end);

            // 从左子树集合中选出一棵左子树，从右子树集合中选出一棵右子树，拼接到根节点上
            for (TreeNode left : leftTrees) {
                for (TreeNode right : rightTrees) {
                    TreeNode currTree = new TreeNode(i);
                    currTree.left = left;
                    currTree.right = right;
                    allTrees.add(currTree);
                }
            }
        }
        return allTrees;
    }
}
```

