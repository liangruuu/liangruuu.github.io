---
title: Leetcode102. 二叉树的层序遍历
tags:
  - 树
  - BFS
categories:
  - 算法
abbrlink: e3e54639
date: 2020-08-20 19:05:55
---

**转载自：[Leetcode题解（负雪明烛），略有增删](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/tao-mo-ban-bfs-he-dfs-du-ke-yi-jie-jue-by-fuxuemin/)**

<!-- more -->

####  一、**套模板！BFS 和 DFS 都可以解决**

BFS使用队列，把每个还没有搜索到的点依次放入队列，然后再弹出队列的头部元素当做当前遍历点。BFS总共有两个模板：

1. 如果不需要确定当前遍历到了哪一层，BFS模板如下。

````c++
// 在while (!queue.isEmpty()) 外部入队第一个元素，或者是第一批元素(使用循环入队)
// 比如说把图中所有度为1的节点先入队...
queue.push(first);

while queue 不空：
    cur = queue.pop()		// 该行代码位置固定，位于队列内元素while循环接下去的那一行
    
  	// ==========================
  	/* 划重点：这里进行主要逻辑判断, 这里举的例子为队头元素为目标值则返回步数(层数)，具体问题具体分析 */
  	// 这部分的代码位置固定紧挨着循环访问图中所有相邻节点上面
  	if (cur is target)
    		return level;
		// ==========================  
  
    for 节点 in cur的所有相邻节点：
    		// 即if(!visited[该节点])
        if 该节点有效且未访问过：
            queue.push(该节点)		// 该行代码位置固定，位于if条件判断接下去的那一行
            visited.push(该节点)
````

2. 如果**要确定当前遍历到了哪一层(对这句话的理解可以参考[Leetcode127-单词接龙](./Leetcode127-单词接龙.md))**，BFS模板如下。这里增加了level表示当前遍历到二叉树中的哪一层了，也可以理解为在一个图中，**现在已经走了多少步了，比如那些算最短距离的题目**。size表示在当前遍历层有多少个元素，也就是队列中的元素数，我们把这些元素一次性遍历完，即把当前层的所有元素都向外走了一步。

```c++
// 在while (!queue.isEmpty())外部入队第一个元素，或者是第一批元素(使用循环入队)
// 比如说把图中所有度为1的节点先入队...
queue.push(first);

level = 0
while queue 不空：
    size = queue.size()
    while (size --) {
        cur = queue.pop()		// 该行代码位置固定，位于队列内元素while循环接下去的那一行
          
        // ==========================
        /* 划重点：这里进行主要逻辑判断, 这里举的例子为队头元素为目标值则返回步数(层数)，具体问题具体分析 */
        // 这部分的代码位置固定紧挨着循环访问图中所有相邻节点上面
        if (cur is target)
           	return level;  
      	// ==========================  
      
        for 节点 in cur的所有相邻节点：
          	// 即if(!visited[该节点])
            if 该节点有效且未被访问过：
                queue.push(该节点)  // 该行代码位置固定，位于if条件判断接下去的那一行
              	visited.push(该节点)
    }
    level ++;
```

上面两个是通用模板，在任何题目中都可以用，是要记住的！

#### 二、**注：BFS里涉及到队列(queue)，一定一定有以下几个步骤**

0. 在while循环外部入队**第一个**队列元素

1. 判队列是否为空（先入队根元素，再判断while queue 不空）

2. 出队列首元素（cur = queue.pop()）

3. 对出队的元素进行操作

4. 遍历所有与队首元素相邻的元素（**方向数组**`vector<pair<int, int>> dirs = { {-1, 0}, {1, 0}, {0, -1}, {0, 1} }`常出现在二维数组遍历中）。**遍历相邻节点的循环设定也是有技巧的，参考[建图方法](./BFS问题模板.md)，相邻节点和什么条件有关就依照这个条件设置循环因子，可以有多个因子，对应多层嵌套循环**

   ```c++
   1. 比如时钟里与0点相邻的有1点和11点，那么就把小时刻度当成循环条件，并且循环2次；
   for(int i = 0; i < 2; i++){}
   
   2. 比如一个字母能一次变成其他25个字母，那么就把字母当成循环条件
   for(char c = 'a'; c <= 'z'; c++) {
   
   3. 比如一个单词通过改变其中一个字母从而转换成另一个单词，那么就把单词下标和字母当成循环条件
   for (int j = 0; j < wordLen; j++) {
   	for(char c = 'a'; c <= 'z'; c++) {}
   }
   
   4. 比如二维数组里改变"九宫格"位置的元素
   // 定义 8 个方向
   int[] dx = {-1, 1, 0, 0, -1, 1, -1, 1};
   int[] dy = {0, 0, -1, 1, -1, 1, 1, -1};
   for (int k = 0; k < 8; k++) {
      int newX = i + dx[k];
      int newY = j + dy[k];
      if (newX < 0 || newX >= board.length || newY < 0 || newY >= board[0].length) {
           continue;
      }
      ...
   }
   ```

5. （可能存在）对已遍历的元素置状态保证之后不再访问（`visited[i] == true`），并把没有访问过的元素推进队列中同时`visited`状态置为true

#### 三、自定义公式

**BFS里能根据具体题目自定义的模板公式不多，因为大部分都是写死的代码，跟具体的题目逻辑无关，能自定义的且和逻辑简单相关(指的是单纯看题目条件就能得出)的就只有循环因子了**

1. 根据题目条件判断循环因子，N个循环因子对应N层嵌套循环
2. 构造相邻节点：请与之前的根据题意判断循环因子区分开，两者的区别就好像**根据条件判断循环因子进行循环就是在向周围伸出`触手`，进行了几次循环就向外伸出多少条触手，而这个创建相邻节点就像是根据题意在`制造一个机甲部件`。那么这个机甲部件长啥样呢？看其他部件长啥样就行，比如看队列里的元素是什么，如果是队列里的元素是字符串，就照猫画虎构造出字符串；如果是数字，就构造出一个数字。具体如何构造，这就得看具体逻辑了**，构造完成之后就等待与触手的结合点进行拼接：`queue.push(该节点)`

```c++
String word = "hello";

// 向周围伸出N * 26条触手
for(int i = 0; i < N; i++){
  char[] charArray = word.toCharArray();

  for(int j = "a"; j < "z"; j++){
    // 创建一个机甲部件，观察队列里的部件长啥样，照猫画虎，假设是字符串
    // 则生成的也是字符串
    charArray[i] = j;
    String nextWord = String.valueOf(charArray);

    // ... 这里省略和很多代码

    if (!visited.contains(nextWord)) {
      // 与触手的结合点结合
      queue.add(nextWord);

      visited.add(nextWord);
    }
  }
}
```

本题要求二叉树的层次遍历，所以同一层的节点应该放在一起，故使用模板二。

使用队列保存每层的所有节点，每次把队列里的原先所有节点进行出队列操作，再把每个元素的非空左右子节点进入队列。因此即可得到每层的遍历。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        vector<vector<int>> res;
        while (que.size() != 0) {
            int size = que.size();
            vector<int> level;
            while (size --) {
                TreeNode* cur = que.front();
                que.pop();
                if (!cur) {
                    continue;
                }
                level.push_back(cur->val);
                que.push(cur->left);
                que.push(cur->right);
            }
            if (level.size() != 0) {
                res.push_back(level);
            }
        }
        return res;
    }
};
```

#### 四、更新（2020-8-27），源自[Sweetiee](https://leetcode-cn.com/problems/01-matrix/solution/2chong-bfs-xiang-jie-dp-bi-xu-miao-dong-by-sweetie/)

* 对于 **「Tree 的 BFS」 （典型的「单源 BFS」）** 大家都已经轻车熟路了：
  * 首先把 root 节点入队，再一层一层无脑遍历就行了。

* 对于 **「图 的 BFS」 （「多源 BFS」）** 做法其实也是一样滴～，与 「Tree 的 BFS」的区别注意以下两条就 ok 辣～
  * Tree 只有 1 个 root，而图可以有多个源点，所以首先需要把多个源点都入队；
  * Tree 是有向的因此不需要标识是否访问过，而对于无向图来说，必须得标志是否访问过哦！并且为了防止某个节点多次入队，需要在其入队之前就将其设置成已访问！