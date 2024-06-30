[【VS code找不到头文件】成功解决 （检测到Include错误，请更新includePath）（明明有头文件，却找不到）_vs为啥一直说include错误_老子姓李！的博客-CSDN博客](https://blog.csdn.net/qq_44078824/article/details/119904218)







# <stdio.h>等系统C文件找不到

查看你的GCC编译器的路径

```
(base) zouhu@ubuntu:~/Desktop$ which gcc
/usr/bin/gcc
```

然后， 在json文件里面修改`compilerPath`

![image-20230929101307868](C:\Users\zouhu\AppData\Roaming\Typora\typora-user-images\image-20230929101307868.png)