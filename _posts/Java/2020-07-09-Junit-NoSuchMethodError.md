---
layout: post
title: IntelliJ运行Junit报错NoSuchMethodError 
categories: [Spark]
description: IntelliJ运行Junit报错NoSuchMethodError 
keywords: IntelliJ,Junit,NoSuchMethodError 
---

## 1.问题描述

我以前项目用Mac编写的，然后拷贝到了win10系统上的Idea上执行Junit测试时，运行出现如下错误：
```$xslt
Exception in thread "main" java.lang.NoSuchMethodError: org.junit.platform.commons.util.ReflectionUtils.getDefaultClassLoader()Ljava/lang/ClassLoader;
	at org.junit.platform.launcher.core.ServiceLoaderTestEngineRegistry.loadTestEngines(ServiceLoaderTestEngineRegistry.java:30)
	at org.junit.platform.launcher.core.LauncherFactory.create(LauncherFactory.java:53)
	at com.intellij.junit5.JUnit5IdeaTestRunner.createListeners(JUnit5IdeaTestRunner.java:39)
	at com.intellij.rt.execution.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:49)
	at com.intellij.rt.execution.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:242)
	at com.intellij.rt.execution.junit.JUnitStarter.main(JUnitStarter.java:70)

Process finished with exit code 1
Empty test suite.
```

我的pom.xml依赖如下
```$xslt
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <!--<scope>test</scope>-->
    </dependency>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.3.2</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.3.2</version>
      <scope>compile</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter -->
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter</artifactId>
      <version>5.6.2</version>
      <!--<scope>test</scope>-->
    </dependency>

  </dependencies>

```

期间我尝试了更换Junit的版本，使用org.junit.jupiter-api，使用org.junit.jupiter(Aggregator)包进行单元测试，宣告失败；
尝试了删除.idea与.imp文件，然后build->rebuild重新编译还是报同样的错误
尝试了File->Invalidate cache/restart 重启之后编译也还是报同样的错误


## 2.问题解决

最后找到了参考链接的帖子，虽然pom.xml中Junit用的4.12版本，但是IntelliJ IDEA尝试使用JUnit5运行我的测试用例。具体原因是junit-jupiter-api这个依赖是专门为Junit5而设计的，结果导致了这个错误。
```$java
at com.intellij.junit5.JUnit5IdeaTestRunner.createListeners(JUnit5IdeaTestRunner.java:39)

```
解决方法：删除pom.xml中的所有org.junit.jupiter依赖，然后重新运行。


## 3.参考

1. [Junit测试出现异常：java.lang.NoSuchMethodError: org.junit.platform.commons.util.ReflectionUtils](https://blog.csdn.net/baidu_27222643/article/details/75020104)
2. [Cannot run tests intelliJ - Spring project - error: java.lang.NoSuchMethodError](https://stackoverflow.com/questions/45004453/cannot-run-tests-intellij-spring-project-error-java-lang-nosuchmethoderror)
3. [mvnrepository JUnit Jupiter API ](https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api)
