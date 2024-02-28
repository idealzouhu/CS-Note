[认证授权基础概念详解 | JavaGuide](https://javaguide.cn/system-design/security/basis-of-authority-certification.html#如何使用-session-cookie-方案进行身份验证)







## Session 的缺陷

Session 主要使用 Cookie 来保存 SessionID。但是，Cookie 难以防止 **CSRF(Cross Site Request Forgery)** 攻击。因此，Session 难以 CSRF 攻击。

> 为了解决 Session 缺陷，Token 因此被引入