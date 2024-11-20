[TOC]

## 一、Spring Security 入门案例

入门案例具体教程查看 [Hello Spring Security :: Spring Security](https://docs.spring.io/spring-security/reference/servlet/getting-started.html)

### 1.1 导入依赖

根据 [Getting Spring Security ](https://docs.spring.io/spring-security/reference/getting-spring-security.html#getting-maven-no-boot) ，在 `pom.xml` 中导入 Spring Security 的相关依赖：

```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-security</artifactId>
        <spring-security.version>6.3.4</spring-security.version>
	</dependency>
</dependencies>
```

Spring Security 会在项目中启用 [默认的身份验证和安全机制](https://docs.spring.io/spring-security/reference/servlet/getting-started.html#servlet-hello-auto-configuration)。无需额外的配置，就可以体验基本的用户认证。



### 1.2 运行项目

在不编写代码的情况下，Spring Boot 应用后，可以看到控制台输出日志：

```bash
Using generated security password: ed803299-ccd3-4ee0-8160-652eed542814

This generated password is for development use only. Your security configuration must be updated before running your application in production.
```

在没有[配置 Authentication](https://docs.spring.io/spring-security/reference/servlet/authentication/index.html) 的情况下，Spring Security 默认创建一个用户名为 `user` 的用户，并生成随机密码。该密码只用于开发环境，生产环境应自定义更安全的认证方式。



### 1.3 测试项目

测试  [Basic HTTP Authentication](https://tools.ietf.org/html/rfc7617) 。

**(1)未验证身份的请求**

通过未验证身份直接访问受保护的路径时，Spring Security 会拒绝访问，并返回 `401 Unauthorized` 状态码：

```
$ curl -i http://localhost:8080/some/path
HTTP/1.1 401
Set-Cookie: JSESSIONID=598AEA648FB18B56CB28ACC1234AC174; Path=/; HttpOnly
WWW-Authenticate: Basic realm="Realm"
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Length: 0
Date: Thu, 07 Nov 2024 03:16:39 GMT
```



**(2) 验证身份的请求**

提供用户名 `user` 和生成的密码后，可以成功通过身份验证。若请求的资源不存在，Spring Boot 将返回 `404 Not Found`：

```bash
$ curl -i -u user:ed803299-ccd3-4ee0-8160-652eed542814 http://localhost:8080/some/path
HTTP/1.1 404
Vary: Origin
Vary: Access-Control-Request-Method
Vary: Access-Control-Request-Headers
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Type: application/json
Transfer-Encoding: chunked
Date: Thu, 07 Nov 2024 03:22:50 GMT

{"timestamp":"2024-11-07T03:22:50.775+00:00","status":404,"error":"Not Found","path":"/some/path"}
```





## 二、Spring Security 简单实现案例

本案例展示了如何使用 Spring Security 实现以下功能：

1. 配置自定义用户名和密码。
2. **基于角色的授权控制**。



### 2.1 导入依赖

根据 [Getting Spring Security ](https://docs.spring.io/spring-security/reference/getting-spring-security.html#getting-maven-no-boot) ，在 `pom.xml` 中导入 Spring Security 的相关依赖：

```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-security</artifactId>
	</dependency>
</dependencies>
```



### 2.2 配置 Spring Security

根据 [Java Configuration :: Spring Security](https://docs.spring.io/spring-security/reference/servlet/configuration/java.html)，可以通过 Java 配置来自定义用户信息。同时，通过 [Authorize HttpServletRequests ](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html) 来定义不同角色的访问控制规则。

```java
/**
 * Spring Security 配置类
 * <p>
 *     基于角色的权限控制使用案例
 * </p>
 *
 * @author zouhu
 * @data 2024-11-07 11:46
 */
@Configuration
@EnableWebSecurity
public class WebSecurityConfig{
    /**
     * 配置自定义用户，并给每个用户分配角色
     */
    @Bean
    protected UserDetailsManager userDetailsService() {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();

        manager.createUser(User.withUsername("user")
                .password("{noop}123456")    // 使用 {noop} 前缀表示明文密码，仅用于开发和测试环境，生产环境推荐使用其他加密方式（如 BCrypt）。
                // .roles("USER")               // 分配 USER 角色，角色会自动添加前缀 ROLE_
                .authorities("USER")         // 使用 authorities 而不是 roles，角色不会添加前缀
                .build());

        manager.createUser(User.withUsername("admin")
                .password("{noop}admin123456")
                // .roles("ADMIN")    // 分配 ADMIN 角色，角色会自动添加前缀 ROLE_
                .authorities("ADMIN")
                .build());

        return manager;
    }

    /**
     * 配置角色的访问规则
     */
    @Bean
    public SecurityFilterChain web(HttpSecurity http) throws Exception {
        http
                .authorizeHttpRequests((authorize) -> {
                        // 管理员角色可访问的资源
                        authorize.requestMatchers("/admin/**").hasAuthority("ADMIN");

                        // 用户角色可访问的资源（注释掉的部分，如果未来需要启用可以取消注释）
                        authorize.requestMatchers("/user/**").hasAuthority("USER");

                        // 允许所有角色访问的资源
                        authorize.requestMatchers("/").permitAll();

                        // 其他所有请求都需要身份验证
                        authorize.anyRequest().authenticated();
                })
                .formLogin(withDefaults())  // 启用表单登录，默认登录页面为 /login
                .httpBasic(withDefaults()); // 启用 HTTP Basic Authentication

        return http.build();
    }
}
```





### 2.3 创建访问资源

```java
@RestController
public class DemoController {
    @GetMapping("/user/secure")
    public String userAccess() {
        return "Hello User!";
    }

    @GetMapping("/admin/secure")
    public String adminAccess() {
        return "Hello Admin!";
    }

    @RequestMapping("/")
    public String home() {
        return "Welcome to the Home Page!";
    }
}

```



### 2.4 测试身份验证和授权

**(1) 验证角色的权限**

使用 [`@WithMockUser`](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html#match-requests) 注解来模拟不同角色的用户进行权限验证。

```java
@SpringBootTest
@AutoConfigureMockMvc
class WebSecurityConfigTest {

    @Autowired
    private MockMvc mvc;

    /**
     *  测试未认证用户访问主页和受限资源
     */
    // @WithMockUser  // 未加 @WithMockUser 表示请求由未认证用户发出
    @Test
    void Unauthenticated() throws Exception {
        this.mvc.perform(get("/"))
                .andExpect(status().isOk())
                .andExpect(content().string("Welcome to the Home Page!")); // 预期返回 200 OK

        this.mvc.perform(get("/admin/secure"))
                .andExpect(status().isUnauthorized())  // 预期返回 401 Unauthorized
                .andExpect(header().string("WWW-Authenticate", "Basic realm=\"Realm\""));
    }


    /**
     * 测试默认用户（无特殊权限）的访问
     */
    @WithMockUser  // 模拟一个没有设置特定权限的默认用户
    @Test
    void NotAuthorityUser() throws Exception {
        this.mvc.perform(get("/"))
                .andExpect(status().isOk())
                .andExpect(content().string("Welcome to the Home Page!")); // 预期返回 200 OK

        this.mvc.perform(get("/user/secure"))
                .andExpect(status().isForbidden()); // 预期返回 403 Forbidden
    }

    /**
     *  测试具有 USER 权限的用户访问受限资源
     */
    @WithMockUser(authorities = "USER")
    @Test
    void UserAuthority() throws Exception {
        this.mvc.perform(get("/user/secure"))
                .andExpect(status().isOk())
                .andExpect(content().string("Hello User!"));  // 预期返回 200 OK

        this.mvc.perform(get("/admin/secure"))
                .andExpect(status().isForbidden());  // 预期返回  403 Forbidden
    }

    /**
     *  测试具有 ADMIN 权限的用户访问受限资源
     */
    @WithMockUser(authorities = "ADMIN")
    @Test
    void AdminAuthority() throws Exception {
        this.mvc.perform(get("/user/secure"))
                .andExpect(status().isForbidden()); // 预期返回 403 Forbidden

        this.mvc.perform(get("/admin/secure"))
                .andExpect(status().isOk())
                .andExpect(content().string("Hello Admin!"));  // 预期返回 200 OK
    }
}
```

**（2）验证用户是否能访问**

使用 `curl` 命令行工具模拟 HTTP 请求，验证不同用户权限。

```bash
# 未认证用户访问受限资源
$ curl -i  http://localhost:8080/admin/secure
HTTP/1.1 401
Set-Cookie: JSESSIONID=7C9FAE078D4DD87F777A6AA4836D12C8; Path=/; HttpOnly
WWW-Authenticate: Basic realm="Realm"
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Length: 0
Date: Thu, 07 Nov 2024 09:21:45 GMT

# 使用管理员账户访问受限资源
$  curl -i -u admin:admin123456 http://localhost:8080/admin/secure
HTTP/1.1 200
X-Content-Type-Options: nosniff
X-XSS-Protection: 0
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Pragma: no-cache
Expires: 0
X-Frame-Options: DENY
Content-Type: text/plain;charset=UTF-8
Content-Length: 12
Date: Thu, 07 Nov 2024 09:22:19 GMT

Hello Admin!
```





## 参考资料

[Java Configuration :: Spring Security](https://docs.spring.io/spring-security/reference/servlet/configuration/java.html#jc-httpsecurity)