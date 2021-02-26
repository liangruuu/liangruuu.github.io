---
title: Leetcode529. 扫雷游戏
tags:
  - BFS
categories:
  - 算法
abbrlink: 7c69f2
date: 2020-08-27 10:42:25
---

建图方法详见和BFS问题模板详见[BFS问题模板](./BFS问题模板.md)

**转载自：[Leetcode题解（Sweetiee），略有增删](https://leetcode-cn.com/problems/minesweeper/solution/cong-qi-dian-kai-shi-dfs-bfs-bian-li-yi-bian-ji-ke/)**

<!-- more -->

**完整代码**

**每部分代码你都能在BFS模板中找到对应原型**

这里要注意的是，与树的 BFS 不一样（每个节点只有一个父亲节点），本题图中的点会由多个点达到，因此需要加上 boolean[][] visited 数组记录访问标志，防止每个点重复入队而超时。

```java
class Solution {
    // 定义 8 个方向
    int[] dx = {-1, 1, 0, 0, -1, 1, -1, 1};
    int[] dy = {0, 0, -1, 1, -1, 1, 1, -1};

    public char[][] updateBoard(char[][] board, int[] click) {
        // 1. 若起点是雷，游戏结束，直接修改 board 并返回。
        int x = click[0], y = click[1];
        if (board[x][y] == 'M') {
            board[x][y] = 'X';
            return board;
        } 

        // 2. 若起点是空地，则将起点入队，从起点开始向 8 邻域的空地进行宽度优先搜索。
        int m = board.length, n = board[0].length;
        boolean[][] visited = new boolean[m][n];
        visited[x][y] = true;
        Queue<int[]> queue = new LinkedList<>();
        queue.offer(new int[] {x, y});
        while (!queue.isEmpty()) {
            int[] point = queue.poll();
            int i = point[0], j = point[1];
            // 判断空地 (i, j) 周围是否有雷
            int cnt = 0;
            for (int k = 0; k < 8; k++) {
                int newX = i + dx[k];
                int newY = j + dy[k];
                if (newX < 0 || newX >= board.length || newY < 0 || newY >= board[0].length) {
                    continue;
                }
                if (board[newX][newY] == 'M') {
                    cnt++;
                }
            }
            // 若空地 (i, j) 周围有雷，则将该位置修改为雷数；
          	// 否则将该位置更新为 ‘B’，并将其 8 邻域中的空地入队，继续进行 bfs 搜索。
            if (cnt > 0) {
                board[i][j] = (char)(cnt + '0');
            } else {
                board[i][j] = 'B';
                for (int k = 0; k < 8; k++) {
                    int newX = i + dx[k];
                    int newY = j + dy[k];
                    if (newX < 0 || newX >= board.length || newY < 0 || newY >= board[0].length 
                        || board[newX][newY] != 'E' || visited[newX][newY]) {
                        continue;
                    }
                    visited[newX][newY] = true;
                    queue.offer(new int[] {newX, newY});
                }
            }
        }
        return board;
    }
}
```

