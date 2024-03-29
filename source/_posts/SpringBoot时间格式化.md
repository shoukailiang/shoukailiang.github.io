---
title: SpringBoot时间格式化
date: 2021-08-13 17:13:18
tags: 
  - java
  - springboot
categories:
 - 后端
---


# Date类型
> java8推出了LocalDateTime 所以不太推荐使用Date了

```yml
spring.jackson.date-format =yyyy-MM-dd HH:mm:ss
spring.jackson.time-zone=GMT+8
```

# LocalDateTime
```yml
spring:
    jackson:
      date-format: yyyy-MM-dd HH:mm:ss
      time-zone: GMT+8

```

```java

package com.shoukailiang.community.edu.config;

import com.fasterxml.jackson.datatype.jsr310.ser.LocalDateTimeSerializer;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.autoconfigure.jackson.Jackson2ObjectMapperBuilderCustomizer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

/**
 * @author shoukailiang
 * @version 1.0
 * @date 2021/8/6 16:53
 */
@Configuration
public class LocalDateTimeSerializerConfig {
    @Value("${spring.jackson.date-format}")
    private String pattern;

    @Bean
    public LocalDateTimeSerializer localDateTimeDeserializer() {
        return new LocalDateTimeSerializer(DateTimeFormatter.ofPattern(pattern));
    }

    @Bean
    public Jackson2ObjectMapperBuilderCustomizer jackson2ObjectMapperBuilderCustomizer() {
        return builder -> builder.serializerByType(LocalDateTime.class, localDateTimeDeserializer());
    }
}
```
