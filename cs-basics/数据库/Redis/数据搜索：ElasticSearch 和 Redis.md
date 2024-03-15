[手摸手之车票搜索为什么用Redis而不是ES？ (yuque.com)](https://www.yuque.com/magestack/12306/zd9wok8w0dn8eig5#fUTKl)





查询的时候只是把日期、目的地、出发地和票的类型作为一个请求体请求数据.

返回前端再用剩下的条件来筛选

![image-20240315212835555](images/image-20240315212835555.png)