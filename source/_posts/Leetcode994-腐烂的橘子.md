---
title: Leecode994. 腐烂的橘子
tags:
  - BFS
categories:
  - 算法
abbrlink: dd7258b1
date: 2020-08-20 21:48:36
---

**转载自：[Leetcode题解（nettee），略有增删](https://leetcode-cn.com/problems/rotting-oranges/solution/li-qing-si-lu-wei-shi-yao-yong-bfsyi-ji-ru-he-xie-/)**

<!-- more -->

BFS 可以看成是层序遍历。从某个结点出发，BFS 首先遍历到距离为 1 的结点，然后是距离为 2、3、4…… 的结点。因此，BFS 可以用来求**最短路径问题**。BFS 先搜索到的结点，一定是距离最近的结点。

再看看这道题的题目要求：返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。翻译一下，实际上就是**求腐烂橘子到所有新鲜橘子的最短路径**。那么这道题使用 BFS，应该是毫无疑问的了。

BFS两个模板详见**[Leetcode102](E:\code\hexo-site\blog\source\_posts\Leetcode102-二叉树的层序遍历.md)**，区分使用两个模板的条件为**是否需要一口气处理完某一次循环中在队列里的全部 n 个结点**

有了计算最短路径的层序 BFS 代码框架，写这道题就很简单了。这道题的主要思路是：

* 一开始，我们找出所有腐烂的橘子，将它们放入队列，作为第 0 层的结点。
* 然后进行 BFS 遍历，每个结点的相邻结点可能是上、下、左、右四个方向的结点，注意判断结点位于网格边界的特殊情况。
* 由于可能存在无法被污染的橘子，我们需要记录新鲜橘子的数量。在 BFS 中，每遍历到一个橘子（污染了一个橘子），就将新鲜橘子的数量减一。如果 BFS 结束后这个数量仍未减为零，说明存在无法被污染的橘子。

```c++
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int min = 0, fresh = 0;
        queue<pair<int, int>> q;
        for(int i = 0; i < grid.size(); i++) {
            for(int j = 0; j < grid[0].size(); j++)
                if(grid[i][j] == 1) fresh++;
                else if(grid[i][j] == 2) q.push({i, j});
        }
        vector<pair<int, int>> dirs = { {-1, 0}, {1, 0}, {0, -1}, {0, 1} };
        while(!q.empty()) {
            int n = q.size();
            bool rotten = false;
            for(int i = 0; i < n; i++) {
                auto x = q.front();
                q.pop();
                for(auto cur: dirs) {
                    int i = x.first + cur.first;
                    int j = x.second + cur.second;
                    if(i >= 0 && i < grid.size() && j >= 0 && j < grid[0].size() && grid[i][j] == 1) {
                        grid[i][j] = 2;
                        q.push({i, j});
                        fresh--;
                        rotten = true;
                    }
                }
            }
            if(rotten) min++;
        } 
        return fresh ? -1 : min;
    }
};
```

