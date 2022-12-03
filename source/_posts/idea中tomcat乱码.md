---
title: idea中tomcat乱码
date: 2021-08-03 12:10:53
tags: 
  - java
  - idea
  - tomcat
categories:
 - 后端
---

在springmvc中，idea中tomcat乱码

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202212031235794.png)
```java
-Dfile.encoding=utf-8
```

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202212031236082.png)
-   #### HELP->Edit Custom VM OPtions中加 -Dfile.encoding=utf-8 重启idea
-   #### 把Tomcat中 server.xml：在 标签中添加 URIEncoding=“UTF-8”

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202212031236967.png)
然后重启电脑，可以解决乱码问题