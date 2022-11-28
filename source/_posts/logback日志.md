---
title: logback日志
date: 2021-04-16 17:35:50
tags: 
  - java
  - 日志
categories:
 - 后端
---
# logback.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration>
    <!-- 彩色日志 -->
    <!-- 彩色日志依赖的渲染类 -->
    <conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter" />
    <conversionRule conversionWord="wex" converterClass="org.springframework.boot.logging.logback.WhitespaceThrowableProxyConverter" />
    <conversionRule conversionWord="wEx" converterClass="org.springframework.boot.logging.logback.ExtendedWhitespaceThrowableProxyConverter" />
    <!-- 彩色日志格式 -->
    <property name="CONSOLE_LOG_PATTERN" value="${CONSOLE_LOG_PATTERN:-%clr(%d{HH:mm:ss.SSS}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}}"/>
    <!-- ch.qos.logback.core.ConsoleAppender 表示控制台输出 -->
    <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
                <pattern>${CONSOLE_LOG_PATTERN}</pattern>
        </layout>
    </appender>
    <!--nacos相关日志级别-->
    <logger name="com.alibaba.nacos.client" level="ERROR" additivity="false"/>
    <root level="info">
        <appender-ref ref="stdout" />
    </root>
</configuration>
```
# 日志级别
logback有5种级别，分别是TRACE < DEBUG < INFO < WARN < ERROR，定义于ch.qos.logback.classic.Level类中。

Trace:是追踪，就是程序推进以下，你就可以写个trace输出，所以trace应该会特别多，不过没关系，我们可以设置最低日志级别不让他输出.

Debug:指出细粒度信息事件对调试应用程序是非常有帮助的.

Info:消息在粗粒度级别上突出强调应用程序的运行过程.

Warn:输出警告及warn以下级别的日志.

Error:输出错误信息日志.

此外OFF表示关闭全部日志，ALL表示开启全部日志。

级别等级
等级从低到高分别是TRACE < DEBUG < INFO < WARN < ERROR
