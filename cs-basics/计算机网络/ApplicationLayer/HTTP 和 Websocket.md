[计算机网络常见面试题总结(上) | JavaGuide](https://javaguide.cn/cs-basics/network/other-network-questions.html#websocket)



### 如何实现服务器主动推送

**在用户不做任何操作的情况下，网页能收到消息并发生变更。**

- HTTP 不断轮询， 长轮询
- webSocket



在简单场景(扫码登录)下，HTTP 轮询可以做到

但是，在复杂场景（游戏主动推送大量数据）时， HTTP 轮询就不行了





### Webocket

Webocket 是全双工的， 完美继承了 TCP 全双工的特点。





如果浏览器想将 http 请求切换成 socket ，在 http 请求头里面添加特殊的首部字段。

