---
title: 设计模式之状态模式
date: 2020-04-30 15:28:38
tags: 
    - 设计模式 
    - java 
    - 面向对象 
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/StatePattern.jpg
categories: 设计模式
summary: The State Pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
---

## 状态模式

### 定义

《Head First》

The State Pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

状态模式允许对象在内部状态改变时改变它的行为，对象看起来像修改它的类。状态模式主要解决的是控制一个对象转换的条件表达式过于复杂的情况，把状态的判断逻辑控制转移导表现不同状态的一系列类当中。

### 使用场景

- 一个对象的行为取决于它的状态,并且它必须在运行时刻根据状态改变它的行为。
-  一个操作中含有庞大的多分支的条件语句，且这些分支依赖于该对象的状态。 

### 结构

状态模式把所研究的对象的行为包装在不同的状态对象里，每一个状态对象都属于一个抽象状态类的一个子类。状态模式的意图是让一个对象在其内部状态改变的时候，其行为也随之改变。

- Context：环境角色，定义客户端所感兴趣的接口，并且保留一个具体状态类的实例。这个具体状态类的实例给出此环境对象的现有状态。
- State：抽象状态角色，定义一个接口，用以封装环境对象的一个特定的状态所对应的行为。
- ConcreteState：每一个具体状态类都实现了环境的一个状态所对应的i行为。

### 类图

![](StatePattern.png)

### 实际案例

- 酒店宾馆的房间有入住状态，预定状态，空闲状态。
- 电梯主要有4种状态：电梯门关闭、电梯门打开、电梯上下运载、电梯停止。电梯在门打开的时候，只能是关闭电梯门，不能是其他的任何操作。
- 红绿灯分为红灯、黄灯、绿灯三种状态。
- 网上购物时，订单的状态有下单，已付款，已发货，送货中，已收货等状态。

### 例子

以上述酒店为例子来实现

#### 类图

![](Room.png)

#### Room

```java
public class Room {
	private State s;

	public void setState(State state) {
		System.out.println("状态");
		s = state;
		state.action();
	}
}
```

#### State

```java
public interface State {

	public void action();
}
```

#### FreeRoom

```java
public class FreeRomm implements State{

	@Override
	public void action() {
		// TODO Auto-generated method stub
		System.out.println("空房，可以预定");
	}

}
```

#### BookedRoom

```java
public class BookedRoom implements State{

	@Override
	public void action() {
		// TODO Auto-generated method stub
		System.out.println("房间已经被预订");
	}

}
```

#### CheckedRoom

```java
public class CheckedRoom implements State{

	@Override
	public void action() {
		// TODO Auto-generated method stub
		System.out.println("房间有人入住");
	}

}
```

#### Client测试

```java
public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Room room = new Room();

		room.setState(new FreeRomm());
		room.setState(new BookedRoom());
		room.setState(new CheckedRoom());
	}

}
```

#### 输出

```java
状态
空房，可以预定
状态
房间已经被预订
状态
房间有人入住
```



## 总结

### 优点

- 状态模式将与特定状态相关的行为局部化到一个状态中，并且将不同状态的行为分割开来，满足“单一职责原则”。
- 减少对象间的相互依赖。将不同的状态引入独立的对象中会使得状态转换变得更加明确，且减少对象间的相互依赖。
- 有利于程序的扩展。通过定义新的子类很容易地增加新的状态和转换。

### 缺点

- 状态模式的使用必然会增加系统的类与对象的个数。
- 状态模式的结构与实现都较为复杂，如果使用不当会导致程序结构和代码的混乱。