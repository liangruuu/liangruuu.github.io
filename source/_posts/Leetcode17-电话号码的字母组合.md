---
title: Leetcode17. 电话号码的字母组合
tags:
  - 回溯
categories:
  - 算法
abbrlink: c3a78369
date: 2020-08-25 09:43:22
---

**转载自：[Leetcode题解（乌鲁木齐001号程序员），略有增删](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/solution/leetcode-17-letter-combinations-of-a-phone-number-/)**

<!-- more -->

**请严格按照[回溯模板](./Leetcode46-全排列.md)所提到的格式分析与编写代码**

```java
class Solution {

private:
  const string letterMap[10] = {
      " ",    // 0
      "",     // 1
      "abc",  // 2
      "def",  // 3
      "ghi",  // 4
      "jkl",  // 5
      "mno",  // 6
      "pqrs", // 7
      "tuv",  // 8
      "wxyz"  // 9
  };

  vector<string> res;
  
  // 路径：记录在 s 中
	// 选择列表：根据输出里包含字母来判断，选择列表是一个字母串.
  // 观察题干字母串的产生是由digits和index和letterMap共同构成的
  // digits和index确定下标，letterMap+下标就能获得选择列表(字母串)
	// 结束条件：已经处理的digits长度个字符
  void findCombination(const string &digits, int index, const string &s) {

    // 满足结束条件
    if (index == digits.size()) {
      res.push_back(s);
      return;
    }

    char c = digits[index];
    string letters = letterMap[c - '0'];
    // 循环遍历选择列表并且做选择
    for (int i = 0; i < letters.size(); i++) {
      findCombination(digits, index + 1, s + letters[i]);
      
      // 这里如果按照标准写法的话
      // 做选择
      // index = index + 1;
      // 进入下一层决策树
      // findCombination(digits, index, s + letters[i]);
      // 取消选择
      // index = index - 1;
    }

    return;
  }

public:
  vector<string> letterCombinations(string digits) {

    res.clear();
    if (digits == "")
      return res;

    findCombination(digits, 0, "");

    return res;
  }
};
```

