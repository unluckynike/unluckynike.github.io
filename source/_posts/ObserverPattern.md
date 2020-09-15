---
title: 设计模式之观察者模式
date: 2020-04-20 18:44:40
tags: 
    - 设计模式 
    - java 
    - 面向对象 
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/ObserverPattern.jpg
categories: 设计模式
summary: The Observer Pattern defines as one-to-many dependcy between objects so that when one object changes state, all if its depends are notified and updated automatically.
---

## 观察者模式

### 定义

《Head First》

The Observer Pattern defines as one-to-many dependcy between objects so that when one object changes state, all if its depends are notified and updated automatically.

定义对象间的一种一对多（变化）的依赖关系，以便当一个对象(Subject)的状态发生改变时，所有依赖于它的对象都得到通知并自动更新。

### 使用场景

- 当一个抽象模型有两个方面, 其中一个方面依赖于另一方面。将这二者封装在独立的对象中以使它们可以各自独立地改变和复用。
- 当对一个对象的改变需要同时改变其它对象, 而不知道具体有多少对象有待改变。
- 当一个对象必须通知其它对象，而它又不能假定其它对象是谁。换言之, 你不希望这些对象是紧密耦合的。

### 结构

- Subject：抽象主题，把所有对观察者对象的引用保存在一个集合中，每个抽象主题角色都可以有任意数量的观察者。抽象主题提供一个接口，可以增加和删除观察者角色。一般用一个抽象类和接口来实现。
- Observer：抽象观察者，为所有具体的观察者定义一个接口，在得到主题的通知时更新自己。
- ConcreteSubject：具体主题，在具体主题内部状态改变时，给所有登记过的观察者发出通知。具体主题角色通常用一个子类实现。
- ConcreteObserver：该角色实现抽象观察者角色所要求的更新接口，以便使本身的状态与主题的状态相协调。通常用一个子类实现。如果需要，具体观察者角色可以保存一个指向具体主题角色的引用。

### 类图

![](ObserverPattern.png)

### 实际案例

- 微信公众号有服务号、订阅号和企业号之分。每当发布一篇博文推送，订阅的用户都能够在发布推送之后及时接收到推送，即可方便地在手机端进行阅读。
- 购票后记录文本日志、购票后记录数据库日志、购票后发送短信、购票送抵扣卷、兑换卷、积分、其他各类活动等
- 当我做作业的时候告诉妈妈，饭做好了喊我吃饭。这里就是观察者模式，我向妈
  妈（系统主题）注册我感兴趣的事（吃饭），妈妈在事情发生的时候，通知系统
  观察者对象（我），做出相应的变化（去吃饭）。
- 股票系统上涨或下跌会自动提示用户

### 例子

以上方股票系统为例，假设当股票价格波动超过5%时，系统会通知所有的股东。

#### 类图

![](OnlineStockSystem.png)

#### 代码实现

```java
public interface Action  {

	public abstract void act();

}
```

```java
public interface Person {

	public abstract void update(double price);

}
```

```java
public class Investor implements Person, Action  {

	public void update(double price) {
		System.out.println("The current stock price is "+price);
		this.act();
	}

	public void act() {
		System.out.println("The stock price fluctuates more than 5%");
	}

}
```

```java
public abstract class Stock {

	protected ArrayList<Person> persons;
	
	public Stock() {
		persons=new ArrayList<Person>();
	}

	public void registerObserver(Person person) {
		persons.add(person);
	}

	public void removeObserver(Person person) {
		persons.remove(person);
	}

	public abstract void notifyObserver(double price);
}
```

```java
public class Stock1 extends Stock {

	private double price;
	
	public Stock1() {
		this(100);
	}
	
	public Stock1(double price) {
		this.price=price;
	}

	public void setPrice(double price) {
		if(Math.abs(price-this.price)>=0.05*this.price) {
			this.notifyObserver(price);
		}
		this.price=price;
		
	}

	public void notifyObserver(double price) {
		System.out.println("The original stock price is "+this.price );
		for(Person person:persons) {
			person.update(price);
		}
	}
}
```

#### 测试

```java
public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Stock1 stock1=new Stock1();
		
		Investor investor=new Investor();
		
		stock1.registerObserver(investor);
		
		stock1.setPrice(98);
		stock1.setPrice(92);
		
	}

}
```

#### 输出

```
The original stock price is 98.0
The current stock price is 92.0
The stock price fluctuates more than 5%
```



## 总结

### 优点

- 观察者和被观察者是抽象耦合的
- 建立一套触发机制

### 缺点

- 1.如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。
- 2.如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。
- 3.观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。