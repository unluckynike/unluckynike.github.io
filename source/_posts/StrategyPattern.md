---
title: 设计模式之策略模式
date: 2020-04-12 13:07:18
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/StrategyPattern.jpg
tags: 
    - 设计模式 
    - java 
    - 面向对象  
categories: 设计模式
summary: The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.
---

## 策略模式

策略模式（Strategy Pattern）中，一个类的行为或其算法可以在运行时更改，这种类型的设计模式属于行为型模式。在策略模式中，我们创建表示各种策略的对象和一个行为随着策略对象改变而改变的context对象，策略对象改变context对象的执行算法。

### 定义

我们先看看《Head First》是怎么定义的

“The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.”

翻译一下，就是说策略模式定义了算法族，分别封装起来，让他们之间可以互相替换，策略模式让算法的变化独立于使用算法的客户。也就是说，我们需要将一个类中经常变化的部分（例如子类覆盖父类的方法）抽离出来，封装成单独的类，然后运用组合的思想，设定为类的成员变量。这样就可以动态的改变该类的行为了（只需要用改变类的成员变量）。这也是书中“favor composition over inheritance”的设计原则。

### 使用场景

1. 针对同一类型问题的多种处理方式，仅仅是具体行为有差别时。
2. 需要安全的封装多种同一类型的操作时。
3. 出现同一抽象类有多个子类，而又需要使用if-else或者switch-case来选择具体子类时。
4. 如果在一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。

### 结构

策略模式涉及到三个角色

- Context: 环境角色，持有一个Strategy的引用,是一个使用了某种策略的类。
- Strategy：抽象策略，这是一个抽象角色，通常由一个接口或抽象类实现，此角色给出所有的具体策略类所需要的接口。
- ConcreteStrategy: 具体策略，包装了相关的算法或者行为。

### 类图

![策略模式](StrategyPattern.png)

### 实际案例

1. 旅行的出游方式，选择骑自行车、坐汽车，每一种旅行方式都是一个策略。
2. JAVA AWT 中的 LayoutManager。
3. 游乐场买门票，普通和会员等不同等级的人会有不同的门票价格。
4. 在极品飞车这款游戏，游戏对车的轮胎是可以更换的，不同的轮胎在高速转弯时有不同的痕迹样式，那么针对汽车的配件轮胎就可以变化，而且轮胎和轮胎之间是可以相互替换的。
5. 电影院电影分类，有2D、3D、4D类型，在不同类型中还分时间段、电影类型、不同类型的电影播放厅。

### 例子

我们就暂且以上述游乐场买票作为实现案例，假设游乐场的门票制度，学生打八折，十岁以下的儿童半价，VIP用户打七折并且随机赠送一个小礼物，并且游乐场在未来可能还会增加用户类型。

我们根据题目描述则可分析出，打折是我们的Strategy（抽象策略），打折的对象是ConcreteStrategy（具体策略），门票便是Context(环境角色)了。

#### 类图

![游乐园打折类图](Discount.png)

#### Discount

```java
public interface Discount {

	public abstract double calculate(double price);

}
```

#### ChildrenDiscount

```java
public class ChildrenDiscount implements Discount {

	public double calculate(double price) {
		return 0.5*price;
	}

}
```

#### StudentDiscount

```java
public class StudentDiscount implements Discount {

	public double calculate(double price) {
		return 0.8*price;
	}

}
```

#### VIPDiscount

```java
public class VIPDiscount implements Discount {

	public double calculate(double price) {
		System.out.println("we give you a gift");
		return 0.7*price;
	}

}
```

#### Ticket

```java
public class Ticket {

	private double price;

	private Discount discount;

	
     public void setPrice(double price) {
    		this.price=price;
     }
     
	public void setDiscount(Discount discount) {
		this.discount=discount;
	}
	
	public double getPrice() {
		return discount.calculate(this.price);
	}

}
```

#### 测试

##### client

```java
public class Client {

	public static void main(String[] args) {
		Ticket ticket = new Ticket();
		ticket.setPrice(80);//设置门票原价八十元
        //儿童打折
		ticket.setDiscount(new ChildrenDiscount());
		System.out.println("Children:" + ticket.getPrice());
       //学生打折
		ticket.setDiscount(new StudentDiscount());
		System.out.println("Student:" + ticket.getPrice());
        //VIP打折
		ticket.setDiscount(new VIPDiscount());
		System.out.println("VIP:" + ticket.getPrice());

	}

}

```

#### 输出

![](Client.png)

## 总结

### 优点

1. 策略模式中算法可以自由切换。
2. 避免了使用多重条件判断。
3. 扩展性良好。

### 缺点

1. 策略类会增多。
2. 所有策略类都需要对外暴露。
3. 如果一个系统的策略多余四个，就需要考虑使用混合模式，解决策略类膨胀的问题。