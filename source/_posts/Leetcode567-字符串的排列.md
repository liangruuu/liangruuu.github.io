---
title: Leetcode567. 字符串的排列
tags:
  - 双指针
  - 滑动窗口
categories:
  - 算法
abbrlink: 43b2fa2e
date: 2020-08-21 20:09:19
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/permutation-in-string/solution/wo-xie-liao-yi-shou-shi-ba-suo-you-hua-dong-chuang/)**

<!-- more -->

滑动窗口模板详见[Leetcode76-最小覆盖子串]()

对于这道题的解法代码，基本上和最小覆盖子串一模一样，只需要改变两个地方：

**1、**本题移动`left`缩小窗口的时机是窗口大小大于`t.size()`时，因为排列嘛，显然长度应该是一样的。

**2、**当发现`valid == need.size()`时，就说明窗口中就是一个合法的排列，所以立即返回`true`。

```c++
class Solution {
public:
    // 判断 s 中是否存在 t 的排列
    bool checkInclusion(string t, string s) {
        unordered_map<char, int> need, window;
        for (char c : t) need[c]++;

        int left = 0, right = 0;
        int valid = 0;
        while (right < s.size()) {
            char c = s[right];
            right++;
            // 进行窗口内数据的一系列更新
            if (need.count(c)) {
                window[c]++;
                if (window[c] == need[c])
                    valid++;
            }

            // 判断左侧窗口是否要收缩
            while (right - left >= t.size()) {
                // 在这里判断是否找到了合法的子串
                if (valid == need.size())
                    return true;
                char d = s[left];
                left++;
                // 进行窗口内数据的一系列更新
                if (need.count(d)) {
                    if (window[d] == need[d])
                        valid--;
                    window[d]--;
                }
            }
        }
        // 未找到符合条件的子串
        return false;
    }
};
```

