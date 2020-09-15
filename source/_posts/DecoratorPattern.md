---
title: 设计模式之装饰者模式
date: 2020-04-22 11:31:41
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/DecoratorPattern.jpg
tags: 
    - 设计模式 
    - java 
    - 面向对象 
categories: 设计模式
summary: The Decorator Pattern attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
---

## 装饰者模式

### 定义

《Head First》

The Decorator Pattern attaches additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.

装饰者模式，动态的将责任附加到对象上，若要扩展功能，装饰者提供了比继承更有弹性的替代方案

### 使用场景

- 需要扩展一个类的功能， 或给一个类增加附加功能；
- 需要动态地给一个对象增加功能， 这些功能可以再动态地撤销
- 需要为一批的兄弟类进行改装或加装功能

### 结构

- Component：抽象角色，给出一个个抽象接口，以规范准备接收附加责任的对象，是被装饰类和装饰类的父接口
- Concrete Compont：具体角色，定义一个要接受附加责任的类，即被装饰的类
- Decorator: 装饰角色，装饰者，持有一个Component（抽象角色）对象的引用，并定义一个与抽象角色接口一直的接口
- Concrete Decorator: 具体装饰角色，具体装饰者，负责给抽象角色附加责任即扩展功能

### 类图

![](DecoratorPattern.png)

### 实际案例

- 我们吃面，我们肚子很饿，吃一碗不够，我们还想加蛋，加青菜，加…但是最后出货也都是面条，就算是鸡蛋面，青菜面最后也都是面。这就是装饰者模式在不改变原有对象的基础之上，将功能附加到对象上。
- 某人要出门约会，你肯定是要决定好你要穿什么样的衣服出门，用衣服来装饰下自己，让自己拥有一个完美的约会。比如，你会穿一件衬衫，然后穿一件西服裤，最后穿皮鞋出门。

- 一家甜品店，出售蛋糕，除了蛋糕外，还可以在蛋糕上布置水果，蜡烛等，但是水果和蜡烛需要额外收费。
- 咖啡店里咖啡中可以加不同的配料：摩卡、牛奶、糖、奶泡；不同的饮品加上不同的配料有不同的价钱。



### 例子

去买冰淇淋，可以买普通的冰淇淋和选择加糖或者加坚果的冰淇淋为例来实现

#### 类图

![](Icecream.png)

#### Icecream类

```java
public interface Icecream {

	public abstract String makeIcecream();

}
```

#### SimpleIcecream类

实现冰淇淋接口，并可以生产普通冰淇淋

```java
public class SimpleIcecream implements Icecream {

	public String makeIcecream() {
		return "生产了一个普通的冰淇淋";
	}

}
```

#### IcecreamDecorator类

装饰角色，后面的附加坚果，糖等其他东西的类需要继承他

```java
public class IcecreamDecorator implements Icecream {

	private Icecream specialIcecream;

	public String makeIcecream() {
		return specialIcecream.makeIcecream();
	}

	public IcecreamDecorator(Icecream specialIcecream) {
        this.specialIcecream=specialIcecream;
	}

}
```

#### HoneyDecorator类

```java
public class HoneyDecorator extends IcecreamDecorator {

	public HoneyDecorator(Icecream specialIcecream) {
          super(specialIcecream);
	}

	public String makeIcercream() {
		return "生产了一个"+this.addHoney()+"的特殊冰淇淋";
	}

	private String addHoney() {
		return "加糖";
	}

}
```

#### NuttyDecorator类

```java
public class NuttyDecorator extends IcecreamDecorator {

	public NuttyDecorator(Icecream specialIcecream) {
      super(specialIcecream);
	}

	public String makeIcecream( ) {
		return "生产了一个"+this.addNuts()+"的特殊冰淇淋";
	}

	private String addNuts( ) {
		return "加坚果";
	}

}
```

#### Client

写出测试类进行验证

```java
public class Test {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
       Icecream simpleIcecream=new SimpleIcecream();
       System.out.println(simpleIcecream.makeIcecream());
       Icecream nutttIcecream=new NuttyDecorator(simpleIcecream);
       System.out.println(nutttIcecream.makeIcecream());
       System.out.println(new  HoneyDecorator(simpleIcecream).makeIcercream());   
	}
}
```

#### 输出结果

```
生产了一个普通的冰淇淋
生产了一个加坚果的特殊冰淇淋
生产了一个加糖的特殊冰淇淋
```

## 总结

### 优点

- 目的在于扩展对象的功能。装饰者模式提供比继承更好的灵活性。装饰是动态的，运行时可以修改的
- 继承是静态的，编译期便已确定好。通过使用不同的装饰类及对它们的排列组合，可以创造出许多不同行为的组合

### 缺点

- 利用装饰者模式，常常造成设计中有大量的小类，数量太多，会造成使用此API的人带来困扰，不容易理解。
- 采用装饰者在实例化组件时，将增加代码复杂度。一旦使用装饰者，不仅要实例化组件，同时要将组件实例包装进装饰者。