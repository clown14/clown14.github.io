---
title: Core Java I 学习笔记(五)
date: 2017-04-08 19:46:50
tags: Java
categories: Java
---

接口，lambda表达式与内部类
<!-- more -->

## 接口

### 概念

在Java中，接口不是类，而是对类的一组需求描述，这些类要遵从接口描述的统一格式进行定义。
接口中的所有方法自动地属于public，接口绝不能含有实例域。
实现接口时，必须把方法声明为public。

为了让类实现一个接口，通常需要下面两个步骤：
1. 将类声明为实现给定的接口。
2. 对接口中的所有方法进行定义。


	public interface Comparable
	{
		int compareTo(Object other);
	}

	class Employee implements Comparable
	{
		public int compareTo(Object otherObject)
		{
			Employee other= (Employee) otherObject;
			return Double.compare(salary, other.salary);
		}
	}

### 接口的特性

接口不是类，不能使用new运算符实例化一个接口：

	x= new Comparable(..); //ERROR

然而，尽管不能构造接口的对象，却能声明接口的变量：

	Comparable x; //OK

接口变量必须应用实现了接口的类对象：

	x= new Employee(..); //OK 

接口中不能包含实例域或静态方法，却可以包含常量。
与接口中的方法都自动被设置为public一样，接口中的域将被自动设为public static final。
每个类只能拥有一个超类，却能实现多个接口。使用逗号将实现的各个接口分隔开。

	class Employee implements Cloneable,Comparable

### 接口与抽象类

使用抽象类表示通用属性存在一个问题：每个类只能扩展于一个类。
Java不支持多继承，而接口可以提供多重继承的大多数好处，同时避免多重继承的复发性和低效性。

## lambda表达式

Java面向对象，不能直接传递代码段，必须构造一个对象，这个对象的类需要有一个方法能包含所需的代码。

lambda表达式就是一个代码块，以及必须传入代码的变量范围。

	(String first, String second)
		-> first.length - second.length

以上时一种lambda表达式形式：参数，箭头(->)以及一个表达式。如果代码要完成的计算无法放在一个表达式中，就可以像写方法一样，把这些代码放进{}中，并包含显式的return语句。如：

	(String first, String second) ->
		{
			if(first.length() < second.length()) return -1;
			else if(first.length() > second.length()) return 1;
			else return 0;
		}

## 内部类

**内部类**是定义在另一个类中的类。

* 内部类方法可以访问该类定义所在的作用域中的数据，包括私有的数据。
* 内部类可以对同一个包中的其他类隐藏起来。
* 但想要定义一个回调函数且不想编写大量代码时，使用**匿名（anonymous）**内部类比较便捷。