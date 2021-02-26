---
title: Leetcode690. 员工的重要性
tags:
  - BFS
  - 树
categories:
  - 算法
abbrlink: 8dda7622
date: 2020-08-27 18:53:45
---

建图方法详见和BFS问题模板详见[BFS问题模板](./BFS问题模板.md)

**转载自：[Leetcode题解（力扣 (LeetCode)），略有增删](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/solution/ncha-shu-de-ceng-xu-bian-li-by-leetcode/)**

<!-- more -->

这道题无非就是**求以某个节点为根节点的树中所有节点值的总和**

**完整代码**
**每部分代码你都能在BFS模板中找到对应原型**

```java
public class Solution {

    int getImportance(List<Employee> employees, int id) {
        Map<Integer, Employee> map = new HashMap<>(employees.size());
        for (Employee employee : employees) {
            map.put(employee.id, employee);
        }

        Queue<Integer> queue = new LinkedList<>();

        int res = 0;
        // 加入队尾
        queue.offer(id);
        while (!queue.isEmpty()) {
            int size = queue.size();
            while(size > 0){
                // 队头拿出
                Integer currentId = queue.poll();

                Employee currentEmployee = map.get(currentId);
                res += currentEmployee.importance;
                for (Integer eid : currentEmployee.subordinates) {
                    queue.offer(eid);
                }
                size--;
            }
        }
        return res;
    }
}
```

