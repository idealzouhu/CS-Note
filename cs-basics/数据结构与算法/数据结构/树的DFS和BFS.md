## 一、二叉树遍历

[102. 二叉树的层序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-level-order-traversal/solutions/244853/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/)

### DFS 遍历

DFS 遍历使用 **递归**：

```
void dfs(TreeNode root) {
    if (root == null) {
        return;
    }
    
    dfs(root.left);
    // 处理节点 root
    dfs(root.right);
}
```

在 DFS 遍历里面，调整访问根节点的位置，即可先序遍历 preorder、中序遍历inorder 或者后序遍历 postorder

[94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/?envType=daily-question&envId=2024-05-03)：给出递归和迭代两种实现方式



```
void dfs(TreeNode root) {
    Stack<TreeNode> stack = new Stack<>();
    TreeNode curr = root;

    // 只要栈不为空或者当前节点不为空，就继续遍历
    while (curr != null || !stack.isEmpty()) {
        // 不断深入左子树，将路径上的节点全部压栈
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }

        // 当左子树为空时，取出栈顶节点（即当前子树的根节点）
        curr = stack.pop();

        // 处理当前节点
        System.out.println(curr.val);  // 这里可以处理当前节点

        // 然后转向右子树
        curr = curr.right;
    }
}
```







### BFS 遍历

BFS 遍历使用**队列**数据结构：

```
void bfs(TreeNode root) {
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll(); // Java 的 pop 写作 poll()
        if (node.left != null) {
            queue.add(node.left);
        }
        if (node.right != null) {
            queue.add(node.right);
        }
    }
}
```

上面的代码**无法区分队列中的结点来自哪一层**, 我们可以将 BFS 遍历可以认为是层序遍历

```
// 二叉树的层序遍历
void bfs(TreeNode root) {
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        int n = queue.size();
        for (int i = 0; i < n; i++) { 
            // 变量 i 无实际意义，只是为了循环 n 次
            TreeNode node = queue.poll();
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
    }
}
```



> 注意，上面的层序遍历代码没有考虑 root 为 null 的情况







## 二、Leetcode 刷题





[循环右移二叉树](https://www.nowcoder.com/questionTerminal/dd954e299e534af3986a73f98dbdd8a7)：面试过程中遇到的题目，没有刷出来。