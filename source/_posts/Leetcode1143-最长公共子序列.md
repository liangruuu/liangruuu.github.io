---
title: Leetcode1143. 最长公共子序列
tags:
  - 动态规划
categories:
  - 算法
abbrlink: 93d25cf
date: 2020-08-22 13:48:57
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/longest-common-subsequence/solution/dong-tai-gui-hua-zhi-zui-chang-gong-gong-zi-xu-lie/)**

<!-- more -->

**虽然烦，但还是要写公式-_-||**

1. 状态：两个字符串的下标(变量)，分别为i和j，`dp[i][j]`的含义是：对于`s1[1..i]`和`s2[1..j]`，它们的 LCS 长度是`dp[i][j]`。比如dp[2] [4] 的含义就是：对于`"ac"`和`"babc"`，它们的 LCS 长度是 2。我们最终想得到的答案应该是`dp[3][6]`。

2. 最小状态：我们专门让索引为 0 的行和列表示空串，`dp[0][..]`和`dp[..][0]`都应该初始化为 0。比如说，按照刚才 dp 数组的定义，`dp[0][3]=0`的含义是：对于字符串`""`和`"bab"`，其 LCS 的长度为 0。因为有一个字符串是空串，它们的最长公共子序列的长度显然应该是 0。

3. 状态转移方程(通过对状态的循环多次，有几个状态就有几层循环，对立相反的状态只需写两行代码即可，不需要循环)：**两个状态，两次嵌套循环，并且要想dp[i] [j]怎么和dp[i - 1] [j - 1]，dp[i] [j + 1]之类的下标元素产生关系**

   用两个指针`i`和`j`从后往前遍历`s1`和`s2`，如果`s1[i]==s2[j]`，那么这个字符**一定在lcs中**；否则的话，`s1[i]`和`s2[j]`这两个字符**至少有一个不在lcs中**，需要丢弃一个。

   ```c++
   for(int i = 1; i <= len1; i++){
     for(int j = 1; j <= len2; j++){
       if(text1[i - 1] == text2[j - 1]){
         dp[i][j] = dp[i - 1][j - 1] + 1;
       }else{
         dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
       }
     }
   }
   ```

4. 返回最终状态：return dp[len1] [len2];

```c++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int len1 = text1.size(), len2 = text2.size();
        vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1, 0));
        for(int i = 1; i <= len1; i++){
            for(int j = 1; j <= len2; j++){
                if(text1[i - 1] == text2[j - 1]){
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }else{
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        }
        return dp[len1][len2];
    }
};
```

