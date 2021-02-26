---
title: Leetcode990. 等式方程的可满足性
tags:
  - 并查集
categories:
  - 算法
abbrlink: 818f3af5
date: 2020-08-25 09:07:07
---

**转载自：[公众号（labuladong），略有增删](https://mp.weixin.qq.com/s/K_oV5JWYpBo9cWTHz6e35Q)**

<!-- more -->

动态连通性其实就是一种等价关系，具有「自反性」「传递性」和「对称性」，其实`==`关系也是一种等价关系，具有这些性质。所以这个问题用 Union-Find 算法就很自然。

核心思想是，**将equations中的算式根据==和!=分成两部分，先处理==算式，使得他们通过相等关系各自勾结成门派；然后处理!=算式，检查不等关系是否破坏了相等关系的连通性**。

**UF的构造函数详见[并查集算法详解](./并查集算法详解.md)**

```java
boolean equationsPossible(String[] equations) {
    // 26 个英文字母
    UF uf = new UF(26);
    // 先让相等的字母形成连通分量
    for (String eq : equations) {
        if (eq.charAt(1) == '=') {
            char x = eq.charAt(0);
            char y = eq.charAt(3);
            uf.union(x - 'a', y - 'a');
        }
    }
    // 检查不等关系是否打破相等关系的连通性
    for (String eq : equations) {
        if (eq.charAt(1) == '!') {
            char x = eq.charAt(0);
            char y = eq.charAt(3);
            // 如果相等关系成立，就是逻辑冲突
            if (uf.connected(x - 'a', y - 'a'))
                return false;
        }
    }
    return true;
}
```

