---
title: Leetcode513. 找树左下角的值
tags:
  - BFS
  - 树
categories:
  - 算法
abbrlink: aeedec6c
date: 2020-08-27 09:14:26
---

建图方法详见和BFS问题模板详见[BFS问题模板](./BFS问题模板.md)

**转载自：[Leetcode题解（quantbruce），略有增删](https://leetcode-cn.com/problems/find-bottom-left-tree-value/solution/bfsgai-jin-fa-jian-dan-yi-dong-jie-jin-shuang-bai-/)**

<!-- more -->

本题要求**最后一层**的元素，涉及到层数，参考BFS模板里第二种形式的描述：如果题目涉及到层数，步数......则使用BFS解题

**注意几点：**

1. 二叉树节点的相邻节点只有左右子节点这两个，所以可以使用两个if替代for循环
2. 先入右节点再入左节点，让右节点先出队

**完整代码**
**每部分代码你都能在BFS模板中找到对应原型**

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
    public int findBottomLeftValue(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<>();
        int result = 0;
        queue.offer(root);
        while(!queue.isEmpty()){
            int size = queue.size();
            while(size > 0){
                TreeNode node = queue.poll();
                result = node.val;
                if(node.right != null){
                    queue.offer(node.right);
                }
                if(node.left != null){
                    queue.offer(node.left);
                }
                size--;
            }
        }
        return result;
    }
}
```

