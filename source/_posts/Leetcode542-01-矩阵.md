---
title: Leetcode542. 01 矩阵
tags:
  - BFS
  - 动态规划
categories:
  - 算法
abbrlink: 376b6661
date: 2020-08-27 14:16:03
---

建图方法详见和BFS问题模板详见[BFS问题模板](./BFS问题模板.md)

**转载自：[Leetcode题解（力扣官方题解），略有增删](https://leetcode-cn.com/problems/01-matrix/solution/01ju-zhen-by-leetcode-solution/)**

<!-- more -->

#### 一、BFS解法

首先把每个源点 0 入队，然后从各个 0 同时开始一圈一圈的向 11 扩散（每个 11 都是被离它最近的 00 扩散到的 ），扩散的时候可以设置 dist 数组来记录距离（即扩散的层次）并同时使用 visitied 数组标志是否访问过。

**完整代码**

**每部分代码你都能在BFS模板中找到对应原型**

```c++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        vector<vector<int>> dist(m, vector<int>(n));
        vector<vector<bool>> visited(m, vector<bool>(n));
        queue<pair<int, int>> q;
        // 将所有的 0 添加进初始队列中
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    q.push({i, j});
                    visited[i][j] = true;
                }
            }
        }

        // 广度优先搜索
        while (!q.empty()) {
            int size = q.size();
            while(size){
                auto [i, j] = q.front();
                q.pop();
                for (int d = 0; d < 4; ++d) {
                    int newX = i + dirs[d][0];
                    int newY = j + dirs[d][1];
                    if (newX >= 0 && newX < m && newY >= 0 && newY < n && !visited[newX][newY]) {
                        dist[newX][newY] = dist[i][j] + 1;
                        q.push({newX, newY});
                        visited[newX][newY] = true;
                    }
                }
                size--;
            }
        }
        return dist;
    }
};
```

#### 二、动态规划解法

1. 状态：二维数组下标(变化量)，用`i`和`j`表示。`dp[i][j]`表示当前位置元素到最近的0的距离

2. 最小状态：如果二维数组某元素本身就是0，则dp[i] [j]就是0

3. 状态转移方程(通过对状态的循环多次，有几个状态就有几层循环，对立相反的状态只需写两行代码即可，不需要循环)：**有两个状态量，则有两层嵌套循环**

   ```c++
   // 很明显在二维数组里，某个位置的元素值就是由它为中心的九宫格位置元素演变过来的，本题是上下左右
   // 但是这道题比较棘手，因为对于简单的二维动态规划，某个位置的值都是从"单侧"递推过来的
   // 比如从左到右，从右下到左上，做右上到左下.....但是本道题是上下左右都能对中间元素值产生影响
   // 根据只能由"已知量推未知量"的原则，无法一次性完成dp数组的构建，所以需要if判断分别从两侧往中间推
   // 最后的结果对二者取最小值
   ```

4. 返回最终状态：return dp[n] [m]

**完整代码**

```c++
class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        // 初始化动态规划的数组，所有的距离值都设置为一个很大的数
        vector<vector<int>> dist(m, vector<int>(n, INT_MAX / 2));
        // 如果 (i, j) 的元素为 0，那么距离为 0
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (matrix[i][j] == 0) {
                    dist[i][j] = 0;
                }
            }
        }
        // 只有 水平向左移动 和 竖直向上移动，注意动态规划的计算顺序
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i - 1 >= 0) {
                    dist[i][j] = min(dist[i][j], dist[i - 1][j] + 1);
                }
                if (j - 1 >= 0) {
                    dist[i][j] = min(dist[i][j], dist[i][j - 1] + 1);
                }
            }
        }
        // 只有 水平向左移动 和 竖直向下移动，注意动态规划的计算顺序
        for (int i = m - 1; i >= 0; --i) {
            for (int j = 0; j < n; ++j) {
                if (i + 1 < m) {
                    dist[i][j] = min(dist[i][j], dist[i + 1][j] + 1);
                }
                if (j - 1 >= 0) {
                    dist[i][j] = min(dist[i][j], dist[i][j - 1] + 1);
                }
            }
        }
        // 只有 水平向右移动 和 竖直向上移动，注意动态规划的计算顺序
        for (int i = 0; i < m; ++i) {
            for (int j = n - 1; j >= 0; --j) {
                if (i - 1 >= 0) {
                    dist[i][j] = min(dist[i][j], dist[i - 1][j] + 1);
                }
                if (j + 1 < n) {
                    dist[i][j] = min(dist[i][j], dist[i][j + 1] + 1);
                }
            }
        }
        // 只有 水平向右移动 和 竖直向下移动，注意动态规划的计算顺序
        for (int i = m - 1; i >= 0; --i) {
            for (int j = n - 1; j >= 0; --j) {
                if (i + 1 < m) {
                    dist[i][j] = min(dist[i][j], dist[i + 1][j] + 1);
                }
                if (j + 1 < n) {
                    dist[i][j] = min(dist[i][j], dist[i][j + 1] + 1);
                }
            }
        }
        return dist;
    }
};
```

