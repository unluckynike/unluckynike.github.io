---
title: MyBatis
date: 2020-08-07 19:49:32
cover: true
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/girl-2067378_1920.jpg
tags: 
   - Mybatis
categories: SSM
summary: mybatis 是一个优秀的基于java的持久层框架，它内部封装了jdbc，使开发者只需要关注sql语句本身，而不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。
---

## MyBatis

> mybatis 是一个优秀的基于java的持久层框架，它内部封装了jdbc，使开发者只需要关注sql语句本身，而不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。
>
> mybatis通过xml或注解的方式将要执行的各种 statement配置起来，并通过java对象和statement中sql的动态参数进行映射生成最终执行的sql语句。
>
> 最后mybatis框架执行sql并将结果映射为java对象并返回。采用ORM思想解决了实体和数据库映射的问题，对jdbc 进行了封装，屏蔽了jdbc api 底层访问细节，使我们不用与jdbc api 打交道，就可以完成对数据库的持久化操作。

**Mybatis官网地址**[http://www.mybatis.org/mybatis-3/](http://www.mybatis.org/mybatis-3/)

## 步骤

1. 添加MyBatis的坐标
2. 创建student数据表
3. 编写Student实体类 
4. 编写映射文件SqlMapConfig.xml
5. 编写核心文件studentMapper.xml
6. 编写测试类

## 导入依赖坐标

**pom.xml**

```xml
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
<!--mybatis坐标-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.5</version>
    </dependency>
<!--mysql驱动坐标-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.6</version>
      <scope>runtime</scope>
    </dependency>
<!--日志坐标-->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.12</version>
    </dependency>
```

## 创建实体类

创建Student类，并且创建好mysql数据库的`student`表，表中分别是`id` ，`name`，`sex`和`score`字段注意命名，以免后面Mapper映射文件冲突。

```java
public class Student {
   private Integer id;
   private String name;
   private String sex;
   private int score;

    public void setSex(String sex) {
        this.sex = sex;
    }
    public String getSex() {
        return sex;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void setScores(int scores) {
        this.score = scores;
    }
    public String getName() {
        return name;
    }
    public int getScores() {
        return score;
    }
    public Integer getId() {
        return id;
    }
    @Override
    public String toString() {
        return "Student{" +java
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", scores=" + score +
                '}';
    }
}
```

## 配置

在resources目录下创建`jdbc.properties`文件和`sqlMapConfig.xml`文件.

**jdbc.properties**

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/testjdbc?useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=
```

**sqlMapConfig**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--properties标签：该标签可以加载外部的properties文件-->
    <properties resource="jdbc.properties"></properties>
<!--environments标签：数据源环境配置标签-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
<!--mappers标签：加载映射配置-->
   <mappers>
       <mapper resource="vip/wulinzeng/mapper/StudentMapper.xml"></mapper>
   </mappers>
</configuration>
```

**environments标签**

数据库环境的配置，支持多环境配置

其中，事务管理器（transactionManager）类型有两种：

•JDBC：这个配置就是直接使用了JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。

•MANAGED：这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。

其中，数据源（dataSource）类型有三种：

•UNPOOLED：这个数据源的实现只是每次被请求时打开和关闭连接。

•POOLED：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来。

•JNDI：这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用

**mapper标签**

该标签的作用是加载映射的，加载方式有如下几种：

•使用相对于类路径的资源引用，例如：

•使用完全限定资源定位符（URL），例如：

•使用映射器接口实现类的完全限定类名，例如：

•将包内的映射器接口实现全部注册为映射器，例如：

**Properties标签**

实际开发中，习惯将数据源的配置信息单独抽取成一个properties文件，该标签可以加载额外配置的properties文件

## 核心文件

在resources中创建**StudentMapper.xml**这里要注意路径问题。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper  namespace="studentMapper">
    <select id="findAll" resultType="vip.wulinzeng.pojo.Student">
        select * from student
    </select>

    <insert id="add" parameterType="vip.wulinzeng.pojo.Student">
        insert into student values(#{id},#{name},#{sex},#{score})
    </insert>

    <update id="update" parameterType="vip.wulinzeng.pojo.Student">
        update student set name=#{name},sex=#{sex},score=#{score}
        where id=#{id}
    </update>

    <delete id="delete" parameterType="int">
        delete from student where id=#{id}
    </delete>

</mapper>
```

## 测试

在test文件下编写测试类，

```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class Test {
    //查询
    @org.junit.Test
    public void queryAll() throws IOException {
        //加载核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        //获得sqlSession工厂对象
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
        //获得sqlSession对象
        SqlSession sqlSession = build.openSession();
        //执行sql语句
        List<Student> student = sqlSession.selectList("studentMapper.findAll");
        System.out.println(student);
        //释放资源
        sqlSession.close();
    }

    //插入
    @org.junit.Test
    public void add() throws IOException {
        Student std=new Student();
        std.setName("貂蝉");
        std.setSex("女");
        std.setScores(89);
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = build.openSession();
        sqlSession.insert("studentMapper.add",std);
        //提交事务
        sqlSession.commit();
        sqlSession.close();
    }

    //修改
    @org.junit.Test
    public void update() throws IOException {
        Student std=new Student();
        std.setId(1);
        std.setName("孙尚香");
        std.setSex("女");
        std.setScores(68);
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = build.openSession();
        sqlSession.update("studentMapper.update",std);
        sqlSession.commit();
        sqlSession.close();
    }

    //删除
    @org.junit.Test
    public void delete() throws IOException {
        Student std=new Student();
        std.setId(6);
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = build.openSession();
        sqlSession.delete("studentMapper.delete",std);
        sqlSession.commit();
        sqlSession.close();
    }
}
```

## Mybatis相应API

**SqlSession工厂构建器SqlSessionFactoryBuilder**

SqlSessionFactory  build(InputStream inputStream)

通过加载mybatis的核心文件的输入流的形式构建一个SqlSessionFactory对象其中， Resources 工具类，这个类在 org.apache.ibatis.io 包中。Resources 类帮助你从类路径下、文件系统或一个 web URL 中加载资源文件。

```java
String resource = "org/mybatis/builder/mybatis-config.xml"; 
InputStream inputStream = Resources.getResourceAsStream(resource); 
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder(); 
SqlSessionFactory factory = builder.build(inputStream);
```

**SqlSession工厂对象SqlSessionFactory**

SqlSessionFactory 有多个个方法创建SqlSession 实例。常用的有如下两个：

| 方法                            | 解释                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| openSession()                   | 会默认开启一个事务，但事务不会自动提交，也就意味着需要手动提交该事务，更新操作数据才会持久化到数据中 |
| openSession(boolean autoCommit) | 参数为是否自动提交，如果设置为true，那么不需要手动提交事务   |

**SqlSession会话对象**

SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有执行语句、提交或回滚事务和获取映射器实例的方法。

执行语句的方法主要有

```java
<T> T selectOne(String statement, Object parameter) 
<E> List<E> selectList(String statement, Object parameter) 
int insert(String statement, Object parameter) 
int update(String statement, Object parameter) 
int delete(String statement, Object parameter)
```

操作事务的方法主要有

```java
void commit()  
void rollback() 
```