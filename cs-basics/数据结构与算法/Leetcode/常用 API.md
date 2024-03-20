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







# String

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





# List

长度

```
List<String> list = new ArrayList<>();
int size = list.size(); // 获取列表的大小
```

