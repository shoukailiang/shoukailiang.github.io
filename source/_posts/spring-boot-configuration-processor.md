---
title: spring-boot-configuration-processor
date: 2021-04-19 14:43:48
tags: 
  - java
  - springboot
categories:
 - 后端
---

spring默认使用yml中的配置，但有时候要用传统的xml或properties配置，就需要使用spring-boot-configuration-processor了

# 依赖
```xml
     <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
        </dependency>
```
# example
以阿里云配置为例
```java
import lombok.Data;

import java.io.Serializable;

/**
 * 阿里云相关配置
 */
@Data
public class AliyunProperties implements Serializable {

    // 阿里云地域端点
    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    // 存储空间名字
    private String bucketName;

    // Bucket域名 访问文件时作为url前缀
    private String bucketDomain;
}
```
```java
import lombok.Getter;
import lombok.Setter;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

import java.io.Serializable;

@Setter
@Getter
@Component
@ConfigurationProperties(prefix = "community.aliyun")
public class ArticleProperties implements Serializable {
    // 会将 community.article.aliyun 绑定到 AliyunProperties 对象上。
    private AliyunProperties aliyun;
}
```
```yml
#  阿里云配置
community:
	aliyun:
	  endpoint: https://oss-cn-hangzhou.aliyuncs.com # oss端点
	  accessKeyId: ceshi
	  accessKeySecret: ceshi
	  bucketName: ceshi
	  # Bucket域名，访问文件时作为URL前缀，注意前面加上 https 和结尾加上
	  bucketDomain: https://ceshi.oss-cn-hangzhou.aliyuncs.com/
 server:
  servlet:
    context-path: /ceshi
```

# 测试
```java
@RestController
@RequestMapping("/test")
public class DemoController {

    @Autowired
    private ArticleProperties articleProperties;

    @GetMapping("/a")
    public String search() {
        AliyunProperties aliyunProperties = articleProperties.getAliyun();
        return aliyunProperties.getAccessKeyId();
    }
}
```
![在这里插入图片描述](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282215029.png)
