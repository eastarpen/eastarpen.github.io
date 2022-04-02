---
title: "JavaWeb 基础"
slug: "JavaWeb-basic"
tags: ["java"]
description:
draft: true
date: 2022-02-11
---

## 1. jsp

### 1.1 页面引用

    ```jsp
    <%-- 将两个页面合二为一--%>
    <%@include file="common/header.jsp"%>

    <%-- 拼接页面 --%>
    <jsp:include page="/common/header.jsp" />
    ```

### 1.2 9大内置对象

   * PageContext

   * Request

   * Response

   * Session

   * Application \[ServletContext]

   * config \[SerlvetConfig]

   * out  

   * page 

   * excepetion

### 1.3 保存数据方式 

   1. pageContext.setAttribute();

      在一个页面中有效 
   
   2. request.setAttribute();

      在一次请求中有效(请求转发会携带数据)

   3. session.setAttribute();

      在一次会话中有效

   4. application.setAttribute();

      在服务器中有效

3. JSP标签, JSTL标签, EL表达式

    * JSTL 表达式依赖

      ```xml
      javax.servlet.jsp.jstl
      taglibs:standard
      ```

   * EL表达式 `${}`

      * 获取数据

      * 执行运算
      
      * 获取 web 开发常用对象

         获取表单数据 `${param.paramName}`

         ```jsp
         <input type = "text" name="username" value="${param.username}">
         ```

   * JSP标签

      ```jsp
      <%-- example --%>
      <jsp:forward page="/index.jsp">
          <jsp:param name="name" value="value"></jsp:param>
      </ jsp:forward>
      ```

    * JSTL 表达式

      用以弥补html标签不足

      使用 JSTL 表达式 必须引用 taglib, 如

      `<%@ taglib prefix="c" url="http://java.sun.com/jsp/jstl/core" %>`

      此外, Tomcat 也需要引入 jstl 包(jstl, jstl:stand), 否则报错: JSTL 解析错误

## 2. Filter

### 2.1 Filter 简介

Filter: 过滤器, 用来过滤网站的数据;

* 处理中文乱码 
* 登录验证 
* 权限判断

### 2.2 Filter 开发步骤

1. MAVEN 导包

2. 编写过滤器

   * 导包 `javax.servlet.Filter`

   * 实现 Filter 接口, 重写对应方法

   * 在 web.xml 中配置 Filter

   ```java
   import ...

   public class CharacterEncodingFilter implements Filter {

       // 初始化: 当 web 服务器启动即初始化, 随时等待过滤对象!
       public void init(...) {}

       /** 
         *  Chain: 链
         *  1. 过滤中的所有代码, 在过滤特定请求的时候都会执行
         *  2. 必须要让过滤器继续同行, 即 chain.doFilter(request, response);
         */
       public void doFilter(..., FilterChain chain) {}

       // 销毁 web 服务器关闭的时候销毁过滤器
       public void destory() {}
   }
   ```

## 3. 监听器

   实现一个监听器接口, 监听各种事件

   1. 实现某个监听器接口

   2. web.xml 中注册监听器

## 4. JDBC

* 导入 jar 包

  ```xml
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-conneter-java</artifactId>
      <version>5.1.47</version>
  </dependency>
  ```

  * javax.sql

  * mysql-conneter-java

数据库环境搭建

实体类

* 必须有一个无参构造

* 属性必须私有化

* 必须有对应得 get/set 方法

一般用来和数据库得字段做映射 

ORM: 对象关系映射

  * 表 --> 类
  
  * 字段 --> 属性 
  
  * 行记录 --> 对象 

## MVC 简介


什么是MVC: Model, View, Controller 模型, 视图, 控制器

约定: Servlet 专注于处理请求以及控制视图跳转, Jsp 专注于显示数据

Model

  * 业务处理: 业务逻辑* (Service)

  * 数据持久层: CRUD (Dao)

View 

  * 展示数据

  * 提供链接发起 Servlet 请求

Controller (Servlet)

  * 接受用户请求 (req, session...)

  * 交给业务层处理对应代码

  * 控制视图跳转
