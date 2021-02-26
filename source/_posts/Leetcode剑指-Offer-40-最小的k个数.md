---
title: Leetcode剑指 Offer 40. 最小的k个数
tags:
  - 堆
categories:
  - 算法
abbrlink: ecb11406
date: 2020-08-21 09:23:10
---

**转载自：[Leetcode题解（Sweetiee），略有增删](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/solution/3chong-jie-fa-miao-sha-topkkuai-pai-dui-er-cha-sou/)**

<!-- more -->

大根堆(前 K 小) / 小根堆（前 K 大),Java，C++中有现成的 PriorityQueue，实现起来最简单

**注：本题是求前 K 小，因此维护一个容量为 K 的大根堆，每次 poll 出最大的数，那堆中保留的就是前 K 小啦（注意不是小根堆！小根堆的话需要把全部的元素都入堆，那是 O(NlogN)O(NlogN)😂，就不是 O(NlogK)O(NlogK)啦～～）**

* 每次 poll 出最大的数只能使用大根堆，因为只有队头才能出队，同样能印证使用大根堆的正确性

```java
// 保持堆的大小为K，然后遍历数组中的数字，遍历的时候做如下判断：
// 1. 若目前堆的大小小于K，将当前数字放入堆中。
// 2. 否则判断当前数字与大根堆堆顶元素的大小关系，如果当前数字比大根堆堆顶还大，这个数就直接跳过；
//    反之如果当前数字比大根堆堆顶小，先poll掉堆顶，再将该数字放入堆中。
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if (k == 0 || arr.length == 0) {
            return new int[0];
        }
        // 默认是小根堆，实现大根堆需要重写一下比较器。
        Queue<Integer> pq = new PriorityQueue<>((v1, v2) -> v2 - v1);
        for (int num: arr) {
            if (pq.size() < k) {
                pq.offer(num);
            } else if (num < pq.peek()) {
                pq.poll();
                pq.offer(num);
            }
        }
        
        // 返回堆中的元素
        int[] res = new int[pq.size()];
        int idx = 0;
        for(int num: pq) {
            res[idx++] = num;
        }
        return res;
    }
}
```

