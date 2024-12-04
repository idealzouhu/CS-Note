## 一、DFS 遍历

### 1.1 代码模板

可以根据自己的习惯调整代码模板。

#### 1.1.1 DFS 遍历 + 邻接矩阵

DFS 遍历的关键点如下：

- **防止重复遍历**： 利用 `visited` 数组来实现。

  - **避免死循环**：在没有终止条件和 `visited` 数组的情况下，DFS 可能会重复遍历某个节点，从而导致死循环。在存在终止条件的情况下，我们有可能省略 `visited` 数组。 

  - **数组实现**：邻接矩阵自身也可以用来作为 visited 数组。如果使用邻接矩阵自身作为筛选条件的话，个人判断`grid[newX][newY] == '.'`  这种类似的条件是否可能也要变化。

- **遍历终止条件**：根据题意设置有效范围和具体条件，防止无效递归。遍历终止条件放在 for 循环里面会更好一点，但要在调用 dfs 之间确定起点不满足终止条件。

- **遍历方向**：模板适用于上下左右的四方向邻接矩阵，也可以扩展到[任意连接方式](https://leetcode.cn/problems/minimum-genetic-mutation/?envType=study-plan-v2&envId=graph-theory)的图。

```java
public void dfs(char[][] grid, int x, int y, boolean[][] visited) {
     // 遍历四个方向的邻居
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    
    // 访问当前节点
    visited[x][y] = true;

    // 遍历当前节点的四个方向(可扩展到更多方向)
    for (int[] direction : directions) {
        int newX = x + direction[0];
        int newY = y + direction[1];
        
         // 判断给定位置是否是有效的下一个节点
        if ( newX >= 0 && newX < grid.length && newY >= 0 && newY < grid[0].length  // 是否在网格范围内
            && !visited[newX][newY] 											    // 是否未访问过
            && grid[newX][newY] == '.'												// 是否满足条件
           ) {
              	// 可选：判断是否满足终止条件
                // if (终止条件) {
                //     return ...; // 返回结果
                // }
 
             	// 递归访问
            	dfs(grid, newX, newY, visited);
        }
    }
}
```



### 1.2 非联通图的遍历

在非连通图中，如果你使用  **DFS** 从某个顶点开始遍历，你只能遍历到与该顶点相连的顶点，即同一个连通分量中的所有顶点。但是，其他不相连的顶点（属于不同连通分量的顶点）是无法被遍历到的。

非连通图的关键点如下: 

- 我们需要从非连通图中每个联通分量中选择初始点，<font color="red">**多次调用遍历过程**</font>。
- 同时，我们**需要一个数组判断顶点是否已经访问**。

访问非联通图的所有节点的代码如下：

```java
int[][] grid = {
    ...
};
boolean[][] visited = new boolean[grid.length][grid[0].length];

// 遍历所有节点
for(int i = 0; i < grid.length; i++){
    for(int j = 0; j < grid[0].length; j++){
        if(!visited[i][j]){
            dfs(grid, i, j, visited);
        }
    }
}
```





## 二、BFS 遍历

### 2.1 代码模板

#### 2.1.1 BFS 遍历 + 邻接矩阵

在 BFS 遍历的 `for` 循环里，通常我们会在循环的内部标记当前节点为已访问

- **避免重复访问**：如果不在 `for` 循环内标记节点为已访问，当前层级的其他节点也可能把该节点再次加入队列，导致重复访问。
- **避免错误的路径判断**：BFS 是逐层扩展的算法，每一层级代表着离起始点的距离。通过在 `for` 循环内标记节点为已访问，可以确保该节点只会被从距离它最近的路径访问到。如果放到循环外，有可能会导致一些路径在未访问标记的情况下重复进入队列，从而干扰最终的路径长度判断。
- **终止条件**：起始点的终止条件代码要单独写出来，while 循环里面的终止条件没有判断起始点是否满足
- **遍历方向**：需要明确的是，这个并不是通用的模板。遍历方向并不一定是上下左右，例如 [433. 最小基因变化 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-genetic-mutation/submissions/580743298/?envType=study-plan-v2&envId=graph-theory)

```java
public void bfs(char[][] grid, int startX, int startY) {
    int rows = grid.length;
    int cols = grid[0].length;
    boolean[][] visited = new boolean[rows][cols];				 // 记录访问状态的数组
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};     // 辅助方向数组，用于上下左右移动(可扩展到更多方向)
  
    // 初始化 BFS 队列
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[]{startX, startY});     // 起始点入队
    visited[startX][startY] = true;				// 标记为已访问

    while (!queue.isEmpty()) {
        // 弹出当前节点
        int[] node = queue.poll();

        //  遍历当前节点的四个方向(可扩展到更多方向)
        for (int[] direction : directions) {
            int newX = node[0] + direction[0];
            int newY = node[1] + direction[1];

            // 检查新位置是否在有效范围内且未访问过
            if ( newX >= 0 && newX < grid.length && newY >= 0 && newY < grid[0].length   // 是否在网格范围内
                 && !visited[newX][newY] 											     // 是否未访问过
                 && grid[newX][newY] == '.'												 // 是否满足条件
               ) {
                
                     // 可选：判断是否满足终止条件
                     // if (终止条件) {
                     //     return ...; // 返回结果
                     // }
                
                    // 标记为已访问并将该节点加入队列
                    visited[newX][newY] = true;
                    queue.offer(new int[]{newX, newY});
            }
        }
        
    }
}
```



#### 2.1.2 BFS 遍历 + 分层

在某些场景中（如计算最短路径），需要记录 BFS 的层级（step）。

如果需要明确弹出节点为那一层，可以使用以下模板。

```java
public void bfs(char[][] grid, int startX, int startY) {
    int rows = grid.length;
    int cols = grid[0].length;
    boolean[][] visited = new boolean[rows][cols];				 // 记录访问状态的数组
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};     // 辅助方向数组，用于上下左右移动(可扩展到更多方向)
   
    // 初始化 BFS 队列
    Queue<int[]> queue = new LinkedList<>();
    queue.offer(new int[]{startX, startY});     // 起始点入队
    visited[startX][startY] = true;				// 标记为已访问
    // 可选：判断是否满足终止条件
    // if (终止条件) {
    //     return steps + 1; // 返回结果
    // }
	
    // 开始 BFS 遍历
    int steps = 0;								// 层级计数器
    while (!queue.isEmpty()) {
        int size = queue.size();			    // 当前层的节点数
        
        // 遍历当前层的所有节点
        for (int i = 0; i < size; i++) {
        	int[] node = queue.poll();

            // 遍历当前节点的四个方向(可扩展到更多方向)
            for (int[] direction : directions) {
                int newX = node[0] + direction[0];
                int newY = node[1] + direction[1];

                // 检查新位置是否在有效范围内且未访问过
                if ( newX >= 0 && newX < grid.length && newY >= 0 && newY < grid[0].length   // 是否在网格范围内
                     && !visited[newX][newY] 											     // 是否未访问过
                     && grid[newX][newY] == '.'												 // 是否满足条件
                   ) {

                     // 可选：判断是否满足终止条件
                     // if (终止条件) {
                     //     return steps + 1; // 返回结果
                     // }

                     // 标记为已访问并将该节点加入队列
                     visited[newX][newY] = true;
                     queue.offer(new int[]{newX, newY});
                }
            }
        }
        
        // 一层遍历完成后增加步数
        steps++
    }
}
```



#### 2.1.3 多源 BFS 遍历

多源 BFS 的核心思想是将**多个起点同时加入队列，并在同一层次进行扩展**。这种方法通常用于计算从多个起点到目标的最短路径、覆盖范围等问题。

多源 BFS 遍历的关键点在于 **多个起点初始化**。具体案例有 [542. 01 矩阵 - 力扣（LeetCode）](https://leetcode.cn/problems/01-matrix/description/?envType=study-plan-v2&envId=graph-theory)

```java
public void multiSourceBFS(char[][] grid, List<int[]> sources) {
    // 辅助方向数组，用于上下左右移动
    int[][] directions = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    int rows = grid.length;
    int cols = grid[0].length;
    boolean[][] visited = new boolean[rows][cols];
    Queue<int[]> queue = new LinkedList<>();
    
    // 初始化：将所有起点加入队列并标记为已访问
    for (int[] source : sources) {
        int x = source[0], y = source[1];
        queue.offer(new int[]{x, y});
        visited[x][y] = true;
    }
	
    // 开始 BFS 遍历
    int steps = 0; // 用于记录步数或层次（根据需要）
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] node = queue.poll();
            int x = node[0], y = node[1];

            // 对四个方向的邻居进行遍历
            for (int[] direction : directions) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                // 检查新位置是否在有效范围内且未访问过
                if (newX >= 0 && newX < rows && newY >= 0 && newY < cols 
                    && !visited[newX][newY] 
                    && grid[newX][newY] == '.') { // 替换 '.' 为有效条件

                    // 如果有终止条件可以在此检查
                    // if (someCondition(newX, newY)) return steps + 1;
                    
                    // 标记为已访问并将该节点加入队列
                    visited[newX][newY] = true;
                    queue.offer(new int[]{newX, newY});                
                }
            }
        }

        // 一层遍历完成后增加步数
        steps++;
    }
    
    // 如果没有提前终止，最终结果可根据问题要求返回
    // System.out.println("Steps: " + steps);
}
```





### 2.2 非联通图的遍历

在非连通图中，如果你使用  **BFS** 从某个顶点开始遍历，你只能遍历到与该顶点相连的顶点，即同一个连通分量中的所有顶点。但是，其他不相连的顶点（属于不同连通分量的顶点）是无法被遍历到的。

非连通图的关键点如下: 

- 我们需要从非连通图中每个联通分量中选择初始点，<font color="red">**多次调用遍历过程**</font>。
- 同时，我们**需要一个数组判断顶点是否已经访问**。

访问非联通图的所有节点的代码如下：

```java
int[][] grid = {
    ...
};
boolean[][] visited = new boolean[grid.length][grid[0].length];

// 遍历所有节点
for(int i = 0; i < grid.length; i++){
    for(int j = 0; j < grid[0].length; j++){
        if(!visited[i][j]){
            bfs(grid, i, j, visited);
        }
    }
}
```









## 三、Leetcode 刷题

[Leetcode 题目](https://leetcode.cn/studyplan/graph-theory/) 总结了图论的基础知识，如标准遍历、广度优先搜索、01矩阵、矩阵图、图论、并查集、拓扑排序、Dijkstra 算法和最小生成树等内容。



### 3.1 深度搜索遍历

#### 3.1.1 具体题目

[547. 省份数量](https://leetcode.cn/problems/number-of-provinces/description/)：邻接矩阵 + DFS。

[841. 钥匙和房间 ](https://leetcode.cn/problems/keys-and-rooms/description/?envType=study-plan-v2&envId=graph-theory)：邻接表 + DFS。该题比较简单，判断图是否为连通图。

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/description/) ： 该题是[岛屿类问题](https://leetcode.cn/problems/number-of-islands/solutions/211211/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-)中的一类问题，解法主要是如何在图上进行深度优先搜索.

[1020. 飞地的数量](https://leetcode.cn/problems/number-of-enclaves/description/?envType=study-plan-v2&envId=graph-theory)：非联通图 + DFS 遍历。本题在遍历邻居时，做了额外的判断。**当需要额外的条件来判断时，尽量使用全局变量**，不要使用方法返回值（避免增加复杂度）。

[802. 找到最终的安全状态](https://leetcode.cn/problems/find-eventual-safe-states/description/?envType=study-plan-v2&envId=graph-theory)：DFS  + 拓扑排序。关键点在于认识到本题主要在求如何判断是否有环



#### 3.1.2 经验教训



### 3.2 广度搜索遍历

#### 3.2.1 具体题目

[1926. 迷宫中离入口最近的出口 ](https://leetcode.cn/problems/nearest-exit-from-entrance-in-maze/description/?envType=study-plan-v2&envId=graph-theory)：邻接矩阵 + BFS。

[433. 最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/?envType=study-plan-v2&envId=graph-theory): BFS。此题的关键在于如何将题目抽象成为图，进而不像传统岛屿类题目那样只有上下左右遍历。<font color="red">**每次基因变化可以看作图中节点之间的边**</font>，而 **BFS** 非常适合解决这类最短路径问题。

[1091. 二进制矩阵中的最短路径 - 力扣（LeetCode）](https://leetcode.cn/problems/shortest-path-in-binary-matrix/description/?envType=study-plan-v2&envId=graph-theory)：BFS。遍历方向包括邻接矩阵的 8 个方向，而不是常规的上下左右。

[934. 最短的桥](https://leetcode.cn/problems/shortest-bridge/description/?envType=study-plan-v2&envId=graph-theory)：DFS + BFS。这题的关键还是在于如何利用 BFS 求最短路径。

[994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/solutions/124765/fu-lan-de-ju-zi-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 多源BFS。在图上进行多源广度优先搜索，实际上多源广度优先搜索可以理解为特殊的单源广度优先搜索。

[1306. 跳跃游戏 III ](https://leetcode.cn/problems/jump-game-iii/?envType=study-plan-v2&envId=graph-theory)：BFS + visited 数组。

[542. 01 矩阵 ](https://leetcode.cn/problems/01-matrix/description/?envType=study-plan-v2&envId=graph-theory)：多源 BFS 遍历。



#### 3.2.2 经验教训

- 根据题目构造图，图不一定是邻接矩阵。图的遍历方向不一定是常规的上下左右遍历。（[433. 最小基因变化](https://leetcode.cn/problems/minimum-genetic-mutation/?envType=study-plan-v2&envId=graph-theory)）





### 3.3 DFS 遍历和 BFS 遍历的区别

- 深度优先搜索（DFS） 和 广度优先搜索（BFS） 都适用于遍历连通图。在连通图中，这两种遍历算法可以从任意一个顶点开始，遍历完所有的顶点，且不会遗漏任何一个顶点。
- DFS 遍历无法求出最短路径，而 BFS 遍历可以求出最短路径。