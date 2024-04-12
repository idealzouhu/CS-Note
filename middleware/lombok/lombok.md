

## 注解

### @AllArgsConstructor

生成全参构造函数（类中的字段定义语句顺序，为函数中参数的顺序），@NonNull标记的字段会以NULL检查标记的字段。

```java
@AllArgsConstructor
public class CustomerServices {
    @NonNull
    private String id;
    private String name;
    private String email;
    private String pwd;
    private int age;
    private  boolean sex; // true 男   false 女

}

//编译后生成的CustomerServices.class文件

public CustomerServices(@NonNull final String id, final String name, final String email, final String pwd, final int age, final boolean sex) {
        if (id == null) {
            throw new NullPointerException("id is marked non-null but is null");
        } else {
            this.id = id;
            this.name = name;
            this.email = email;
            this.pwd = pwd;
            this.age = age;
            this.sex = sex;
        }
    }
```

