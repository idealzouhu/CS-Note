### 使用 API

- `next()` 读取下一个 **以空白字符（如空格、Tab、换行符等）分隔的字符串**
- `nextLine()` 读取输入的**整行文本**，包括空格，直到遇到换行符为止。
- `nextInt()` 方法会跳过空白符（包括空格和换行符），顺序读取每一个整数
- `hasNext()` 判断是否有下一个





### ACM 模式输入

```

```





[3. A+B问题III](https://kamacoder.com/problempage.php?pid=1002)

```
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        while (scanner.hasNext()) {
            int a = scanner.nextInt();
            int b = scanner.nextInt();
            if (a == 0 && b == 0) {
                break;
            }
            System.out.println(a + b);
        }
    }
}
```





字符串

```
String[] input = sc.nextline().split(",");
```









遇到的问题

nextInt()



在读取下一行的数据之前，必须再读取 nextLine()

