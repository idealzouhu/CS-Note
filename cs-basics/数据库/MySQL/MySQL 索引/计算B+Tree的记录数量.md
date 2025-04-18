## 预备知识

在B-tree中，非叶子节点存储的是索引页，叶子节点存储的是数据页。

**页的大小固定为 16KB**。

在索引页中，行记录主要有：

- id ：对应页中记录的最小记录 id 值；
- 页号：地址是指向对应页的指针，固定为 4B。

在数据页中，**行记录按照 Compact 格式来存储**。





### 计算过程

**索引页：** 假设内容区的大小为 15KB，索引页中其他的辅助信息大小为 1KB 左右。假设Id 为 8B， 则行记录为 12B。则索引页中行记录数目为 15*1024/12 ≈ 1280

**数据页**：假设行数据为 1KB， 数据页中其他辅助信息大小为 1KB 左右， 则数据页中的行记录数目为 15 

对于二层B+树， 总记录数 ≈ 1280 * 15 = 19200

对于三层B+树， 总记录数 ≈ 1280 * 1280 * 15 = 2457600 ≈ 2.45kw





### 经验

MySQL 单表不要超过 2000W 行