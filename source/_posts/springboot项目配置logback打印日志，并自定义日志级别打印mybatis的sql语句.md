---
title: springboot项目配置logback打印日志，并自定义日志级别打印mybatis的sql语句
date: 2019-11-07 22:39:31
tags:
    - Java
    - SpringBoot
    - LogBack
categories:
        - 后端
---
#### 本文旨在教你如何在springboot项目配置logback打印日志，并自定义日志级别打印mybatis的sql语句

前言：该博客主要是记录自己成长的点滴，当然也希望能够帮助到读者，本人小白一枚，难免会出错，如果文中有任何错误的地方，请务必留言指出，感激不尽，大佬们不喜勿喷，谢谢~~~
<!-- more -->
##### 默认情况下，springboot项目就会用logback来记录日志，并输出到控制台。实际开发中我们不需要直接添加logback日志依赖。 你会发现spring-boot-starter 其中包含了spring-boot-starter-logging，该依赖内容就是 springboot 默认的日志框架logback。

#### 第一步，创建logback-spring.xml 配置文件，并且放在 src/main/resources下，如下
提示：根据不同的日志系统，你可以按如下规则组织配置文件名，就能被正确加载：
###### Logback：logback-spring.xml, logback-spring.groovy, logback.xml, logback.groovy 
###### Log4j：log4j-spring.properties, log4j-spring.xml, log4j.properties, log4j.xml 
###### Log4j2：log4j2-spring.xml, log4j2.xml 
###### JDK (Java Util Logging)：logging.properties
springboot官方推荐优先使用带有 -spring 的文件名作为你的日志配置（如比使用 logback-spring.xml ，而不是logback.xml），命名为logback-spring.xml的日志配置文件，spring boot可以为它添加一些spring boot特有的配置项（下面会提到）。 

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- 此xml在spring-boot-1.5.3.RELEASE.jar里 -->
    <include resource="org/springframework/boot/logging/logback/defaults.xml" />
    <include resource="org/springframework/boot/logging/logback/console-appender.xml" />
    <!-- 开启后可以通过jmx动态控制日志级别(springboot Admin的功能) -->
    <!--<jmxConfigurator/>-->

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!--<File>/home/hfw-client/hfw_log/stdout.log</File>-->
        <File>D:/log/hfw-client/hfw_log/stdout.log</File>
        <encoder>
            <pattern>%date [%level] [%thread] %logger{60} [%file : %line] %msg%n</pattern>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 添加.gz 历史日志会启用压缩 大大缩小日志文件所占空间 -->
            <!--<fileNamePattern>/home/hfw-client/hfw_log/stdout.log.%d{yyyy-MM-dd}.log</fileNamePattern>-->
            <fileNamePattern>D:/log/hfw-client/hfw_log/stdout.log.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory><!--  保留30天日志 -->
        </rollingPolicy>
    </appender>
    <!-- 设置包打印日志级别 -->
    <logger name= "com.jinhaoxun.acapply.dao.applyMapper" level="TRACE" />
    <logger name= "com.jinhaoxun.acapply.dao.shiroMapper" level="TRACE" />

    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
</configuration>
```
###### 解释：
<logger name= "com.jinhaoxun.acapply.dao.applyMapper" level="TRACE" /> 此处name是扫描需要打印sql语句的mapper包，可配置多个，而level则是打印日志的级别。
###### 级别分为：TRACE < DEBUG < INFO < WARN < ERROR < FATAL
只能展示大于或等于设置的日志级别的日志；也就是说springboot默认级别为INFO，那么在控制台展示的日志级别只有INFO 、WARN、ERROR、FATAL

#### 此时启动项目便可自动打印出mybatis的sql语句。

#### 第二步，代码里打印日志，在pom.xml文件中添加依赖，如下
```
<!-- 打印日志 @Slf4j 注解依赖 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.16</version>
</dependency>
```

#### 第三步，IDEA添加lombok插件，选择file—>setting—>plugins，搜索lombok插件进行安装，如下图
![2.jpg](https://upload-images.jianshu.io/upload_images/16847375-b27d744120656b80.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 第四步，直接在需要打印日志的类上添加 @Slf4j 注解去声明式注解日志对象，然后就直接可以使用了，如下图
![3.jpg](https://upload-images.jianshu.io/upload_images/16847375-26780d64db001a56.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### log.后面的方法名代表打印级别，包括log.trace()，log.debug()，log.info()，log.warn()，log.error()等方法，打印结果如下

![4.jpg](https://upload-images.jianshu.io/upload_images/16847375-4cd2233e6000aca6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

后记：本次的“springboot项目配置logback打印日志，并自定义日志级别打印mybatis的sql语句”教程到此结束，有任何意见或建议请留言，谢谢~~~
