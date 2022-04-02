---
title: "Java 注解与反射"
slug: "java-annotation-and-reflection"
tags: ["java"]
description:
date: 2022-02-11
---

## 1. 注解

### 1.1 什么是注解

1. JDK5.0 开始引入注解
2. Annotation 不是程序本身, 可以对程序做出解释
3. Annotation 可以被其他程序(如编译器)读取

### 1.2 内置注解

1. `@Override` 重写方法
2. `@Deprecated` 废弃方法， 不推荐使用但可以使用
3. `SuppressWarnings("all")` 关闭警告

### 1.3 元注解

元注解负责注解其他注解

* `Target` 用于描述注解的使用范围

* `Retention` 表示需要在什么级别保存该注解信息, 用于描述注解的生命周期
  
   `SOURCE < CLASS < RUNTIME`

* `@Document` 是否能将注解生成在 JavaDoc 中

* `@Inherited` 子类是否能基础父类注解

### 1.4 定义一个注解

```java
@Target(value = {ElementType.METHOD, ElementType.TYPE})
@Retention(value=RententionPolicy.RUNTIME)
public @interface MyAnnotation{
    // 参数类型为 String, 参数名称为 value
    String value() default "";
}
```

* 可以通过 default 来声明参数的默认值, 如果没有默认值, 该参数必须赋值
* 当只有一个参数时, 赋值时可以不用写参数名直接赋值.
* 当只有一个参数, 一般将其称为 value

## 2. 反射

### 2.1 什么是反射

一个类在内存中只有一个 Class 对象
一个类被加载后, 类的整个结构都会被封装在 Class 对象中
 
