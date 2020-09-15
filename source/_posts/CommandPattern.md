---
title: 设计模之命令模式
date: 2020-04-11 10:56:47
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/command.jpg
tags: 
    - 设计模式 
    - java 
    - 面向对象 
categories: 设计模式
summary: The Command Pattern encapsulates a request as an object, thereby letting you parameterize other objects with different requests, qucue or log requests, and support undoable operations.
---

## 命令模式

首先，命令模式是一种行为模式。

### 定义

《Head First》

The Command Pattern encapsulates a request as an object, thereby letting you parameterize other objects with different requests, qucue or log requests, and support undoable operations.

命令模式是一个高内聚的模式，将一个请求封装成一个对象，从而让你使用不同的请求把客户端参数化，对请求排队或者记录日志，可以提供命令的撤消和恢复功能。

### 结构

顾名思义，命令模式就是对命令的封装。命令模式可以将请求发送者和接收者完全解耦，发送者与接收者之间没有直接引用关系，发送请求的对象只需要知道如何发送请求，而不必知道如何完成请求。

- Command : 抽象类，对需要执行的命令进行声明，通常我们设置一个execute方法来用来执行命令。
- ConcreteCommand :  这是Command的实现类，对抽象类中的方法进行实现。
- Invoker ：调用者，负责调用命令。
- Receiver ：接收者，负责接收命令并进行执行。

执行顺序：调用者——>接收者——>命令

### 类图

![命令模式](Command.png)

### 使用场景

需要对行为进行记录，撤销，重做等事务处理时需要抽象出待执行的动作，然后以参数的形式提供出来。

### 实际案例

1. GUI中每一个按钮都是一条命令，同样遵循命令模式的设计原则。
2. 我们使用遥控器更换电视节目，电视剧遥控器（命令发送者）通过按钮（具体命令）来遥控电视剧（命令接收者）。
3. 计算机键盘上的“功能建”。
4. 我们去到饭店吃饭，服务员把客户点的菜告诉厨师，厨师在根据菜单做菜。

### 示例

我们就拿上述到饭店吃饭为例，客户来的饭店，要求服务员（Servant）点羊肉串（MuttonString）和鸡肉串（Chicken），服务员传达给后厨厨师（Chef）,厨师负责烤串。



我们先分析来分析哪一个是调用者？哪一个是接收者？

由此我们得到类图

![类图示例](Barbercue.png)

#### 代码实现

#### Command

```java
public interface Command {

	public abstract void order();

}
```

#### Chicken

```java
public class Chicken implements Command {

	private Chef chef;

	public Chicken() {
		// TODO Auto-generated constructor stub
		chef = new Chef();
	}

	public void order() {
		chef.produceChicken();
	}

}
```

#### MuttonString

```java
public class MuttonString implements Command {

	private Chef chef;

	public MuttonString() {
		// TODO Auto-generated constructor stub
	   chef=new Chef();
	}
	public void order() {
       chef.produceMuttonString();
	}

}
```

如此我们已经完成了抽象类和抽象方法的实现，接下来便是调用者与接收者了

#### Chef

```java
public class Chef {

	public void produceChicken() {
        System.out.println("鸡肉串儿");
	}

	public void produceMuttonString() {
        System.out.println("羊肉串儿");
	}

}
```

#### Servant

```java
public class Servant {

	private Command chickencommand, muttonStringCommand;

	public Servant(Command chickencommand, Command muttonStringCommand) {
		// TODO Auto-generated constructor stub
		this.chickencommand = chickencommand;
		this.muttonStringCommand = muttonStringCommand;
	}

	public void produceChicken() {
		this.chickencommand.order();
	}

	public void produceMuttonString() {
		this.muttonStringCommand.order();
	}

}
```

我们再写个客户端测试类测试一下

#### Client

```java
public class Client {
	public static void main(String[] args) {
             Command cCommand,mCommand;
             cCommand = new Chicken();
             mCommand =new MuttonString(); 
             Servant servant=new Servant(cCommand, mCommand);
             servant.produceChicken();
             servant.produceMuttonString();
	}
}
```

#### 输出

![测试结果](Client.png)

## 总结

### 优点

1. 类间解耦，调用者角色与接受者角色之间没有任何依赖关系，调用者实现功能只需要调用Command抽象的execute方法就可以，不需要知道到底是哪一个接收者执行。
2. 可扩展性，Command子类可以非常容易的扩展，而调用者Invoker和高层次的模块Client不产生严重的代码耦合。

### 缺点

1. 可能会导致某些系统有过多的具体命令类。