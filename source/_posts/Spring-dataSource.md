---
title: Spring配置数据源
date: 2020-08-15 21:34:30
cover: true
tags: 
    - Spring
categories: SSM
summary: 数据库连接池（Connection pooling）是程序启动时建立足够的数据库连接，并将这些连接组成一个连接池，由程序动态地对池中的连接进行申请，使用，释放。
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/Ee1nldeXkAAGWpI.jpg
---

> 数据库连接池（Connection pooling）是程序启动时建立足够的数据库连接，并将这些连接组成一个连接池，由程序动态地对池中的连接进行申请，使用，释放。
>
> 简单说，它是存储连接通道对象，能提高程序性能。如事先实例化数据源，初始化部分连接资源；使用连接资源时从数据源中获取；使用完毕后将连接资源归还给数据源
>
> 常见的数据源(连接池)：DBCP、C3P0、BoneCP、Druid等

## 手动创建

**导入坐标**

```xml
<!-- C3P0连接池 -->
<dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
</dependency>
<!-- Druid连接池 -->
<dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.10</version>
</dependency>
<!-- mysql驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.32</version>
</dependency>
```

在`test`下创建测试类，并在`mysql`数据库创建`testjdbc`

**Druid**

```java
    @Test
    //测试手动创建druid
    public void test1() throws SQLException {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/testjdbc");
        dataSource.setUsername("root");
        dataSource.setPassword("");
        DruidPooledConnection connection = dataSource.getConnection();
        System.out.println("Druid: " + connection);
        connection.close();
    }
```

控制台出现`Druid: com.mysql.jdbc.JDBC4Connection@1176dcec`信息则表示创建成功

**C3P0**

```java
    @Test
    //测试手动创建c3p0
    public void test0() throws PropertyVetoException, SQLException {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/testjdbc");
        dataSource.setUser("root");
        dataSource.setPassword("");
        Connection connection = dataSource.getConnection();
        //打印数据源地址
        System.out.println("C3P0: " + connection);
        connection.close();
    }
```

同样控制台出现`C3P0: com.mchange.v2.c3p0.impl.NewProxyConnection@71b1176b`类似信息则表示创建成功
## spring容器创建

导入坐标

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.8.RELEASE</version>
    </dependency>
```

**jdbc.properties**写入数据连接信息

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/testjdbc?useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=
```

**抽取jdbc配置文件**

applicationContext.xml加载jdbc.properties配置文件获得连接信息.首先，需要引入context命名空间和约束路径

> 命名空间：xmlns:context="http://www.springframework.org/schema/context"
>
> 约束路径：http://www.springframework.org/schema/context                        
>
> ​                   http://www.springframework.org/schema/context/spring-context.xsd

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation=
               "http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:property-placeholder location="classpath:jdbc.properties"/>
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```

在`test`下测试数据源

```java
    @Test
    //测试从容器中获取数据源
    public void test2() throws SQLException {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        DataSource dataSource = (DataSource) applicationContext.getBean("dataSource");
        Connection connection = dataSource.getConnection();
        System.out.println("数据源："+connection);
    }
```

出现 `数据源：com.mchange.v2.c3p0.impl.NewProxyConnection@7283d3eb `类似信息则表示创建成功