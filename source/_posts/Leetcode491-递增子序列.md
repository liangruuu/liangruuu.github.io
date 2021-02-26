---
title: Leetcode491. 递增子序列
tags:
  - 回溯
categories:
  - 算法
abbrlink: 7df2bdfd
date: 2020-08-27 15:55:26
---

**转载自：[Leetcode题解（Sweetiee），略有增删](https://leetcode-cn.com/problems/increasing-subsequences/solution/jin-tian-wo-you-shuang-ruo-zhuo-neng-miao-dong-la-/)**

<!-- more -->

**请严格按照[回溯模板](./Leetcode46-全排列.md)所提到的格式分析与编写代码**

**几点需要注意**

1. 在回溯模板里，是使用`visited`来确保元素不会重复访问，但是本题是求**递增**的一位数组，是从左往右的线性变化，访问过了就再也回不去了(哲学中时间的线性性质)，这跟在一维数组里**求组合数**还不一样，从左到右已经访问过的元素是没用了的；跟二维数组也不一样，二维数组里某个位置元素可以**从它下面的元素向上访问**，**也可以从它右边的元素向左访问**....所以需要设置`visited`数组。本题通过`int i = idx + 1`就可以很好保证元素不用重复访问

```java
class Solution {
    // 定义全局变量保存结果
    List<List<Integer>> res = new ArrayList<>();
    
    public List<List<Integer>> findSubsequences(int[] nums) {
        // idx 初始化为 -1，开始 dfs 搜索。
        dfs(nums, -1, new ArrayList<>());
        return res;
    }

    private void dfs(int[] nums, int idx, List<Integer> curList) {
        // 只要当前的递增序列长度大于 1，就加入到结果 res 中，然后继续搜索递增序列的下一个值。
        if (curList.size() > 1) {
            res.add(new ArrayList<>(curList));
        }

        // 在 [idx + 1, nums.length - 1] 范围内遍历搜索递增序列的下一个值。
        // 借助 set 对 [idx + 1, nums.length - 1] 范围内的数去重。
        Set<Integer> set = new HashSet<>();
        for (int i = idx + 1; i < nums.length; i++) {
            // 1. 如果 set 中已经有与 nums[i] 相同的值了，说明加上 nums[i] 后的所有可能的递增序列之前已经被搜过一遍了，因此停止继续搜索。
            if (set.contains(nums[i])) { 
                continue;
            }
            set.add(nums[i]);
            // 2. 如果 nums[i] >= nums[idx] 的话，说明出现了新的递增序列，因此继续 dfs 搜索
            if (idx == -1 || nums[i] >= nums[idx]) {
                curList.add(nums[i]);
                dfs(nums, i, curList);
                curList.remove(curList.size() - 1);
            }
        }
    }
}
```

