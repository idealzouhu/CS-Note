## 一、Token 概述

### 1.1 什么是 Token

Token（令牌）是在计算机科学和网络安全领域中广泛使用的概念，它通常用于**表示某种凭证或标识符**，用于验证用户身份、授权访问或进行安全交换。



### 1.2 Token 应用场景

Token作为一种**轻量级的凭证机制**，具有灵活、安全、可扩展等特点，在身份验证、授权、访问控制、会话管理等场景中被广泛应用。具体来说，Token可以分为多种类型，适用于不同的 token ：

- **身份验证 Token**：用于验证用户的身份。例如，用户登录时系统会颁发一个包含用户信息的身份验证 Token，用户在后续请求中使用该 Token 来证明自己的身份。
- **访问令牌（Access Token）**：用于授权访问特定资源。例如，OAuth 2.0 中的访问令牌用于授权客户端访问受保护的资源服务器上的资源。

- **会话令牌（Session Token）**：用于管理用户会话。例如，Web 应用程序中的会话令牌可以用于标识用户的会话状态，并在用户与服务器之间保持会话。





## 二、身份 Token

身份验证 Token 是一种用于验证用户身份的令牌，通常用于在网络通信中证明用户的身份。它是一种轻量级的凭证，用于在不安全的环境中传递和验证用户身份信息。

身份验证 Token 在身份验证过程中发挥着关键的作用，它通常包含了用户的身份信息、权限范围、有效期等相关信息。用户在通过身份验证后，系统会颁发一个包含用户身份信息的 Token，用户在后续的请求中需要携带该 Token，以证明自己的身份。

身份验证 Token 的使用使得系统能够实现无状态的身份验证机制，避免了传统的基于会话的身份验证方式带来的服务器端存储和管理的负担。同时，通过加密和签名等技术手段，身份验证 Token 也能够提供较高的安全性，防止了身份信息被窃取或篡改。







### 2.1 Token 生成和验证机制

身份验证 Token 的工作流程通常如下：

1. 用户提供身份信息（如用户名和密码）进行身份验证。
2. 身份验证成功后，服务器会生成一个身份验证 Token 给用户。这个Token可能是一个短字符串，它包含诸如用户ID，时间戳，以及其他重要信息。**为了保证Token的安全，这个Token会被加密**。
3. 用户在后续的请求中携带 Token，将其发送给服务端。
4. 服务端接收到 Token 后，验证 Token 的有效性和真实性。
5. 如果 Token 有效且真实，则认为用户是合法的，允许用户访问相关资源或执行相关操作。





## 三、细节补充

## session vs token

- **session**是需要空间进行存储的，如果是多服务器**session**需要同步信息，但是**token**在服务器是可以不需要存储用户信息的。
- **token**可以使用浏览器的**localStorge**等，**APP**也可以使用自带[数据库存储](https://cloud.tencent.com/product/crs?from_column=20065&from=20065)字符串。且不会出现**cookies**出现跨域问题。
- **token**可以用**JWT**来携带部分不太敏感的信息比如用户**ID**等，服务器只要解密**token**即可使用部分信息。