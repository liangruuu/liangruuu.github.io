---
title: Leetcode559. N叉树的最大深度
tags:
  - BFS
categories:
  - 算法
abbrlink: 330cf4b
date: 2020-08-27 15:06:33
---

建图方法详见和BFS问题模板详见[BFS问题模板](./BFS问题模板.md)

**转载自：[Leetcode题解（Iris_bupt），略有增删](https://leetcode-cn.com/problems/maximum-depth-of-n-ary-tree/solution/c-san-chong-jing-dian-fang-fa-jie-ti-by-pris_bupt/)**

<!-- more -->

本道题目的是求树的最大高度，不管是N叉树还是二叉树，像这种有明显**层级**概念的题用BFS做就对了

**完整代码**

**每部分代码你都能在BFS模板中找到对应原型**

```c++
int maxDepth(Node* root) {
    if (!root) return 0;
    queue<Node*>queue;
    queue.push(root);
    int max_depth = 0;
    while (!queue.empty()) {
        max_depth++;			
        for (int size = queue.size(); size; size--) {
            Node* curr = queue.front(); queue.pop();
            for (Node* it : curr->children)
                queue.push(it);
        }
    }
    return max_depth;
}
```

