---
title: Leetcode1079. 活字印刷
tags:
  - 回溯
categories:
  - 算法
abbrlink: ff8df00a
date: 2020-08-25 17:20:14
---

**转载自：[Leetcode题解（congwang357），略有增删](https://leetcode-cn.com/problems/letter-tile-possibilities/solution/cdian-xing-de-dfshui-su-zhong-dian-shi-qu-zhong-by/)**

<!-- more -->

**请严格按照[回溯模板](./Leetcode46-全排列.md)所提到的格式分析与编写代码，因为此题和Leetcode面试题-08-08几乎一样，所以完整的格式分析请参照该题**

**这道题和[Leetcode面试题-08-08-有重复字符串的排列组合](./Leetcode面试题-08-08-有重复字符串的排列组合.md)**一样，涉及到重复字母的排序，但是这道题的回溯结束条件不一样：这道题没有回溯结束条件，因为不涉及到结果字符串长度必须和输入字符串长度保持一致

```c++
class Solution {
public:
    int ans = 0;
    void dfs(string &str, vector<int> &visit){
      	// 原本在这一块是有回溯结束条件的
      	// ........
      
        for (int i = 0; i < str.size();i++){
            if (i > 0 && (str[i] == str[i - 1] && (visit[i - 1] == false)))
                continue;
            if(!visit[i]){
                visit[i] = true;
                ans++;
                dfs(str, visit);
                visit[i] = false;
            }
        }
        return;
    }
    int numTilePossibilities(string tiles) {
        vector<bool> visit = vector<bool>(tiles.size(), 0);
        sort(tiles.begin(), tiles.end());
        dfs(tiles, visit);

        return ans;
    }
};
```

