[认证授权基础概念详解 | JavaGuide](https://javaguide.cn/system-design/security/basis-of-authority-certification.html#如何使用-session-cookie-方案进行身份验证)







## Session 的缺陷

Session 主要使用 Cookie 来保存 SessionID。但是，Cookie 难以防止 **CSRF(Cross Site Request Forgery)** 攻击。因此，Session 难以 CSRF 攻击。

> 为了解决 Session 缺陷，Token 因此被引入



在前后端分离架构下，session不支持跨域请求







# 参考资料

[10分钟助你弄懂cookie、session、token 区别、用途！！！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1at421G7YC/?spm_id_from=333.1007.tianma.2-2-5.click&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)