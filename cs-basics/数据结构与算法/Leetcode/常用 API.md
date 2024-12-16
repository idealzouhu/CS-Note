## 位运算

[1738. 找出第 K 大的异或坐标值 - 力扣（LeetCode）](https://leetcode.cn/problems/find-kth-largest-xor-coordinate-value/?envType=daily-question&envId=2024-05-26)

```
^   重要数学知识： 对 x 异或两次 ， 结果仍是 x .   x 异或 0 依旧为 x
```





## 比较器

```
  Arrays.sort(numbers, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2; // 升序排列
            }
        });
        
        
Arrays.sort(numbers, (o1, o2) -> o1 - o2); // 升序排列


Collections.sort(results, new Comparator<Integer>() {
            public int compare(Integer num1, Integer num2) {
                return num2 - num1;
            }
});
```



比较器的compare方法返回值的含义如下：

- 如果dist[o1]小于dist[o2]，则返回一个负数，表示o1应该在o2之前。
- 如果dist[o1]等于dist[o2]，则返回0，表示o1和o2的优先级相同。
- 如果dist[o1]大于dist[o2]，则返回一个正数，表示o1应该在o2之后。

对于基本数据类型的数组排序（如`int[]`、`double[]`等），默认是**按照升序进行排序**。我们可以按照默认顺序来记住这个返回值和优先级的关系。



### 数组

```
 return new int[]{0, 1};
return new int[0];

// 数组克隆
int[][] intervals；
intervals[i].clone();
```





## Arrays

最小值：

```
 // 对数组进行排序
Arrays.sort(nums);

// 获取最小数（排序后的数组的第一个元素）
int min = nums[0];

 // 使用 Arrays.stream() 将数组转换为流，然后调用 min() 方法获取最小值（ Optional 对象）
 int min = Arrays.stream(nums).min().getAsInt();
 
 Arrays.sort(nums);
```





数组初始化：

```
boolean[] used = new boolean[nums.length];
Arrays.fill(used, false);

boolean[][] dp = new boolean[len][target + 1];
// 遍历每一行，填充每一行的元素为 false
for (int i = 0; i < len; i++) {
    Arrays.fill(dp[i], false);
}
```



数组长度 和 克隆

在 Java 中，数组是一种特殊的对象，数组对象有一个公共的 `length` 属性，用于表示数组的长度。因此，**可以直接通过数组对象的 `length` 属性获取数组的长度，而不需要调用方法**。

`size()` 方法通常用于获取集合（如列表、集、映射等）的大小，`length` 属性则通常用于获取数组的长度，length()方法 用于得到字符串的长度

```
int[] originalArray = {1, 2, 3, 4, 5};

// 获取数组长度
int length = originalArray.length;
System.out.println("数组长度：" + length);

// 克隆数组
int[] clonedArray = originalArray.clone();
```



将 `dp` 数组转换为一个固定大小的 `List`

```
Integer[] dp;
Arrays.asList(dp);
```

其中，数据类型必须为 包装类型。

将 `dp` 数组转换为一个非固定大小的 `List`

```
Integer[] dp = {1, 2, 3};
List<Integer> list = new ArrayList<>(Arrays.asList(dp));
```



```
Arrays.equals(arr1, arr2);
```



## 包装类

```java
Integer.MIN_VALUE

Integer.MAX_VALUE
```



通常使用的时候，以 [1491. 去掉最低工资和最高工资后的工资平均值 - 力扣（LeetCode）](https://leetcode.cn/problems/average-salary-excluding-the-minimum-and-maximum-salary/?envType=daily-question&envId=2024-05-03) 为例，min 要赋值最大值，这样在后面的循环中才能正确地更新 min

```
int min = Integer.MAX_VALUE;
```



## Math 库

```java
Math.max(pre + x, x);
Math.min();
Math.abs()
```







## String

长度

```
		String str = "Hello, world!";
        
        // 获取字符串长度
        int length = str.length();
```

```
substring（i， j） ： 提取字符串的子串 [i, j)
```



数组和String 之间相互转换 [49. 字母异位词分组 - 力扣（LeetCode）](https://leetcode.cn/problems/group-anagrams/solutions/520469/zi-mu-yi-wei-ci-fen-zu-by-leetcode-solut-gyoc/?envType=study-plan-v2&envId=top-100-liked)

```
String str;
char[] array = str.toCharArray();

String key = new String(array);
```



String 切割

```
String s = "abcdbcd";
String[] arr = s.split("b");  // 分割字符串 "abcdbcd" 使用字符 'b' 为分隔符
String[] arr = s.split("bc");  // 使用 "bc" 作为分隔符
```



```
  String number = "-7";
 
  // 返回基本类型 int
  int result = Integer.parseInt(number);
  // 返回基本类型 Integer
  Integer result2 = Integer.valueOf(number);
```



常见的错误:

```
# 不合法.String 类是不可变类，这意味着你无法直接修改字符串中的字符。
s.charAt(i)  = ‘b’
```



## StringBuider

| 方法                                      | 描述                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| `append(String str)`                      | 将指定的字符串追加到此字符序列。                             |
| `insert(int offset, String str)`          | 将字符串插入此字符序列中。                                   |
| `delete(int start, int end)`              | 删除此字符序列的子字符串，该子字符串从指定的 `start` 位置开始，到 `end-1` 位置结束。 |
| `deleteCharAt(int index)`                 | 移除此字符序列指定位置的 char。                              |
| `replace(int start, int end, String str)` | 用指定字符串中的字符替换此字符序列中的子字符串。             |
| `reverse()`                               | 将此字符序列用其反转形式取代。                               |
| `charAt(int index)`                       | 返回指定索引处的 char 值。                                   |
| `toString()`                              | 返回此序列中数据的字符串表示形式。                           |



## 排序

`Collections.sort()` 方法接受一个 `List` 参数，并对其进行升序排序。

如果要对 `List` 中的元素进行降序排序，可以使用 `Collections.revers()` 方法来反转列表





## ArrayList

| 方法                        | 描述                                                  |
| --------------------------- | ----------------------------------------------------- |
| `add(E e)`                  | 向 ArrayList 尾部添加一个元素。                       |
| `add(int index, E element)` | 在指定索引位置插入一个元素。                          |
| `get(int index)`            | 获取指定索引位置的元素。                              |
| `set(int index, E element)` | 替换指定索引位置的元素。                              |
| `remove(int index)`         | 移除指定索引位置的元素。                              |
| `remove(Object o)`          | 移除第一个匹配的指定元素。                            |
| `size()`                    | 返回 ArrayList 的大小（元素个数）。                   |
| `isEmpty()`                 | 判断 ArrayList 是否为空。                             |
| `clear()`                   | 清空 ArrayList 中的所有元素。                         |
| `contains(Object o)`        | 判断 ArrayList 是否包含指定元素。                     |
| `indexOf(Object o)`         | 返回指定元素第一次出现的索引，如果不存在则返回 -1。   |
| `lastIndexOf(Object o)`     | 返回指定元素最后一次出现的索引，如果不存在则返回 -1。 |

长度

```
List<String> list = new ArrayList<>();
int size = list.size(); // 获取列表的大小
```



泛型使用时的注意事项

```
// 当你使用嵌套泛型时，应该在右边使用与左边完全一致的泛型类型
// List<Integer> 和 ArrayList<Integer> 不是相同的泛型
List<List<Integer>> ans = new ArrayList<ArrayList<Integer>>(); // 错误
 
 
// 同时，我们使用菱形操作符来简化
 List<List<Integer>>  ans = new ArrayList<>(); // 正确
```



添加元素

```java
// Arrays.asList() 会返回一个定长的 List，然后将它传递给 ArrayList 构造函数来进行初始化
List<String> list = new ArrayList<>(Arrays.asList("item1", "item2", "item3", "item4"));

// 使用 Collections.addAll() 添加多个元素
List<String> list = new ArrayList<>();
Collections.addAll(list, "item1", "item2", "item3", "item4");

```



ArrayList 构造方法如何处理数组

构造方法

```java
Map<String, List<String>> map = new HashMap<>();

....

return new ArrayList<List<String>>(map.values());
```



```java
// 参数 a 会作为 ArrayList 的初始化数据来源， a 必须是一个实现了 Collection 接口的集合对象
List<String> list = new ArrayList<>(a);

// 例如
List<Integer> a = Arrays.asList(1, 2, 3);
ArrayList<Integer> list = new ArrayList<>(a);
System.out.println(list); // 输出：[1, 2, 3]
```







`int[]` 只是一个数组类型，**数组类型本身是一个引用类型**。尽管数组包含基本数据类型，但在 Java 中，数组被视为对象（引用类型）。

```
 // List 里面的元素可以是 数组
 List<int[]> ans = new ArrayList<>();
```





```
// 将 ArrayList 中的元素转换为一个 Object 数组
public Object[] toArray()

// 将 ArrayList 中的元素复制到指定类型的数组里面。如果指定的数组足够大，ArrayList 元素将被放入该数组中；否则，将创建一个新数组。
public <T> T[] toArray(T[] a)

// 使用例子
List<int[]> ans = new ArrayList<>();
ans.toArray( new int[ans.size()][] )
```





## Map  哈希表

| 方法                                                 | 描述                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| `put(key, value)`                                    | 将指定的键值对存储到 HashMap 中。**如果键已存在，则替换对应的值。** |
| `get(key)`                                           | 返回指定键所映射的值，如果键不存在，则返回 null。            |
| `remove(key)`                                        | 删除 HashMap 中指定键所映射的值。如果键不存在，则不进行任何操作。 |
| `containsKey(key)`                                   | 判断 HashMap 中是否包含指定的键。                            |
| `containsValue(value)`                               | 判断 HashMap 中是否包含指定的值。                            |
| `size()`                                             | 返回 HashMap 中键值对的数量。                                |
| `isEmpty()`                                          | 判断 HashMap 是否为空。                                      |
| `clear()`                                            | 清空 HashMap 中的所有键值对。                                |
| `keySet()`                                           | 返回 HashMap 中所有键构成的 Set 集合。                       |
| `values()`                                           | 返回 HashMap 中所有值构成的 Collection 集合。                |
| `entrySet()`                                         | 返回 HashMap 中所有键值对(Map.entry)构成的 Set 集合。        |
| `putAll(map)`                                        | 将指定 Map 中的所有键值对存储到 HashMap 中。                 |
| `replace(key, value)`                                | 替换 HashMap 中指定键所映射的值。如果键不存在，则不进行任何操作。 |
| `putIfAbsent(key, value)`                            | 将指定的键值对存储到 HashMap 中，仅当键不存在时才执行存储操作。 |
| `computeIfAbsent(K key, Function remappingFunction)` | 判断一个map中是否存在这个key，如果存在则处理value的数据，如果不存在，则创建一个满足value要求的数据结构放到value中<br>返回值为 value |
| `getOrDefault(key, defaultValue)`                    | 返回指定键对应的值，如果键不存在，则返回指定的默认值。       |



```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> hashtable = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            if (hashtable.containsKey(target - nums[i])) {
                return new int[]{hashtable.get(target - nums[i]), i};
            }
            hashtable.put(nums[i], i);
        }
        return new int[0];
    }
}
```



双括号 `{{ }}` 是一个语法技巧，被称为**双括号初始化**。它的本质是创建一个匿名类（即 `HashMap` 的匿名子类），然后在其中包含一个实例初始化块。

<font color="red">**实例初始化块** `{}` </font>允许你在对象构造时执行代码块，就像构造函数一样。因此在这个匿名类的初始化块中，可以调用 `put()` 方法来初始化 `HashMap`。

```
  private static final Map<Character,Character> map = new HashMap<Character,Character>(){{
        put('{','}'); put('[',']'); put('(',')'); put('?','?');
    }};
```



判断一个map中是否存在这个key，如果存在则处理value的数据，如果不存在，则创建一个满足value要求的数据结构放到value中.

```
 for (int i = 0; i < n; i++) {
            pos.computeIfAbsent(nums.get(i), x -> new ArrayList<>()).add(i);
        }
```

代码中，我们使用了匿名函数 lambda 表达式 **x -> new ArrayList<>()** 作为重新映射函数，prices.computeIfAbsent() 将 lambda 表达式返回的新值关联到 x。





[347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/?envType=study-plan-v2&envId=top-100-liked)

```
// 将 Map 的 values 转为 Integer[]，再转换为 int[]
int[] frequence = map.values().stream().mapToInt(Integer::intValue).toArray();

```







## Map.Entry

`Map.Entry` 是 Java 中的一个接口，用于表示 Map 中的键值对。它定义了一组方法，可以获取键和值，并且允许在迭代 Map 时访问 Map 中的键值对。





以 [447. 回旋镖的数量 - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-boomerangs/solutions/994189/hui-xuan-biao-de-shu-liang-by-leetcode-s-lft5/?envType=daily-question&envId=2024-05-04) 为例，HashMap 的遍历

```
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("orange", 3);

for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println("Key: " + key + ", Value: " + value);
}
```





## HashSet

| 方法                | 描述                                                       |
| ------------------- | ---------------------------------------------------------- |
| `add(element)`      | 向集合中添加指定的元素。如果元素已存在，则不进行任何操作。 |
| `remove(element)`   | 从集合中删除指定的元素。如果元素不存在，则不进行任何操作。 |
| `contains(element)` | 判断集合中是否包含指定的元素。                             |
| `size()`            | 返回集合中元素的数量。                                     |
| `isEmpty()`         | 判断集合是否为空。                                         |
| `clear()`           | 清空集合中的所有元素。                                     |
| `iterator()`        | 返回集合的迭代器，用于遍历集合中的元素。                   |



```
Set<String> set = new HashSet<>();
```



## Deque 栈

事实上，`Deque` 还提供有 `push()` 和 `pop()`  `size()`  等其他方法，可用于模拟栈。

peek() :  查看栈顶元素;  当栈为空时，`peek()` 方法返回 `null`。



```
 Deque<Character> stack = new LinkedList<Character>();
        for (int i = 0; i < n; i++) {
            char ch = s.charAt(i);
            if (pairs.containsKey(ch)) {
                if (stack.isEmpty() || stack.peek() != pairs.get(ch)) {
                    return false;
                }
                stack.pop();
            } else {
                stack.push(ch);
            
        }
```

```
Deque<Character> stack = new LinkedList<Character>();
```



```
Deque<Integer> stack = new ArrayDeque<Integer>();
```





## Queue

普通的队列

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



在优先队列（Priority Queue）中，元素通常按照其优先级进行排序。对于整数元素，通常情况下，较小的整数被认为具有更高的优先级，因此会被放置在队列的前面。

```
class Solution {
    public int magicTower(int[] nums) {
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>();
        int ans = 0;
        long hp = 1, delay = 0;
        for( int num : nums){
            pq.offer(num);
            hp += num;
            if(hp <= 0){
                int cur =  pq.poll();      // 调整一次 
                hp -= cur;
                delay += cur;
                ans += 1;
    
            }
        }
        hp += delay;  // delay 可以认为是降序排列的元素的和
        return hp < 0 ? -1 : ans;

    }
}
```





## 流

分析一下以下语句

```

List<StringBuilder> res = new ArrayList<>();
String[] answer = new String[res.size()];
       for(int i = 0; i < res.size(); i++){
            answer[i] = res.get(i).toString();
       } 
       
       
       String[] answer = res.stream().map(StringBuilder::toString).toArray(String[]::new);
```

