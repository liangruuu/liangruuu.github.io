---
title: Leetcode322. 零钱兑换
tags:
  - 动态规划
  - 贪心算法
categories:
  - 算法
abbrlink: 69caf82b
date: 2020-08-22 20:09:58
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/coin-change/solution/dong-tai-gui-hua-tao-lu-xiang-jie-by-wei-lai-bu-ke/)**

<!-- more -->

**虽然烦，但还是要写公式-_-||**

1. 状态：数组下标(变化量)，使用`i`来表示；零钱总数(变化量0)，使用`j`来表示，则`dp[i][j]`表示最少使用多少枚前`i`个枚硬币才能刚好凑出总共`j`的零钱，如果不能刚好凑出则返回-1

2. 最小状态：`dp[0][...] = -1`表示用前0枚硬币能刚好凑出目的零钱，显然不可能，`dp[...][0] = 0`表示用前...枚硬币凑出0块钱，显然有一枚硬币都不需要

3. 状态转移方程(通过对状态的循环多次，有几个状态就有几层循环，对立相反的状态只需写两行代码即可，不需要循环)：**两个状态，两次嵌套循环**

   ```c++
   for(int i = 1; i <= len; i++){
     for(int j = 1; j <= amount; j++){
       // 主要这里的coins[i - 1]主要是因为coins[0]代表前1个硬币，两者存在一个偏差
       if(j >= coins[i - 1]){
         dp[i][j] = min(dp[i - 1][j], dp[i][j - coins[i - 1]] + 1);
       } else {
         dp[i][j] = dp[i - 1][j];
       }
     }
   }
   ```

4. 返回最终状态：dp[amount] == amount + 1 ? -1 : dp[amount];

```c++
class Solution {
public:
  int coinChange(vector<int> &coins, int amount) {
    // vector<int> dp(amount + 1, amount + 1);
    // dp[0] = 0;
    int len = coins.size();
    vector<vector<int>> dp(len + 1, vector<int>(amount + 1, amount + 1));

    for(int i = 0; i <= len; i++){
        dp[i][0] = 0;
    }
    for(int i = 1; i <= len; i++){
        for(int j = 1; j <= amount; j++){
            // 主要这里的coins[i - 1]主要是因为coins[0]代表前1个硬币，两者存在一个偏差
            if(j >= coins[i - 1]){
                dp[i][j] = min(dp[i - 1][j], dp[i][j - coins[i - 1]] + 1);
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    return dp[len][amount] == amount + 1 ? -1 : dp[len][amount];
  }
};
```

#### 贪心解法详见[贪心算法](./贪心算法.md)

