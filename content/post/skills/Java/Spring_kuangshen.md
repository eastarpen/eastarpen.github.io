---
title: "Spring-FrameWork 基础"
slug: "Spring-FrameWork-basic"
tags: ["java", "Spring"]
description:
draft: true
date: 2022-02-11
---

# Spring

## 1. 简介

### 1.1 什么是 Spring

* Spring 是一个开源的免费框架(容器)
* Spring 是轻量级的, 非入侵式的框架
* 特点: 控制反转(IOC), 面向切面编程(AOP)
* 支持事务的处理, 对框架整合的支持

### 1.2 Spring 历史

* 2002 首次推出 Spring 框架的雏形: interface21 框架
* 2004 正式推出 Spring 1.0
* Spring 理念: 使现有的技术更加容易使用
* Spring 目的: 解决企业开发复杂性问题
* SSM: SpringMvc, Spring, Mybats

### 1.3. 相关文档/项目

* [官网](https://spring.io/projects/spring-framework#overview)
* [下载地址](https://repo.spring.io/ui/native/release/org/springframework/spring)
* [github](https://github.com/spring-projects/spring-framework)

### 1.4 包

* 5.2.0 maven
   
   ```xml
   <!-- 这个包会自动导入其他包 -->
   <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-webmvc</artifactId>
       <version>5.2.0.RELEASE</version>
   </dependency>

   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-jdbc</artifactId>
       <version>5.2.0.RELEASE</version>
   </dependency>
   ```

### 1.5 扩展

**Spring Boot 构建一切, Spring Cloud 协调一切, Spring Cloud 连接一切**

* Spring Boot

  * 一个快速开发的脚手架
  * 基于 SpringBoot可以快速开发单个微服务
  * 约定大于配置!
  * 学习 SpringBoot 要完全掌握 Spring 以及 SpringMVC

* Spring Cloud

  * Spring Cloud 是基于 SpringBoot 实现的

## 2. IOC 理论指导

### 2.1 IOC 体会

```java
public QueryServiceImpl implements QueryService {
    private QueryDao queryDao = new QueryMysqlDao();

    public void query(){
        queryDao.query();
    }
}
```

上面的代码 service 层调用了 dao 层, 而 dao 层接口是固定的 QueryMysqlDao(在程序员编写代码时就写死)

如果 service 需求发生变更, 只能更改 QueryServiceImpl 或添加一个类

```java
public QueryServiceImpl implements QueryService {
    private QueryDao queryDao;

    public void query(){
        queryDao.query();
    }

    public void setQueryDao(QueryDao queryDao) {
        this.queryDao = queryDao;
    }
}
```

上面的代码 queryDao 没有写死, 接口控制权在调用者手中 QueryDao 仅限制了接口

两段代码发生了 QueryServiceImpl 对 queryDao 的权限发生了变化, 从原来的主动实例化对象变为被动的接受对象, 这就是控制反转, 这就是 IOC 思想。

## 3. HelloSpring

**对象由 Spring 创建，管理，装配**

1. 创建实体类

2. 在 resources 目录下创建 `beans.xml` 文件

   这是 Spring 的配置文件, 可以认知命名, 需要加上 Spring 约束

3. 写入Spring 约束
   
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
           http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       <!-- bean definitions here -->
   </beans>
   ```
4. 通过 `bean` 标签添加实例

   ```xml 
   <!-- 通过 Spring 管理对象
        类型为 com.eastarpen.Hello, 名称为 hello
        设置对象属性 "str" 值为 "Spring"
        str 必须有对应的 set 方法
    -->
   <bean id="hello" class="com.eastarpen.Hello">
       <property name="str" value="Spring" />
       <!-- value 是具体赋值
            ref   引用 Spring 中创建好的对象
        -->
   </bean>
   ```

5. 通过 `ClassPathXmlApplicationContext` 调用
   
   ```java
   // 获取 Spring 上下文对象
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   // 对象通过 Spring 管理, 如果需要使用, 直接通过 getBean取出
   Hello hello = (Hello)context.getBean("hello");

   // Hello hello = context.getBean("hello", Hello.Class);
   ```

6. beans.xml 被加载后所有对象都会被实例化

## 4. IOC 创建对象的方式

TODO 自己编写调试探究构造过程

1. 默认使用无参构造创建对象

2. 手动设置有参构造器

   * 下标赋值

     ```xml
     <bean id="hello" class="com.eastarpen.Hello">
         <constructor-arg index="0" value="Spring" />
     </bean>
     ```

   * 类型赋值

     ```xml
     <bean id="hello" class="com.eastarpen.Hello">
         <constructor-arg type="java.lang.String" value="Spring" />
     </bean>
     ```

     不建议使用, 若多个同类型参数, 较麻烦

   * 通过参数名

     ```xml
     <bean id="hello" class="com.eastarpen.Hello">
         <constructor-arg name="str" value="Spring" />
     </bean>
     ```

## 5. Spring 配置

### 5.1 别名

```xml
<bean id="hello" class="com.eastarpen.Hello">
   <property name="str" value="Spring" />
</bean>

<alias name="hello" value="helloAlias" />
```

hello 对象可以通过 "hello" 或 "helloAlias" 获取

### 5.2 Bean 的配置

* `id` bean 的唯一标识符
* `class` bean 对象 所有的对应的全限命名(包名+类名)
* `name` 也是别名, 且可以有多个 **如需别名建议使用**

   ```xml
   <bean id="hello" class="com.eastarpen.Hello" name="hello1, hello2" />
   ```

### 5.2 import

一般用户团队开发, 可以将多个 配置文件 合并为一个

一般将多个 `beans.xml` 导入到 `applicatinoContext.xml` 中, 最后直接调用 `applicationContext.xml` 即可

## 6. DI 依赖注入

### 6.1 构造器注入

就是调用构造方法

### 6.2 Set 方式注入

**前言**

* 依赖注入一般指 Set 注入
* 依赖: bean 对象的创建依赖于容器
* 注入: bean 对象中的所有属性, 由容器来注入

**注入方式**

1. 普通值注入

   ```xml
   <bean id="hello" class="com.eastarpen.Hello">
       <property name="str" value="hello" />
   </bean>
   ```

   设置 String 类型的属性 "str" 值为 "hello"

2. bean 注入

   ```xml
   <bean id="hello" class="com.eastarpen.Hello" />
   <bean id="test" class="com.eastarpen.Test">
       <property name="hello" ref="hello" />
   </bean>
   ```

   设置 test 的 name 属性为预先创建的 id 为 hello 的 bean 

3. 数组注入

   ```xml
   <property name="books">
       <array>
           <value>红楼梦</value>
           <value>西游记</value>
       </array>
   </property>
   ```

   设值对象属性 `String[] name` 值为 `["红楼梦", "西游记"]`

4. list 注入 

   ```xml
   <property name="books">
       <list>
           <value>红楼梦</value>
           <value>西游记</value>
       </list>
   </property>
   ```

   设值对象属性 `List<String> name` 值为 `["红楼梦", "西游记"]`

5. map 注入 

   ```xml
   <property name="cards">
       <map>
           <entry key="card-1" value="value-1" />
           <entry key="card-2" value="value-2" />
       </map>
   </property>
   ```

   设值对象属性 `map<String, String> cards` 值

5. set 注入 

   ```xml
   <property name="games">
       <set>
           <value>LOL</value>
           <value>COC</value>
       </set>
   </property>
   ```

6. null 注入 

   ```xml
   <property name="test">
       <null />
   </property>
   ```

7. properties 注入 

   ```xml
   <property name="info">
       <props>
           <prop key="name">name-01</prop>
       </props>
   </property>
   ```

   Properties 对象

### 6.3 扩展注入

* c 命名空间注入 [官网](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/core.html#beans-c-namespace)
   
  xml 约束 ` xmlns:c="http://www.springframework.org/schema/c"`

  依靠构造器

* p 命名空间注入 [官网](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/core.html#beans-p-namespace)

  xml 约束 `xmlns:p="http://www.springframework.org/schema/p"`

  依靠 setter

## 7. bean 作用域


### 7.1 简介

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/core.html#beans-factory-scopes-singleton) | (Default) Scopes a single bean definition to a single object instance for each Spring IoC container. |
| [prototype](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/core.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/core.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request. That is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/core.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/core.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring-framework/docs/5.3.19-SNAPSHOT/reference/html/web.html#websocket-stomp-websocket-scope) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |

### 7.2 解释

1. `singleton` 默认的作用域, 即单例模式

2. `prototype` 每次从容器中get 的时候都会产生一个新对象
   
   ```xml 
   <bean id="accountService" class="com.something.DefaultAccountService" scope="prototype"/>
   ```

3. `request, seesion, application` 在 web 开发中使用

## 8. bean 的自动装配

自动装配是Spring满足bean以来的一种方式

Spring会在上下文自动寻找, 并自动给bean装配属性

### 8.1 bean autowired  属性自动装配

```xml 
<bean id="hello" class="com.eastarpen.Hello" autowired="byName"/>
```

* byName 需要待注入的 bean id 和需要自动注入的属性的set方法一致

  如 `autowired="byName"` Class Hello 中含有方法 `setDog` 则 bean `dog` 能被注入, `dog-01` 不能

* byType 需要待注入的 bean 的 Class 唯一并且和需要自动注入的属性类型一致

  如 `autowired="byType"` Class Hello 含有属性 `Dog dog;` 若容器中只有一个 `<bean class="com.eastarpen.Dog"` 则 该 bean 可以被装配, 否则不行

### 8.2 Annotation 自动装配

jdk 1.5 支持注解, Spring 2.5 支持注解

Spring 4 之后使用注解开发需要导入 `aop`

**开启注解支持**

* 导入 contex 约束
* 配置注解支持
* 模板
  
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
          https://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
  
      <context:annotation-config/>
  
  </beans>
  ```

**使用**

对需要自动装配的属性上加上 `@Autowired` 注解 (由于注解采用反射实现, 甚至不需要 setter)

注意被自动装配的 bean 的id应和被装配的属性名相同, 或属性通过 `@Qualifier()` 注解

Autowired 先 byType 再 byName

**补充**

javax 的 `@Resource` 也可以实现自动装配, 默认 byName, 找不到 byType, 还可以设置 `name`

### 8.3 Spring 注解概述

0. xml 准备

   ```xml
   <!-- Spring 约束省略 -->

   <!-- 指定要扫描的包, 包下的注解就会生效 -->
   <context:component-scan bas-package="com.eastarpen.package"/>

   <!-- TODO 不准确 -->
   <!-- 注解驱动 -->
   <context: config />
   ```

1. bean 

   在需要纳入 Spring 容器管理的实体类上加上注解 `@Component`

   相当于 `<bean id="" class="" />`

2. 属性注入

   在属性或对应的setter上添加注解 `@Value("value")`, 相当于 `<property name="" value="">`

3. 衍生注解

   component 的衍生注解, 功能和 component 一致

   * dao         `@Repository`
   * service     `@Service`
   * controller  `@Controller`

4. 自动装配

   Resource 和 Autowired

5. 作用域

   在实体类上加上 `@Scope("scope")` 设置 bean 的作用域

**tip**

* 使用 xml 配置适用性强, 维护简单
* 注解 使用便捷, 维护复杂
* 推荐 xml 用来管理 bean, 注解注入属性

## 9. 使用 java 配置 

JavaConfig 原是 Spring 的一个子项目, Spring 4 之后成为核心功能

TODO 自行测试各类注解

```java
@Configuration
public class MyConfig {

    @Bean
    public User getUser() {
        return new User;
    }
}
```

* MyConfig 类相当于 `beans.xml` 
* 加了 `@Bean` 注解的方法就是 `bean` 标签
* 方法名就是 bean id, 返回类型就是 bean class

调用方式

```java
public void static main(String[] args) {
    ApplicationContext context = new AnnotationApplicationContext(MyConfig.class);
}
```

## 10. AOP 

### 10.1 前言 

代理模式就是 SpringAop 的底层

**代理模式**

TODO 跟着狂神做 p17-19
  
* 静态代理
* 动态代理

### 10.2 静态代理

**角色分析**

* 抽象角色: 用抽象类或接口实现
* 真实角色: 被代理的角色
* 代理角色: 代理真实角色, 一般会实现一些附属操作
* 客户    : 访问代理对象的人

**优点**

* 真实角色的操作更加纯粹, 不需要去关注公共业务
* 公共业务由代理角色完成, 实现业务的分工
* 公共业务发生扩展的时候, 方便集中管理

**缺点**

* 需要增添代理角色的代码 

### 10.3 动态代理

**动态代理底层是反射, 代理类是动态生成的**

动态代理两大类: 基于接口, 基于类

> TIP 了解动态代理原理关键字  
> 基于接口 JDK 动态代理   
> 基于类   cglib  
> Java字节码 javasist  

**InvocationHandler**

InvocationHandler 是由代理实例调用的

> public interface InvocationHandler  
>  
> InvocationHandler is the interface implemented by the invocation handler of a proxy instance.  
> Each proxy instance has an associated invocation handler. When a method is invoked on a proxy instance, the method invocation is encoded and dispatched to the invoke method of its invocation handler.

**Proxy**

> Proxy provides static methods for creating objects that act like instances of interfaces but allow for customized method invocation.  

```java
public static Object newProxyInstance(ClassLoader loader,  
                                      Class<?>[] interfaces,  
                                      InvocationHandler h)  
```

**模板及调用示例**

```java
public class ProxyInvocationHandler implements InvocationHandler {

    // 被代理的接口
    private Object target;

    public void setTarget(Object target) {
        this.target = target;
    }

    // 生成代理类
    public Object getProxy() {
        return  Proxy.newProxyInstance(this.getClass().getClassLoader(),
                                       target.getClass().getInterfaces(),
                                       this);
    }

    // 处理代理实例并返回结果
    public void invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = method.invoke(target, args);
        return result;
    }

    // 额外功能的方法, 记得加入到 invoke
}
```

实例

```java
public static void main(String[] args) {
    // 需要代理的真实角色
    UserService userService = new UserServiceImpl();

    ProxyInvocationHandler pih = new ProxyInvocationHandler();
    pih.setTarget(userService);

    // 动态生成代理对象
    UserService proxy = (UserService) pih.getProxy();

    // 代理对象调用真实角色方法
    proxy.run();
}
```

**优点**

1. 一个动态代理类代理一个接口(即可代理实现该接口的所有类), 一般就是对应一个业务

### 10.4 使用 Spring实现 AOP 

**导包**

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
    <scope>runtime</scope>
</dependency>
```

**Aop在Spring中的作用**

提供声明式事务, 允许用户自定义切面

**相关名词**

* 横切关注点: 跨越应用程序多个模块的方法或功能。即, 与业务逻辑无关的, 但需要关注的部分，例如日志，安全，缓存，事务等

* 切面(Aspect) 横切关注点被模块化的特殊对象(一个类)

* 通知(Advice) 切面必须要完成的一个工作, 类中的一个方法

* 目标(Target) 被通知对象

* 代理(Proxy) 向目标对象应用通知后创建的对象

* 切入点(PointCut) 切面通知执行的地点的定义

* 连接点(JointPoint) 与切入点匹配的执行点

**方式一  使用 Spring 接口**

1. 业务逻辑接口和实现类

2. 实现 Spring 提供的 Aop 接口 `AfterReturningAdvice, MethodoBeforeAdvicd` 等

3. Spring 注册bean

4. Spring 配置 aop(注意导入 aop 约束)

   ```xml
   <aop:config>
       <!-- aop:pointcut 切入点标签
            id           唯一标识
            expression   要执行的位置
            TODO 了解 execution
            execution(* *.*(..)) (返回类型 全限定类名.方法(参数)) .. 表示任意参数, 通过它识别需要执行的类与方法  
        -->
       <aop:pointcut id="pointcut" expression="execution(* *(..))" />

       <!-- 执行环绕增加 -->
       <aop:advisor advice-ref="bean" pointcut-ref="pointcut" />
   </ aop:config>
   ```

**方式二  自定义类**

1. 自定义切面类

2. 注册 bean

3. 配置 aop

   ```xml
   <bean id="aspect" class="" />
   <aop:config>
       <!-- 切面 -->
       <aop:aspect ref="aspect">
           <aop:pointcut id="pointcut" expression="execution(* *(..))" />
           <aop:advisor advice-ref="bean" pointcut-ref="pointcut" />
           <aop:before method="before" pointcut-ref="pointcut" />
           <!-- method="before" ""中是切面类的方法 -->
       </aop:aspect>
   </ aop:config>
   ```

**方式三 使用注解**

TODO 跟着狂神做 p22

1. 自定义只用注解修饰的切面类

   ```java
   @Aspect
   public class MyAspect {

       // 注意要导入 Spring 的 Before 注解
       @Before("execution()")
       public void method(){}
   }

2. 注册 bean

3. 开启注解支持
   
   ```xml
   <aop:aspectj-autoproxy/>
   ```

## 11. Spring 中的 Mybatis

[mybatis-spring doc](http://mybatis.org/spring/zh/index.html)

需要添加 `spring-jdbc, Mybatis-Spring`
