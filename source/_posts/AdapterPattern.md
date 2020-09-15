---
title: 设计模式之适配器模式
date: 2020-05-02 23:02:08
tags:
    - 设计模式
    - java
    - 面向对象
categories: 设计模式
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/AdapterPattern.jpg
summary: The Adapter Pattern converts the interface of a class into another interface the clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
---

## 适配器模式

### 定义

《Head First》

The Adapter Pattern converts the interface of a class into another interface the clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

将一个类的接口，转换成客户期望的另一个接口。适配器让原本接口不兼容的类可以合作无间。

适配器模式常见的有三种，思想都是一样的，只不过要适配的内容不一样而已。

1. 类适配器模式
2. 对象适配器模式
3. 接口适配器模式

### 使用场景

- 重复使用现有的类，而此类的接口不符合系统的需求。在遗留代码复用、类库迁移等方面非常有用。
- 想要建立一个可以重用的类，用于与一些彼此之间没有太大关联的一些类，包括一些可能在将来引进的类一起工作。
- 使用第三方组件或中间组件，组件接口定义喝自己定义的不同，不希望修改自己的接口，单是要使用第三方接口的功能，避免重复造轮子。

### 结构

- Target(目标抽象类)：目标抽象类定义客户所需的接口，可以是一个抽象类或接口，也可以是具体类。在类适配器中，由于C#语言不支持多重继承，所以它只能是接口。
- Adapter(适配器类)：它可以调用另一个接口，作为一个转换器，对Adaptee和Target进行适配。它是适配器模式的核心。
- Adaptee(适配者类)：适配者即被适配的角色，它定义了一个已经存在的接口，这个接口需要适配，适配者类包好了客户希望的业务方法。

### 类图

![](AdaterPattern.png)

### 实际案例

- 美国电器 110V，中国 220V，就要有一个适配器将 110V 转化为 220V。
- 在 LINUX 上运行 WINDOWS 程序。
- 笔记本电脑拓展屏幕使用装接线将电脑与另一个屏幕相连才可以使用。
- JAVA 中的 jdbc，两者本来无联系，通过使用Driver便可访问数据库。

### 例子

以电器转接头为例子实现代码实现

#### 类适配器

##### 类图

![](Adapter_1.png)

类图上可看出就是把一个类包装成另外一个类

##### 代码实现

```java
public interface Target {
	//目标方法：原本5V电压 现在要输出220V
   public void TargetOperation() ;
}

public class Adaptee {

	public int AdapteeOperation() {
		return 220;
	}
}

public class Adapter extends Adaptee implements Target {
    //自己是5V，要输出220V
	@Override
	public void TargetOperation() {
		// TODO Auto-generated method stub
       System.out.println("未适配，5V");
       System.out.println("适配后："+this.AdapteeOperation()+"V");
	}

}

```

##### 测试

```java
public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
     Adapter adapter=new Adapter();
     adapter.TargetOperation();
	}

}
```

##### 输出

```java
未适配，5V
适配后：220V
```

#### 对象适配器

##### 类图

![](Adapter_2.png)

区别在于Adapter类中，在里面要创建一个Adaptee，不需要继承，对象调用。

##### 代码实现

```java
public interface Target {
	//目标方法：原本5V电压 现在要输出220V
   public void TargetOperation() ;
}

public class Adaptee {

	public int AdapteeOperation() {
		return 220;
	}
}

public class Adapter implements Target {
    //自己是5V，要输出220V
	private Adaptee adaptee;
	
	public Adapter(Adaptee adaptee) {
		// TODO Auto-generated constructor stub
	 this.adaptee=adaptee;
	}
	
	@Override
	public void TargetOperation() {
		// TODO Auto-generated method stub
       System.out.println("未适配，5V");
       System.out.println("适配后："+adaptee.AdapteeOperation()+"V");
	}

}
```

##### 测试

```java
public class Client {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
     Adapter adapter=new Adapter(new Adaptee());
     adapter.TargetOperation();
	}

}
```

##### 输出

```
未适配，5V
适配后：220V
```

#### 接口适配器

##### 类图

![](Adapter_3.png)

核心是抽象类实现了Target接口。Target接口提供了大量的方法，但是我们适配的时候不想要适配这么多，只想要适配其中一个或者几种。于是创建Concrete5V和Concrete220Vand5V重写想要适配的方法.

##### 代码实现

```java
public interface Target {
	public void output5V();

	public void output220V();

	public void output360V();
}

public abstract class AbstractAdapter implements Target {

	@Override
	public void output5V() {
		// TODO Auto-generated method stub
		System.out.println("输出5V");
	}

	@Override
	public void output220V() {
		// TODO Auto-generated method stub
		System.out.println("输出220V");
	}

	@Override
	public void output360V() {
		// TODO Auto-generated method stub
		System.out.println("输出360V");
	}

}

public class Concrete5V extends AbstractAdapter {

	@Override
	public void output5V() {
		// TODO Auto-generated method stub
		super.output5V();
	}

	@Override
	public void output220V() {
		// TODO Auto-generated method stub
		super.output220V();
	}

}

public class Concrete220Vand5V extends AbstractAdapter {

	public void output() {

		super.output220V();
		super.output5V();
	}

}


```

##### 测试

```java
public class Clinet {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		AbstractAdapter adapter = new Concrete5V();
		adapter.output5V();

		AbstractAdapter adapter2 = new Concrete220Vand5V();
		adapter2.output5V();
		adapter2.output220V();
	}

}
```

##### 输出

```
输出5V
输出5V
输出220V
```

## 总结

### 优点

#### 类适配器

- 由于适配器类是适配者类的子类，因此可以在适配器中置换一些适配者的方法，使得适配器的灵活性更强

#### 对象适配器

- 把多个不同的适配者适配到同一个目标，也就是说，同一个适配器可以把适配者和他的子类适配到目标接口

### 缺点

实现适配器所需要的工作和目标接口的大小成正比，接口越复杂适配器越复杂

#### 类适配器

- 对于不能多继承的语言，一次最多只能适配一个适配者类，而且目标抽象类只能为接口，不能为类，其使用有一定的局限性，不能将一个适配者类和其他的子类同时适配到目标接口

#### 对象适配器

- 与类适配器模式相比，想要置换适配者类的方法不容易