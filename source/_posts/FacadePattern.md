---
title: 设计模式之外观模式
date: 2020-04-24 23:37:36
tags:
    - 设计模式
    - java
    - 面向对象
categories: 设计模式
summary: The Facade Pattern provides a unified interface to a set of interfaces in subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
---

## 外观模式

### 定义

《Head First》

The Facade Pattern provides a unified interface to a set of interfaces in subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.

外观模式提供了一个统一的接口，用来访问子系统中的一群接口。外观定义了一个高层接口，让子系统更容易使用。外观模式实现了最少知识原则（Least Knowledge principle）,这个原则希望不要让太多的类耦合在一起，对用户来说只和一个外观类打交道了，达到客户和一群子系统的解耦。

### 使用场景

- 为复杂的模块或子系统提供外界访问的模块
- 子系统相互独立
- 在层析结构中，可以使用外观模式定义系统的每一层的入口。

### 结构

- Facade: 外观角色，在客户端可以调用它的方法，在外观角色中可以知道相关的（一个或者多个）子系统的功能和责任；在正常情况下，它将所有从客户端发来的请求委派到相应的子系统去，传递给相应的子系统对象处理。
- SubSystem: 子系统角色，在软件系统中可以有一个或者多个子系统角色，每一个子系统可以不是一个单独的类，而是一个类的集合，它实现子系统的功能；每一个子系统都可以被客户端直接调用，或者被外观角色调用，它处理由外观类传过来的请求；子系统并不知道外观的存在，对于子系统而言，外观角色仅仅是另外一个客户端而已。

### 类图

![](FacadePattern.png)

### 实际案例

- 每个Computer都有CPU、Memory、Disk。在Computer开启和关闭的时候，相应的部件也会开启和关闭
- 保安系统的灯、录像机、警报器、遥感器等、操作人员需要将这些仪器启动或者关闭

### 例子

以上面保安系统为例子使用外观模式来操作这个保安系统

#### 类图

![](SecuritySystem.png)

#### SecuritySystem

这是外观角色，客户操作便是它。

```java
public class SecuritySystem {

	private VCRA va = new VCRA();

	private VCRB vb = new VCRB();

	private LampA la = new LampA();

	private LampB lb = new LampB();

	private LampC lc = new LampC();

	private Annunciator a = new Annunciator();

	private RemoteControl r = new RemoteControl();

	public void turnOn() {
        va.turnOn();
        vb.turnOn();
        la.turnOn();
        lb.turnOn();
        lc.turnOn();
        a.turnOn();
        r.turnOn();
	}

	public void turnOff() {
        va.turnOff();
        vb.turnOff();
        la.turnOff();
        lb.turnOff();
        lc.turnOff();
        a.turnOff();
        r.turnOff();
	}
}
```

#### Lamp

实现三个灯

```java
   //第一个灯
public class LampA {

	public void turnOn() {
		System.out.println("打开电灯A");
	}

	public void turnOff() {
		System.out.println("关闭电灯A");
	}

}

//第二个灯
public class LampB {

	public void turnOn() {
		System.out.println("打开电灯B");
	}

	public void turnOff() {
		System.out.println("关闭电灯B");
	}

}

//第三个灯
public class LampC {

	public void turnOn() {
		System.out.println("打开电灯C");
	}

	public void turnOff() {
		System.out.println("关闭电灯C");
	}

}
```

#### VCR

实现两个录像机

```java
//第一个录像机
public class VCRA {

	public void turnOn() {
		System.out.println("打开录像机A");
	}

	public void turnOff() {
		System.out.println("关闭录像机A");
	}
}

//第二个录像机
public class VCRB {

	public void turnOn() {
          System.out.println("打开录像机B");
	}

	public void turnOff() {
		System.out.println("关闭录像机B");
	}
}
```

#### Annunciator

警报器

```java
public class Annunciator {

	public void turnOn() {
		System.out.println("打开报警器");
	}

	public void turnOff() {
		System.out.println("关闭报警器");
	}
}
```

#### RemoteControl

遥控器

```java
public class RemoteControl {

	public void turnOn() {
		System.out.println("打开遥控器");
	}

	public void turnOff() {
		System.out.println("关闭遥控器");
	}
}
```

#### Client

测试类实现一下看看效果

```java
public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
       SecuritySystem securitySystem=new SecuritySystem();
       securitySystem.turnOn();
       System.out.println("**********");
       securitySystem.turnOff();
	}

}
```

#### 输出

```
打开录像机A
打开录像机B
打开电灯A
打开电灯B
打开电灯C
打开报警器
打开遥控器
**********
关闭录像机A
关闭录像机B
关闭电灯A
关闭电灯B
关闭电灯C
关闭报警器
关闭遥控器
```

## 总结

### 优点

- 降低了子系统与客户端之间的耦合度，使得子系统的变化不会影响调用它的客户类。

-  对客户屏蔽了子系统组件，减少了客户处理的对象数目，并使得子系统使用起来更加容易。
-  降低了大型软件系统中的编译依赖性，简化了系统在不同平台之间的移植过程，因为编译一个子系统不会影响其他的子系统，也不会影响外观对象。

### 缺点

- 不能很好地限制客户使用子系统类。
- 增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。