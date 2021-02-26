---
title: Leetcode125. 验证回文串
tags:
  - 数组
  - 双指针
categories:
  - 算法
abbrlink: e4078e0d
date: 2020-08-21 08:48:43
---

**注：答案因为字符和数字判断方面有错，但是思想是对的，通多两端双指针的方式不断向内部逼近，通过循环中的循环方式移除所有非数字非字母元素**

<!-- more -->

```c++
class Solution {
private:
    bool isalpha(char a){
        return ((a >= 'a' && a <= 'z') || (a >= 'A' && a <= 'Z'))? true: false;
    }
public:
    bool isPalindrome(string s) {
        int i = 0, j = s.size() - 1;
        cout << (isalpha(s[i]) || isdigit(s[i])) << endl;
        while(i < j){
            while(!(isalpha(s[i]) || isdigit(s[i]))) i++;
            while(!(isalpha(s[j]) || isdigit(s[j]))) j--;
            if(tolower(s[i++]) != tolower(s[j--])) break;
        }
        return i == j? true : false;
    }
};
```

