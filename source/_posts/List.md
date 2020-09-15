---
title: List集合
date: 2020-04-17 11:43:25
top: true
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/List.jpg
tags:
   - java 
   - 集合
categories: java
summary: public interface List<E> extends Collection<E>
---

## List集合

List集合代表一个元素有序、可重复的集合，集合中每个元素都有其对应的顺序索引。List集合允许使用重复元素，可以通过索引来访问指定位置的集合元素 。List集合默认按元素的添加顺序设置元素的索引，例如第一个添加的元素索引为0，第二个添加的元素索引为1......

 List作为Collection接口的子接口，可以使用Collection接口里的全部方法。而且由于List是有序集合，因此List集合里增加了一些根据索引来操作集合元素的方法。

### 定义

```java
public interface List<E> extends Collection<E>
```



### 接口中定义的方法

- void add(int index, Object element): 在列表的指定位置插入指定元素（可选操作）。
-  boolean addAll(int index, Collection<? extends E> c) : 将集合c 中的所有元素都插入到列表中的指定位置index处。
-  Object get(index):返回列表中指定位置的元素。
-  int indexOf(Object o): 返回此列表中第一次出现的指定元素的索引；如果此列表不包含该元素，则返回 -1。
-  int lastIndexOf(Object o):返回此列表中最后出现的指定元素的索引；如果列表不包含此元素，则返回 -1。
-  Object remove(int index): 移除列表中指定位置的元素。
-  Object set(int index, Object element):用指定元素替换列表中指定位置的元素。
-  List subList(int fromIndex, int toIndex): 返回列表中指定的 fromIndex（包括 ）和 toIndex（不包括）之间的所有集合元素组成的子集。
-  Object[] toArray(): 返回按适当顺序包含列表中的所有元素的数组（从第一个元素到最后一个元素）。

### List特点

- List是有序的collection，因此用户可以根据元素的索引（元素在列表中的位置）来找到。
- List中的元素是可以重复的。

### List特有的迭代器

##### Iterator

- hasNext（）
- next（）
- remove（）

##### List集合的调优

如果知道每次大概增长的数量，就可以传一个参数，改变集合的初始大小，减少容器传输的次数。

## ArrayList

ArrayList 类提供了快速的基于索引的成员访问方式，对尾部成员的增加和删除支持较好。使用 ArrayList 创建的集合，允许对集合中的元素进行快速的随机访问，不过，向 ArrayList 中插入与删除元素的速度相对较慢。

```
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

### 构造方法

- ArrayList（） 构造一个初始容量为十的空列表
- ArrayList（Collection<? extends E>C）构造一个包含指定集合的元素的列表，按照他们由集合的迭代器返回的顺序
- ArrayList（int initialCapacity）构造具有指定初始容量的空列表

### 集合特点

- 数组结构  
- 有连续下标。增删慢，查询。(因为他是有序的，有下标，所有每增加一个或减少一个，整个集合的下标都要跟着改变，所以增删慢。但是正是因为它有下标所以可以根据下标来查询，所以查询速度快) 
- 线程不同步 
- 增长因子为1.5

![](ArrayList.png)

### 方法摘要

- boolean add(E e) 将指定的元素追加到此列表的末尾
- void add(int index,E element) 在此列表中的指定位置插入指定的元素
- boolean addAll（Collection<? extends E>c）按指定集合的Iterator返回的顺序讲指定集合中所有的元素追加到此列表的末尾
- void addAll（int index,Collection<? extends E>c）将指定集合中的所有元素插入到此列表中，从指定的位置开始
- void clean（）从列表中删除所有元素
- boolean contains（Object o）如此此列表包含指定的元素，则返回true
- E get（int index）返回此列表中指定位置的元素
- int indexOf（Object o） 返回此列表中指定元素的第一次出现的索引，如果此列表不包含元素，则返回-1
- int lastIndexOf(Object o) 返回此集合中最后一次出现指定元素的索引，如果此集合不包括该元素，则返回-1
- boolean isEmpty（）如果此列表不包含元素，则返回true
- Iterator<E> iterator（）以正确的顺序返回该列表中的元素的迭代器
- E remove（int index）删除该列表中指定位置的元素
- boolean remove（Object o）从列表中删除指定元素的第一出现（如果存在）
- boolean removeAll（Collection<?> c）从此列表中删除指定集合中包含的所有元素
- E set（int index,E element）用指定的元素替换此列表中指定位置的元素
- int size（）返回此列表中的元素数
- void sort（Comparator<? super E> c）使用提供的Comparator对此列表进行排序以比较元素
- List<E> subList（int fromIndex, int tolndex）返回一个新的集合，新集合中包含fromIndex和toIndex索引之间的所有元素。

### 例

Student类

```java

public class Student {
	private String nameString;
	private String ageString;

	public Student(String nameString, String ageString) {
		this.nameString = nameString;
		this.ageString = ageString;
	}

	public String getNameString() {
		return nameString;
	}

	public void setNameString(String nameString) {
		this.nameString = nameString;
	}

	public String getAgeString() {
		return ageString;
	}

	public void setAgeString(String ageString) {
		this.ageString = ageString;
	}

	@Override
	public String toString() {
		return "Student [nameString=" + nameString + ", ageString=" + ageString + "]";
	}

}

```

ArrayListDemo类

```java

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ArrayListDemo {
	public static void main(String[] args) {
		Student student1 = new Student("吕布", "男");
		Student student2 = new Student("貂蝉", "女");
		Student student3 = new Student("刘琦", "男");
		Student student4 = new Student("吕蒙", "男");
		Student student5 = new Student("曹仁", "男");

		List list = new ArrayList();
		list.add(student1);
		list.add(student2);
		list.add(student3);
		list.add(student5);
		list.add(student4);
		System.out.println("集合是否为空：" + list.isEmpty());
		System.out.println("集合元素数量：" + list.size());
		System.out.println("全部的元素");
		for (int i = 0; i < list.size(); i++) {
			Student student = (Student) list.get(i);
			System.out.println(student.toString());// 也可以直接输出student
		}

		System.out.println();

		List subList = new ArrayList();
		subList = list.subList(2, 4);// 截取存到subList
		System.out.println("截取的元素");
		Iterator iterator = subList.iterator();
		while (iterator.hasNext()) {
			System.out.println(iterator.next() + "  ");
		}
	}
}
```

输出

```
集合是否为空：false
集合元素数量：5
全部的元素
Student [nameString=吕布, ageString=男]
Student [nameString=貂蝉, ageString=女]
Student [nameString=刘琦, ageString=男]
Student [nameString=曹仁, ageString=男]
Student [nameString=吕蒙, ageString=男]

截取的元素
Student [nameString=刘琦, ageString=男]  
Student [nameString=曹仁, ageString=男]  
```

#### indexOf与lastIndexOf

前者是获得指定对象的最小索引位置，而后者是获得指定对象的最大索引位置。前提条件是指定的对象在 List 集合中有重复的对象，否则这两个方法获取的索引值相同。

```java

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ArrayListDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
       List list=new ArrayList();
       list.add("Alice");
       list.add("Jack");
       list.add("Mark");
       list.add("Peter");
       list.add("Park");
       list.add("Eric");
       list.add("Jack");
       list.add("Jack");
       list.add("Peter");
       System.out.println("集合元素数量："+list.size());
       System.out.println("Jack在集合中第一次出现的索引是："+list.indexOf("Jack"));//从0开始
       System.out.println("Jack在集合中最后一次出现的索引是："+list.lastIndexOf("Jack"));
	   System.out.println("集合元素");
	   Iterator iterator=list.iterator();
	   while(iterator.hasNext()) {
		   System.out.print(iterator.next()+" ");
	   }
	}
}
```

输出

```
集合元素数量：9
Jack在集合中第一次出现的索引是：1
Jack在集合中最后一次出现的索引是：7
集合元素
Alice Jack Mark Peter Park Eric Jack Jack Peter 
```

## LinkedList

LinkedList 类采用链表结构保存对象，这种结构的优点是便于向集合中插入或者删除元素。需要频繁向集合中插入和删除元素时，使用 LinkedList 类比 ArrayList 类效果高，但是 LinkedList 类随机访问元素的速度则相对较慢。这里的随机访问是指检索集合中特定索引位置的元素。

### 构造方法

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```

### 集合特点

![](LinkedList.png)

- 链表结构（双链表结构，当前元素知道自己上一个元素和下一个元素）

- 增删改查慢，有连续下标
- 线程同步
- 增长因子2

#### 链表结构

链表结构是由许多节点构成的，每个节点都包含两部分

- 数据部分：保存该节点的实际数据（值）
- 地址部分：保存下一个节点的地址（下标）

单向链表：只能获取自己下一个元素的节点

双向链表：能获取自己上一个元素和下一个元素的节点

#### LinkedList可以实现堆栈存储结构和队列存储结构

- 堆栈：先进后出
- 队列：先进先出

### 方法摘要

- void addFirst(E e) 将指定元素添加到此集合的开头
- void addLast(E e) 将指定元素添加到此集合的末尾
- E getFirst() 返回此集合的第一个元素
- E getLast() 返回此集合的最后一个元素
- E removeFirst() 删除此集合中的第一个元素
- E removeLast() 删除此集合中的最后一个元素

### 例

Student类同上ArrayList

```java
package List;

import java.util.LinkedList;

public class LinkedListDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		LinkedList<Student> linkedList = new LinkedList<Student>();
		Student student1 = new Student("吕布", "男");
		Student student2 = new Student("貂蝉", "女");
		Student student3 = new Student("刘琦", "男");
		Student student4 = new Student("吕蒙", "男");
		Student student5 = new Student("曹仁", "男");
		linkedList.add(student1);
		linkedList.add(student2);
		linkedList.add(student3);
		linkedList.add(student4);
		for (int i = 0; i < linkedList.size(); i++) {
			Student student = (Student) linkedList.get(i);
			System.out.println(student);
		}

		linkedList.addLast(student5);// 添加到末尾
		Student student6 = new Student("孔明", "男");
		linkedList.addFirst(student6);// 添加到开头
		System.out.println("***************************");
		for (int i = 0; i < linkedList.size(); i++) {
			Student student = (Student) linkedList.get(i);
			System.out.println(student);
		}
        
		System.out.println("开头第一个元素是："+linkedList.getFirst());
		System.out.println("最后一个元素是："+linkedList.getLast());
		
		linkedList.removeLast();//移除末尾元素
		linkedList.removeFirst();//移除开头元素
		
		System.out.println("*************************");
		for (int i = 0; i < linkedList.size(); i++) {
			Student student = (Student) linkedList.get(i);
			System.out.println(student);
		}
	}

}
```

输出

```
Student [nameString=吕布, ageString=男]
Student [nameString=貂蝉, ageString=女]
Student [nameString=刘琦, ageString=男]
Student [nameString=吕蒙, ageString=男]
***************************
Student [nameString=孔明, ageString=男]
Student [nameString=吕布, ageString=男]
Student [nameString=貂蝉, ageString=女]
Student [nameString=刘琦, ageString=男]
Student [nameString=吕蒙, ageString=男]
Student [nameString=曹仁, ageString=男]
开头第一个元素是：Student [nameString=孔明, ageString=男]
最后一个元素是：Student [nameString=曹仁, ageString=男]
*************************
Student [nameString=吕布, ageString=男]
Student [nameString=貂蝉, ageString=女]
Student [nameString=刘琦, ageString=男]
Student [nameString=吕蒙, ageString=男]
```

