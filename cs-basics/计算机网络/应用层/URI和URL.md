# 一、统一资源标识符URI

## 1.1 什么是URI

URI 是 Uniform Resource Identifier 的缩写。RFC2396 分别对这 3 个单 词进行了如下定义。

- **Uniform**  规定统一的格式可方便处理多种不同类型的资源，而不用根据上下文 环境来识别资源指定的访问方式。另外，加入新增的协议方案（如 http: 或 ftp:）也更容易。
- **Resource**  资源的定义是“可标识的任何东西”。除了文档文件、图像或服务（例 如当天的天气预报）等能够区别于其他类型的，全都可作为资源。另 外，资源不仅可以是单一的，也可以是多数的集合体。
- **Identifier**  表示可标识的对象。也称为标识符。

综上所述，<font color="blue">**URI 就是由某个协议方案表示的资源的定位标识符**</font>。协议方案是指访问资源所使用的协议类型名称。

> 采用 HTTP 协议时，协议方案就是 http。除此之外，还有 ftp、 25 mailto、telnet、file 等。标准的 URI 协议方案有 30 种左右，由隶属于 国际互联网资源管理的非营利社团 ICANN（Internet Corporation for Assigned Names and Numbers，互联网名称与数字地址分配机构）的 IANA（Internet Assigned Numbers Authority，互联网号码分配局）管理颁布。
>
> [IANA - Uniform Resource Identifier (URI) SCHEMES（统一资源 标识符方案）](https://www.iana.org/assignments/uri-schemes/uri-schemes.xhtml)



“RFC3986：统一资源标识符（URI）通用语法”中列举了几种 URI 例 子，如下所示:

```
ftp://ftp.is.co.za/rfc/rfc1808.txt
http://www.ietf.org/rfc/rfc2396.txt
ldap://[2001:db8::7]/c=GB?objectClass?one
mailto:John.Doe@example.com
news:comp.infosystems.www.servers.unix
tel:+1-816-555-1212
telnet://192.0.2.16:80/
urn:oasis:names:specification:docbook:dtd:xml:4.1.2
```



# 1.2 URI格式

常见的URI分为：

- 绝对URI：涵盖全部必要信息
- 相对URI：指从浏览器中基本 URI 处指定的 URL， 形如 /image/logo.gif





# 二、统一资源定位符URL

## 2.1 URL 组成结构

URL（Uniform Resource Locator）包括协议、主机名、端口号、路径和查询参数、锚点。

[ URL 组成结构 | JavaGuide](https://javaguide.cn/cs-basics/network/the-whole-process-of-accessing-web-pages.html#url-的组成结构)



# 三、统一资源名称URN







# 三、URI和URL联系

URI 不仅包括 URL，还包括 URN（统一资源名称），它们之间的关系如下

![img](images/aHR0cHM6Ly9pbWcyMDE4LmNuYmxvZ3MuY29tL2Jsb2cvMTUxNTExMS8yMDIwMDEvMTUxNTExMS0yMDIwMDExMDIwMTM0MjUwMy02MDIyNDQ1NS5wbmc)



# 参考资料

图解HTTP.上野宣 （教材）

[看完这篇HTTP，跟面试官扯皮就没问题了-CSDN博客](https://blog.csdn.net/qq_36894974/article/details/103930478?ops_request_misc=%7B%22request%5Fid%22%3A%22168912444016782427479000%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=168912444016782427479000&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-103930478-null-null.142^v88^control_2,239^v2^insert_chatgpt&utm_term=HTTP协议&spm=1018.2226.3001.4187)