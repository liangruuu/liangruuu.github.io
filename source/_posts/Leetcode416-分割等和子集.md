---
title: Leetcode416. 分割等和子集
tags:
  - 动态规划
categories:
  - 算法
abbrlink: 7b167e22
date: 2020-08-22 09:25:00
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/partition-equal-subset-sum/solution/0-1-bei-bao-wen-ti-bian-ti-zhi-zi-ji-fen-ge-by-lab/)**

<!-- more -->

对于这个问题，看起来和背包没有任何关系，为什么说它是背包问题呢？

首先回忆一下背包问题大致的描述是什么：

给你一个可装载重量为`W`的背包和`N`个物品，每个物品有重量和价值两个属性。其中第`i`个物品的重量为`wt[i]`，价值为`val[i]`，现在让你用这个背包装物品，最多能装的价值是多少？

那么对于这个问题，我们可以先对集合求和，得出`sum`，把问题转化为背包问题：

**给一个可装载重量为sum/2的背包和N个物品，每个物品的重量为nums[i]。现在让你装物品，是否存在一种装法，能够恰好将背包装满**？

你看，这就是背包问题的模型，甚至比我们之前的经典背包问题还要简单一些，**下面我们就直接转换成背包问题**

**虽然烦，但还是要写公式-_-||**

1. 状态：数组元素(变化量)，用下标i来表示，元素和容量(变化量)，用下标j来表示；`dp[i][j] = x`表示，对于前`i`个物品，当前背包的容量为`j`时，若`x`为`true`，则说明可以恰好将背包装满，若`x`为`false`，则说明不能恰好将背包装满。比如说，如果`dp[4][9] = true`，其含义为：对于容量为 9 的背包，若只是用前 4 个物品，可以有一种方法把背包恰好装满。

2. 最小状态：`dp[..][0] = true`和`dp[0][..] = false`，因为背包没有空间的时候，就相当于装满了，而当没有物品可选择的时候，肯定没办法装满背包。

3. 状态转移方程(通过对状态的循环多次，有几个状态就有几层循环，对立相反的状态只需写两行代码即可，不需要循环)：**两个状态，两次嵌套循环**

   ```c++
   for (int i = 1; i <= n; i++) {
     for (int j = 1; j <= sum; j++) {
       if (j - nums[i - 1] < 0) {
         // 背包容量不足，不能装入第 i 个物品
         dp[i][j] = dp[i - 1][j]; 
       } else {
         // 装入或不装入背包
         dp[i][j] = dp[i - 1][j] | dp[i - 1][j-nums[i-1]];
       }
     }
   }
   ```

   

4. 返回最终状态：return `dp[N][sum/2]`

```c++
class Solution {

public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for (int num : nums) sum += num;
        // 和为奇数时，不可能划分成两个和相等的集合
        if (sum % 2 != 0) return false;
        int n = nums.size();
        sum = sum / 2;
        vector<vector<bool>> dp(n + 1, vector<bool>(sum + 1, false));
        // base case
        for (int i = 0; i <= n; i++)
            dp[i][0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum; j++) {
                if (j - nums[i - 1] < 0) {
                		// 背包容量不足，不能装入第 i 个物品
                    dp[i][j] = dp[i - 1][j]; 
                } else {
                    // 装入或不装入背包
                    dp[i][j] = dp[i - 1][j] | dp[i - 1][j-nums[i-1]];
                }
            }
        }
        return dp[n][sum];
    }
};
```