

### 为什么需要应用层协议

TCP 协议是基于字节流的。

因此，TCP 协议存在**粘包问题**， 无法区分01串之间的边界。

为了解决这个问题，上层协议用消息头 + 消息体的格式包装该数据。





### HTTP 协议和 RPC 协议

**HTTP 主要用于 B/S 架构**， 

**RPC 更多用于 C/S 架构**， 性能要比 HTTP/1.1 好一点







### 参考资料

[3.8 既然有 HTTP 协议，为什么还要有 RPC？ | 小林coding (xiaolincoding.com)](https://xiaolincoding.com/network/2_http/http_rpc.html)

[既然有HTTP为什么还要有RPC？_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Qv4y127B4/?spm_id_from=333.788&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)