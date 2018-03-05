---
title: Core Java I 学习笔记(七)
date: 2017-04-22 19:35:18
tags: Java
categories: Java
---

集合
<!-- more -->

## Java集合框架

与现代的数据结构类库的常见情况一样，Java集合类库也将**接口（interface）**与**实现（implementation）**分离。

在Java类库中，集合类的基本接口是Collection接口。有两个基本方法：

	public interface Collection<E>{
		boolean add(E element);
		Iterator<E> iterator;
		...
	}

* add方法用于向集合中添加元素。
* iterator方法用于返回一个实现了Iterator接口的对象。可以使用这个迭代器对象依次访问集合中的元素。

Iterator接口包含4个方法：

	public interface Iterator<E>{
		E next();
		boolean hasNext();
		void remove;
		default void forEachRemaining(Consumer<? super E> action);
	}

remove方法将会删除上次调用next方法时返回的元素，next和remove方法的调用具有互相依赖性。如果调用remove之前没有调用next将是非法的。

Java集合框架为不同类型的集合定义了大量接口：

<img src="/img/集合框架的接口.png" alt="图片名称" title="">

## 具体的集合

 | 集合类型     | 描述| 
|:--------:|:---------:|
| ArrayList| 一种可以动态增长和缩减的索引序列| 
| LinkedList| 一种可以在任何位置进行高效地插入和删除操作的有序序列| 
| ArrayDeque| 一种用循环数组实现的双端队列| 
| HashSet| 一种没有重复元素的无序集合| 
| TreeSet| 一种有序集| 
| EnumSet| 一种包含枚举类型值的集| 
| LinkedHashSet| 一种可以记住元素插入次序的集| 
| PriorityQueue| 一种允许高效删除最小元素的集合| 
| HashMap| 一种存储键/值关联的数据结构| 
| TreeMap| 一种键值有序排列的映射表| 
| EnumMap| 一种键值属于枚举类型的映射表| 
| LinkedHashMap| 一种可以记住键/值项添加次序的映射表| 
| WeakHashMap| 一种其值无用武之地后可以被垃圾回收器回收的映射表| 
| IdentityHashMap| 一种用==而不是equals比较键值的映射表| 


<img src="/img/集合框架中的类.png" alt="图片名称" title="">

Notes：
在java中，所有链表实际上都是**双向链接（doubly linked）**，即每个结点还存放着前驱结点的引用。
**散列表（hash table）**可以快速地查找说需要的对象，散列表为每个对象计算一个整数，称为**散列码（hash code）**。
TreeSet类与散列集十分相似，不过它比散列集有所改进。树集是一个**有序集合（sorted collection）**。可以以任意顺序将元素插入到集合中。

## 映射

集是一个集合，它可以快速地查找现有的元素。但是，要查看一个元素，需要有要查找元素的精确副本。通常，我们知道某些键的信息，并想要查找与之对应的元素。**映射（map）**数据结构就是为此设计的。映射用来存放键/值对。

### 基本映射操作

Java类库为映射提供了两个通用的实现：HashMap和TreeMap。
散列映射对键进行散列，树映射用键的整体顺序对元素进行排序，并将其组织成搜索树。散列或比较函数只能作用于键。与键关联的值不能进行散列或比较。
与集一样，散列稍微快一点，如果不需要按照排列顺序访问键，最好选择散列。

下列代码键为存储的员工信息建立一个散列映射：

	Map<String, Employee> staff= new HashMap<>();
	Employee harry= new Employee("Harry Hacker");
	staff.put("987-98-9996",harry);
	...

每当往映射中添加对象时，必须同时提供一个键。在这里，键是一个字符串，对应的值是Employee对象。
要想检索一个对象，必须使用一个键。

	String id= "987-98-9996";
	e= staff.get(id); //get harry

如果在映射中没有对应的值，get将返回null。
remove方法用于从映射中删除给定键对应的元素。size方法用于返回映射中的元素数。
