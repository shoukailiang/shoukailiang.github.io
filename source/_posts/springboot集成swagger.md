---
title: springboot集成swagger
date: 2021-04-19 14:48:23
tags: 
  - java
  - springboot
categories:
 - 后端
---
使用国人写的com.spring4all的方式
# 依赖
```xml
  <dependency>
     <groupId>com.spring4all</groupId>
     <artifactId>swagger-spring-boot-starter</artifactId>
     <version>1.9.1.RELEASE</version>
 </dependency>
```
# 在启动类上增加注解
```java
@EnableSwagger2Doc
```
# yml
```yml
#swagger配置
swagger:
  enable: true
  title: OSS服务
  description: OSS基础服务API
  version: ${project.version}
  base-package: com.shoukailiang.it_community
  base-path: /**
  exclude-path: /error
  authorization:
    key-name: Authorization
```

```java
@GetMapping("/a")
    @ApiOperation(value ="获取accesskey" )
    public String search() {
        AliyunProperties aliyunProperties = articleProperties.getAliyun();
        return aliyunProperties.getAccessKeyId();
    }
```
# 测试
![在这里插入图片描述](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282216622.png)
