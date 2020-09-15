---
title: Vector
date: 2020-04-27 11:41:49
tags: 
   - java 
   - 集合
categories: java
summary: public class Vector<E>   extends AbstractList<E> implements List<E>
---

## Vector

Vector类实现了可扩展的对象数组。  像数组一样，它包含可以使用整数索引访问的组件。 但是， Vector的大小可以根据需要增长或缩小，以适应在创建Vector之后添加和删除项目。

### 与List区别

Vector 类实现了一个动态数组。和 ArrayList 很相似，但是两者是不同，vector为存储的对象分配一块连续的地址空间，因此对vector中的元素随机访问效率很高。在vecotor中插入或者删除某个元素，需要将现有元素进行复制，移动。对于简单的小对象，vector的效率优于list。vector在每次扩张容量的时候，将容量扩展2倍，这样对于小对象来说，效率是很高的。list中的对象是离散存储的，随机访问某个元素需要遍历list。在list中插入元素，尤其是在首尾插入元素，效率很高，只需要改变元素的指针。

- Vector 是同步访问
- Vector 包含了许多传统的方法，这些方法不属于集合框架
- Verctor 适用于对象数量变化少，简单的对象并且随机访问元素频繁
- List 适用于对象数量变化打，对象复杂并且插入和删除频繁
- Vector 主要用在事先不知道数组的大小，或者只是需要一个可以改变大小的数组的情况。

### 定义

```java
public class Vector<E>
    extends AbstractList<E>
    implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

### 构造方法

Verctor类支持四种构造方法

1. Vector()   构造一个空间向量，使其内部数据数组的大小为10，标准容量增量为零
2. Vector (Collection<? extends E> c) 构造一个包含指定集合元素的向量，按照集合的迭代器返回顺序
3. Vector(int initialCapacity) 构造具有指定初始容量并且其容量增量等于零的空向量
4. Vector(int initialCapacity，int capacityIncrement) 构造具有指定的初始容量和容量增量的空向量

### 方法

- int capacity() 返回此向量的当前容量
- int setSize(int newSize)设置此向量的大小
- int size()返回此向量中的组件数
- void sort(Comparator<? super E ?> c )使用提供的Comparatory对此列表进行排序以比较元素
- Spliterator<E> spliterator() 在此列表中的元素上创建late-binding和故障切换Spliterator
- Object clone() 返回此向量的克隆
- void ensureCapacity(int minCapacity) 如果需要，增加此向量的容量，以确保他可以至少保存最小容量参数指定的组件数
- Object[] toArry()以正确的顺序返回一个包含此Vector中所有元素的数组
- String toString()返回此Vector的字符串表示形式，其中包含每个元素的String表示形式
- void trimToSize()修改该向量的容量成为向量当前大小

#### 增

- boolean add(E e) 讲指定的元素追加到此Vector的末尾
- void add(int index,E element) 在此Vector中的指定位置插入指定元素
- boolean addAll(Collection <? extends E> c) 将指定集合中的所有元素追加到该向量的末尾，按照它们由指定集合的迭代器返回的顺序
- boolean addAll(int index,Collection<? extends E> c) 将指定集合中的所有元素插入到此向量的指定位置
- void addElement(E obj) 将指定的组件添加到此向量的末尾，将其大小增加1
- void insertElementAt(E obj,int index)在指定的index插入指定对象作为该向量的一个index

#### 删

- void clear() 从此Vector中删除所有元素
- E remove(int index) 删除此向量中指定位置的元素
- boolean remove(Object o)删除此向量中指定元素的第一次出现如果Vector不包含元素，则它不会 更改
- boolean removeAll(Collection<?> c)从此Vector中删除指定集合中包含的所有元素
- void removeAllElements()从该向量中删除所有组件，并将其大小设置为零
- boolean removeElement(Object obj)从此向量中删除参数的第一个（最低索引）出现次数
- void removeElementt(int index)删除指定索引初的组件
- boolean removeIf(Predicate<?super> E filter)删除满足给定谓词的此集合的所有元素
- Protected void removeRange(int fromIndex,int toIndex)从此列表中删除所有索引文fromIndex(含)和tiIndex之间的元素

#### 改

- void replaceAll(UnaryOperator<E> operator)该列表的每个元素替换为将该运算符应用于该元素的结果
- boolean retainAll(Collection<?> c) 仅保留此向量中包含在指定集合的元素
- E set(int index,E element) 用指定的元素替换此Vector中指定位置的元素
- void setElementAt(E obj,E element)设置在指定的组件，index此向量的要指定的对象

#### 查

- boolean contains(Object o) 如果此向量包含指定的元素，则返回true
- boolean contains (Collection<?> c) 如果此向量包含指定集合中所有的元素，则返回ture
- copyInto(Object[]  anarray) 将此向量的组件复制到指定的数组中
- E elementAt(int index) 返回指定索引处的组件
- Enumeratin<E> element()返回此向量的组件的枚举
- boolean equals(Object o)将指定的对象与此向量进行比较以获得相等性
- E firstElement()返回此向量的第一个组件即索引号为0的项目
- void forEach(Consumer<?super E> action) 对Iterable的每个元素执行给定的操作，直到所有元素都被处理或者动作引发异常
- E get(int index) 返回此向量中指定位置的元素
- int hashCode()返回此Vector的哈希码值
- int indexOf(Object o) 返回此向量中指定元素的第一次出现的索引，如果此向量不包含元素，则返回-1
- int indexOf(Object o,int index)返回此向量中指定元素的第一次出现的索引，从index向前index,如果未找到该元素，则返回-1
- int lastIndexOf(Object o)返回此向量中指定元素的最后异常出现的索引，如果此向量不包含元素，则返回-1
- int lastIndexOf(Object o,int index)返回此向量中指定元素最后异常出现的索引，从index,如果未找到元素，则返回-1
- Iterator<E> iterator()以正确的顺序返回该列表中的元素的迭代器
- boolean isEmpty() 测试此矢量是否没有组件
- E lastElement()返回向量的最后一个组件
- List<E>  subList(int formIndex,int toIndex)返回此列表之间的formIndex（包含）和toIndex之间的独占视图

### 例

#### 遍历

```java
import java.util.Enumeration;
import java.util.Iterator;
import java.util.Vector;
import java.util.function.Consumer;

public class VectorDemo {
	public static void main(String[] args) {
	Vector<String> vector=new Vector<String>();
	vector.add("徐庶");
	vector.add("曹仁");
	vector.add("吕蒙");
	vector.add("周瑜");
	//Ⅰ
	for(String v:vector) {
		System.out.print(v+" ");
	}
	System.out.println();
	//Ⅱ
	vector.forEach(new Consumer<String>() {

		@Override
		public void accept(String t) {
			// TODO Auto-generated method stub
			System.out.print(vector+" ");
		}
	});
	System.out.println();
	//Ⅲ
	for(int i=0;i<vector.size();i++) {
		System.out.print(vector.get(i)+" ");
	}
	System.out.println();
	//Ⅳ
	Iterator<String> iterator=vector.iterator();
	while (iterator.hasNext()) {
		String string=(String)iterator.next();
		System.out.print(string+" ");
	}
	System.out.println();
	//Ⅴ
	Enumeration<String> enumeration=vector.elements();
	while(enumeration.hasMoreElements()) {
		System.out.print(enumeration.nextElement().toString()+" ");
	}
	}
}
```

##### 输出

```
徐庶 曹仁 吕蒙 周瑜 
[徐庶, 曹仁, 吕蒙, 周瑜] [徐庶, 曹仁, 吕蒙, 周瑜] [徐庶, 曹仁, 吕蒙, 周瑜] [徐庶, 曹仁, 吕蒙, 周瑜] 
徐庶 曹仁 吕蒙 周瑜 
徐庶 曹仁 吕蒙 周瑜 
徐庶 曹仁 吕蒙 周瑜 
```

