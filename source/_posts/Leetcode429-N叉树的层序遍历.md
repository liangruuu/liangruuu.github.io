---
title: Leetcode429. N叉树的层序遍历
tags:
  - BFS
  - 树
categories:
  - 算法
abbrlink: fbf15006
date: 2020-08-27 10:10:12
---

建图方法详见和BFS问题模板详见[BFS问题模板](./BFS问题模板.md)

**转载自：[Leetcode题解（力扣 (LeetCode)），略有增删](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/solution/ncha-shu-de-ceng-xu-bian-li-by-leetcode/)**

<!-- more -->

**完整代码**
**每部分代码你都能在BFS模板中找到对应原型**

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {      
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            List<Integer> level = new ArrayList<>();
            int size = queue.size();
          
            for (int i = 0; i < size; i++) {
                Node node = queue.poll();
                level.add(node.val);
              	
              	// 这行代码等价于对节点相邻元素的遍历
              	// 因为题目给定了一个节点的所有子节点children, 直接拿来用即可
                queue.addAll(node.children);
            }
            result.add(level);
        }
        return result;
    }
}
```

