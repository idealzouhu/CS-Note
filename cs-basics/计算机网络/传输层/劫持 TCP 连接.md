## 劫持 TCP 连接

实现原理：通过二分法不断地猜测序列号，进而得到序列号，最后劫持 TCP 连接

具体实现细节：怎么利用二分法猜测？—— >  根据二分法原理，我们要知道猜测的序列号和 目标序列号之间的关系（谁大谁小）。那我们怎么知道这一关系？



后果：猜对了正确的序列号后，服务器会误认为执行劫持的机器才是真正的客户端



## 思考

提高效率的操作却为Hacker留下了路径





## 参考资料

[劫持TCP连接，这操作太骚了！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1jz421e7xa/?spm_id_from=333.880.my_history.page.click&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)