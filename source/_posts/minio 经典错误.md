---
title: minio 经典错误
date: 2022-12-22 22:09:36
tags: 
  - java
  - oss
  - minio
categories:
 - 后端
---


# minio ：The difference between the request time and the server‘s time is too large.

> 系统时区与硬件时区不一致导致的
使用时间服务器上的时间同步的方法

先下载
```
yum -y install ntp ntpdate
```