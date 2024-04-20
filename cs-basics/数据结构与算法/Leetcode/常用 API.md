## Array

最小值：

```
 // 对数组进行排序
Arrays.sort(nums);

// 获取最小数（排序后的数组的第一个元素）
int min = nums[0];

 // 使用 Arrays.stream() 将数组转换为流，然后调用 min() 方法获取最小值（ Optional 对象）
 int min = Arrays.stream(nums).min().getAsInt();
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





## Math 库

```java
Math.max(pre + x, x);
```





## String

长度

```
		String str = "Hello, world!";
        
        // 获取字符串长度
        int length = str.length();
```



substring（i， j） ： 提取字符串的子串 [i, j)





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





## Map  哈希表

containsKey

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





## Deque 栈

事实上，`Deque` 还提供有 `push()` 和 `pop()` 等其他方法，可用于模拟栈。

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





## Queue

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

