---
title: Map集合
date: 2020-04-13 20:21:03
top: true
img: https://cdn.jsdelivr.net/gh/unluckynike/blogimg/images/Map.jpg
tags:
   - java 
   - 集合
categories: java
summary:  public interface Map<K, V>
---

## Map

### 什么是Map

```java
java.util
public interface Map<K, V>
An object that maps keys to values. A map cannot contain duplicate keys; each key can map to at most one value.
This interface takes the place of the Dictionary class, which was a totally abstract class rather than an interface.
The Map interface provides three collection views, which allow a map's contents to be viewed as a set of keys, collection of values, or set of key-value mappings. The order of a map is defined as the order in which the iterators on the map's collection views return their elements. Some map implementations, like the TreeMap class, make specific guarantees as to their order; others, like the HashMap class, do not.
```

Map是一种依照键（key）存储元素的容器，什么是键呢，它就像是下标，在List中下标是整数，而在Map中键可以是任意类型的对象，Map中不能有重复的键,没一个键都有一个对象的值（Value）。一个键和它对应的值构成Map集合的一个元素，Map中的元素是两个对象，一个对象作为键，一个对象作为值，键不可以重复但是值可以重复。

### Map的特点

1. 包含键值对
2. 键唯一
3. 键对应的值唯一
4. Map是一个接口

![Map集合](map.png)

### Map和Collection的区别

1. Map集合存储的元素是成对出现的，Map集合的键是唯一的，值是可以重复的。
2. Collection集合存储元素的单独出现的，Collection的子体系Set的唯一的，List是可重复的。
3. Map集合的数据结构只针对键有效，跟值无关。
4. Collection集合的数据结构是针对元素有效的。
5. Map是双列集合，Collection是单列集合。

- HashMap: 基于哈希表的Map接口，保证键的唯一性。

-  LinkedHashMap: Map接口的哈希表和链接列表，保证键的唯一性，具有可预知的迭代顺序（添加的顺序和输出的顺序完全一致）。


-  TreeMap: 红黑树的Map接口，保证键的顺序和唯一性。


## 常见方法

暂且列举出常用方法，具体其他实例参考API

### 长度

- int size()  返回集合中键值对的对数

### 添加

- V put（K key，V value）如果键是第一次存储，就直接存储元素，返回null。如果键不是第一次存储，就用值把以前的值替换掉，返回以前的值。

### 删除

- void clear（）移除所有的键值对元素。
- V remove（Object key）根据键值对元素，返回删除的值。

### 判断

- boolean containsKey(Object key)  判断集合是否包含指定的键
- boolean containsValue(Object value) 判断集合是否包含指定的值
- boolean isEmpty（）判断集合是否为空

### 获取

- V get(Object key) 根据键获取值
- Set keySet（）获取集合中所有键的集合
- Collection values（）获取集合中所有值得集合

## 例

### V put（K Key,V value）方法，返回值为V类型

```java
package map;

import java.util.HashMap;
import java.util.Map;

public class MapDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//创建集合对象
      Map<String, String> map=new HashMap<String, String>();
      //添加元素
      //如果键是第一次存储，就直接存储元素，返回null
      //如果键不是第一次存储，就用值把以前的值替换掉，返回以前的值。（键已经存在，值就覆盖）
      map.put("许褚","三军主将" );
      map.put("曹操", "主公");
      map.put("荀彧", "汉臣");
      map.put("许攸", "汉臣");
      System.out.println(map.put("董卓", "尚父"));
    //输出结果 null
      System.out.println(map.put("董卓", "汉贼"));
   //输出结果 尚父
      map.put("孙坚", "大破董卓，杀进洛阳");
      System.out.println(map);
   //输出结果{许攸=汉臣, 许褚=三军主将, 荀彧=汉臣, 曹操=主公, 孙坚=大破董卓，杀进洛阳}
      map.put("孙坚", "卒于荆州");
      System.out.println(map);
   //输出结果{许攸=汉臣, 许褚=三军主将, 荀彧=汉臣, 曹操=主公, 孙坚=卒于荆州} 
	}
}
```

### 删除和判断方法

```java
package map;

import java.util.HashMap;
import java.util.Map;

public class MapDemo {
	public static void main(String[] args) {
        Map<Integer, String> map=new HashMap<Integer, String>();
        map.put(0, "陈公台");
        map.put(1, "张辽");
        map.put(2, "曹仁");
        map.put(3, "司马懿");
        map.put(4, "张琳");
        System.out.println(map);
       //删除元素。传入键，返回对应的值，若键不存在，返回null
        System.out.println(map.remove(1));
       //张辽 
        System.out.println(map.remove(5));
       //null 
       System.out.println(map.isEmpty());
       //false
       //判断
       System.out.println(map.containsKey(0)+"  "+map.containsValue("张琳"));
       //true   true
       System.out.println(map);
	}
}

```

### get 方法

```java
package map;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class MapDemo2 {
	public static void main(String[] args) {
     Map<String, String> map=new HashMap<String, String>();
     map.put("周瑜","周公瑾");
     map.put("鲁肃","鲁子敬");
     map.put("孙权", "孙仲谋");
     map.put("吕蒙", "吕子明");
   //根据键获取值
     System.out.println(map.get("周瑜"));
    //周公瑾
     //获得所有键的集合
     Set<String> keySet=map.keySet();
     for(String k:keySet) {
    	 System.out.println(k);
     }
     // 鲁肃 孙权  周瑜  吕蒙 输出所有的键
     System.out.println("*******");   
    //获取所有值的集合
    Collection<String> values=map.values();
    for(String v:values) {
    	System.out.println(v);
    }
    //鲁子敬 孙仲谋 周公瑾 吕子明输出所有的值
	}
}
```

### Map集合的遍历

```java
package map;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class MapDemo {
	public static void main(String[] args) {
      Map<String, String> map=new HashMap<String, String>();
      map.put("周瑜","周公瑾");
      map.put("鲁肃","鲁子敬");
      map.put("孙权", "孙仲谋");
      map.put("吕蒙", "吕子明");
      
      //Ⅰ得到Map集合中所有的键的集合遍历键的集合再根据键去找对应的值
      Set<String> keySet=map.keySet();
      for(String k:keySet) {
    	  String valueString=map.get(k);
    	  System.out.println(k+"="+valueString);
      }
      
      //Ⅱ 获取所有Map集合中所有键值对象的集合，遍历键值对象的集合得到每一个键值对象
      Set<Map.Entry<String, String>> mapSet=map.entrySet();
      for(Map.Entry<String, String> ms:mapSet) {
    	  String keyString=ms.getKey();
    	  String valueString=ms.getValue();
    	  System.out.println(keyString+"="+valueString);
      }
      
      //Ⅲ 取得所有键值对对象的迭代器，迭代得到每一个与元素都是键值对对象
      Iterator<Map.Entry<String, String>> iterator=map.entrySet().iterator();
      while (iterator.hasNext()) {
		Map.Entry<String, String> entry=iterator.next();
		System.out.println(entry.getKey()+"="+entry.getValue());
	}
      
      //Ⅳ 通过Map.values遍历所有的value 但不能遍历key
      for(String v:map.values()) {
    	  System.out.println(v);
      }
	}
}
```

输出结果

```
前三种遍历输出结果都为：
鲁肃=鲁子敬
孙权=孙仲谋
周瑜=周公瑾
吕蒙=吕子明

第四中不能遍历key，结果为：
鲁子敬
孙仲谋
周公瑾
吕子明
```

## HashMap

HashMap：是基于哈希表的Map接口实现，哈希表的作用是用来保证键的唯一性。HashMap数据结构为数组加链表，其中链表的节点存储的是一个Entry对象，每个Entry对象存储四个属性（hash，key，value，next）

![](hashmap.png)

- 整体是一个数组
- 数组每个位置是一个链表
- 链表每个节点的Value即我们存储的Object

### 键和值都是String

```java
package map;

import java.util.HashMap;
import java.util.Set;

public class HashMapDemo {
	public static void main(String[] args) {
		HashMap<String, String> hashMap = new HashMap<String, String>();
      hashMap.put("关羽", "斩颜良诛文丑");
      hashMap.put("赵云", "七进七出");
      hashMap.put("徐庶", "进曹营");
      hashMap.put("诸葛亮", "空城计");
      hashMap.put("诸葛亮", "火烧赤壁");//会覆盖掉上面的 空城计
      Set<String> keySet=hashMap.keySet();
      for(String k:keySet) {
    	  String valueString=hashMap.get(k);
    	  System.out.println(k+"="+valueString);
      }
	}
}
```

输出

```
关羽=斩颜良诛文丑
诸葛亮=火烧赤壁
赵云=七进七出
徐庶=进曹营
```

### 键是Integer值是String

```java
package map;

import java.util.HashMap;
import java.util.Set;

public class HashMapDemo {
  public static void main(String[] args) {
		HashMap<Integer, String> hashMap = new HashMap<Integer, String>();
	      hashMap.put(001, "斩颜良诛文丑");
	      hashMap.put(002, "七进七出");
	      hashMap.put(003, "进曹营");
	      hashMap.put(004, "空城计");
	      
	      hashMap.put(007, "三英战吕布");
	      
	      Set<Integer> keySet=hashMap.keySet();
	      for(Integer k:keySet) {
	    	  String valueString=hashMap.get(k);
	    	  System.out.println(k+"="+valueString);
	      }
	      System.out.println();
	      //集合元素的字符串表示，并非遍历
	      System.out.println(hashMap);
}
}
```

输出

```
1=斩颜良诛文丑
2=七进七出
3=进曹营
4=空城计
7=三英战吕布

{1=斩颜良诛文丑, 2=七进七出, 3=进曹营, 4=空城计, 7=三英战吕布}
```

### 键是Integer值是Student

Student类

```java
package map;

public class Student {
	private String name;
	private int age;

	public Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

}
```

HashMap

```java
package map;

import java.util.HashMap;
import java.util.Set;

public class HashMapDemo {
	public static void main(String[] args) {
		HashMap<Integer, Student> hashMap = new HashMap<Integer, Student>();
		Student student = new Student("张三", 20);
		Student student1 = new Student("李四", 19);
		Student student2 = new Student("王五", 14);

		hashMap.put(01, student);
		hashMap.put(02, student1);
		hashMap.put(03, student2);
		
		Set<Integer> keySet=hashMap.keySet();
		for(Integer k:keySet) {
			Student stu=hashMap.get(k);
			System.out.println(k+"="+stu.getName()+" "+stu.getAge());
		}
	}
}
```

输出

```
1=张三 20
2=李四 19
3=王五 14
```

## LinkedHashMap

Map接口的哈希表和链接列表实现，具有可预知的迭代顺序。

输出是有序的，什么顺序输进去就按照什么顺序输出来，当键相同时，值也被覆盖了。

```java
package map;

import java.util.Set;

public class LinkedHashMap {

	public static void main(String[] args) {
		java.util.LinkedHashMap<String, String> linkedHashMap=new java.util.LinkedHashMap<String, String>();
		linkedHashMap.put("关羽", "温酒斩华雄");
		linkedHashMap.put("吕布", "方天画戟杀董卓");
		linkedHashMap.put("曹操", "煮酒论英雄");
		linkedHashMap.put("关羽", "过五关斩六将");
		
		Set<String> keySet=linkedHashMap.keySet();
		for(String k:keySet) {
			String valueString=linkedHashMap.get(k);
			System.out.println(k+" "+valueString);
		}
	}
}

```

输出

```
关羽 过五关斩六将
吕布 方天画戟杀董卓
曹操 煮酒论英雄
```

