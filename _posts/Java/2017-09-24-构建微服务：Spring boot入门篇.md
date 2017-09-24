---
layout: post
title: 构建微服务：Spring boot入门篇
categories: Linux
description: 构建微服务：Spring boot入门篇
keywords: Spring boot, 微服务
---
## 1. 什么是spring boot

Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。它可以脱离tomcat等外部容器直接运行，快速构建微服务(比如短信服务、邮件服务)。

## 2. 使用spring boot有什么好处

平时如果我们需要搭建一个spring web项目的时候需要怎么做呢？

1. 配置web.xml，加载spring和spring mvc
2. 配置数据库连接、配置spring事务
3. 配置加载配置文件的读取，开启注解
4. 配置日志文件
...

配置完成之后部署tomcat 调试
...

现在非常流行微服务，如果我这个项目仅仅只是需要发送一个邮件,我都需要这样折腾一遍!
 
但是如果使用spring boot呢？
很简单，我仅仅只需要非常少的几个配置就可以迅速方便的搭建起来一套web项目或者是构建一个微服务！

## 3. 快速入门
### 3.1 maven构建项目

1、访问http://start.spring.io/

2、选择构建工具Maven Project、Spring Boot版本1.3.6以及一些工程基本信息，点击“Switch to the full version.”java版本选择1.7，可参考下图所示：
                                                                                             
![Alt text](https://github.com/gongenbo/gongenbo.github.io/raw/master/img/linux/20170924_springboot.png)   

3、点击Generate Project下载项目压缩包

4、解压后，使用eclipse、idea编辑

主类添加测试代码

```java
package com.example.springbootdemo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class SpringbootDemoApplication {
	@RequestMapping("/hello")
	public String hello(){
		return"Hello world!";
	}
	public static void main(String[] args) {
		SpringApplication.run(SpringbootDemoApplication.class, args);
	}
}
```
maven pom依赖

```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>springboot-demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springboot-demo</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.7.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<!--spring-boot-start-web包含了spring webmvc和tomcat等web开发的特性-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<!--如果我们要直接Main启动spring，那么以下plugin必须要添加，否则是无法启动的。如果使用maven的spring-boot:run的话是不需要此配置的。（我在测试的时候，如果不配置下面的plugin也是直接在Main中运行的。）-->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<compilerVersion>1.7</compilerVersion>
					<source>1.7</source>
					<target>1.7</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>

```

5、 将项目打成jar包
 `mvn package`
 
## 4、 运行项目

### 4.1 方法一：独立运行(不依赖tomcat容器) 
运行： `java -jar  springboot-demo-0.0.1-SNAPSHOT.jar`

测试：

方法一：从浏览器直接访问"http://localhost:8080/hello"

方法二：在服务器端直接使用curl请求
`curl "http://localhost:8080/hello"`

返回`hello word!!`代表请求成功


### 4.2 方法二：在tomcat中运行
把spring-boot项目按照平常的web项目一样发布到tomcat容器下

一、修改打包形式

在pom.xml里设置 <packaging>war</packaging>

二、移除嵌入式tomcat插件

在pom.xml里找到spring-boot-starter-web依赖节点，在其中添加如下代码，

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <!-- 移除嵌入式tomcat插件 -->
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

三、添加servlet-api的依赖

下面两种方式都可以，任选其一

```java
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

```java
<dependency>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>tomcat-servlet-api</artifactId>
    <version>8.0.36</version>
    <scope>provided</scope>
</dependency>
```

四、修改启动类，并重写初始化方法

我们平常用main方法启动的方式，都有一个App的启动类，代码如下：

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

我们需要类似于web.xml的配置方式来启动spring上下文了，在Application类的同级添加一个SpringBootStartApplication类，其代码如下:

```java
/**
 * 修改启动类，继承 SpringBootServletInitializer 并重写 configure 方法
 */
public class SpringBootStartApplication extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        // 注意这里要指向原先用main方法执行的Application启动类
        return builder.sources(Application.class);
    }
}
```

五、打包部署

在项目根目录下（即包含pom.xml的目录），在命令行里输入： 
mvn clean package即可， 等待打包完成，出现[INFO] BUILD SUCCESS即为打包成功。 
然后把target目录下的war包放到tomcat的webapps目录下，启动tomcat，即可自动解压部署。 
最后在浏览器中输入

http://localhost:[端口号]/[打包项目名]/
发布成功

http://localhost:8080/springboot-demo-0.0.1-SNAPSHOT/hello

方法二：直接在服务器使用curl请求：`http://localhost:8080/springboot-demo-0.0.1-SNAPSHOT/hello`


项目地址：https://gitee.com/gongenbo/springboot-demo

## 5、其他
### 5.1 修改spring boot默认的8080端口为其它端口

方法一：可以通过实现EmbeddedServletContainerCustomizer接口来实现，代码：

```java
import javafx.application.Application;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.context.embedded.ConfigurableEmbeddedServletContainer;
import org.springframework.boot.context.embedded.EmbeddedServletContainerCustomizer;
import org.springframework.boot.context.web.SpringBootServletInitializer;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@EnableAutoConfiguration
public class TestClass extends SpringBootServletInitializer implements EmbeddedServletContainerCustomizer{
    @RequestMapping("/")
    String home() {
        return "hi...";
    }

    public static void main(String[] args) {
        SpringApplication.run(TestClass.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(Application.class);
    }

    @Override
    public void customize(ConfigurableEmbeddedServletContainer container) {
        container.setPort(8081);
    }
}
```

方法二：更加简单
在src/resource下增加文件application.properties
加入server.port=8081
即可


