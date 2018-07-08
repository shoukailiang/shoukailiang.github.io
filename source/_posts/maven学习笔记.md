---
title: maven学习笔记
date: 2018-07-07 15:42:55
tags: 
  - 后端
  - java
  - 工具
categories:
  - 后端
---

# 基本命令
```
mvn -v 查看版本
compile 编译
test 测试
package 打包
clean 删除target
install 安装jar包到本地仓库
```
# 自动建立项目骨架
```
mvn archetype:generate   按照提示进行选择
```
>  若卡在 [INFO] Generating project in Interactive mode
## 更改
```
加个参数 -DarchetypeCatalog=internal 让它不要从远程服务器上取catalog:
mvn archetype:generate -DarchetypeCatalog=internal 
```
或者直接设置
```
mvn archetype:generate -DarchetypeCatalog=internal -DgroupId=org.sonatype.mavenbook.simple -DartifactId=simple -Dpackage=org.sonatype.mavenbook -Dversion=1.0-SNAPSHOT
```
```
DgroupId 组织名等等
DartifactId 项目名-模块名
Dversion 版本号
Dpackage 项目所在的包名
```
# 设置阿里的源
```
// 安装文件的conf文件下的setting.xml
// 更改
<!--设置镜像中央仓库为阿里云-->
<mirror>
      <id>alimaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>
```
# 默认存放位置
```
C:\Users\shouk\.m2   用户下
```
修改默认安装位置,也是配置文件里面
```
  <localRepository>D:/maven/repository</localRepository>
  将setting.xml也复制到D:/maven/repository下
```
# maven生命周期
```
clean compile test package install 
```
# pom.xml
```
// 反写的公司网址+项目名
  <groupId>org.sonatype.mavenbook.simple</groupId>
  // 项目名+模块名
  <artifactId>simple</artifactId>
  // 版本号 第一个大版本号，第二个分支版本，第三个小版本号
  <version>1.0-SNAPSHOT</version>
  // 默认是jar 
  <packaging>jar</packaging>
  // 项目描述名
  <name>simple</name>
  // 项目地址
  <url>http://maven.apache.org</url>
  
  
  // 依赖
   <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      // 依赖范围，测试范围有用
      <scope>test</scope>
    </dependency>
  </dependencies>
```
