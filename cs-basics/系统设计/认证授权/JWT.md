### 什么是 JWT

JWT （JSON Web Token） 是目前最流行的跨域认证解决方案，是一种**基于 Token 的认证授权机制**。 



### JWT 的好处

-  JWT **自身包含了验证用户身份所需的所有信息**，不需要在服务端存储 Session 信息，从而减轻服务端的压力。
- JWT 是无状态的，更符合 RESTAPI 的设计原则。

[JWT 身份认证优缺点分析 | JavaGuide](https://javaguide.cn/system-design/security/advantages-and-disadvantages-of-jwt.html#有效避免了-csrf-攻击)



### JWT 组成结构

JWT 是由三部分组成的字符串，三部分用点（`.`）分隔：

- **Header（头部）**：包含令牌的类型（通常为 `JWT`）和使用的签名算法（如 `HS256` 或 `RS256`）。
- **Payload（负载）**：包含声明（Claims）。声明可以是关于用户的任何信息（如用户 ID、权限、过期时间等）。这里的声明分为三种类型：
  - **注册声明**（如 `iss`，`exp`，`sub` 等，具有标准意义）；
  - **公共声明**（用户自定义的声明）；
  - **私有声明**（两个方之间自定义的声明，通常不公开）。
- **Signature（签名）**：通过指定的算法，结合头部和负载，使用密钥（对称或非对称）对 JWT 进行签名。签名的目的是确保数据的完整性，并验证令牌的发送者。

**JWT 安全的核心在于数字签名，而数字签名的核心在于密钥。**



### 身份验证流程

无论是生成 token 还是处理 token， 这两个工作都是服务端进行处理的。

**生成 token ：**

- 用户向服务器发送用户名、密码以及验证码用于登陆系统。

- 如果用户用户名、密码以及验证码校验正确的话，服务端会返回已经签名的 Token，也就是 JWT。

**请求携带 token：** 

- 客户端每次向后端发请求都在 Header 中中的 `Authorization` 字段 带上这个 JWT 

- 服务端则解码 JWT，验证签名是否合法、检查 JWT 是否过期、提取并使用有效的声明信息来验证用户的身份和权限。





### 参考资料

[JWT 基础概念详解 | JavaGuide](https://javaguide.cn/system-design/security/jwt-intro.html)

[10分钟助你弄懂cookie、session、token 区别、用途！！！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1at421G7YC/?spm_id_from=333.1007.tianma.2-2-5.click&vd_source=52cd9a9deff2e511c87ff028e3bb01d2)