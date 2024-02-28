

# 一、Tomcat 目录结构

Tomcat 目录结构的文件树如下：

```
Tomcat_Home/
├── bin/                  # 存放Tomcat的可执行文件
│   ├── catalina.sh       # Tomcat启动和停止脚本（Unix/Linux）
│   ├── catalina.bat      # Tomcat启动和停止脚本（Windows）
│   ├── startup.sh        # 启动Tomcat脚本（Unix/Linux）
│   └── shutdown.sh       # 停止Tomcat脚本（Unix/Linux）
├── conf/                 # 存放Tomcat的配置文件
│   ├── server.xml        # Tomcat主要配置文件，包括连接器设置等
│   ├── web.xml           # Web应用程序的默认部署描述符
│   └── context.xml       # 配置Web应用程序的上下文信息，例如数据库连接池
├── lib/                  # 存放Tomcat运行所需的所有库文件
│   ├── catalina.jar      # Tomcat的核心类库
│   ├── servlet-api.jar   # Servlet API的实现
│   └── jsp-api.jar       # JSP API的实现
├── logs/                 # 存放Tomcat生成的日志文件
│   ├── catalina.out      # 控制台输出日志
│   └── localhost_access_log.txt  # 访问日志文件
├── webapps/              # 存放所有部署在Tomcat上的Web应用程序
│   ├── Your_Web_App/     # 你部署的具体Web应用程序的目录
│   │   ├── WEB-INF/      # 存放Web应用程序的配置信息和部署描述符
│   │   └── ...           # 其他Web应用程序相关的文件和目录
│   └── ...               # 其他部署在Tomcat上的Web应用程序
├── work/                 # 临时工作目录，存放JSP编译后的临时文件等
│   └── ...               # 其他临时工作文件
├── temp/                 # 临时文件目录，存放Tomcat运行时生成的临时文件
│   └── Tomcat.pid        # 包含Tomcat进程ID的文件
└── temp/Catalina/        # 存放虚拟主机的临时文件目录
    └── Your_Web_App/     # 你部署的具体Web应用程序的虚拟主机临时文件目录
        └── ...           # 其他虚拟主机相关的临时文件

```











# 参考资料

[JavaWeb 速通Tomcat-阿里云开发者社区 (aliyun.com)](https://developer.aliyun.com/article/1320690?spm=a2c6h.12873639.article-detail.71.59d615faaK7wnT&scm=20140722.ID_community@@article@@1320690._.ID_community@@article@@1320690-OR_rec-V_1-RL_community@@article@@1088114#slide-8)