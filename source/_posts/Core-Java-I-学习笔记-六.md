---
title: Core Java I 学习笔记(六)
date: 2017-04-15 19:31:01
tags: Java
categories: Java
---

泛型
<!-- more -->

## 为什么要使用泛型程序设计

**泛型程序设计**意味着编写的代码可以被很多不同类型的对象所重用。

### 类型参数的好处

在Java增加泛型类之前，泛型程序设计使用继承实现的。ArayList类只维护一个Object引用的数组：

	public class ArrayList
	{
		private Object[] elementData;
		...
		public Object get(int i){...}
		public void add(Object o){...}
	}

这种方法有两个问题
* 但获取一个值时必须进行强制类型转换
* 没有错误检查，可以向数组列表添加任何类的对象。

泛型提供了一个更好的解决方案：**类型参数（type parameters）**。ArrayList类有一个类型参数来只是元素类型：
	
	ArrayList<String> files= new ArrayList<>();

参数类型的魅力在于：使得程序具有更好的可读性和安全性。

## 定义简单泛型类

一个**泛型类（generic class）**就是具有一个或多个类型变量的类。

	public class Pair<T>
	{
		private T first;
		private T second;

		public Pair() {first = null; second = null;}
		public Pair(T first, T secont){this.first = first; this.second = secont;}

		public T getFirst(){return first;}
		public T getSecont(){return second;}

		public void setFirst(T newValue){first= newValue;}
		public void setSecot(T newValue){second= newValue;}
	}

泛型类可以有多个类型变量。如：

	public class Pair<T, U>{...}

用具体的类型替换类型变量就可以实例化泛型类型，如：

	Pair<String>

可以将结果想象成带有构造器的普通类：
	
	Pair<String>()
	Pair<String>(String, String)

和方法：
	
	String getFirst()
	String getSecond()
	void setFirst(String)
	void setSecond(String)

换句话说，泛型类可以看作普通类的工厂。

## 泛型方法

	class ArrayAlg{
		public void <T> T getMiddle(T...a){
			return a[a.length / 2];
		}
	}

类型变量放在修饰符的后面，放在类型的前面。泛型方法可以定义在普通类中，也可以定义在泛型类中。但调用一个泛型方法时，在方法名前的尖括号中放入具体的类型：

	String middle= ArrayAlg.<String>getMiddle("John","Q.","Public");

一般，方法调用中可以省略<String>类型参数。

## 类型变量的限定

有时，类或方法需要对类型变量加以约束：

	public static <T extends Comparable> T min(T[] a)....

现在，泛型的min方法只能被实现了Comparable接口的类的数组调用。
一个类型变量或通配符可以有多个限定：
	
	T extends Comparable & Serializable

注：java中可以根据需要拥有多个接口超类型，但限定中至多有一个类。如果一个类作为限定，必须是限定列表中的第一个。

## 泛型代码和虚拟机

关于Java泛型转换的事实：
* 虚拟机中没有泛型，只有普通的类和方法；
* 所以的类型参数都用它们的限定类型替换；
* 桥方法被合成来保护多态；
* 为保持类型安全性，必要时插入强制类型转换。

### 类型擦除

无论何时定义一个泛型类型，都自动提供了一个相应的**原始类型（raw type）**。原始类型的名字就是删除类型参数后的泛型类型名。**擦除（erased）**类型变量，并替换为限定类型（无限定的变量用Object）。

如，Pair<T>的原始类型如下：

	public class Pair
	{
		private Object first;
		private Object second;

		public Pair() {first = null; second = null;}
		public Pair(Object first, Object secont){this.first = first; this.second = secont;}

		public Object getFirst(){return first;}
		public Object getSecont(){return second;}

		public void setFirst(Object newValue){first= newValue;}
		public void setSecot(Object newValue){second= newValue;}
	}

## 约束与局限性

* 不能用基本类型实例化类型参数；
* 运行时类型查询只适用于原始类型；
* varargs警告；
* 不能实例化类型变量；
* 不能构造泛型数组；
* 泛型类的静态上下文中类型变量无效；
* 不能抛出或捕获泛型类的实例；
* 可以消除对受查异常的检查；
* 注意擦除后的冲突。

## 泛型类型的继承规则

* Manager继承Employee，但Pair < Employee >与Pair < Manager >没有什么联系。
* 永远可以将参数化类型转换为一个原始类型。
* 泛型类可以扩展或实现其他的泛型类。

## 通配符类型

### 通配符概念

通配符类型中，允许类型参数变化：

	Pair <? extends Employee>

表示任何泛型Pair类型，它的类型参数是Employee的子类。

### 通配符的超类型限定

	? super Manager

这个通配符限制为Manager的所有超类型。可以为方法提供参数，但不能使用返回值。

	void setFirst(? super Manager)
	? super Manager getFirst()

这不是真正的Java语法，但可以看出编译器知道什么，编译器无法知道setFirst方法的具体类型，只能传递Manager类型的对象，或某个子类型对象。调用getFirst，不能保证返回对象的类型。只能把它赋给一个Object。

### 无限定通配符

类型Pair<?>有以下方法：
	
	? getFirst()
	void setFirst(?)

getFirst的返回值只能赋给一个Object。setFirst方法不能被调用。

### 通配符捕获

通配符不是类型变量，因此，不能在编写代码中使用”?“作为一种类型。可以通过写一个辅助函数解决。