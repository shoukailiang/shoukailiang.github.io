---
title: springboot常见问题
date: 2021-04-22 10:38:19
tags: 
  - java
  - spingboot
categories:
 - 后端
---

# Method annotated with @Bean is called directly. Use dependency injection instead.
未添加@Configuration注解，导致@Bean之间相互调用出错

因此把类名上面增加@Configuration注解即可解决。