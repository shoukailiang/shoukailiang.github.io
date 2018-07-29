---
title: spring-boot两小时入门笔记
date: 2018-07-29 16:23:22
tags: 
  - java
  - spring
  - mysql
  - spring-boot
categories:
  - 后端 
---
初始化好工程之后
# 入门
新建一个HelloController.java
```
package com.example.girl1;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
  @RequestMapping(value = "/girl",method = RequestMethod.GET)
  public String say(){
    return "1111111";
  }
}

```
在浏览器中输入localhost:8080/girl 
内容就是1111111
# 启动方式
## 1. idea直接启动
## 2. mvn 
- mvn spring-boot:run
## 3. 打包成jar包启动
- mvn install 
- cd target 
- java -jar jar包名
- 
# 项目属性配置
resource 目录下 将application.xxx修改成application.yml
```
server:
  port: 8080
  servlet:
    context-path: /first
```
之后访问就得 ：http://localhost:8080/first/girl

## 配置文件属性的使用
```
server:
  port: 8080
cupSize: A

```
HelloController.java中
```
@RestController
public class HelloController {
  // 通过注解把配置文件中的cupSize注入到变量中
  @Value("${cupSize}")
  private String cupSize;
  @RequestMapping(value = "/girl",method = RequestMethod.GET)
  public String say(){
    return cupSize;
  }
}
```
打开浏览器就能看到cupSize了 

### 属性多的时候就加个前缀来区分
#### 新建一个类 GirlProperties.java
```
package com.example.girl1;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

// 获取前缀是girl的配置
@Component
@ConfigurationProperties(prefix = "girl")
public class GirlProperties {
  private  String cupSize;
  private  Integer age;

  public Integer getAge() {
    return age;
  }

  public void setAge(Integer age) {
    this.age = age;
  }


  public String getCupSize() {
    return cupSize;
  }

  public void setCupSize(String cupSize) {
    this.cupSize = cupSize;
  }
}

```
配置文件
```
server:
  port: 8080
girl:
  cupSize: A
  age: 18
```
HelloController
```
@RestController
public class HelloController {
  @Autowired
  private GirlProperties girlProperties;
  @RequestMapping(value = "/girl",method = RequestMethod.GET)
  public Integer say(){
    return girlProperties.getAge();
  }
}
```
这样就能做到用前缀来区分，自己打开浏览器看看就明白了

### 配置的开发环境和生产环境

- application.yml
```
// application.yml
 spring:
  profiles:
    active: dev   
```
- application-dev.yml
```
server:
  port: 8080
girl:
  cupSize: B
  age: 18

```
- application-prod.yml
```
server:
  port: 8081
girl:
  cupSize: A
  age: 20

```

- 可以在idea中启动开发环境，
- 在mvn install 后生成的jar包启动开发环境
```
java -jar jar包名 --spring.profiles.active=prod
```

# Controller的使用
- @Controller  处理http请求
- @RestController Spring4之后新加的注解，原来返回json需要@RequestBody配合@Controller(RestController相当于这两个的组合RequestBody和Controller)
- @RequestMapping 配置url映射

## @RequestMapping 
```
// 如果想访问两个
@RequestMapping(value = {"/girl","/hello"},method = RequestMethod.GET)
```
```
// 可以给类加，访问就得 ：localhost:8080/hello/girl了
@RestController
@RequestMapping("/hello")
public class HelloController {
  @Autowired
  private GirlProperties girlProperties;
  @RequestMapping(value = {"/girl"},method = RequestMethod.GET)
  public Integer say(){
    return girlProperties.getAge();
  }
}
// RequestMethod方式有很多
可以使用postman来测试api
```
## 处理url里面的参数
- @PathVariable 获取url里面的数据
- @RequestParam  获取请求参数的值
- @GetMapping  组合注解
### @PathVariable
```
@RestController
public class HelloController {
  @Autowired
  private GirlProperties girlProperties;
  @RequestMapping(value = {"/girl/{id}"},method = RequestMethod.GET)
  public Integer say(@PathVariable("id") Integer myId){
    return myId;
  }
}
// localhost:8080/girl/100
```
### @RequestParam
```
//localhost:8080/girl?id=100
@RestController
public class HelloController {
  @Autowired
  private GirlProperties girlProperties;
  @RequestMapping(value = {"/girl"},method = RequestMethod.GET)
  public Integer say(@RequestParam("id") Integer myId){
    return myId;
  }
}
// 另一种写法
@RestController
public class HelloController {
  @Autowired
  private GirlProperties girlProperties;
  @RequestMapping(value = {"/girl"},method = RequestMethod.GET)
  // 默认值和是否必须传入
  public Integer say(@RequestParam(value = "id",required = false,defaultValue = "10") Integer myId){
    return myId;
  }
}
// RequestMapping 可以写成GetMapping
```
# 数据库操作
在pom.xml导入包
```
// jpa和mysql的
  <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
  </dependency>
```
结束后导入一下包
### 配置文件的配置
```
// 运行前先新建一个dbgirl的数据库，utf8mb4
// application.yml
spring:
  profiles:
    active: dev
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/dbgirl
    username: root
    password: 123456
  jpa:
    hibernate:
      ddl-auto: update // create会把之前的表删掉在新建一个
    show-sql: true
```
### 新建一个Girl类
```
package com.example.girl1;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
// 这个注解表示这个类就是对应数据库中的表
@Entity
public class Girl {
  @Id
  @GeneratedValue
  private Integer id;

  private String cupSize;

  private Integer age;

  public Girl() {

  }


  public Integer getAge() {
    return age;
  }

  public void setAge(Integer age) {
    this.age = age;
  }

  public Integer getId() {
    return id;
  }

  public void setId(Integer id) {
    this.id = id;
  }

  public String getCupSize() {
    return cupSize;
  }

  public void setCupSize(String cupSize) {
    this.cupSize = cupSize;
  }
}
// 之后表中的那些字段就是girl类中对应过去的
```
## 接口编写
```
get    /girls    获取女生列表

post    /girls    创建一个女生

get    /girls/id   通过id查询一个女生

put    /girls/id    通过id更新一个女生

delete    /girls/id    通过id删除一个女生
```
### 获取女生列表
新建一个GirlRepository.java的接口
```
package com.example.girl1;

import org.springframework.data.jpa.repository.JpaRepository;


// Girl类名，Integer是id的类型
public interface GirlRepository extends JpaRepository<Girl,Integer> {

}

```
新建GirlController.java
```
package com.example.girl1;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
@RestController
public class GirlController {
  @Autowired
  private GirlRepository girlRepository;
  /*
   * 查询女生列表
   * */
  @GetMapping(value = "/girls")
  public List<Girl> girlList(){
    return girlRepository.findAll();
  }
}
// 使用postman测试
```
### 创建一个女生
```
 /*
   * 新增一个女生
   * */
  @PostMapping(value = "/girls")
  public Girl girlAdd(@RequestParam("cupSize") String cupSize,
                      @RequestParam("age") Integer age){
    Girl girl = new Girl();
    girl.setCupSize(cupSize);
    girl.setAge(age);
    return  girlRepository.save(girl);
  }
```
### 通过id查询一个女生
```
 // 查询一个女生
  @GetMapping(value = "/girls/{id}")
  public Girl girlFindOne(@PathVariable("id") Integer id){
    return  girlRepository.findById(id).orElse(null);
  }
```
### 通过id更新一个女生
```
  // 更新
  @PutMapping(value = "/girls/{id}")
  public Girl  girlUpdate(@PathVariable("id") Integer id,@RequestParam("cupSize") String cupSize,
                          @RequestParam("age") Integer age){
    Girl girl = new Girl();
    girl.setId(id);
    girl.setCupSize(cupSize);
    girl.setAge(age);
    return girlRepository.save(girl);

  }
``` 
### 通过id删除一个女生
```
 // 删除
    @DeleteMapping(value = "/girls/{id}")
    public  void  girlDelete(@PathVariable("id") Integer id){
         girlRepository.deleteById(id);
    }
```
## 通过年龄查询女生列表
```
 // 通过年龄查询女生列表
    @GetMapping(value = "/girls/age/{age}")
    public List<Girl> girlListByAge(@PathVariable("age") Integer age){
        return  girlRepository.findByAge(age);
    }
```
GirlRepository.java中

```
package com.first.first;

import org.springframework.data.jpa.repository.JpaRepository;

import java.util.List;

public interface GirlRepository extends JpaRepository<Girl,Integer> {
    // 通过年龄查询
    public List<Girl> findByAge(Integer age);
}

```

# 事务管理
> 需求是如果我有两条数据，一条插入失败的话另一条就不能插入，这时候需要加一个事务

GirlService.java
```
package com.example.girl1;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class GirlService {
  @Autowired
  private GirlRepository girlRepository;
  @Transactional
  public void insertTwo(){
    Girl girlA= new Girl();
    girlA.setAge(10);
    girlA.setCupSize("B");
    girlRepository.save(girlA);

    Girl girlB= new Girl();
    girlA.setAge(11);
    girlA.setCupSize("F");
    girlRepository.save(girlB);
  }
}

// 在Girlcontroller里新增

@Autowired
  private GirlService girlService;
  
@PostMapping(value = "/girls/two")
  public void girlTwo(){
    girlService.insertTwo();
  }

```