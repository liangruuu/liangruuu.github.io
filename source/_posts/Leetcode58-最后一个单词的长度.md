---
title: Leetcode58. 最后一个单词的长度
tags:
  - 数组
  - 单指针
categories:
  - 算法
abbrlink: 210a93b5
date: 2020-08-20 19:53:49
---

**转载自：[Leetcode题解（画手大鹏），略有增删](https://leetcode-cn.com/problems/length-of-last-word/solution/hua-jie-suan-fa-58-zui-hou-yi-ge-dan-ci-de-chang-d/)**

<!-- more -->

标签：字符串遍历
从字符串末尾开始向前遍历，其中主要有两种情况

1. 第一种情况，以字符串"Hello World"为例，从后向前遍历直到遍历到头或者遇到空格为止，即为最后一个单词"World"的长度

2. 第二种情况，以字符串"Hello World "为例，需要先将末尾的空格过滤掉，再进行第一种情况的操作，即认为最后一个单词为"World"，长度为5

3. 所以完整过程为先从后过滤掉空格找到单词尾部，再从尾部向前遍历，找到单词头部，最后两者相减，即为单词的长度时间复杂度：O(n)，n为结尾空格和结尾单词总体长度

```java
class Solution {
    public int lengthOfLastWord(String s) {
        int end = s.length() - 1;
        while(end >= 0 && s.charAt(end) == ' ') end--;
        if(end < 0) return 0;
        int start = end;
        while(start >= 0 && s.charAt(start) != ' ') start--;
        return end - start;
    }
}
```

**这里对循环条件多说几句：**

1. 明确跳出循环的条件，比如这题循环是为了获得非空字符，那么循环条件就是空字符`s.charAt(end) == ' '`，同时循环还不能使数组越界`end >= 0`。`即我为了获得一个东西，把循环条件置成不能获得这个东西的条件`

2. 根据**每一个**循环条件判断循环之后要干什么，当不满足`end>=0`这个循环条件时，跳出循环的时候则表示这串字符就是空字符串了，则`if(end < 0) return 0`；若不满足`s.charAt(end) == ' '`就表示已经排除掉了所有空字符，可以进行下一步操作了

3. 把第一个条件再次运用一遍：我为了获取空字符，从而把循环条件置成不能获得空字符的条件，即`s.charAt(start) != ' '`

4. 如果循环的条件为多个且是`或`的关系，则需要在循环体内对条件进行判断处理

   ```java
   while(i >= 0 || j >=0){
     	if(i < 0) ...
        if(j < 0) ...
     	i--;
     	j--;
   }
   ```

   或

   ```java
   while(i >= 0 || j >=0){
     	if(i >= 0) ...
        if(j >= 0) ...
     	i--;
     	j--;
   }
   ```

   

