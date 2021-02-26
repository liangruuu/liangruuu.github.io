---
title: Leetcode76. 最小覆盖子串
tags:
  - 双指针
  - 滑动窗口
categories:
  - 算法
abbrlink: '75953e30'
date: 2020-08-21 19:43:25
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/minimum-window-substring/solution/hua-dong-chuang-kou-suan-fa-tong-yong-si-xiang-by-/)**

<!-- more -->

**滑动窗口算法(也是快慢指针的一种类型，像一只蠕动的虫子)的代码框架**

**这里面有几个巧记点**

1. 滑动窗口，窗户的英文为window，所以需要一个window容器来存储
2. 为什么need容器需要用map<char, int>来定义呢，因为子串中的字母不定全部不等，有可能出现aaaaaaaaa重复元素的情况，所以需要用map记录每个字符和它出现的次数
3. 其中 valid 变量表示窗口中满足 need 条件的字符个数

```c++
/* 滑动窗口算法框架 */
void slidingWindow(string s, string t) {
    unordered_map<char, int> need, window;
    for (char c : t) need[c]++;
    
    int left = 0, right = 0;
    int valid = 0; 
    while (right < s.size()) {
        // c 是将移入窗口的字符
        char c = s[right];
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新，一般来说就以下两条操作即可
      	// 1. 判断字符c是否存在于s中，若要是window容器也容纳进该字符，若该字符已经存在于window容器内，则只需要把值再加1
      	// 2. 如果window容器内某个字符满足need容器内的条件就让valid++
        ...
        
        // 判断左侧窗口是否要收缩，也不一定非得是while，也可能是用if判断
        while (window needs shrink) {
          	// =======
          	// 根据很多题的试验，发现跟题目相关的业务代码主要集中在这一块，其他部分几乎全部是一模一样复制粘贴的模板
          	// =======
          
            // d 是将移出窗口的字符
            char d = s[left];
            // 左移窗口
            left++;
          	// 进行窗口内数据的一系列更新，一般来说就以下两条操作即可
      			// 1. 判断字符d是否存在于s中，若要是window容器也删除该字符，若该字符已经存在于window容器内，则只需要把值再减1
      			// 2. 如果window容器内某个字符满足need容器内的条件就让valid--，valid只需要减一次就行了
            ...
        }
    }
}
```

**套模板时需要思考以下几个问题**

1、当移动 right 扩大窗口，即加入字符时，应该更新哪些数据？

2、什么条件下，窗口应该暂停扩大，开始移动 left 缩小窗口？

3、当移动 left 缩小窗口，即移出字符时，应该更新哪些数据？

4、我们要的结果应该在扩大窗口时还是缩小窗口时进行更新？

如果一个字符进入窗口，应该增加 window 计数器；如果一个字符将移出窗口的时候，应该减少 window 计数器；当 valid 满足 need 时应该收缩窗口；应该在收缩窗口的时候更新最终结果。

## 回到题目本身

```c++
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> need, window;
        for (char c : t) need[c]++;

        int left = 0, right = 0;
        int valid = 0;
        // 记录最小覆盖子串的起始索引及长度
        int start = 0, len = INT_MAX;
        while (right < s.size()) {
            // c 是将移入窗口的字符
            char c = s[right];
            // 右移窗口
            right++;
            // 进行窗口内数据的一系列更新
            if (need.count(c)) {
                window[c]++;
                if (window[c] == need[c])
                    valid++;
            }

            // 判断左侧窗口是否要收缩
            while (valid == need.size()) {
                // 在这里更新最小覆盖子串
                if (right - left < len) {
                    start = left;
                    len = right - left;
                }
                // d 是将移出窗口的字符
                char d = s[left];
                // 左移窗口
                left++;
                // 进行窗口内数据的一系列更新
                if (need.count(d)) {
                    if (window[d] == need[d])
                        valid--;
                    window[d]--;
                }                    
            }
        }
        // 返回最小覆盖子串
        return len == INT_MAX ?
            "" : s.substr(start, len);
    }
};
```

