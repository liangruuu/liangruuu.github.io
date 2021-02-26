---
title: Leetcode300. 最长上升子序列
tags:
  - 动态规划
categories:
  - 算法
abbrlink: dcbd00a3
date: 2020-08-21 21:12:11
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/longest-increasing-subsequence/solution/dong-tai-gui-hua-she-ji-fang-fa-zhi-pai-you-xi-jia/)**

<!-- more -->

动态规划模板详见[Leetcode121-买卖股票的最佳时机]()

**虽然烦，但还是要写公式-_-||**

1. 状态(即下标)：数组元素下标(变化量)，`设为i`，意思是从数组下表0开始到数组下标i为止的最长上升子序列长度，**至于说为什么不设两个状态量，一个起始下标i，一个终止下标j，dp[i] [j]意思是以i起点，j为终点的子数组的最长上升子序列，em...，因为**

2. 最小状态：`dp[i] = 1`，因为第i个元素本身就能看成是一个上升元素

3. 状态转移方程(通过对状态的循环多次，有几个状态就有几层循环，对立相反的状态只需写两行代码即可，不需要循环)：

   ```c++
   for (int i = 1; i < nums.size(); i++)
     for (int j = 0; j < i; j++)
       if (nums[i] > nums[j])
         // 下标为i的元素若比之前的任意一个元素大，上升子序列长度就+1
         dp[i] = max(dp[i], 1 + dp[j]);
   ```

   **这里需要注意一点，如果看过动态规划模板的话，可能会感到奇怪，不是说了有几个状态才有几个嵌套循环吗，为什么这里只有1个状态，却有两层循环。这是因为在模板文章里也说明了，在状态转移方程里是涉及到循环的，但这个循环却不是因为多出了一个状态而循环，比如这里的j元素最大才遍历到i，j的值受到i的制约，单纯是为了得出dp[i]所设出的下标，这种收其他状态影响的元素并不适合以上的`状态-循环`定理**

4. 返回最终状态：和一般的返回`return dp[n - 1]`不同，因为我们状态转移方程书写的问题

```c++
例子：[1,3,6,7,9,4,10,5,6]，nums[8]和nums[6]比较后无法进入if体内，导致该次比较后的dp[8]仍然为1
最后的结果：dp[8] == 5(1,3,4,5,6)，而正确答案为最长上升序列长度为6(1,3,6,7,9,10)
```

```c++
class Solution {
public:
  int lengthOfLIS(vector<int> &nums) {
    if (nums.size() == 0)
      return 0;
    
    vector<int> dp(nums.size(), 1);
    for (int i = 1; i < nums.size(); i++)
      for (int j = 0; j < i; j++)
        if (nums[i] > nums[j])
          dp[i] = max(dp[i], 1 + dp[j]);
    int res = dp[0];
    for (int i = 1; i < nums.size(); i++)
      res = max(res, dp[i]);
    return res;
  }
};
```

