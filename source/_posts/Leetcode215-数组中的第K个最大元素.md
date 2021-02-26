---
title: Leetcode215. 数组中的第K个最大元素
tags:
  - 分而治之
  - 快速排序
  - 堆
categories:
  - 算法
abbrlink: 2e4c4b4c
date: 2020-08-21 09:43:32
---

**转载自：[Leetcode题解（liweiwei1419），略有增删](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/solution/partitionfen-er-zhi-zhi-you-xian-dui-lie-java-dai-/)**

<!-- more -->

题目要求我们找到“数组排序后的第 kk 个最大的元素，而不是第 kk 个不同的元素” ，

语义是从右边往左边数第 kk 个元素（从 11 开始），那么从左向右数是第几个呢，我们列出几个找找规律就好了。

一共 6 个元素，找第 2 大，索引是 4；
一共 6 个元素，找第 4 大，索引是 2。
因此，升序排序以后，**目标元素的索引是 len - k**。

### 方法一：暴力解法

```c++
#include <iostream>
#include <vector>

using namespace std;

class Solution {
public:
    int findKthLargest(vector<int> &nums, int k) {
        int size = nums.size();
        sort(begin(nums), end(nums));
        return nums[size - k];
    }
};
```

### 方法二：借助 partition 操作定位到最终排定以后索引为 `len - k` 的那个元素

**快速排序里的partition操作每次都能把一个元素固定在它最后存在的那个位置，我们只需要使用到这个性质定位到len-k这个位置即可，没必要把数组变成有序，不需把快排的全部代码粘过来**

主函数

```c++
public int findKthLargest(int[] nums, int k) {
    int len = nums.length;
    int left = 0;
    int right = len - 1;

    // 转换一下，第 k 大元素的索引是 len - k
    int target = len - k;

    while (true) {
        int index = partition(nums, left, right);
        if (index == target) {
         	 	return nums[index];
        } else if (index < target) {
          	left = index + 1;
        } else {
          	right = index - 1;
        }
    }
}
```

快速排序的主函数，**可以看到还是由很大不同的，但是partition部分代码是完全一致的**

```c++
public QuickSort(int[] nums, int low, int high){
  	if(low < high){
      	int index = partition(nums, low, high);
      	QuickSort(nums, low, index - 1);
      	QuickSort(nums, index + 1, high);
    }
}
```

全部代码

```c++
public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        int left = 0;
        int right = len - 1;

        // 转换一下，第 k 大元素的索引是 len - k
        int target = len - k;

        while (true) {
          	// 分而治之
            int index = partition(nums, left, right);
            if (index == target) {
                return nums[index];
            } else if (index < target) {
                left = index + 1;
            } else {
                right = index - 1;
            }
        }
    }
  
    public int partition(int[] nums, int left, int right) {
        int pivot = nums[left];
        while(left < right){
          	// 将比pivot小的元素移动到左边
          	while(left < right && nums[right] >= pivot) right--;
          	nums[left] = nums[right];
          	// 将比pivot大的元素移动到右边
          	while(left < right && nums[left] <= pivot) left++;
          	nums[right] = nums[left];
        }
      	nums[left] = pivot; // 最后将pivot放在最后的那个位置
      	return left;
    }
}
```

### 方法三：优先队列

方法论详见[Leetcode剑指-Offer-40-最小的k个数](./Leetcode剑指-Offer-40-最小的k个数.md)

* 使用全部`len`个容量

```java
import java.util.PriorityQueue;

public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        // 使用一个含有 len 个元素的最小堆，默认是最小堆，可以不写 lambda 表达式：(a, b) -> a - b
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(len, (a, b) -> a - b);
        for (int i = 0; i < len; i++) {
            minHeap.add(nums[i]);
        }
        for (int i = 0; i < len - k; i++) {
            minHeap.poll();
        }
        return minHeap.peek();
    }
}
```

* 只用 `k` 个容量的优先队列，而不用全部 `len` 个容量。
  * 我要的是k个最大元素中的最小值，所以用小根堆
  * 我要的是k个最大的元素，所以最小的len-k个元素全部滚出去！！小值元素出队同样能印证使用小根堆的正确性，因为只有最小的元素才能在队头出队

```java
import java.util.PriorityQueue;

public class Solution {

    public int findKthLargest(int[] nums, int k) {
        int len = nums.length;
        // 使用一个含有 k 个元素的最小堆
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k, (a, b) -> a - b);
        for (int i = 0; i < k; i++) {
            minHeap.add(nums[i]);
        }
        for (int i = k; i < len; i++) {
            // 看一眼，不拿出，因为有可能没有必要替换
            Integer topEle = minHeap.peek();
            // 只要当前遍历的元素比堆顶元素大，堆顶弹出，遍历的元素进去
            if (nums[i] > topEle) {
                minHeap.poll();
                minHeap.add(nums[i]);
            }
        }
        return minHeap.peek();
    }
}
```

