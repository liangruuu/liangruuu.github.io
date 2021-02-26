---
title: Leetcode67. 二进制求和
tags:
  - 数组
  - 双指针
categories:
  - 算法
abbrlink: 323e9a8b
date: 2020-08-20 22:15:28
---

**转载自：[Leetcode题解（画手大鹏），略有增删](https://leetcode-cn.com/problems/add-binary/solution/hua-jie-suan-fa-67-er-jin-zhi-qiu-he-by-guanpengch/)**

<!-- more -->

双指针同时从末尾进行遍历计算，得到最终结果。

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder ans = new StringBuilder();
        int carry = 0, i = a.length() - 1, j = b.length() - 1;
        while(i >= 0 || j >= 0) {
            int sum = carry;
            sum += i >= 0 ? a.charAt(i) - '0' : 0;
            sum += j >= 0 ? b.charAt(j) - '0' : 0;
            ans.append(sum % 2);
            carry = sum / 2;
            i--;
            j--;
        }
        ans.append(carry == 1 ? carry : "");
        return ans.reverse().toString();
    }
}
```

