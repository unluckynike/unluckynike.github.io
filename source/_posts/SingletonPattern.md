---
title: 设计模式之单例模式
date: 2020-05-05 09:55:10
tags: 
    - 设计模式 
    - java 
    - 面向对象 
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/Singleton.jpg
categories: 设计模式
summary: The Singleton Pattern ensure a class has only one instance, and provides a global point of access to it.
---

## 单例模式

### 定义

《Head First》

The Singleton Pattern ensure a class has only one instance, and provides a global point of access to it.

确保一个类只有一个实例，并提供一个全局访问点

属于创建型模式，提供了一种创建对象的最佳方式，涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建，这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

注意

- 单例类只能有一个实例
- 单例类必须自己创建自己的唯一实例
- 单例类必须给所有其他对象提供这个一实例
- getInstance() 方法中需要使用同步锁 synchronized (Singleton.class) 防止多线程同时进入造成 instance 被多次实例化。

### 使用场景

- 当您想控制实例数目，节省系统资源的时候。

### 结构

- Singleton:单例

### 类图

![](Singleton.png)

### 实际案例

- 一个班级只有一个班主任
- Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行
- 打印池在操作系统中,打印池Print Soo是)一个用于管理打印任务的应用程序,通过打印他用户可以删除、中止或者改变打印任务的优先级，在一 一个系统中只允许运行一个打印池对象, 如果重复创建打印池则抛出异常。

### 例子

实例以上述打印池为例

#### 类图

![](Print.png)

#### 代码实现

```java
public class PrintSpooler {

	private static PrintSpooler printspooler=null;
	
	public PrintSpooler() {
	}
    
	public void setPrintSpooler() {       
	}

	public static PrintSpooler getPrintSpooler() throws PrintSpoolerException {
		if (printspooler==null) {
			System.out.println("新建了一个打印池");
			printspooler=new PrintSpooler();
		}
		else {
			throw new PrintSpoolerException("正在打印");
		}
		return printspooler;
	}

	public void delete() {
      System.out.println("刪除");
	}

	public void abort() {
     System.out.println("中止");
	}

	public void change() {
     System.out.println("修改");
	}
}


public class PrintSpoolerException extends Exception {

	public PrintSpoolerException(String erro) {
          super(erro);
	}
}
```

#### 测试

```java
public class Client {
   public static void main(String[] args) {
	   PrintSpooler p1,p2;
	   try {
		p1=PrintSpooler.getPrintSpooler();
	} catch (PrintSpoolerException e) {
		// TODO Auto-generated catch block
		System.out.println(e.getMessage());
		e.printStackTrace();
	}
	   System.out.println();
	   
	   try {
		p2=PrintSpooler.getPrintSpooler();
	} catch (PrintSpoolerException e) {
		// TODO Auto-generated catch block
		System.out.println(e.getMessage());
		//e.printStackTrace();
	}
	   finally {
		System.out.println("警告，重复创建");
	}
}
}
```

#### 输出

```
新建了一个打印池

正在打印
警告，重复创建
```

### 创建方式

#### 懒汉式，线程不安全

懒汉式其实是一种比较形象的称谓。既然懒，那么在创建对象实例的时候就不着急。会一直等到马上要使用对象实例的时候才会创建，懒人嘛，总是推脱不开的时候才会真正去执行工作，因此在装载对象的时候不创建对象实例。

下方代码简单明了，而且使用了懒加载模式，但是却存在致命的问题。当有多个线程并行调用 getInstance() 的时候，就会创建多个实例。也就是说在多线程下不能正常工作。

```java
public class Singleton {
    private static Singleton instance;
    private Singleton (){}

    public static Singleton getInstance() {
     if (instance == null) {
         instance = new Singleton();
     }
     return instance;
    }
}
```

#### 懒汉式，线程安全

为了解决上面的问题，最简单的方法是将整个 getInstance() 方法设为同步（synchronized）。

下方代码虽然做到了线程安全，并且解决了多实例的问题，但是它并不高效。因为在任何时候只能有一个线程调用 getInstance() 方法。但是同步操作只需要在第一次调用时才被需要，即第一次创建单例实例对象时。这就引出了双重检验锁。

```java
public static synchronized Singleton getInstance() {
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

#### 双重检验锁

双重检验锁模式（double checked locking pattern），是一种使用同步块加锁的方法。程序员称其为双重检查锁，因为会有两次检查 instance == null，一次是在同步块外，一次是在同步块内。为什么在同步块内还要再检验一次？因为可能会有多个线程一起进入同步块外的 if，如果在同步块内不进行二次检验的话就会生成多个实例了。

```java
public static Singleton getSingleton() {
    if (instance == null) {                         //Single Checked
        synchronized (Singleton.class) {
            if (instance == null) {                 //Double Checked
                instance = new Singleton();
            }
        }
    }
    return instance ;
}
```

JVM流程

1. 给 instance 分配内存
2. 调用 Singleton 的构造函数来初始化成员变量
3. 将instance对象指向分配的内存空间（执行完这步 instance 就为非 null 了）。

但是在 JVM 的即时编译器中存在指令重排序的优化。也就是说上面的第二步和第三步的顺序是不能保证的，最终的执行顺序可能是 1-2-3 也可能是 1-3-2。如果是后者，则在 3 执行完毕、2 未执行之前，被线程二抢占了，这时 instance 已经是非 null 了（但却没有初始化），所以线程二会直接返回 instance，然后使用，然后顺理成章地报错。

将 instance 变量声明成 volatile 就可可以了

```java
public class Singleton {
    private volatile static Singleton instance; //声明成 volatile
    private Singleton (){}

    public static Singleton getSingleton() {
        if (instance == null) {                         
            synchronized (Singleton.class) {
                if (instance == null) {       
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
   
}
```

使用 volatile 的主要原因是其另一个特性：禁止指令重排序优化。也就是说，在 volatile 变量的赋值操作后面会有一个内存屏障（生成的汇编代码上），读操作不会被重排序到内存屏障之前。

#### 饿汉式 static final field

饿汉式其实是一种比较形象的称谓。既然饿，那么在创建对象实例的时候就比较着急，饿了嘛，于是在装载类的时候就创建对象实例。这种方法非常简单，因为单例的实例被声明成 static 和 final 变量了，在第一次加载类到内存中时就会初始化，所以创建实例本身是线程安全的。

缺点是它不是一种懒加载模式（lazy initialization），单例会在加载类后一开始就被初始化，即使客户端没有调用 getInstance()方法。

```java
public class Singleton{
    //类加载时就初始化
    private static final Singleton instance = new Singleton();
    
    private Singleton(){}

    public static Singleton getInstance(){
        return instance;
    }
}
```

#### 静态内部类 static nested class

```java
public class Singleton {  
    private static class SingletonHolder {  
        private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
        return SingletonHolder.INSTANCE; 
    }  
}
```

#### 枚举 Enum

可以通过EasySingleton.INSTANCE来访问实例，这比调用getInstance()方法简单多了。创建枚举默认就是线程安全的，所以不需要担心double checked locking，而且还能防止反序列化导致重新创建新的对象。

```java
public enum EasySingleton{
    INSTANCE;
}
```

## 总结

一般来说，单例模式有五种写法：懒汉、饿汉、双重检验锁、静态内部类、枚举。一般情况下直接使用饿汉式就好了，如果明确要求要懒加载（lazy initialization）倾向于使用静态内部类。如果涉及到反序列化创建对象时会试着使用枚举的方式来实现单例。

### 优点

- 在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）
- 避免对资源的多重占用（比如写文件操作）

### 缺点

- 没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化

