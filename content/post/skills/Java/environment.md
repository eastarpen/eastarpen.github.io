## 1. Maven

### 1.1 hello maven

**Windows**

1. download maven (apache maven download)

2. config environment variables

   * M2_HOME: maven/bin/
   * MAVEN_HOME: maven/
   * path add `%MAVEN_HOME%/bin/`

3. cofig maven

   conf/settings.xml

   * mirror

     [aliyun guide](https://developer.aliyun.com/mirror/maven)
     
   * localRepository

     ```xml
     <localRepository>maven_home\repo</localRepository>
     <!-- using full path -->
     ```

4. maven in Intellij Idea

   setting -> (search)maven 

   * `Maven home path` using maven you just downloaded

   * `User settings file` using the config file you just wrote

   * `Local repository` using the repository you just made

   Tips: IDEA using `~/.m2/` as default repository

5. [mave repository](https://mvnrepository.com/)

### 1.2 understand maven

1. file path 
   
   https://blog.csdn.net/u013514928/article/details/79360870

## 2. Tomcat
