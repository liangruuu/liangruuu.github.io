---
title: Leetcode面试题 08.08. 有重复字符串的排列组合
tags:
  - 回溯
categories:
  - 算法
abbrlink: 6289af
date: 2020-08-25 11:05:59
---

**转载自：[Leetcode题解（上弦月），略有增删](https://leetcode-cn.com/problems/permutation-ii-lcci/solution/c-qu-zhong-dfsyi-dong-by-shang-xian-yue-4/)**

<!-- more -->

如果这道题目字符串中没有相同字母那直接回溯就可以得到正确答案，但假设出现了相同字母。如 "abb" ，直接回溯会**将两个b看作不同的字母**，两个b的相对位置不同也会被纳入解中，（b1ab2、b2ab1)

我们怎样去重？ 其实很简单，前面的b不需要考虑后面的b怎么排列（因为后面的b属于一个新的排列），遍历到后面的b时，若前面的b处于未访问的状态，这种状态就是重复状态，因为前面的b已经将以b开头的所有子排列添加到了答案中，不需要第二个以b开头的子排列

**请严格按照[回溯模板](./Leetcode46-全排列.md)所提到的格式分析与编写代码**

```c++
class Solution {
private:
    // 路径：记录在 track 中
    // 选择列表：根据输出里包含字母来判断，选择列表是一个字母串.
  	// 因为题干提供的本身就是字母，我们直接拿题干当选择列表即可。
    // 结束条件：路径长度等于源字符串长度
    void backtrack(vector<string> &result, string s, string track, vector<bool> &visited){
        if(track.size() == s.size()){
            result.push_back(track);
            return;
        }
        for(int i = 0; i < s.size(); i++){
          	// 剪枝
            if((i > 0 && s[i] == s[i-1] && !visited[i-1]) || visited[i]){
                continue;
            }
            track.push_back(s[i]);
            visited[i] = true;
            backtrack(result, s, track, visited);
            track.pop_back();
            visited[i] = false;
        }
    }

public:
    vector<string> permutation(string s) {
        string track = "";
        vector<string> result;
        vector<bool> visited(s.size(), false);
      	// 这里先排序，把相同的字母都放在相邻的位置
        sort(s.begin(),s.end());
        backtrack(result, s, track, visited);

        return result;
    }
};
```

**注：回溯体内的循环可以简化成**

```java
for(int i = 0; i < s.size(); i++){
  // 剪枝
  if((i > 0 && s[i] == s[i-1] && !visited[i-1]) || visited[i]){
    continue;
  }
  visited[i] = true;
  backtrack(result, s, track + s[i], visited);
  visited[i] = false;
}
```

