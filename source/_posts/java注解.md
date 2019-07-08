---
title: java注解
date: 2019-06-01 22:43:41
tags: 
  - java
categories:
 - java
---
# 系统自带的注解

-  @Override   
-  @Deprecated   申明该方法已经被废弃
-  @SuppressWarnings("deprecation") 我想强制使用这个被废弃的方法


# 元注解
　元注解的作用就是负责注解其他注解。Java5.0定义了4个标准的meta-annotation类型，它们被用来提供对其它 annotation类型作说明。Java5.0定义的元注解：

- @Target
- @Retention
- @Documented
- @Inherited
## @Target
> @Target说明了Annotation所修饰的对象范围：Annotation可被用于 packages、types（类、接口、枚举、Annotation类型）、类型成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数）。在Annotation类型的声明中使用了target可更加明晰其修饰的目标。

取值(ElementType)有：
```
1.CONSTRUCTOR:用于描述构造器
2.FIELD:用于描述域
3.LOCAL_VARIABLE:用于描述局部变量
4.METHOD:用于描述方法
5.PACKAGE:用于描述包
6.PARAMETER:用于描述参数
7.TYPE:用于描述类、接口(包括注解类型) 或enum声明
```
## @Retention
> @Retention定义了该Annotation被保留的时间长短：某些Annotation仅出现在源代码中，而被编译器丢弃；而另一些却被编译在class文件中；编译在class文件中的Annotation可能会被虚拟机忽略，而另一些在class被装载时将被读取（请注意并不影响class的执行，因为Annotation与class在使用上是被分离的）。使用这个meta-Annotation可以对 Annotation的“生命周期”限制。
```
1.SOURCE:在源文件中有效（即源文件保留）
2.CLASS:在class文件中有效（即class保留）
3.RUNTIME:在运行时有效（即运行时保留）
```
## @Inherited：
> @Inherited 元注解是一个标记注解，@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。

　　注意：@Inherited annotation类型是被标注过的class的子类所继承。类并不从它所实现的接口继承annotation，方法并不从它所重载的方法继承annotation。
## @Documented:
> @Documented用于描述其它类型的annotation应该被作为被标注的程序成员的公共API，因此可以被例如javadoc此类的工具文档化。Documented是一个标记注解，没有成员。

# 自定义注解
1：使用@interface关键字定义注解<br>
2：成员方法以无参无异常的方式声明<br>
3：可以使用default为成员方法指定一个默认值<br>
4：成员的类型是有限制的，合法的成员类型包括原始类型/String/Class/Annotation/Enumeration<br>
```java
import java.lang.annotation.*;

@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Description {
    // 如果注解只有一个成员，成员名必须取名为value
    String desc(); //成员使用无参数，无异常声明，类型是受限的.包括原始类型，String ,Class,Annotation,Enumeration
    String author();
    int age() default 18;// 可以自带默认值
}

// 注解可以没有成员，叫做标识注解
```
　