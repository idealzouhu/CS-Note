## 错误格式匹配

当格式不匹配时, printf 和 fprintf 执行自动类型转换会出现问题

**(1) printf**

运行以下代码:

```
int intValue = 123456789; 
printf("intValue: %f\n", intValue);  // intValue: 0.000000
```

根据 C 语言的规范，`printf` 函数会尝试将 `intValue` 转换为一个 `double` 类型，然后打印它的浮点数值。由于整数没有小数部分，因此转换为浮点数后会显示为 `0.000000`。

**(2) fprintf**

运行以下代码:

```
FILE *file = fopen("output.txt", "w");
int intValue = 123;
fprintf(file, "intValue: %f\n", intValue);
```

`output.txt` 文件里面的内容如下:

```
intValue: 0.000000
```

