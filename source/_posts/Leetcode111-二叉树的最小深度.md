---
title: Leetcode111. 二叉树的最小深度
tags:
  - DFS
  - BFS
  - 树
categories:
  - 算法
abbrlink: 1be921e7
date: 2020-08-26 08:26:22
---

**使用[递归分析模板](./分而治之和递归的区别.md)将会十分清晰**

**转载自：[Leetcode题解（王小二），略有增删](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/solution/li-jie-zhe-dao-ti-de-jie-shu-tiao-jian-by-user7208/)**

<!-- more -->

#### 一、DFS

1. 状态：树节点(变化量)，用root表示；recursion(root)表示以该root节点为根节点的树的最小深度

2. 递归终止条件：

   ```java
   if(root == null) return 0;
   ```

3. 状态转移方程(可能涉及到多次if条件判断)：

   ```java
   //1.左孩子和有孩子都为空的情况，说明到达了叶子节点，直接返回1即可
   if(root.left == null && root.right == null) return 1;
   //2.如果左孩子和由孩子其中一个为空，那么需要返回比较大的那个孩子的深度        
   int m1 = minDepth(root.left);
   int m2 = minDepth(root.right);
   //这里其中一个节点为空，说明m1和m2有一个必然为0，所以可以返回m1 + m2 + 1;
   if(root.left == null || root.right == null) return m1 + m2 + 1;
   
   //3.最后一种情况，也就是左右孩子都不为空，返回最小深度+1即可
   return Math.min(m1,m2) + 1; 
   ```

   **用动态规划角度来看的转移方程为**

   ```java
   if(dp[root.left] == null && dp[root.right] == null){
     return 1;
   } else if(dp[root.left] == null || dp[root.right] == null){
     return dp[root.left] + dp[root.right] + 1;
   } else{
     return Math.min(dp[root.left], dp[root.right]) + 1; 
   }
   ```

4. 返回最终状态：return recursion(n)；

**完整代码**

```java
class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        //这道题递归条件里分为三种情况
        //1.左孩子和有孩子都为空的情况，说明到达了叶子节点，直接返回1即可
        if(root.left == null && root.right == null) return 1;
        //2.如果左孩子和由孩子其中一个为空，那么需要返回比较大的那个孩子的深度        
        int m1 = minDepth(root.left);
        int m2 = minDepth(root.right);
        //这里其中一个节点为空，说明m1和m2有一个必然为0，所以可以返回m1 + m2 + 1;
        if(root.left == null || root.right == null) return m1 + m2 + 1;
        
        //3.最后一种情况，也就是左右孩子都不为空，返回最小深度+1即可
        return Math.min(m1,m2) + 1; 
    }
}
```

#### 二、BFS

最小高度其实和层数是有关的，记录层数恰好符合BFS的思路，详情参考[BFS问题模板](./BFS问题模板.md)。其实如果按照层序遍历来解题的话，从上下到，从左到右，直到有一个节点的左右孩子都是空就返回这个高度

```java
class Solution {
    //bfs。一层一层的遍历，直到有一个节点的左右孩子都是空就返回这个高度
    public int minDepth(TreeNode root) {
        if(root == null) return 0;//为空就返回0
        int height = 1;//初始高度为1
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            int count = queue.size();//每一层遍历，有双子节点都为空就返回当前高度
            while(count > 0){
                TreeNode cur = queue.poll();
                if(cur.left == null && cur.right == null){
                    return height;
                }
                if(cur.left!=null){
                    queue.add(cur.left);
                }
                if(cur.right!=null){
                    queue.add(cur.right);
                }
                count--;
            }
            height++;
        }
        return height;
    }
}
```

