---
title: Leetcode3. 无重复字符的最长子串
tags:
  - 双指针
  - 滑动窗口
categories:
  - 算法
abbrlink: 4c0a5d16
date: 2020-08-21 20:56:52
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/solution/hua-dong-chuang-kou-tong-yong-si-xiang-jie-jue-zi-/)**

<!-- more -->

滑动窗口模板详见[Leetcode76-最小覆盖子串]()

```c++
class Solution {
public:
  int lengthOfLongestSubstring(string s) {
    // 双指针问题
    int left = 0, right = 0;
    unordered_map<char, int> window;
    int res = 0;
    while (right < s.size()) {
      char c = s[right];
      window[c]++;
      right++;
      // 如果window中出现重复字符
      // 开始移动left缩小窗口
      while (window[c] > 1) {
        char d = s[left];
        window[d]--;
        left++;
      }
      res = max(res, right - left);
    }
    return res;
  }
};
```

这就是变简单了，连`need`和`valid`都不需要，而且更新窗口内数据也只需要简单的更新计数器`window`即可。

当`window[c]`值大于 1 时，说明窗口中存在重复字符，不符合条件，就该移动`left`缩小窗口了嘛。

唯一需要注意的是，在哪里更新结果`res`呢？我们要的是最长无重复子串，哪一个阶段可以保证窗口中的字符串是没有重复的呢？

这里和之前不一样，**要在收缩窗口完成后更新res**，因为窗口收缩的 while 条件是存在重复元素，换句话说收缩完成后一定保证窗口中没有重复嘛。