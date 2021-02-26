---
title: Leetcode20. 有效的括号
tags:
  - 栈
categories:
  - 算法
abbrlink: 3163c5fe
date: 2020-08-24 10:36:41
---

**栈模板[使用栈解题思想](./使用栈解题思想.md)**

<!-- more -->

先从使用栈的几个前提条件出发：

1. 是否涉及到**比较**：括号匹配，肯定涉及到左右括号的比较

2. 是否涉及到**延迟比较**：

   ```c++
   // 这种情况就是不涉及到延迟比较
   输入: "()[]{}"
   输出: true
   // 这种情况就是涉及到了延迟比较，最左侧的大括号要和最右侧的大括号比较，之间隔了一堆中括号
   输入: "{[]}"
   输出: true
   ```

**满足了延迟比较的特点，所以使用栈来辅助解题**

**我们根据栈模板中提到的`必写代码`来分析如下答案**

```c++
class Solution {
public:
    bool isValid(string s) {
        stack<char> stk;
      	// 1. 必写代码之一：对原始数据的循环遍历
        for(int i = 0; i < s.size(); i++){
          	// 对元素数据里的每一个元素都进行栈操作
            char c = s[i];
            if(c == '(' || c == '[' || c == '{'){
              	// 2. 必写代码之五：入栈
                stk.push(s[i]);
            } else {
              	// 3. 必写代码之二：对栈的判空，不管是用while还是if
                if(stk.empty()){
                    return false;
                }
              	// 4. 必写代码之三：取出栈顶元素
                char topChar = stk.top();
              	// 5. 必写代码之四：出栈
                stk.pop();
              	// 获取栈顶元素之后的具体比较逻辑
                if(c == ')' && topChar != '(')
                    return false;
                if(c == ']' && topChar != '[')
                    return false;
                if(c == '}' && topChar != '{')
                    return false;
            }
        }
        return stk.empty();
    }
};
```

