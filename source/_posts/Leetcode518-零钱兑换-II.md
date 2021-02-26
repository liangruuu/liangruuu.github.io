---
title: Leetcode518. 零钱兑换 II
tags:
  - 动态规划
categories:
  - 算法
abbrlink: 4f8ba50a
date: 2020-08-22 09:54:30
---

**转载自：[公众号（labuladong），略有增删](https://mp.weixin.qq.com/s/zGJZpsGVMlk-Vc2PEY4RPw)**

<!-- more -->

**这道题是完全背包问题的变体，和0-1背包问题的区别就是：完全背包问题里的物品能重复装入，而在0-1背包问题中，物品不能重复装入**

**我们可以把这个问题转化为背包问题的描述形式**：

有一个背包，最大容量为`amount`，有一系列物品`coins`，每个物品的重量为`coins[i]`，**每个物品的数量无限**。请问有多少种方法，能够把背包恰好装满？

**虽然烦，但还是要写公式-_-||**

1. 状态：物品(变化量)：用下标i表示，背包容量，即可承受最大物品重量(变化量)：用下标j表示；dp数组含义：**若只使用前i个物品，当背包容量为j时，有dp[i] [j]种方法可以装满背包**。换句话说，翻译回我们题目的意思就是：**若只使用coins中的前i个硬币的面值，若想凑出金额j，有dp[i] [j]种凑法**。

2. 最小状态：`dp[0][..] = 0， dp[..][0] = 1`。因为如果不使用任何硬币面值，就无法凑出任何金额；如果凑出的目标金额为 0，那么“无为而治”就是唯一的一种凑法。

3. 状态转移方程(通过对状态的循环多次，有几个状态就有几层循环，对立相反的状态只需写两行代码即可，不需要循环)：

   ```c++
   for (int i = 1; i <= n; i++) {
     for (int j = 1; j <= amount; j++)
       if (j - coins[i-1] >= 0)
         dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]];
       else 
         dp[i][j] = dp[i - 1][j];
   }
   ```

   **注：这里和0-1背包问题的区别就在一处，我们来看下两者的区别**

   ```c++
   // 0-1背包问题的状态转移方程
   for (int i = 1; i <= N; i++) {
     for (int w = 1; w <= W; w++) {
       if (w - wt[i-1] < 0) {
         dp[i][w] = dp[i - 1][w];
       } else {
         dp[i][w] = max(dp[i - 1][w - wt[i-1]] + val[i-1], dp[i - 1][w]);
       }
     }
   }
   ```

   **区别就在0-1背包问题中`dp[i - 1][w - wt[i-1]]`，而完全背包问题为`dp[i][j - coins[i-1]]`，因为完全背包问题能重复使用元素**

4. 返回最终状态：`return dp[n][amount];`

```c++
class Solution {
  public int change(int amount, int[] coins) {
    int n = coins.length;
    int[][] dp = new int[n + 1][amount + 1];
    // base case
    for (int i = 0; i <= n; i++) 
        dp[i][0] = 1;

    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= amount; j++)
            if (j - coins[i-1] >= 0)
                dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i-1]];
            else 
                dp[i][j] = dp[i - 1][j];
    }
    return dp[n][amount];
  }
}
```

