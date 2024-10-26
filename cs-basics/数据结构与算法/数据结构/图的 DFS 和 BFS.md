## 联通图的遍历

**深度优先搜索（DFS）** 和 **广度优先搜索（BFS）** 都适用于遍历连通图。在连通图中，这两种遍历算法可以从任意一个顶点开始，遍历完所有的顶点，且不会遗漏任何一个顶点。





## 非联通图的遍历

在非连通图中，如果你使用 **DFS** 或 **BFS** 从某个顶点开始遍历，你只能遍历到与该顶点相连的顶点，即同一个连通分量中的所有顶点。但是，其他不相连的顶点（属于不同连通分量的顶点）是无法被遍历到的。

因此，我们需要从非连通图中每个联通分量中选择初始点，<font color="red">**多次调用遍历过程**</font>。同时，我们**需要一个数组判断顶点是否已经访问**。



### DFS 遍历

[200. 岛屿数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/solutions/211211/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-/)

网格 DFS 遍历的框架代码：

```
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    // 如果坐标 (r, c) 超出了网格范围，直接返回
    if (!inArea(grid, r, c)) {
        return;
    }
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```

<font color="red">**如何避免重复遍历呢？答案是标记已经遍历过的格子。**</font>以岛屿问题为例，我们需要在所有值为 1 的陆地格子上做 DFS 遍历。每走过一个陆地格子，就把格子的值改为 2，这样当我们遇到 2 的时候，就知道这是遍历过的格子了。也就是说，每个格子可能取三个值：

- 0 —— 海洋格子
- 1 —— 陆地格子（未遍历过）
- 2 —— 陆地格子（已遍历过）

```
void dfs(int[][] grid, int r, int c) {
    // 判断 base case
    if (!inArea(grid, r, c)) {
        return;
    }
    // 如果这个格子不是岛屿，直接返回
    if (grid[r][c] != 1) {
        return;
    }
    grid[r][c] = 2; // 将格子标记为「已遍历过」
    
    // 访问上、下、左、右四个相邻结点
    dfs(grid, r - 1, c);
    dfs(grid, r + 1, c);
    dfs(grid, r, c - 1);
    dfs(grid, r, c + 1);
}

// 判断坐标 (r, c) 是否在网格中
boolean inArea(int[][] grid, int r, int c) {
    return 0 <= r && r < grid.length 
        	&& 0 <= c && c < grid[0].length;
}
```





### BFS 遍历

BFS 通常用于求**最短路径问题**。

[994. 腐烂的橘子 - 力扣（LeetCode）](https://leetcode.cn/problems/rotting-oranges/solutions/129542/yan-du-you-xian-sou-suo-python3-c-by-z1m/?envType=study-plan-v2&envId=top-100-liked)





## DFS 遍历和 BFS 遍历的区别

DFS 遍历无法求出最短路径，而 DFS 遍历可以求出最短路径。

