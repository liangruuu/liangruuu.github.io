---
title: Leetcode329. 矩阵中的最长递增路径
tags:
  - BFS
  - DFS
categories:
  - 算法
abbrlink: 472e8caf
date: 2020-08-28 10:11:00
---

**本题设计到记忆化搜索，因为若不使用，则不管是DFS还是BFS都会超时**

**转载自：[Leetcode题解（力扣 (LeetCode)），略有增删](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/solution/ncha-shu-de-ceng-xu-bian-li-by-leetcode/)**

<!-- more -->

#### 一、BFS

建图方法详见和BFS问题模板详见[BFS问题模板](./BFS问题模板.md)，对是否使用`visited`数组的分析参考[visited数组的使用方法](./visited数组的使用方法.md)

**转载自：[Leetcode题解（lartecy），略有增删](https://leetcode-cn.com/problems/longest-increasing-path-in-a-matrix/solution/dfshe-bfsliang-chong-xie-fa-by-lartecy/)**

1. 为了防止重复计算，可以使用一个`memo[][]`数组来记录之前计算过的值，为什么需要记忆化搜索呢，因为计算a->b->c之间的最长递增序列长度，需要先计算a->b的最长递增序列长度，明显可以看出有重复计算。
2. 在每个起点处，维护一个队列，存入需要遍历的坐标，同时维护一个表示路径长度(层数)的变量count，找到合法的位置坐标就加入队列，同时更新memo，保证memo表示的是当前最大的长度


```java
class Solution {
  public int longestIncreasingPath(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
      return 0;
    }
    int m = matrix.length;
    int n  = matrix[0].length;
    int[][] memo = new int[m][n];
    int[][] dirs = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    int res = 1;
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (memo[i][j] > 0) {
          continue;
        }
        Deque<int[]> queue = new ArrayDeque<>();
        queue.add(new int[]{i, j});
        int count = 1;
        while (!queue.isEmpty()) {
          int len = queue.size();
          count++;
          for (int k = 0; k < len; k++) {
            int[] cur = queue.poll();
            for (int[] d : dirs) {
              int newX = cur[0] + d[0];
              int newY = cur[1] + d[1];
              if (newX < 0 || newX >= m || newY < 0 || newY >= n || 
                  matrix[newX][newY] <= matrix[cur[0]][cur[1]] || count <= memo[newX][newY]) {
                continue;
              }
              memo[newX][newY] = count;
              res = Math.max(res, count);
              queue.add(new int[]{newX, newY});
            }
          }
        }
      }
    }
    return res;
  }
}
```

#### 二、DFS

1. 状态：二维数组下标，用`row和col`表示。`recursion(matrix, row, col)`表示以[row, col]为起点的最长递增序列长度，matrix为原始数组，但是因为本题需要用到记忆化搜素，所以用一个memo数组保存已经计算过的点的值：`recursion(int[][] matrix, int row, int column, int[][] memo)`

2. 递归终止条件：没有递归终止条件，单纯返回一个当前位置的最长递增序列长度`return memo[row][column];`

   **用动态规划的角度表示最小子问题为：**`return dp[row][column]`

3. 状态转移方程(可能涉及到多次if条件判断)：

   ```java
   public int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
   int newRow = row + dir[0], newColumn = column + dir[1];
   
   if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns 
   		&& matrix[newRow][newColumn] > matrix[row][column]) {
     memo[row][column] = Math.max(memo[row][column], dfs(matrix, newRow, newColumn, memo) + 1);
   }
   ```

   **用动态规划的角度表示转移方程为：**

   ```java
   if (newRow >= 0 && newRow < rows && newColumn >= 0 && newColumn < columns 
   		&& matrix[newRow][newColumn] > matrix[row][column]) {
    	dp[row][col] = Math.max(memo[row][column], dp[newRow, newColumn] + 1)
   }
   ```

4. 返回最终状态：return recursion(row, col)；

   **用动态规划的角度表示最终返回值为：`return dp[row][col]`**

**完整代码**

```c++
class Solution {
public:
    static constexpr int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
    int rows, columns;

    int longestIncreasingPath(vector< vector<int> > &matrix) {
        if (matrix.size() == 0 || matrix[0].size() == 0) {
            return 0;
        }
        rows = matrix.size();
        columns = matrix[0].size();
        auto memo = vector< vector<int> > (rows, vector <int> (columns));
        int ans = 0;
        for (int i = 0; i < rows; ++i) {
            for (int j = 0; j < columns; ++j) {
                ans = max(ans, dfs(matrix, i, j, memo));
            }
        }
        return ans;
    }

    int dfs(vector< vector<int> > &matrix, int row, int column, vector< vector<int> > &memo) {
        if (memo[row][column] != 0) {
            return memo[row][column];
        }
        ++memo[row][column];
        for (int i = 0; i < 4; ++i) {
            int newRow = row + dirs[i][0], newColumn = column + dirs[i][1];
            if (newRow >= 0 && newRow < rows && newColumn >= 0 && 
                		newColumn < columns && matrix[newRow][newColumn] > matrix[row][column]) {
                memo[row][column] = max(memo[row][column], dfs(matrix, newRow, newColumn, memo) + 1);
            }
        }
        return memo[row][column];
    }
};
```



