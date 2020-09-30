---
title: SSM配置
date: 2020-08-17 08:55:16
cover: true
top: true
tags: 
    - Spring
    - SpringMVC
    - MyBatis
categories: SSM
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/wulinzengblog/Ee1nlx7WAAEuyMu.jpg
summary: SSM框架集由Spring、SpringMVC、MyBatis三个开源框架整合而成，常作为数据源较简单的web项目的框架。
---

> SSM框架集由Spring、SpringMVC、MyBatis三个开源框架整合而成，常作为数据源较简单的web项目的框架。

> Eclipse

## 文件结构

```text
│  web.xml
│  
├─config
│  │  db.properties
│  │  log4j.properties
│  │  
│  ├─mybatis
│  │  │  mybatis-config.xml
│  │  │  
│  │  └─mapper
│  ├─spring
│  │      applicationContext.xml
│  │      
│  └─springmvc
│          springmvc.xml
│          
└─lib
        aspectjweaver-1.8.9.jar
        c3p0-0.9.1.jar
        commons-beanutils-1.7.0.jar
        commons-collections-3.2.jar
        commons-fileupload-1.3.3.jar
        commons-io-2.2.jar
        commons-lang-2.3.jar
        commons-logging-1.1.jar
        ezmorph-1.0.4.jar
        gson-1.6.jar
        hamcrest-core-1.3.jar
        jackson-annotations-2.8.0.jar
        jackson-core-2.8.9.jar
        jackson-databind-2.8.9.jar
        javax.servlet-api-3.1.0.jar
        json-lib-2.2.1-jdk15.jar
        json-simple-1.1.1.jar
        jsp-api-2.2.jar
        jsqlparser-1.0.jar
        jstl-1.2.jar
        junit-4.12.jar
        log4j.jar
        mybatis-3.4.4.jar
        mybatis-generator-core-1.3.5.jar
        mybatis-spring-1.3.1.jar
        mysql-connector-java-5.1.29.jar
        pagehelper-5.0.3.jar
        spring-aop-4.3.9.RELEASE.jar
        spring-aspects-4.3.9.RELEASE.jar
        spring-beans-4.3.9.RELEASE.jar
        spring-context-4.3.9.RELEASE.jar
        spring-core-4.3.9.RELEASE.jar
        spring-expression-4.3.9.RELEASE.jar
        spring-jdbc-4.3.9.RELEASE.jar
        spring-test-4.3.9.RELEASE.jar
        spring-tx-4.3.9.RELEASE.jar
        spring-web-4.3.9.RELEASE.jar
        spring-webmvc-4.3.9.RELEASE.jar
```

## web.xml

> web.xml是Java Web项目的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  
  	<!-- 中文乱码处理 -->
  	<filter>
  		<filter-name>CharacterEncodingFilter</filter-name>
  		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  	
  		<init-param>
  			<param-name>encoding</param-name>
  			<param-value>UTF-8</param-value>
  		</init-param>
  		<init-param>
  			<param-name>forceEncoding</param-name>
  			<param-value>true</param-value>
  		</init-param>
  	</filter>
  	
  	<filter-mapping>
  		<filter-name>CharacterEncodingFilter</filter-name>
  		<url-pattern>/*</url-pattern>
  	</filter-mapping>
  	
  	<filter>
  		<filter-name>HiddenHttpMethodFilter</filter-name>
  		<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  	</filter>
  	
  	<filter-mapping>
  		<filter-name>HiddenHttpMethodFilter</filter-name>
  		<url-pattern>/*</url-pattern>
  	</filter-mapping>
  
  	<!-- Spring配置文件信息 -->
  	<context-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:config/spring/applicationContext.xml</param-value>
  	</context-param>
  	<!-- ContextLoaderListener监听器 -->
  	<listener>
  		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  	</listener>
  	<!-- 日志配置 -->
	  <context-param>
	   <param-name>log4jConfigLocation</param-name>
	   <param-value>classpath:config/log4j.properties</param-value>
	</context-param>
	<listener>
	   <listener-class>org.springframework.web.util.Log4jConfigListener</listener-class>
	</listener>
  
  	<!-- 配置前端控制器 -->
	<servlet>
		<servlet-name>DispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:config/springmvc/springmvc.xml</param-value>
		</init-param>
		
		<load-on-startup>1</load-on-startup>
	</servlet>
	
	<servlet-mapping>
		<servlet-name>DispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	
	<error-page>
		<error-code>404</error-code>
		<location>/WEB-INF/errors/404.jsp</location>
	</error-page>

	<error-page>
		<error-code>500</error-code>
		<location>/WEB-INF/errors/500.jsp</location>
	</error-page>
  <!-- 描述项目运行的首页 -->
	  <welcome-file-list>
	    <welcome-file>index.jsp</welcome-file>
	  </welcome-file-list>
  
</web-app>
```

## db.properties

> mysql数据库
>
> db_xxx  数据库名称
>
> 8.0以上url添加时区 serverTimezone=UTC

```
datasource.connection.driver_class=com.mysql.jdbc.Driver
datasource.connection.url=jdbc:mysql://localhost:3306/db_xxx?useUnicode=true&characterEncoding=utf-8
datasource.connection.username=root
datasource.connection.password=
#连接池保持的最小连接数,default : 3（建议使用）
datasource.connection.minPoolSize=3
#连接池中拥有的最大连接数，如果获得新连接时会使连接总数超过这个值则不会再获取新连接，而是等待其他连接释放，所以这个值有可能会设计地很大,default : 15（建议使用）
datasource.connection.maxPoolSize=15
#连接的最大空闲时间，如果超过这个时间，某个数据库连接还没有被使用，则会断开掉这个连接。如果为0，则永远不会断开连接,即回收此连接。default : 0 单位 s（建议使用）
datasource.connection.maxIdleTime=0
#连接池在无空闲连接可用时一次性创建的新数据库连接数,default : 3（建议使用）
datasource.connection.acquireIncrement=3
#连接池为数据源缓存的PreparedStatement的总数。由于PreparedStatement属于单个Connection,所以这个数量应该根据应用中平均连接数乘以每个连接的平均PreparedStatement来计算。同时maxStatementsPerConnection的配置无效。default : 0（不建议使用）
datasource.connection.maxStatements=0
#连接池为数据源单个Connection缓存的PreparedStatement数，这个配置比maxStatements更有意义，因为它缓存的服务对象是单个数据连接，如果设置的好，肯定是可以提高性能的。为0的时候不缓存。default : 0（看情况而论）
datasource.connection.maxStatementsPerConnection=0
#连接池初始化时创建的连接数,default : 3（建议使用）
datasource.connection.initialPoolSize=3
#用来配置测试空闲连接的间隔时间。测试方式还是上面的两种之一，可以用来解决MySQL8小时断开连接的问题。因为它保证连接池会每隔一定时间对空闲连接进行一次测试，从而保证有效的空闲连接能每隔一定时间访问一次数据库，将于MySQL8小时无会话的状态打破。为0则不测试。default : 0(建议使用)
datasource.connection.idleConnectionTestPeriod=0
#连接池在获得新连接失败时重试的次数，如果小于等于0则无限重试直至连接获得成功。default : 30（建议使用）
datasource.connection.acquireRetryAttempts=30
#如果为true，则当连接获取失败时自动关闭数据源，除非重新启动应用程序。所以一般不用。default : false（不建议使用）
datasource.connection.breakAfterAcquireFailure=false
#性能消耗大。如果为true，在每次getConnection的时候都会测试，为了提高性能,尽量不要用。default : false（不建议使用）
datasource.connection.testConnectionOnCheckout=false
#配置当连接池所有连接用完时应用程序getConnection的等待时间。为0则无限等待直至有其他连接释放或者创建新的连接，不为0则当时间到的时候如果仍没有获得连接，则会抛出SQLException。其实就是acquireRetryAttempts*acquireRetryDelay。default : 0（与上面两个，有重复，选择其中两个都行）
datasource.connection.checkoutTimeout=30000
#如果为true，则在close的时候测试连接的有效性。default : false（不建议使用）
datasource.connection.testConnectionOnCheckin=false
#配置一个表名，连接池根据这个表名用自己的测试sql语句在这个空表上测试数据库连接,这个表只能由c3p0来使用，用户不能操作。default : null（不建议使用）
datasource.connection.automaticTestTable=c3p0TestTable
#连接池在获得新连接时的间隔时间。default : 1000 单位ms（建议使用）
datasource.connection.acquireRetryDelay=1000
#为0的时候要求所有的Connection在应用程序中必须关闭。如果不为0，则强制在设定的时间到达后回收Connection，所以必须小心设置，保证在回收之前所有数据库操作都能够完成。这种限制减少Connection未关闭情况的不是很适用。建议手动关闭。default : 0 单位 s（不建议使用）
datasource.connection.unreturnedConnectionTimeout=0
#这个配置主要是为了快速减轻连接池的负载，比如连接池中连接数因为某次数据访问高峰导致创建了很多数据连接，但是后面的时间段需要的数据库连接数很少，需要快速释放，必须小于maxIdleTime。其实这个没必要配置，maxIdleTime已经配置了。default : 0 单位 s（不建议使用）
datasource.connection.maxIdleTimeExcessConnections=0
#配置连接的生存时间，超过这个时间的连接将由连接池自动断开丢弃掉。当然正在使用的连接不会马上断开，而是等待它close再断开。配置为0的时候则不会对连接的生存时间进行限制。default : 0 单位 s（不建议使用）
datasource.connection.maxConnectionAge=0
```

## log4j.properties

> 日志打印

```
### direct log message to stdout ###
log4j.appender.stdout.Target = System.out
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern =  %d{ABSOLUTE} %5p %c{1}:%L - %m%n

log4j.rootLogger=INFO, stdout
```

## mapper

**mapper**

> mapper 下为实体类映射文件 xxxMapper.xml


```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

**mybatis-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 暂时不需做任何配置 -->
</configuration>
```

## applicationContext.xml

> 包替换 vip.wulinzeng
>
> 数据源（连接池）：C3P0

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
  	 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
 	 http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd">

	<context:component-scan base-package="vip.wulinzeng">
		<context:include-filter type="annotation"
			expression="org.springframework.stereotype.Component" />
		<context:include-filter type="annotation"
			expression="org.springframework.stereotype.Repository" />
		<context:include-filter type="annotation"
			expression="org.springframework.stereotype.Service" />
	</context:component-scan>

	<!-- 加载配数据源配置文件 db.properties -->
	<context:property-placeholder location="classpath:config/db.properties" />

	<!-- 配置 C3P0 数据源 -->

	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" destroy-method="close">
		<property name="driverClass" value="${datasource.connection.driver_class}"/>
		<property name="jdbcUrl" value="${datasource.connection.url}"/>
		<property name="user" value="${datasource.connection.username}"/>
		<property name="password" value="${datasource.connection.password}"/>
		<property name="minPoolSize" value="${datasource.connection.minPoolSize}"/>
		<!--连接池中保留的最大连接数。Default: 15 -->
		<property name="maxPoolSize" value="${datasource.connection.maxPoolSize}"/>
		<!--最大空闲时间,60秒内未使用则连接被丢弃。若为0则永不丢弃。Default: 0 -->
		<property name="maxIdleTime" value="${datasource.connection.maxIdleTime}"/>
		<!--当连接池中的连接耗尽的时候c3p0一次同时获取的连接数。Default: 3 -->
		<property name="acquireIncrement" value="${datasource.connection.acquireIncrement}"/>
		<!--JDBC的标准参数，用以控制数据源内加载的PreparedStatements数量。但由于预缓存的statements 属于单个connection而不是整个连接池。所以设置这个参数需要考虑到多方面的因素。
            如果maxStatements与maxStatementsPerConnection均为0，则缓存被关闭。Default: 0 -->
		<property name="maxStatements" value="${datasource.connection.maxStatements}"/>
		<!--maxStatementsPerConnection定义了连接池内单个连接所拥有的最大缓存statements数。Default: 0 -->
		<property name="maxStatementsPerConnection"
				  value="${datasource.connection.maxStatementsPerConnection}"/>
		<!--初始化时获取三个连接，取值应在minPoolSize与maxPoolSize之间。Default: 3 -->
		<property name="initialPoolSize" value="${datasource.connection.initialPoolSize}"/>
		<!--每60秒检查所有连接池中的空闲连接。Default: 0 -->
		<property name="idleConnectionTestPeriod"
				  value="${datasource.connection.idleConnectionTestPeriod}"/>
		<!--定义在从数据库获取新连接失败后重复尝试的次数。Default: 30 -->
		<property name="acquireRetryAttempts"
				  value="${datasource.connection.acquireRetryAttempts}"/>
		<!--获取连接失败将会引起所有等待连接池来获取连接的线程抛出异常。但是数据源仍有效 保留，并在下次调用getConnection()的时候继续尝试获取连接。如果设为true，那么在尝试
            获取连接失败后该数据源将申明已断开并永久关闭。Default: false -->
		<property name="breakAfterAcquireFailure"
				  value="${datasource.connection.breakAfterAcquireFailure}"/>
		<!--因性能消耗大请只在需要的时候使用它。如果设为true那么在每个connection提交的 时候都将校验其有效性。建议使用idleConnectionTestPeriod或automaticTestTable
            等方法来提升连接测试的性能。Default: false -->
		<property name="testConnectionOnCheckout"
				  value="${datasource.connection.testConnectionOnCheckout}"/>
		<property name="checkoutTimeout" value="${datasource.connection.checkoutTimeout}"/>
		<property name="testConnectionOnCheckin"
				  value="${datasource.connection.testConnectionOnCheckin}"/>
		<property name="automaticTestTable" value="${datasource.connection.automaticTestTable}"/>
		<property name="acquireRetryDelay" value="${datasource.connection.acquireRetryDelay}"/>
		<!--自动超时回收Connection-->
		<property name="unreturnedConnectionTimeout" value="${datasource.connection.unreturnedConnectionTimeout}"/>
		<!--超时自动断开-->
		<property name="maxIdleTimeExcessConnections" value="${datasource.connection.maxIdleTimeExcessConnections}"/>
		<property name="maxConnectionAge" value="${datasource.connection.maxConnectionAge}"/>

	</bean>

	<!-- 事务管理器 （JDBC） -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

	<!-- 启动声明式事务驱动 -->
	<tx:annotation-driven transaction-manager="transactionManager" />


	<!-- spring 通过 sqlSessionFactoryBean 获取 sqlSessionFactory 工厂类 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<!-- 扫描 po 包，使用别名 -->
		<property name="typeAliasesPackage" value="vip.wulinzeng.entity"></property>
		<!-- 扫描映射文件 -->
		<property name="mapperLocations" value="classpath:config/mybatis/mapper/*.xml"></property>
	</bean>

	<!-- 配置扫描 dao 包，动态实现 dao 接口，注入到 spring 容器中 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="vip.wulinzeng.dao" />
		<!-- 注意使用 sqlSessionFactoryBeanName 避免出现spring 扫描组件失效问题 -->
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
	</bean>

	<bean id="gson" class="com.google.gson.Gson" scope="prototype"></bean>

</beans>
```

## springmvc.xml

>包替换 vip.wulinzeng

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:task="http://www.springframework.org/schema/task"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd
	 	http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.2.xsd">

	<!-- 只需要扫描包中的 Controller 注解 -->
	<context:component-scan base-package="vip.wulinzeng.controller">
	<context:include-filter type="annotation"
		expression="org.springframework.stereotype.Controller" />
	</context:component-scan>

	<!-- 启动 mvc 注解驱动 -->
	<mvc:annotation-driven></mvc:annotation-driven>
	
	<!-- 启动定时任务 -->
	<task:annotation-driven/>
	
	<!-- 静态资源处理 -->
	<mvc:default-servlet-handler/>
	
	<!-- 配置视图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 文件上传 -->
	<bean id="multipartResolver" 
		class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<!-- 上传文件大小限制 -->
		<property name="maxUploadSize">  
            <value>10485760</value>  
        </property>  
        <!-- 请求的编码格式, 和 jsp 页面一致 -->
        <property name="defaultEncoding">
            <value>UTF-8</value>
        </property>
	</bean>
	
	<!-- 后台访问拦截器 -->
	
	 <mvc:interceptors>
		<mvc:interceptor>
			<mvc:mapping path="/**"/>
			<!--<mvc:mapping path="/grade/*"/>-->
			<mvc:exclude-mapping path="/system/login"/>
			<mvc:exclude-mapping path="/system/get_cpacha"/>
			<mvc:exclude-mapping path="/h-ui/**"/>
			<mvc:exclude-mapping path="/easyui/**"/>
			<mvc:exclude-mapping path="/home-resources/**"/>
			<mvc:exclude-mapping path="/home/**"/>
			<bean class="com.ischoolbar.programmer.interceptor.LoginInterceptor"></bean>
		</mvc:interceptor>
	</mvc:interceptors> 
</beans>
```

>  IDEA maven 项目

## Maven坐标

pom.xml

```xml
 <!--spring相关-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>

        <!--servlet和jsp-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.0</version>
        </dependency>

        <!--mybatis相关-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>1.3.1</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
```

