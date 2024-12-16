## 一、二叉树遍历

[102. 二叉树的层序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-level-order-traversal/solutions/244853/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/)

### 1.1 DFS 遍历

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

在 DFS 遍历里面，调整访问根节点的位置，即可先序遍历 `preorder`、中序遍历 `inorder` 或者后序遍历 `postorder`

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







### 1.2 BFS 遍历

BFS 遍历使用**队列**数据结构：

```java
void bfs(TreeNode root) {
     if(root == null){
        return;
    }
    
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

```java
// 二叉树的层序遍历
void bfs(TreeNode root) {
    if(root == null){
        return;
    }
    
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

总结 [Re：从零开始的力扣刷题生活 ](https://leetcode.cn/circle/discuss/E3yavq/#树与二叉树篇) 中的题目

### 2.1 经验教训

- 树的大部分题目都离不开递归。

- 对二叉树进行遍历时，除了使用递归的方法以外，建议尝试使用非递归的方法。

- 同时，我们要熟练掌握树的各种遍历方式及其特点。

  

### 2.2 二叉树的遍历

[144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

[94. 二叉树的中序遍历 ](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

[145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

[103. 二叉树的锯齿形层序遍历 ](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/description/)：使用 LinkedList 存储遍历结果。LinkedList 支持从头部插入 addLast() 和 从尾部插入 addFirst()， 进而便于实现锯齿形层序遍历。 



### 2.3 构造二叉树

[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)：前序遍历确定根节点， **中序遍历确定子树的长度**。在每一趟遍历中，确定根节点后，可以分别确定左子树、右子树的前序遍历和中序遍历。



### 2.4 二叉树的搜索问题





### 2.5 二叉树的路径问题

[124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/?envType=study-plan-v2&envId=top-100-liked)：后序遍历。逻辑较为复杂。

[437. 路径总和 III ](https://leetcode.cn/problems/path-sum-iii/?envType=study-plan-v2&envId=top-100-liked)：DFS + 多重递归。在求路径总和的时候，不一定要使用加法，使用减法可能更加方便，尤其是在使用递归的时候。





### 2.6 二叉树的属性问题



### 2.7 二叉树公共祖先问题





### 2.8 二叉搜索树





### 2.9 平衡二叉树





### 2.10 其他

[114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/description/) :  将右子树插入到左子树的最右节点, 左子树插入到根节点的右子树位置. 



[循环右移二叉树](https://www.nowcoder.com/questionTerminal/dd954e299e534af3986a73f98dbdd8a7)：面试过程中遇到的题目，没有刷出来。





