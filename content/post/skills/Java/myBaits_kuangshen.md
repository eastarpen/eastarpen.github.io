## 1. MyBatis 简介

### 1.1 什么是 MyBatis

1. MyBatis 是一款**持久层**框架
2. 定制化 SQL, 存储过程以及高级映射
3. 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集

### 1.2 获得 MyBatis

1. [maven 仓库](https://mvnrepository.com/artifact/org.mybatis/mybatis)

    狂神使用 3.5.2

2. [github 仓库](https://github.com/mybatis/mybatis-3)

3. [中文文档](https://mybatis.org/mybatis-3/zh/index.html)

### 1.3 持久化

**数据持久化**

* 持久化就是将程序的数据在持久状态和瞬时状态转化的过程
* 数据库, io 文件持久化

**为什么要持久化**

  * 内存**断电即失**, 某些对象不能丢失

### 1.4 持久层

* 完成持久化工作的代码块
* 层界限十分明显

### 1.5 优势

* 帮助将数据存入数据库
* 方便
* 简化, 自动化
* sql 和代码分离

## 2. Hello MyBatis

### 2.1 搭建环境

1. 搭建数据库

    ```mysql
    CREATE DATABASE Mybatis;
    
    USE `Mybatis`;
    
    CREATE TABLE `user` ( `id`
            int(20) NOT NULL PRIMAEY
            KEY AUTO INCREANENT,
            `name` varchar(30)
            DEFAULT NULL, `pwd`
            varchar(30) DEFAULT
            NULL,) ENGINE=INNODB
    DEFAULT CHARSET=utf8mb4 ;
  
    INSERT INTO `user` (`name`,
            `pwd`) VALUES ("test-01",
                "pwd_01") ("test-02",
                    "pwd_02")
                ("test-03", "pwd_03")
                ; 
    ```

2. 新建一个 Maven 项目 `MybatisStudy`

    注意 maven 不要使用 idea 自带的

3. 导入 Maven 依赖

    junit, mysql-connector-java,
    mybatis

4. 新建模块 `Mybatis-01`

    * src.main.java.resources 目录下创建 `mybatis-config.xml` 文件

    * 文件内容示例

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?> 
      <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-config.dtd">
      <configuration> 
          <environments default="development">
              <environment id="development">
                  <transactionManager type="JDBC"/> 
                  <dataSource type="POOLED"> 
                      <property name="driver" value="${driver}"/> 
                      <property name="url" value="${url}"/>
                      <property name="username" value="${username}"/> 
                      <property name="password" value="${password}"/>
                  </dataSource> 
              </environment>
          </environments> 
          <mappers>
              <mapper resource="org/mybatis/example/BlogMapper.xml"/>
          </mappers> 
      </configuration>
      ```
  
      注意 xml 中 `&` 需写为 `&amp`

### 2.2 实例

1. 编写 `MybatisUtils` [参考视频 约 28min](https://www.bilibili.com/video/BV1NE411Q7Nx?p=2)

2. 设计代码

    * 实体类
    * Dao/Mapper 接口
    * Mapper 配置文件

    注意点

    * 核心配置文件中注册 Mapper
    * pom.xml 配置导出资源文件 p2 55min
    * 关闭 sqlSession p 1h:07min

## 3. CRUD

### 3.1 基础

* id 对应的 namespace 中的方法名
* resultType 返回值类型, 带有完整的包名

  如 `com.eastarpen.entity.User`

* parameterType 参数类型

* 只有一个基本类型参数可以省略 parameterType

* 通过 `#{}` 获取方法传递的参数

* 通过 `#{}` 直接获取实体类对象的属性

### 3.2 实例

```xml
<!-- 在对应接口的 Mapper.xml 中 --> 
<mapper namespace="com.eastarpen.dao.UserMapper">

    <select id="getUserList" resultType="com.eastarpen.entity.User">
        SELECT * FORM Mybatis.user; 
    </select>

    <select id="getUserById" parameterType="int" resultType="com.eastarpen.entity.User">
        SELECT * FORM Mybatis.user WHERE id = #{id}; 
    </select>

    <insert id="addUser" parameterType="com.eastarpen.entity.User">
    INSERT INTO Mybatis.user (name, pwd) VALUES(#{name}, #{pwd});
    </insert>

    <update id="updateUser" parameterType="com.eastarpen.entity.User">
        UPDATE TABEL Mybatis.user SET `name`=#{name}, `pwd`=#{pwd} WHERE id=#{id}; 
    </update>

    <delete id="updateUser" parameterType="com.eastarpen.entity.User">
        DELETE FROM Mybatis.user WHERE id=#{id}; 
    </delete>
</mapper>
```

注意 增删改 需要提交事务

### 3.3 troubleshooting

1. namespace 中包用 `.` 分割
2. mybatis-config.xml 中需要使用 `/`

### 3.4 map 作为参数

p5

当实体类属性过多, 考虑使用 map 简化

## 4. 配置解析

注意各字段有特定的相对位置

### 4.1 环境配置 (environments)

* Mybatis 可以配置多个环境, 通过 default 选择

* Mybatis 默认的事务管理器是 JDBC, 默认的连接池类型是 POOLED

### 4.2 属性(properties)

* 可以通过 properties 来实现应用配置文件

* 需要设置 properties 及其 resource 属性

* 可以在 properties 中增加 property, 若存在重名, 优先外部引用

### 4.3 类型别名(typeAliases)

p7

```xml
<typeAliases> 
<typeAlias type="com.eastarpen.User" alias="User"/> 
<package type="com.eastarpen"/> 
<typeAliases>
```

* 可以为类和包起别名

* 为包起别名不能自定义, 如 `User` 在 `com.eastarpen` 内, 为 `com.eastarpen` 注册后可以用 `user` 或 `User` 访问, 推荐小写

* 可以对类使用注解规定别名

* 基本类型加 `_` 如 `int` -> `Integer` 而 `_int` -> `int`

### 4.4 设置(settings)

https://mybatis.org/mybatis-3/zh/configuration.html#settings

### 4.5 映射器 (mapper)

* resource

  ```xml 
  <mappers> 
      <mapper resource="com/eastarpen/com/UserMapper.xml"/>
      <mapper resource="com/eastarpen/com/*Mapper.xml"/>
  </mappers> 
  ```

* class

  ```xml
  <mappers>
      <mapper class="com.kuang.dao.UserDao" />
  </mappers> 
  ```

  注意接口和配置文件必须同名且在同一包下

* package

  ```xml
  <mappers> 
      <package ="com.kuang.dao" /> 
  </mappers> 
  ```

  注意接口和配置文件必须同名且在同一包下

### 4.6 生命周期和作用域

生命周期和作用域的错误使用会导致严重的**并发问题**

**SqlSessionFactoryBuilder**

* 一旦创建了SqlSessionFactory就不再需要它
* 应作为局部变量

**SqlSessionFactory**

* 类似于数据库连接池
* SqlSessionFactory 一旦创建就应该在应用的运行期间一直存在, **没有理由丢弃它或重新创建另一个实例**
* 最佳作用域是 应用作用域
* 最简单的是使用**单例模式**或静态单例模式

**SqlSession**

* 连接到连接池的一个请求
* SqlSession 的实例不是线程安全的, 因此是不能被共享的, 最佳作用域是请求或方法作用域
* *用完即关, 防止资源占用

每一个 Mapper 就代表一个具体的业务


## 5. 解决类属性和表字段不匹配问题

### 5.1 引言

若存在一个表含有字段 `id, user, pwd` 存在一个对象 `id, user, password`, 若通过 `SELECT * FROM user WHERE id=#{id};` 得到 User 实例, 则 `password` 值为 `null`

这是由于 user 表没有 password 字段导致的

### 5.2 解决方案

* 更改sql语句(为字段去别名)

   `SELECT id, user, pwd AS Password FROM user WHERE id=#{id};`

* resultMap 结果集映射

   ```xml
   <!--在对应的 Mapper 中 -->
   <resultMap id="resultMap" type="User">
       <!-- column 对应字段, property 对应熟悉 -->
       <result column="pwd" property="password"/>
   </resultMap>
   ```

   * 新建一个 `resultMap` 标签, 并设置 id 和 type 

   * 映射字段和属性

   * 在需要的位置使用

     ```xml
     <select id="selectUser" resultMap="resultMap">
        SELECT * FROM user;
     </select>
     ```

     resultMap 的值就是上面的 `resultMap`标签的id, 注意不用设置 resultType 属性

### 5.3 resultMap标签认知

resultMap 的设计思想是, 对于简单的语句不需要配置显式的结果映射, 对于复杂的语句只需要描述他们的关系

## 6. 日志

### 6.1 日志工厂

* 如果一个数据库操作出现了异常, 可以通过日志排错

* 在 Mybatis 中具体使用哪一个日志需要设置.

* 可选的日志工厂

   * SLF4J
   * **LOG4J**
   * LOG4J2
   * JDK_LOGGING
   * COMMONS_LOGGING
   * **STDOUT_LOGGING**
   * NO_LOGGING

* example

   ```xml
   <!-- mybatis-config.xml>
   <settings>
       <setting name="logImpl" value="STDOUT_LOGGING" />
   </settings>
   ```

* LOG4J 需要导包, STDOUT_LOGGING 可以直接用

### 6.2 LOG4J

**认识 LOG4J**

* LOG4J 是 Apache 的一个开源项目, 可以通过它控制日志输出的目的地(控制台、文件、gui、服务器)
* 可以控制日志输出格式
* 可以定义日志等级
* 通过配置文件配置

**使用LOG4J**

* 导入 LOG4J 包

  ```xml
  <!-- https://mvnrepository.com/artifact/log4j/log4j -->
  <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
  </dependency>
  ```

* resources 目录下创建 log4j.properties 并进行配置

* 配置 LOG4J 为日志的实现

**自定义类使用 LOG4J**

* 在要使用 LOG4J 的类中, 导入包 `import org.apache.log4j.Logger;` 并添加静态属性 logger

   ```java
   static Logger logger = Logger.getLogger(TestClass.class);
   ```

   TestClass 换成具体的类

* 设计日志

## 7. 分页

### 7.1 sql 基础

```mysql
/* 从第 0 条开始, 搜索 5 条数据*/
SELECT * FROM user LIMIT 0, 5;

/* 只有一个参数, 默认从 0 开始*/
SELECT * FROM user LIMIT 3;
```

### 7.2 Mybatis 实现分页

**核心是sql**

1. 接口

   ```java
   List<User> getUserByLimit(Map<String, Integer> map);
   ```

2. Mapper.xml

   ```xml
   <select id="getUserByLimit" parameterType="map" resultType="package.User">
       SELECT * FROM user LIMIT #{startIndex}, #{pageSize};
   </select>
   ```

3. 测试
    
   ```java
   public void getUserByLimitTest() {
       // 自己实现得工具类 MybatisUtils
       SqlSession sqlSession = MybatisUtils.getSqlSession();

       UserMapper mapper  = sqlSession.getMapper(UserMapper.class);

       HashMap<String, Integer> map = new HashMap<String, Integer>();
       map.put("startIndex", 0);
       map.put("pageSize", 2);

       List<User> userList = mapper.getUserByLimit(map);

       sqlSession.close();
   }
   ```

### 7.3 RowBounds 实现分页

**不建议开发使用, 面向对象没有 sql 快**

本质是先查询所有数据, 然后通过在 java 代码层面实现分页

### 7.4 分页插件

1. [Mybatis PageHelper](https://pagehelper.github.io/docs/)

## 8. 使用注解开发

**注解通过反射实现, 底层是动态代理**

### 8.1 基础使用

1. 注解在接口上实现

   ```java
   @SELECT("SELECT * FROM user;")
   List(User) getUsers();
   ```

2. 核心配置文件绑定接口

   ```xml
   <mappers>
       <mapper class="com.eastarpen.User" />
   </mappers>
   ```

缺点, 若属性和字段存在冲突, 难以解决

简单的可以用

### 8.2 带参数使用

* 若存在多个参数每个必须用注解声明, 建议单个也声明

   ```java
   @Select("SELECT * FROM user WHERE id = #{id}"
   User getUserById(@Param("id") int id);
   ```
   
* 取参数值`#{}` 的时候以注解为准
* 基本类型或String @Param 声明, 引用类型不用


   ```java
   @Insert("INSERT INTO user (name, pwd) VALUES(#{name}, #{pwd});")
   int addUser(User user);
   ```

## 9. Mybatis 流程

加载配置文件 -> 实例化 SqlSessionFactoryBuilder 构造器 -> 解析配置文件流 XMLConfigBuilder -> Configuration 对象 -> 实例化 SqlSessionFactory -> transaction 事务管理器 -> 创建executor 执行器 -> 创建 sqlSession -> 实现 CRUD*(可能回滚) -> 提交事务(可能回滚) -> 关闭

可以在创建工具类得时候自动提交事务(不建议开发使用, 可以debug观察流程学习用)

```java
// MybatisUtils
public static SqlSession getSqlSession() {
    return sqlSessionFactory.openSession(true);
}
```

## 9. Lombok

**偷懒用**

使用步骤

1. Idea Plugins install Lombok
2. 导入包
   
   ```xml
   <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <version>1.18.20</version>
       <scope>provided</scope>
   </dependency>
   ```

   3. 添加注解使用(可在Plugins 界面查看注解 index)

    ```java
    // 无参构造, get, set, tostring, hashCode, equals.... 通过 做的 Structure 查看
    @Data 
    public class User {
        private int id;
        private int String;
        private String name;
    }
    ```

## 10. 多对一和一对多案例

### 10.1 案例环境

1. 数据库新建表格 student, teacher, student 中 tid 字段为外键, 指向 teacher id 

2. 构建实体类 Teacher, Student

   * Student 属性 `int id, String name, Teacher teacher`

   * Teacher 属性 `int id, String name, List<Student> studentList`

3. 添加 TeacherMapper, StudentMapper 接口

4. 添加对应得注册文件且绑定接口

**Tip** 分别完成一对多和多对一(简化实体类属性), 然后尝试多对多(完整得实体类属性)

### 10.2 多对一 查询学生信息

1. 按照查询嵌套处理
   
   * **思路**

     * id 和 name 在 student 表格中可以直接查询
     * 先获取 tid 然后通过 tid 获取 teacher

   * **实现**

     ```xml
     <select id="getStudent" resultMap="StudentTeacher">
         SELECT * FROM student;
     </select>
     
     <resultMap id="StudentTeacher" type="Student">
         <!-- student 中有直接查询 -->
         <result property="id" column="id" />
         <result property="name" column="name" />
     
         <!-- student 中没有teacher 需要单独处理-->
         <!-- 结果是对象: association 是集合 collection-->
         <association property="teacher" column="tid" JavaType="Teacher" select="getTeacher" />
     </resultMap>
     
     <select id="getTeacher" resultType="Teacher">
         SELECT * FROM teacher WHERE id=#{tid};
     </select>
     ```

2. 按照结果嵌套处理

   * **实现**

     ```xml
     <select id = "getStudent" resultMap="StudentTeacher">
         SELECT s.id sid, s.name sname, t.name tname 
         FROM student s, teacher t
         WHERE s.tid = t.id;
     </select>
    
     <resultMap id="StudentTeacher" type="Student">
        <result property="id" column="sid" />
        <result property="name" column="sname" />
        <association property="teacher" javaType="Teacher">
            <result property="name" column="tname" />
        </association>
     </resultMap>
     ```

### 10.3 一对多查询 老师

1. 按照结果嵌套处理

   ```xml
   <select id="getTeacherById" resultMap="getTeacherStudent">
       SELECT s.id sid, s.name sname, s.tid stid, t.id tid, t.name tname 
       FROM student s, teacher t
       WHERE tid=s.tid AND tid=#{tid};
   </select>

   <resultMap id="getTeacherStudent" type="Teacher">
       <result property="id" column="tid" />
       <result property="name" column="sname" />

       <!-- javaType 获取属性得种类, ofType 获取泛型得类-->
       <!-- 这里 studentList 是 List<Student> javaType 是 List, ofType 是 Student -->
       <collection property="studentList" ofType="Student">
           <result property="id" column="sid" />
           <result property="name" column="sname" />
           <result property="tid" column="stid" />
       </collection>
   </resultMap>
   ```

2. 按照查询嵌套处理

TODO: 仔细探究 `id, tid` 的传递 P21

```xml
<select id="getTeacherById" resultMap="TeacherStudent">
    SELECT * FROM teacher WHERE id=#{id};
</select>

<select id="getStudentByTid" resultType="Student">
    SELECT * FROM student WHERE tid=#{tid};
</select>

<resultMap id="TeacherStudent" resultType="Teacher">
    <!-- 这里省略了 id, name, TODO 探究为什么可以省略 -->
    <collection property="studentList" JavaType="List" ofType="Student" select="getStudentByTid" column="id" />
</resultMap>
```

## 11. 动态 sql

### 11.1 案例环境

1. Blog 实体类 属性 `id, title, authoer, Date createtime, view`

**Tip**

* 可以用 java.util.UUID 类生成uuid

* MyBatis 设置文件通过 `mapUnderscoreToCamelCase` 可以将数据库的 `_` 分割样式改为 驼峰命名法

  如 `c_test` -> `cText`

### 11.2 if

只有满足条件才会添加 `if` 里的字段

```xml
<select id="queryBlogIF" parameterType="map" resultType="blog">
    SELECT * FROM blog WHERE 1=1
    <if test="title != null">
        AND title= #{title}
    </if>
    <if test="author != null">
        AND authoer = #{author}
    </if>
</select>
```

### 11.3 choose(when, otherwise)

choose 相当于 switch

```xml
<!-- 对比 if 看 -->
<select id="queryBlogChoose" parameterType="map" resultType="blog">
    SELECT * FORM blog 
    <where>
        <choose>
            <when test="title != null">
                title= #{title}
            </when>
            <when test="author != null">
                AND authoer = #{author}
            </when>
            <otherwise>
                AND views = #{views}
            </otherwise>
        <choose>
    </where>
```

### 11.4 trim (where, set)

where tag 可以智能选择是否在 sql 中添加 `AND, OR, WHERE` 关键字

```xml
<!-- 对比 if 看 -->
<select id="queryBlogWhere" parameterType="map" resultType="blog">
    SELECT * FORM blog 
    <where>
        <if test="title != null">
            AND title= #{title}
        </if>
        <if test="author != null">
            AND authoer = #{author}
        </if>
    </where>
```

set 可以只能选择是否需要加上 `,` 用以分割不同关键字

```xml
<update id="updateBlog" parameterType="map">
    UPDATE blog
    <set>
        <if test="title != null">
            title= #{title}
        </if>
        <if test="author != null">
            authoer = #{author}
        </if>
    </set>
    where id = #{uid};
</update>
```

where, set 就是用 trim 实现的

```xml
<trim prefix="SET" suffixOverrides=",">
...
</trim>

<trim prefix="WHERE" prefixOverrides="AND|OR">
...
</trim>
```

### 11.5 foreach

遍历列表进行相关测试

建议通过 map 传入 list

```xml
<foreach item="id" index="index" collection="list"
    open="(" separator="," close=")">
    #{item}
</foreach>
```

### 11.6 sql 片段

* 将一些公共部分抽取出来, 方便复用
* 通过 include tag 应用
   
  ```xml
  <sql id="sql-id">
      <if test="title != null">
          title= #{title}
      </if>
      <if test="author != null">
          authoer = #{author}
      </if>
  </sql>
 
  <!-- 通过 <include refid="sql-id" /> 引用 --> 
  ```

* 注意

  * 最好基于单表编写 sql 片段
  * 不要存在 where 标签

## 12. Mybatis 缓存

### 12.1 简介

1. 什么是缓存

   * 存在内存中的临时数据
   * 将经常查询的结果放到内存中, 若下次查询同一数据, 直接从内存中读取, 从而提高了查询效率, 解决了高并发系统的性能问题

2. 为什么使用缓存

   * 减少和数据库的交互次数, 减少系统开销, 提高系统效率

3. 什么数据使用缓存

  * 经常查询且不常改变的数据

### 12.2 Mybats 缓存

* Mybats默认定义了两级缓存: 一级缓存和二级缓存

* 默认只有一级缓存开启 (SqlSession 级别的缓存, 又称本地缓存)

* 二级缓存需要手动开启配置, 基于 namespace 级别的缓存

* Bybats 定义了缓存接口 Cache, 通过实现 Cache 接口定义二级缓存

* 配合日志使用

* 先看二级缓存有没有, 再看一级缓存有没有, 最后查询数据库

### 12.3 一级缓存

* 与数据库同一次会话期间查询到的数据会放在本地缓存。

* 同意会话相同的查询直接从内存调用

* 增删改会刷新缓存

* 一级缓存本质是 map

### 12.4 二级缓存 

**工作机制**

1. sqlSession 关闭或提交, 将一级缓存提交到二级缓存
2. 不同的 Mapper 有不同的二级缓存 

**使用**

1. 开启缓存

   mybatis 配置文件 显式开启缓存

   `<setting name="cachedEnabled" value="true">` 

2. 在要使用二级缓存的 Mapper.xml 中 
   
   `<cache />` 可自定义参数 
**注意**

1. `Caused by: java.io.NotSerializableException` 未将实体类序列化
   
   需要将实体类实现接口 `Serializable`

### 12.5 ehcache

**不用深究, 了解即可**

* ehcache 是一种广泛应用的开源Java分布式缓存。 主要面向通用缓存

* 导包
   
  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
  <dependency>
      <groupId>org.mybatis.caches</groupId>
      <artifactId>mybatis-ehcache</artifactId>
      <version>1.1.0</version>
  </dependency>
  ```

* mapper.xml中

  `<cache type="org.mybatis.caches.ehcache.EncacheCache" />` 根据包设置

* resources 目录下创建 `ehcache.xml` 配置文件
