# 一、基础语法

## 1.1 变量

```shell
# 定义变量
name="John"
age=25

# 使用变量，只要在变量名前面加美元符号$即可
echo $name
echo "My name is ${name}！"    # 加花括号是为了帮助解释器识别变量的边界

# readonly 关键字可以将变量设置为只读，即不允许修改。
readonly age

# 删除变量
unset variable_name

```



## 1.2 字符串

字符串可以用单引号，也可以用双引号，也可以不用引号。

|          | 单引号                                                       | 双引号           |
| -------- | ------------------------------------------------------------ | ---------------- |
| 变量     | <font color="red">**所有字符都会被视为普通字符，没有特殊含义**</font>；<br>不能使用变量 | 可以使用变量     |
| 转义字符 | 没有转义字符                                                 | 可以出现转义字符 |
| 其他     | 不能出现单独一个的单引号（对单引号使用转义符后也不行）       |                  |



## 1.3 数组

数组是一种用于存储多个数值或字符串的数据结构。

Shell支持一维数组，数组元素可以通过索引访问。



### 1.3.1 数组定义

```shell
# 定义数组： 使用空格分隔数组元素
my_array=(value1 value2 value3)

# 或者使用单独的语句定义数组元素
my_array[0]=value1
my_array[1]=value2
my_array[2]=value3

```



### 1.3.2 数组访问

```shell
#通过索引访问数组元素，索引从0开始。
echo ${my_array[0]}   # 输出第一个元素
echo ${my_array[1]}   # 输出第二个元素

# 使用 [@] 或 [*] 可以获取数组的所有元素。
echo ${my_array[@]}   # 输出所有元素
echo ${my_array[*]}   # 输出所有元素

```



### 1.3.3 数组长度

使用 `#` 符号可以获取数组的长度

```shell
length=${#my_array[@]}
echo "Array length: $length"

```



### 1.3.4 关联数组

Bash 支持关联数组，可以使用任意的字符串、或者整数作为下标来访问数组元素。

[Shell 数组 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-shell-array.html)





## 1.4 反引号 ` `` ` 和  `$()` 

反引号` `` ` 和 `$()` 在Shell中是一种用于<font color="blue">**执行命令并获取其输出**</font>的特殊语法。

当你使用反引号包裹命令时，Shell会执行该命令，并将其输出作为字符串返回，可以将这个字符串赋给一个变量或在脚本中直接使用。

例如，如果你想获取当前目录下的文件列表，可以使用`ls`命令，并将其输出保存到变量中：

```
file_list=`ls`
echo "Files in current directory: $file_list"
```

或者使用 `$()` 替代反引号的语法：

```
file_list=$(ls)
echo "Files in current directory: $file_list"
```



需要注意的是，反引号的用法在较新版本的Shell中仍然有效，但为了更好的可读性和嵌套能力，通常推荐使用`$()`语法。在一些较老版本的Shell中可能不支持`$()`，这时可以使用反引号。



## 二、传递参数

[Shell 传递参数 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-shell-passing-arguments.html)