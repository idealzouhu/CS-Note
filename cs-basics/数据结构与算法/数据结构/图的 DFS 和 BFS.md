## 一、联通图的遍历

**深度优先搜索（DFS）** 和 **广度优先搜索（BFS）** 都适用于遍历连通图。在连通图中，这两种遍历算法可以从任意一个顶点开始，遍历完所有的顶点，且不会遗漏任何一个顶点。





## 二、非联通图的遍历

在非连通图中，如果你使用 **DFS** 或 **BFS** 从某个顶点开始遍历，你只能遍历到与该顶点相连的顶点，即同一个连通分量中的所有顶点。但是，其他不相连的顶点（属于不同连通分量的顶点）是无法被遍历到的。

因此，我们需要从非连通图中每个联通分量中选择初始点，<font color="red">**多次调用遍历过程**</font>。同时，我们**需要一个数组判断顶点是否已经访问**。





### 2.1 DFS 遍历 + 邻接矩阵

#### 2.1.1 模板

```java
public void dfs(char[][] grid, int x, int y, boolean[][] visited) {
    // 辅助方向数组，用于上下左右移动
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int rows = grid.length;
    int cols = grid[0].length;

    // 访问当前节点
    visited[x][y] = true;

    // 遍历四个方向的邻居
    for (int[] direction : directions) {
        int newX = x + direction[0];
        int newY = y + direction[1];

        // 检查新位置是否在有效范围内且未访问过
        if (newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY] && grid[newX][newY] == '.') {
            // 递归访问邻居节点
            dfs(grid, newX, newY, visited);
        }
    }
}
```



#### 2.1.2 应用案例

在遍历的时候一定要注意终止条件，避免图里面的循环。

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





### 2.2 BFS 遍历

#### 2.2.1 模板

在 BFS 遍历的 `for` 循环里，通常我们会在循环的内部标记当前节点为已访问

- **避免重复访问**：如果不在 `for` 循环内标记节点为已访问，当前层级的其他节点也可能把该节点再次加入队列，导致重复访问。
- **避免错误的路径判断**：BFS 是逐层扩展的算法，每一层级代表着离起始点的距离。通过在 `for` 循环内标记节点为已访问，可以确保该节点只会被从距离它最近的路径访问到。如果放到循环外，有可能会导致一些路径在未访问标记的情况下重复进入队列，从而干扰最终的路径长度判断。

```java
public void bfs(char[][] grid, int startX, int startY) {
    // 辅助方向数组，用于上下左右移动
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int rows = grid.length;
    int cols = grid[0].length;
    boolean[][] visited = new boolean[rows][cols];
    Queue<int[]> queue = new LinkedList<>();

    // 初始化：将起始节点加入队列并标记为已访问
    queue.offer(new int[]{startX, startY});
    visited[startX][startY] = true;

    while (!queue.isEmpty()) {
        int[] node = queue.poll();
        int x = node[0], y = node[1];

        // 对四个方向的邻居进行遍历
        for (int[] direction : directions) {
            int newX = x + direction[0];
            int newY = y + direction[1];

            // 检查新位置是否在有效范围内且未访问过
            if (newX >= 0 && newX < rows && newY >= 0 && newY < cols && !visited[newX][newY] && grid[newX][newY] == '.') {
                // 标记为已访问并将该节点加入队列
                visited[newX][newY] = true;
                queue.offer(new int[]{newX, newY});
            }
        }
    }
}
```





#### 实际应用

[1926. 迷宫中离入口最近的出口 - 力扣（LeetCode）](https://leetcode.cn/problems/nearest-exit-from-entrance-in-maze/?envType=study-plan-v2&envId=graph-theory)





## 三、DFS 遍历和 BFS 遍历的区别

### 3.1 应用场景

DFS 遍历无法求出最短路径，而 BFS 遍历可以求出最短路径。





## 四、Leetcode 刷题

[743. 网络延迟时间](https://leetcode.cn/problems/network-delay-time/description/)：最短路径问题，使用 迪杰斯特拉算法来解决

[994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/solutions/124765/fu-lan-de-ju-zi-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 多源BFS。在图上进行多源广度优先搜索，实际上多源广度优先搜索可以理解为特殊的单源广度优先搜索。

深度遍历

[547. 省份数量](https://leetcode.cn/problems/number-of-provinces/description/)：邻接矩阵 + DFS。

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/description/) ： 该题是[岛屿类问题](https://leetcode.cn/problems/number-of-islands/solutions/211211/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-)中的一类问题，解法主要是如何在图上进行深度优先搜索

[1926. 迷宫中离入口最近的出口 ](https://leetcode.cn/problems/nearest-exit-from-entrance-in-maze/description/?envType=study-plan-v2&envId=graph-theory)：邻接矩阵 + BFS。