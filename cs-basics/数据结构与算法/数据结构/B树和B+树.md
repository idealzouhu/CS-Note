

# 一、B树

B-树，也称为B树，是一种平衡的多叉树(可以对比一下平衡二叉查找树)，它比较适用于对外查找。

- 一个节点里面可以有多个有序排列的数据。



![image-20240406213452927](images/image-20240406213452927.png)



## 缺陷

查找性能不稳定。

范围查找问题。





# 二、B+ 树

![image-20240406213524783](images/image-20240406213524783.png)



## 优点

- B+树更加扁平。当利用B+树作为Mysql索引时，I/O 查询次数更少。

- 查询性能稳定。只有叶子节点存在数据。





## 参考资料

[一个动画搞懂MySQL索引原理！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1pJ4m1j7Pm/?spm_id_from=333.880.my_history.page.click&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)