>  [LeetCode 热题 100 - 学习计划 - 力扣（LeetCode）](https://leetcode.cn/studyplan/top-100-liked/) 
>
>  [Re：从零开始的力扣刷题生活](https://leetcode.cn/circle/discuss/E3yavq/) 

## 一、算法目录

### 模拟

最基本的思路，在某种程度上类似蛮力法

[2079. 给植物浇水 - 力扣（LeetCode）](https://leetcode.cn/problems/watering-plants/?envType=daily-question&envId=2024-05-08)

[2644. 找出可整除性得分最大的整数 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-maximum-divisibility-score/description/) ： 初始值设置为 -1 ， 可以避免一些特殊情况

[1535. 找出数组游戏的赢家 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-winner-of-an-array-game/submissions/533140088/?envType=daily-question&envId=2024-05-19)



### 前后缀分解

[1652. 拆炸弹 - 力扣（LeetCode）](https://leetcode.cn/problems/defuse-the-bomb/solutions/2765768/python3javacgotypescript-yi-ti-shuang-ji-lk9a/?envType=daily-question&envId=2024-05-05)： 数组的倒数元素可以通过循环遍历取模实现





### 滑动窗口

> 滑动窗口提单: [分享丨【题单】滑动窗口（定长/不定长/多指针） - 力扣（LeetCode）](https://leetcode.cn/circle/discuss/0viNMK/)

[3. 无重复字符的最长子串 ](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-100-liked)：窗口多指针。使用两个指针表示字符串中的某个子串（或窗口）的左右边界， 利用哈希集合判断是否有重复的字符。

[1052. 爱生气的书店老板](https://leetcode.cn/problems/grumpy-bookstore-owner/submissions/526349960/?envType=daily-question&envId=2024-04-23)

[2831. 找出最长等值子数组](https://leetcode.cn/problems/find-the-longest-equal-subarray/description/?envType=daily-question&envId=2024-05-23): 窗口不定长



### 动态规划

求最值的问题，当前问题可以缩减成子问题

[53. 最大子数组和 ](https://leetcode.cn/problems/maximum-subarray/solutions/228009/zui-da-zi-xu-he-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 动态规划。$f(i)$ 代表以第 i 个数结尾的「连续子数组的最大和」。值得注意的是，最大子数组和并不需要用到滑动窗口。

[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/solutions/822940/wan-quan-ping-fang-shu-by-leetcode-solut-t99c/?envType=study-plan-v2&envId=top-100-liked)：完全背包 + 动态规划。状态转移方程涉及多个子问题，确实相对复杂。当前  `dp[i]` 的解需要通过遍历多个子问题  `dp[i - j^2]`  来确定。

[322. 零钱兑换](https://leetcode.cn/problems/coin-change/solutions/132979/322-ling-qian-dui-huan-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：完全背包 + 动态规划。解题思路和[完全平方数](https://leetcode.cn/problems/perfect-squares/solutions/822940/wan-quan-ping-fang-shu-by-leetcode-solut-t99c/?envType=study-plan-v2&envId=top-100-liked)基本类似，状态转移方程涉及多个子问题，但是部分子问题是无解的。



### 双指针

双指针可以将两趟 for 循环做到的事只用 一趟for 循环即可实现。

[160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/?envType=study-plan-v2&envId=top-100-liked)：  利用双指针消除两个链表的长度差，找到两个链表的相交点

[283. 移动零](https://leetcode.cn/problems/move-zeroes/?envType=study-plan-v2&envId=top-100-liked)

[11. 盛最多水的容器 - 力扣（LeetCode）](https://leetcode.cn/problems/container-with-most-water/solutions/207215/sheng-zui-duo-shui-de-rong-qi-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：双指针作为容器的左右边界。由于容纳的水量为 两个指针指向的数字中较小值∗指针之间的距离， 每次只用移动数字较小的指针即可。

[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-100-liked)：滑动窗口多指针 + 哈希表。使用两个指针表示字符串中的某个子串（或窗口）的左右边界， 利用哈希集合判断是否有重复的字符。

[15. 三数之和 - 力扣](https://leetcode.cn/problems/3sum/?envType=study-plan-v2&envId=top-100-liked)：双指针 + 排序。该题最重要的是如何通过排序来去重，找到不重复的三元组。



### 回溯

[78. 子集](https://leetcode.cn/problems/subsets/description/?envType=study-plan-v2&envId=top-100-liked)：子集问题。对于输入的数组 nums，考虑每个 nums[i] 是选还是不选，由此组合出 2^n个不同的子集。

[17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/solutions/388738/dian-hua-hao-ma-de-zi-mu-zu-he-by-leetcode-solutio/?envType=study-plan-v2&envId=top-100-liked)：组合问题。






### DFS 和 BFS

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/description/) ： 该题是[岛屿类问题](https://leetcode.cn/problems/number-of-islands/solutions/211211/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-)中的一类问题，解法主要是如何在图上 DFS





## 二、结构目录

### 子串

[3. 无重复字符的最长子串 ](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/?envType=study-plan-v2&envId=top-100-liked)：滑动窗口多指针 + 哈希表。使用两个指针表示字符串中的某个子串（或窗口）的左右边界， 利用哈希集合判断是否有重复的字符。

[560. 和为 K 的子数组个数](https://leetcode.cn/problems/subarray-sum-equals-k/description/?envType=study-plan-v2&envId=top-100-liked)：前缀和 + 哈希表。前缀和为 pre - k 的元素，与当前元素前缀和为 pre 的元素，和刚好为k。同时，前缀和直接等于 k 的情况较为特殊，应当初始化 `map.put(0, 1)`



### 数组

[53. 最大子数组和 ](https://leetcode.cn/problems/maximum-subarray/solutions/228009/zui-da-zi-xu-he-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 动态规划。$f(i)$ 代表以第 i 个数结尾的「连续子数组的最大和」。值得注意的是，最大子数组和并不需要用到滑动窗口。

[56. 合并区间](https://leetcode.cn/problems/merge-intervals/solutions/203562/he-bing-qu-jian-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：排序算法。实现的语法稍微复杂。



### 字符串

[2981. 找出出现至少三次的最长特殊子字符串 I ](https://leetcode.cn/problems/find-longest-special-substring-that-occurs-thrice-i/description/)：分类讨论问题，没有用到什么算法，但是**逻辑比较复杂**



### 矩阵

[73. 矩阵置零 - 力扣（LeetCode）](https://leetcode.cn/problems/set-matrix-zeroes/solutions/669901/ju-zhen-zhi-ling-by-leetcode-solution-9ll7/?envType=study-plan-v2&envId=top-100-liked) ： 利用问题本身提供的数组来当作标记数组，节省空间以便实现原地算法



### 单调栈

[496. 下一个更大元素 I ](https://leetcode.cn/problems/next-greater-element-i/submissions/528626883/)：经典的单调栈使用方法。

[155. 最小栈 - 力扣（LeetCode）](https://leetcode.cn/problems/min-stack/description/?envType=study-plan-v2&envId=top-100-liked): 自定义栈。根据栈的特点，可以在常数时间内找出栈中的最小元素。



### 哈希表

[105. 从前序与中序遍历序列构造二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/): 利用哈希表快速确定根节点在中序遍历数组中的位置



### 链表

[160. 相交链表 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists/?envType=study-plan-v2&envId=top-100-liked)：  利用双指针消除两个链表的长度差，找到两个链表的相交点

[234. 回文链表 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-linked-list/description/)：利用快慢指针找到链表的中间位置

[21. 合并两个有序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-two-sorted-lists/?envType=study-plan-v2&envId=top-100-liked)：使用哑节点优化代码

[2. 两数相加 ](https://leetcode.cn/problems/add-two-numbers/solutions/435246/liang-shu-xiang-jia-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：双指针直接遍历两个链表即可。

[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/solutions/441131/huan-xing-lian-biao-ii-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)：快慢指针或者哈希表。这一题比较难理解，我直接用哈希表简单地解决。



### 树

树的大部分题目都离不开递归

[114. 二叉树展开为链表 - 力扣（LeetCode）](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/description/) :  将右子树插入到左子树的最右节点, 左子树插入到根节点的右子树位置. 







### 图论

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/description/) ： 该题是[岛屿类问题](https://leetcode.cn/problems/number-of-islands/solutions/211211/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-)中的一类问题，解法主要是如何在图上进行深度优先搜索

[743. 网络延迟时间](https://leetcode.cn/problems/network-delay-time/description/)：最短路径问题，使用 迪杰斯特拉算法来解决

[994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/solutions/124765/fu-lan-de-ju-zi-by-leetcode-solution/?envType=study-plan-v2&envId=top-100-liked)： 在图上进行多源广度优先搜索，实际上多源广度优先搜索可以理解为特殊的单源广度优先搜索。





### 技巧

[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/description/?envType=study-plan-v2&envId=top-100-liked)：利用异或运算的特性，在线性时间复杂度和常数空间复杂度中找到数组里面只出现一次的数字。





## 尚未解决的题目

