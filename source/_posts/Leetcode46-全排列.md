---
title: Leetcode46. 全排列
tags:
  - 回溯
categories:
  - 算法
abbrlink: 15896e6d
date: 2020-08-21 19:26:38
---

**转载自：[Leetcode题解（labuladong），略有增删](https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-xiang-jie-by-labuladong-2/)**

<!-- more -->

**解决一个回溯问题，实际上就是一个决策树的遍历过程**。你只需要思考 3 个问题：

1、路径：也就是已经做出的选择。

2、选择列表：也就是你当前可以做的选择，这里提供一个获取选择列表的简易方法：**只需要看题目最终要的到什么，然后去已知条件里看，能获得最终所要的答案的地方就是选择列表**，说起来有点抽象，我们举几个例子

* [Leetcode17. 电话号码的字母组合](./Leetcode17-电话号码的字母组合.md)：题目的输出为["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]，这些就是题目最终要得到的值，这些答案是**字母**的排列组合，所以**选择列表必定是一串字母**，那么我们去题干里看，如何才能获得**字母**？结果是通过手机号码**数字**，相当于下标去获得**字母串**
* 针对本题全排列来说：输出为[[1,2,3], [1,3,2]....]，这些答案是**数字**的排列组合，所以**选择列表必定是一串数字**，那么我们去题干里看，如何才能获得**数字**？因为题干提供的本身就是数字，我们直接拿题干当选择列表即可。

3、剪枝：如果有必要的话，在选择列表里的某几个选择是无法使用的，此时就需要把这些选择**排除在外**

* 就比如这道全排列题，数组里的每个元素只能用一次，所以使用`visited`数组用来标记是否需要排除。**在第一次处理该选择之后排除该选择，同时别忘了在取消选择的时候再把这个被排除的选择包含进来**

4、结束条件：也就是到达决策树底层，无法再做选择的条件。

代码方面，回溯算法的框架：

```python
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

**其核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」**，特别简单。

```java
/* 主函数，输入一组不重复的数字，返回它们的全排列 */
List<List<Integer>> permute(int[] nums) {
    // 记录「路径」
    LinkedList<Integer> track = new LinkedList<>();
  	List<List<Integer>> res = new LinkedList<>();`
 		boolean[] visited = new boolean[nums.length];
    backtrack(res, nums, track, visited);
    return res;
}

// 路径：记录在 track 中
// 选择列表：nums 中不存在于 track 的那些元素
// 结束条件：nums 中的元素全都在 track 中出现
void backtrack(List<List<Integer>> res, int[] nums, LinkedList<Integer> track, boolean[] visited) {
    // 触发结束条件
    if (track.size() == nums.length) {
        res.add(new LinkedList(track));
        return;
    }

    for (int i = 0; i < nums.length; i++) {
        // 排除不合法的选择，即剪枝
      	if (visited[i]) continue;

        // 做选择
        track.add(nums[i]);
      	visited[i] = true;
        // 进入下一层决策树
        backtrack(res, nums, track, visited);
        // 取消选择
        track.removeLast();
      	visited[i] = false;
    }
}
```

