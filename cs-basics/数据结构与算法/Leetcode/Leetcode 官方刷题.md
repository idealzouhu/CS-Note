>  [LeetCode 热题 100 - 学习计划 - 力扣（LeetCode）](https://leetcode.cn/studyplan/top-100-liked/) 
>
>  [Re：从零开始的力扣刷题生活](https://leetcode.cn/circle/discuss/E3yavq/) 

## 一、算法目录

### 模拟

最基本的思路，在某种程度上类似蛮力法

[2079. 给植物浇水 ](https://leetcode.cn/problems/watering-plants/?envType=daily-question&envId=2024-05-08)

[2644. 找出可整除性得分最大的整数](https://leetcode.cn/problems/find-the-maximum-divisibility-score/description/) ： 初始值设置为 -1 ， 可以避免一些特殊情况

[1535. 找出数组游戏的赢家](https://leetcode.cn/problems/find-the-winner-of-an-array-game/submissions/533140088/?envType=daily-question&envId=2024-05-19)



### 前后缀分解

[1652. 拆炸弹](https://leetcode.cn/problems/defuse-the-bomb/solutions/2765768/python3javacgotypescript-yi-ti-shuang-ji-lk9a/?envType=daily-question&envId=2024-05-05)： 数组的倒数元素可以通过循环遍历取模实现





### 滑动窗口

> 滑动窗口提单: [分享丨【题单】滑动窗口（定长/不定长/多指针） - 力扣（LeetCode）](https://leetcode.cn/circle/discuss/0viNMK/)

[3. 无重复字符的最长子串 ](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-100-liked)：窗口多指针。使用两个指针表示字符串中的某个子串（或窗口）的左右边界， 利用哈希集合判断是否有重复的字符。

[1052. 爱生气的书店老板](https://leetcode.cn/problems/grumpy-bookstore-owner/submissions/526349960/?envType=daily-question&envId=2024-04-23)

[2831. 找出最长等值子数组](https://leetcode.cn/problems/find-the-longest-equal-subarray/description/?envType=daily-question&envId=2024-05-23): 窗口不定长

[438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/description/?envType=study-plan-v2&envId=top-100-liked): 窗口定长。比较经典的一种题目。





### 贪心算法

贪心算法的难点在于如何证明一系列的局部最优解能够找到最优解。

[45. 跳跃游戏 II ](https://leetcode.cn/problems/jump-game-ii/description/?envType=study-plan-v2&envId=top-100-liked)：经典贪心算法。可以正向思考，也可以反向思考。



### 动态规划

求最值的问题，当前问题可以缩减成子问题

**(1) 动态规划**

[118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/description/?envType=study-plan-v2&envId=top-100-liked)： 动态规划。dp 数组初始化稍微有点不同。

[53. 最大子数组和 ](https://leetcode.cn/problems/maximum-subarray/solutions/228009/zui-da-zi-xu-he-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 动态规划。$f(i)$ 代表以第 i 个数结尾的「连续子数组的最大和」。值得注意的是，最大子数组和并不需要用到滑动窗口。

[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/solutions/822940/wan-quan-ping-fang-shu-by-leetcode-solut-t99c/?envType=study-plan-v2&envId=top-100-liked)：完全背包 + 动态规划。状态转移方程涉及多个子问题，当前  `dp[i]` 的解需要通过遍历多个子问题  `dp[i - j^2]`  来确定。

[322. 零钱兑换](https://leetcode.cn/problems/coin-change/solutions/132979/322-ling-qian-dui-huan-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：完全背包 + 动态规划。解题思路和[完全平方数](https://leetcode.cn/problems/perfect-squares/solutions/822940/wan-quan-ping-fang-shu-by-leetcode-solut-t99c/?envType=study-plan-v2&envId=top-100-liked)基本类似，状态转移方程涉及多个子问题，但是部分子问题是无解的。同时，dp 数组所有元素的初始化也比较有意思。

[300. 最长递增子序列 ](https://leetcode.cn/problems/longest-increasing-subsequence/description/?envType=study-plan-v2&envId=top-100-liked)：动态规划。状态转移方程涉及多个子问题。

[152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/solutions/250015/cheng-ji-zui-da-zi-shu-zu-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked): 动态规划 + 多个 dp 数组。此题的关键由于正负性对乘积的影响，使用多个 dp 数组来分类讨论。

**(2) 多维动态规划**

多维动态规划的状态转移方程比较难推理。事实上，在已经确定 dp 数组的时候，状态转移方程一定要根据前面的 dp 数组元素来推理。

[62. 不同路径 ](https://leetcode.cn/problems/unique-paths/description/?envType=study-plan-v2&envId=top-100-liked)：多维动态规划。

[1143. 最长公共子序列 ](https://leetcode.cn/problems/longest-common-subsequence/description/?envType=study-plan-v2&envId=top-100-liked)：多维动态规划。

[5. 最长回文子串 ](https://leetcode.cn/problems/longest-palindromic-substring/description/?envType=study-plan-v2&envId=top-100-liked)： 多维动态规划。本题枚举子串长度和左边界， 而不是依次枚举左边界和右边界。这个是根据当前问题如何缩减成自问题来判断的。

[64. 最小路径和 ](https://leetcode.cn/problems/minimum-path-sum/submissions/568504855/?envType=study-plan-v2&envId=top-100-liked)：动态规划。这个二维数组 dp 的初始化稍微有点特殊。

[72. 编辑距离 ](https://leetcode.cn/problems/edit-distance/submissions/575145767/?envType=study-plan-v2&envId=top-100-liked)：多维动态规划。该题的关键在于如何初始化，以及如何推理动态转移方程。



### 双指针

双指针可以将两趟 for 循环做到的事只用 一趟for 循环即可实现。

[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/?envType=study-plan-v2&envId=top-100-liked)：  利用双指针消除两个链表的长度差，找到两个链表的相交点

[283. 移动零](https://leetcode.cn/problems/move-zeroes/?envType=study-plan-v2&envId=top-100-liked)

[11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/solutions/207215/sheng-zui-duo-shui-de-rong-qi-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：双指针作为容器的左右边界。由于容纳的水量为 两个指针指向的数字中较小值∗指针之间的距离， 每次只用移动数字较小的指针即可。

[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-100-liked)：滑动窗口多指针 + 哈希表。使用两个指针表示字符串中的某个子串（或窗口）的左右边界， 利用哈希集合判断是否有重复的字符。

[15. 三数之和](https://leetcode.cn/problems/3sum/?envType=study-plan-v2&envId=top-100-liked)：双指针 + 排序。该题最重要的是如何通过排序来去重，找到不重复的三元组。

[75. 颜色分类](https://leetcode.cn/problems/sort-colors/?envType=study-plan-v2&envId=top-100-liked)：双指针 。关键在于如何交换数据。



### 二分查找





### 回溯

[78. 子集](https://leetcode.cn/problems/subsets/description/?envType=study-plan-v2&envId=top-100-liked)：子集问题。对于输入的数组 nums，考虑每个 nums[i] 是选还是不选，由此组合出 2^n个不同的子集。

[17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/solutions/388738/dian-hua-hao-ma-de-zi-mu-zu-he-by-leetcode-solutio/?envType=study-plan-v2&envId=top-100-liked)：组合问题 + 多阶树。

[39. 组合总和](https://leetcode.cn/problems/combination-sum/?envType=study-plan-v2&envId=top-100-liked)：组合问题 + 二阶树。

[22. 括号生成 ](https://leetcode.cn/problems/generate-parentheses/submissions/570509199/?envType=study-plan-v2&envId=top-100-liked)：组合问题 + 二阶树 + 限制条件。关键点在于选择分支时存在条件限制。

[79. 单词搜索](https://leetcode.cn/problems/word-search/?envType=study-plan-v2&envId=top-100-liked)：搜索问题 + 树枝去重 + 图 DFS。利用 visited 数组标识每个位置是否被访问过，给出了在图中如果进行回溯法的案例。



### 贪心算法

[55. 跳跃游戏](https://leetcode.cn/problems/jump-game/description/?envType=study-plan-v2&envId=top-100-liked) 经典贪心算法。关键点在于证明每一步所作的贪婪选择最终导致问题的整体最优解。




### DFS 和 BFS

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/description/) ： 该题是[岛屿类问题](https://leetcode.cn/problems/number-of-islands/solutions/211211/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-)中的一类问题，解法主要是如何在图上 DFS





## 二、结构目录

### 子串

[3. 无重复字符的最长子串 ](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-100-liked)：滑动窗口多指针 + 哈希表。使用两个指针表示字符串中的某个子串（或窗口）的左右边界， 利用哈希集合判断是否有重复的字符。

[560. 和为 K 的子数组个数](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked)：前缀和 + 哈希表。前缀和为 pre - k 的元素，与当前元素前缀和为 pre 的元素，和刚好为k。同时，前缀和直接等于 k 的情况较为特殊，应当初始化 `map.put(0, 1)`



### 数组

[53. 最大子数组和 ](https://leetcode.cn/problems/maximum-subarray/solutions/228009/zui-da-zi-xu-he-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 动态规划。$f(i)$ 代表以第 i 个数结尾的「连续子数组的最大和」。值得注意的是，最大子数组和并不需要用到滑动窗口。

[56. 合并区间](https://leetcode.cn/problems/merge-intervals/solutions/203562/he-bing-qu-jian-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：排序算法。实现的语法稍微复杂。

[238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/?envType=study-plan-v2&envId=top-100-liked)：前缀和问题。

[189. 轮转数组](https://leetcode.cn/problems/rotate-array/description/?envType=study-plan-v2&envId=top-100-liked): 模运算。空间复杂度较高，后续可以优化。





### 字符串

[2981. 找出出现至少三次的最长特殊子字符串 I ](https://leetcode.cn/problems/find-longest-special-substring-that-occurs-thrice-i/description/)：分类讨论问题，没有用到什么算法，但是**逻辑比较复杂**



### 矩阵

[73. 矩阵置零 ](https://leetcode.cn/problems/set-matrix-zeroes/solutions/669901/ju-zhen-zhi-ling-by-leetcode-solution-9ll7/?envType=study-plan-v2&envId=top-100-liked):  利用问题本身提供的数组来当作标记数组，节省空间以便实现原地算法

[54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/description/?envType=study-plan-v2&envId=top-100-liked)：螺旋遍历矩阵。

[48. 旋转图像 ](https://leetcode.cn/problems/rotate-image/description/?envType=study-plan-v2&envId=top-100-liked): 推理过程很复杂，不是很会做。

[240. 搜索二维矩阵 II ](https://leetcode.cn/problems/search-a-2d-matrix-ii/description/?envType=study-plan-v2&envId=top-100-liked)：从右上角开始搜索。如果当前值比目标值大，则向左移动一列；如果当前值比目标值小，则向下移动一行。另外，可以利用二分查找法进行优化。



### 栈

[155. 最小栈](https://leetcode.cn/problems/min-stack/description/?envType=study-plan-v2&envId=top-100-liked): 自定义栈。根据栈的特点，可以在常数时间内找出栈中的最小元素。

[394. 字符串解码](https://leetcode.cn/problems/decode-string/description/?envType=study-plan-v2&envId=top-100-liked)：辅助栈。利用辅助栈来处理括号内嵌套括号。

单调栈的使用方法：

[496. 下一个更大元素 I ](https://leetcode.cn/problems/next-greater-element-i/submissions/528626883/)：经典的单调栈使用方法。

[42. 接雨水 ](https://leetcode.cn/problems/trapping-rain-water/submissions/566234094/?envType=study-plan-v2&envId=top-100-liked)： 极度困难。









### 哈希表

哈希表通常是用来快速判断一个值是否存在，关键在于如何设计 key 和 value。

同时，在使用哈希表并且不需要 value 信息的过程中，我们也可以使用 HashSet 来替换 HashMap。

[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/): 利用哈希表快速确定根节点在中序遍历数组中的位置

[49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/description/?envType=study-plan-v2&envId=top-100-liked)：哈希表 + 排序。排序后的字母异位词具有相同的键。

[128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/?envType=study-plan-v2&envId=top-100-liked)：哈希表。使用 HashSet 快速判断某个元素是否存在。以 以 num 为连续序列的起点， 则 nums[] 数组不包元素 num -1。



### 链表

在链表中，双指针通常用来降低时间复杂度，减少一趟遍历。同时，在环形链表等题型中，双指针还可以用来降低空间复杂度，避免使用哈希表。

**(1) 链表基本操作**

[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/submissions/566240336/?envType=study-plan-v2&envId=top-100-liked)： 链表插入节点 + 哑节点。

[19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/?envType=study-plan-v2&envId=top-100-liked)：链表删除节点 + 双指针 +哑节点。 

[24. 两两交换链表中的节点 ](https://leetcode.cn/problems/swap-nodes-in-pairs/?envType=study-plan-v2&envId=top-100-liked)： 链表交换节点 + 递归。

[138. 随机链表的复制 ](https://leetcode.cn/problems/copy-list-with-random-pointer/submissions/569419046/?envType=study-plan-v2&envId=top-100-liked): 链表创建+哈希表。哈希表用于避免重复创建相同的节点，并解决链表中可能出现的循环问题。这个有点类似 Spring 的三级缓存机制。

[146. LRU 缓存](https://leetcode.cn/problems/lru-cache/description/?envType=study-plan-v2&envId=top-100-liked)：双向链表 + 哈希表。难点在于数据结构的选型和双向链表的基本操作。链表头节点为最近使用的结点。

**(2) 双指针**

[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/?envType=study-plan-v2&envId=top-100-liked)：  利用双指针消除两个链表的长度差，找到两个链表的相交点

[21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/?envType=study-plan-v2&envId=top-100-liked)：使用哑节点优化代码。

[2. 两数相加 ](https://leetcode.cn/problems/add-two-numbers/solutions/435246/liang-shu-xiang-jia-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：双指针直接遍历两个链表即可。

**(3) 快慢指针**

[234. 回文链表 ](https://leetcode.cn/problems/palindrome-linked-list/description/)：快慢指针。利用快慢指针找到链表的中间位置。

[141. 环形链表 ](https://leetcode.cn/problems/linked-list-cycle/description/?envType=study-plan-v2&envId=top-100-liked)：快慢指针， 或者使用哈希表判断是否有重复的点。该题比较简单，仅需判断是否有环。

[142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/solutions/441131/huan-xing-lian-biao-ii-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：快慢指针 + 数学推理。此题的关键在于数学推理：当快慢指针相遇时，如果指针 ptr 从链表头部再走 a 的距离，指针 ptr 和 慢指针 slow 最终在环的入口相遇。





### 树

树的大部分题目都离不开递归。同时，我们要熟练掌握树的各种遍历方式及其特点。

[114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/description/) :  将右子树插入到左子树的最右节点, 左子树插入到根节点的右子树位置. 

[124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/?envType=study-plan-v2&envId=top-100-liked)：后序遍历。逻辑较为复杂。

[437. 路径总和 III ](https://leetcode.cn/problems/path-sum-iii/?envType=study-plan-v2&envId=top-100-liked)：DFS + 多重递归。在求路径总和的时候，不一定要使用加法，使用减法可能更加方便，尤其是在使用递归的时候。



### 图论

[743. 网络延迟时间](https://leetcode.cn/problems/network-delay-time/description/)：最短路径问题，使用 迪杰斯特拉算法来解决

[994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/solutions/124765/fu-lan-de-ju-zi-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 在图上进行多源广度优先搜索，实际上多源广度优先搜索可以理解为特殊的单源广度优先搜索。

深度遍历

[547. 省份数量](https://leetcode.cn/problems/number-of-provinces/description/)：邻接矩阵 + DFS。

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/description/) ： 该题是[岛屿类问题](https://leetcode.cn/problems/number-of-islands/solutions/211211/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-)中的一类问题，解法主要是如何在图上进行深度优先搜索







### 堆

[215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/?envType=study-plan-v2&envId=top-100-liked)：堆排序 + 大顶堆。将大顶堆中的堆顶元素移动 k - 1 次到有序区， 大顶堆元素此时为第 K 个最大元素。

[347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/description/?envType=study-plan-v2&envId=top-100-liked)：堆排序 + 小顶堆。设置堆中的元素只为 k 个， 将剩余的元素逐个添加到堆里面，移去堆顶（最小值）。此时，堆中的 k 个元素为前 k 个高频元素。





### 技巧

[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/description/?envType=study-plan-v2&envId=top-100-liked)：利用异或运算的特性，在线性时间复杂度和常数空间复杂度中找到数组里面只出现一次的数字。





## 尚未解决的题目

