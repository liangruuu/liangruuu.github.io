---
title: Leetcode面试题 01.06. 字符串压缩
tags:
  - 数组
  - 双指针
categories:
  - 算法
abbrlink: 50df684c
date: 2020-08-21 13:41:15
---

原创

<!-- more -->

**双指针分为`快慢双指针和前后双指针`，这里为快慢双指针，索引j用作不断向前探索与s[i]相同的元素，直到不相同，则记录下标，并且与i相减获得相同数组元素个数；以此循环往复**

**这里复习一下[Leetcode58-最后一个单词的长度](E:\code\hexo-site\blog\source\_posts\Leetcode58-最后一个单词的长度.md)有关循环的技巧**

```java
class Solution {
    public String compressString(String s) {
        int len = s.length();
        StringBuffer res = new StringBuffer();
        if(len == 0){
            return "";
        }
        if(len == 1){
            res.append(s.charAt(0) + "1");
        } else {
            int i = 0, j = 1;
            while(i < len && j <= len){
                if(j == len || s.charAt(i) != s.charAt(j)){
                    res.append(s.charAt(i));
                    res.append(j - i);
                    i = j;
                    j++;
                } else {
                    j++;
                }
            }
        }

        return res.toString().length() >= len? s:res.toString();
    }
}
```

