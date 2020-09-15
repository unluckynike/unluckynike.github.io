---
title: 设计模式之模板方法模式
date: 2020-04-22 08:24:18
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/TemplateMethodPattern.jpg
tags:
  - 设计模式
  - java
  - 面向对象 
categories: 设计模式
summary: The Template Method Pattern defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclass redefine certain steps of an algorithm without changing the algorithm's structure.
---

## 模板方法模式

### 定义

《Head First》

The Template Method Pattern defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclass redefine certain steps of an algorithm without changing the algorithm's structure.

模板方法模式在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以在不改变算法结构的情况下，重新定义算法中的某些步骤。

模板方法就是一个固定步骤的“算法”骨架方法。这个算法的可变部分通过继承，在子类中重载实现。这样就可以在算法骨架不变的情况下，算法细节步骤根据不同的需求进行适应的改变

### 使用场景

- 一次性实现一个算法的不变的部分，并将可变的行为留给子类来实现
- 各子类中公共的行为应该被提取出来并集中到一个公共父类中避免代码重复
- 控制子类的扩展

### 结构

- AbstractClass: 抽象类，定义抽象原语操作，具体子类将重定义他们以实现一个算法，实现一个模板方法，定义一个算法的骨架。改模板方法不仅调用原语操作，也调用定义。
- ConcreteClass: 具体子类，实现原语操作以完成算法中与特点子类相关的步骤

### 类图

![](TemplateMethodPattern.png)

### 实际案例

- 泡茶，都需要先煮沸水，然后加入茶叶，然后根据需求加入调料（柠檬，蜂蜜），再将泡好的茶水倒入杯子
- 去银行办理业务，先要排号，然后办理相关业务（取款，存款），再为本次服务评分，完成业务办理

### 例子

以上述银行业务为例子，取款，存款可以为服务打分，转账可以不用打分

#### 类图

![](Bank.png)

#### Bank 抽象类

```java
public abstract class Bank {

	final void prepareBussiness() {
		getNumber();
		doBusiness();
		if (isJudge()) {
			judgeOrder();
		}
		System.out.println("完成业务");
	}
	
	public void getNumber() {
        System.out.println("已经开始排号");
	}

	public abstract void doBusiness(); 

	public void judgeOrder() {
		System.out.println("完成评分");
	}

	public boolean isJudge() {
		return true;
	}

}
```

#### Deposit存款类

需要继承银行类，并实现自己的存款业务

```java
public class Deposit extends Bank {

	public void doBusiness() {
         System.out.println("存款");
	}

}
```

#### WithdrawMoney取款类

```java
public class WithdrawMoney extends Bank {

	public void doBusiness() {
          System.out.println("取款");
	}

}
```

#### Transfer转账类

转账不需要评分，所以需要重写isJudge方法，让他返回值为false

```java
public class Transfer extends Bank {

  public void doBusiness() {
          System.out.println("转账");
	}

	public boolean isJudge() {
            return false;
	}

}
```

#### Client

再写个测试类运行一下

```java
public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		Deposit deposit=new Deposit();
		System.out.println("*********存款业务***********");
		deposit.prepareBussiness();
	    System.out.println();
	    WithdrawMoney withdrawMoney=new WithdrawMoney();
	    System.out.println("********取款业务**************");
	    withdrawMoney.prepareBussiness();
	    System.out.println();
	    Transfer transfer=new Transfer();
	    System.out.println("*********转账业务*************");
	    transfer.prepareBussiness();	
	}

}
```

#### 输出

```
*********存款业务***********
已经开始排号
存款
完成评分
完成业务

********取款业务**************
已经开始排号
取款
完成评分
完成业务

*********转账业务*************
已经开始排号
转账
完成业务
```



## 总结

### 优点

- 提高代码复用性，将相同部分的代码放在抽象的父类中
- 提高了拓展性，将不同的代码放入不同的子类中，通过对子类的扩展增加新的行为
- 实现了反向控制，通过一个父类调用其子类的操作，通过过对子类的扩展增加新的行为实现了反向控制，符合开闭原则

### 缺点

- 引入了抽象类，每一个不同的实现都需要一个子类来实现，导致类的个数增加，从而增加了系统实现的复杂度