# SpringBoot 单模块

[手摸手之创建SpringBoot单模块 (yuque.com)](https://www.yuque.com/magestack/12306/vyyb2fa1lv3pd0xp)





# SpringBoot多模块

[手摸手之创建SpringBoot多模块 (yuque.com)](https://www.yuque.com/magestack/12306/fx2og75a98hg5gz9)





打开 Parent 项目的根 Pom.xml 文件，新增下方代码。

```
<packaging>pom</packaging>

<modules>
    <module>bootdemo-remote-api</module>
</modules>
```





创建后的 Module 项目的 Parent 信息是 SpringBoot 配置，修改后如下。

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>com.nageoffer.demo</groupId>
		<artifactId>spring-boot-module-demo</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<artifactId>bootdemo-remote-api</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>bootdemo-remote-api</name>
	<description>bootdemo-remote-api</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
</project>

```

