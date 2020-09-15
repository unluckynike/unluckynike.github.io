---
title: Spring-IOC-Set注入
date: 2020-08-06 15:31:36
cover: true
tags: 
    - Spring
categories: SSM
summary: 控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/pictures/person-677770_1920.jpg
---

## IOC

> IOC

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方.

**IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从IoC容器中取出需要的对象。

## pom文件

导入依赖

```xml
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!--导入spring的contenxt坐标 context依赖core beans expression-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.8.RELEASE</version>
    </dependency>
```

## Set注入

> Set注入

要求被注入的属性 , 必须有set方法 , set方法的方法名由set + 属性首字母大写 , 如果属性是boolean类型 , 没有set方法 , 是 is .

```java
public class Salary {
    private double baseSalary;

    public void setBaseSalary(double baseSalary) {
        this.baseSalary = baseSalary;
    }
    public double getBaseSalary() {
        return baseSalary;
    }
}
```

```java
import java.util.*;

public class Staff {

    private String name;
    private Salary baseSalary;
    private String[] task;
    private List<String> hobbys;
    private Map<String, String> card;
    private Set<String> games;
    private Properties infor;

    public void setName(String name) {
        this.name = name;
    }

    public void setBaseSalary(Salary baseSalary) {
        this.baseSalary = baseSalary;
    }

    public void setTask(String[] task) {
        this.task = task;
    }

    public void setHobbys(List<String> hobbys) {
        this.hobbys = hobbys;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public void setInfor(Properties infor) {
        this.infor = infor;
    }

    public void show() {
        System.out.println("name:" + name);
        System.out.println("salary:" + baseSalary.getBaseSalary());
        System.out.print("task:");
        for (String t : task) {
            System.out.println(" " + t);
        }
        System.out.println("hobbys=" + hobbys);
        System.out.println("card=" + card);
        System.out.println("games=" + games);
        System.out.println("infor=" + infor);
    }
}
```

**applicationContext.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--常量注入-->
    <bean id="Salary" class="vip.wulingzeng.pojo.Salary">
        <property name="baseSalary" value="5000"></property>
    </bean>

    <bean id="Staff" class="vip.wulingzeng.pojo.Staff">
        <property name="name" value="Tom"></property>
<!--        引用使用ref-->
        <property name="baseSalary" ref="Salary"></property>
<!--        数组注入-->
        <property name="task" >
            <array>
                <value>写一篇文案</value>
                <value>做一份ppt</value>
                <value>做周报总结</value>
                <value>列出下周任务目标</value>
            </array>
        </property>
<!--        List注入-->
       <property name="hobbys">
           <list>
               <value>唱歌</value>
               <value>写作</value>
               <value>弹吉他</value>
           </list>
       </property>
<!--        map注入-->
        <property name="card">
            <map>
                <entry key="建设银行" value="54131654867631"></entry>
                <entry key="工商银行" value="21354854564154"></entry>
            </map>
        </property>
<!--            set注入-->
        <property name="games">
            <set>
                <value>地下城与勇士</value>
                <value>英雄联盟</value>
            </set>
        </property>
<!--        proprerties注入-->
        <property name="infor">
            <props>
                <prop key="学历">本科</prop>
                <prop key="住址">太原街</prop>
                <prop key="姓名">Tom</prop>
            </props>
        </property>
    </bean>

</beans>
```

**Test**

```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    
    @org.junit.Test
    public void test01() {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
        Staff staff = (Staff) app.getBean("Staff");
        staff.show();
    }
}
```

**结果**

```
name:Tom
salary:5000.0
task: 写一篇文案
 做一份ppt
 做周报总结
 列出下周任务目标
hobbys=[唱歌, 写作, 弹吉他]
card={建设银行=54131654867631, 工商银行=21354854564154}
games=[地下城与勇士, 英雄联盟]
infor={姓名=Tom, 住址=太原街, 学历=本科}
```

