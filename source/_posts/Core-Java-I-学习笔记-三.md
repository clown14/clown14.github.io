---
title: Core Java I 学习笔记(三)
date: 2017-03-25 19:10:05
tags: Java
categories: Java
---

对象和类
<!-- more -->

## 面向对象概述

Java完全面向对象。

### 类

类是构造对象的模板或蓝图。

**封装（encapsulation，也称数据隐藏）是与对象有关的重要概念。对象中的数据称为实例域（instance field），操作数据的过程称为方法（method）。对于每个特定的类实例（对象）都有一组特定的实例域值。这些值的集合就是这个对象的当前状态（state）。**

实现封装的关键在于不然类中的方法直接地访问其他类的实例域。

### 对象

对象的三个主要特性：

1. 对象的行为（behavior）--可以对对象施加哪些操作/方法？
2. 对象的状态（state）--当施加那些方法时，对象如何响应？
3. 对象标识（identity）--如何辨别具有相同行为与状态的不同对象？

### 对象与对象变量

Java中使用构造器（constructor）构造新实例。构造器是一种特殊的方法，并用构造器初始化对象。
构造器的名字与类名相同，想构造一个Date对象，需要在构造器前加上new操作符。如：

	new Date();

这个表达式构造了一个新对象，这个新对象被初始化为当前的日期和时间。
上面的例子中的对象只用了一次，通常希望构造的对象可以多次使用，因此需要将对象存放在一个变量中：

	Date birthday= new Date();

在对象与对象变量之间存在一个重要区别。如：

	Date deadline;// deadline doesn't refer to any object

**变量对象deadline可以引用Date类型的对象，当deadline不是一个对象，也没有引用对象，此时不能将任何Date方法应用于这个变量上。简单来说，一个变量对象并没有实际包含一个对象，而仅仅是引用一个对象。java中任何对象变量的值都是对存储在另外一个地方的一个对象的引用。new操作符的返回值也是一个引用。**

## 用户自定义类

通常，用户自定义类没有main方法，却有自己的实例域和实例方法。如：

	class Employee
	{
		//instance fields
		private String name;
		private double salary;
		private LocalDat hireDay;

		//constructor
		public Employee(String n,double s,int year,int month,int day)
		{
			name= n;
			salary= s;
			hireDaty= LocalDate.of(year,month,day);
		}

		//a method
		public String getName()
		{
			return name;
		}

		//more methods
		...
	}

### 隐式参数与显示参数

方法用于操作对象以及存取它们的实例域。如：

	public void raiseSalary(double byPercent)
	{
		double raise= salary* byPercent/ 100;
		salary+= raise;
	}

raiseSalary方法有两个参数，第一个称为隐式（implicit）参数，出现在方法名前的Employee类对象。第二个参数位于方法名后面括号中，这是一个显示（explicit）参数。

在每个方法中，关键字this表示隐式参数。如果需要，raiseSalary可以如下编写：
	
	public void raiseSalary(double byPercent)
	{
		double raise= this.salary* byPercent/ 100;
		this.salary+= raise;
	}

### final实例域

可以将实例域定义为final。构建对象时必须初始化这样的域。也就是说，必须确保在每一个构造器执行之后，这个域的值被设置，并且在后面的操作中，不能够再对它进行修改。

## 静态域与静态方法

如果将域定义为static，每个类中只有一个这样的域。而每一个对象对于所有的实例域缺都有自己的一份拷贝。

	class Employee
	{
		private static int nextId= 1;

		private int id;
		...
	}

静态变量使用得比较少，当**静态常量**用得比较多。如，Math中

	public class Math
	{
		...
		public static final double PI= 3.14159265358979323846;
	}

在程序中可以采用Math.PI的形式获得这个常量。

**静态方法**是一种不能向对象实施操作的方法。可以认为静态方法时没有this参数的方法。如：

	public static int getNextId()
	{
		return nextId;
	}

在下面两种情况下使用静态方法：
* 一个方法不需要访问对象状态，其所需参数都是通过显式参数提供。
* 一个方法只需要访问类的静态域。

## 方法参数

在程序设计语言中，将参数传递给方法（函数）分两种。
1. **按值调用（call by value）**表示方法接收的是调用者提供的值。
2. **按引用调用（call by reference）**表示方法接收的是调用者提供的变量地址。

Java中总是采用按值调用。也就是说，方法得到的是所有参数值的一个拷贝，特别是，方法不能修改传递给它的任何参数变量的内容。如：

	public static void swap(Employee x,Employee y) // doesn't work
	{
		Employee temp= x;
		x= y;
		y= temp;
	}

如果Java采用按引用调用，那么这个方法应该可以实现交换数据的效果：

	Employee a= new Employee("Alice",..);
	Employee b= new Employee("Bob",..);
	swap(a,v);
	//does a now refer to Bob,b to Alice?

但方法并没有改变变量a,b中的对象引用。swap方法的参数x,y被初始化为两个对象引用的拷贝，这个方法交换的是这两个拷贝。在方法结束时参数变量x,y被丢弃了，变量a,b仍然引用这个方法调用之前所引用的对象。

实际上，**对象引用是按值传递的**。

## 对象构造

### 重载

重载（overloading）：方法名相同，参数不同。Java允许重载任何方法。

### 默认域初始化

如果构造器中没有显式地给域赋予初值，那么就会被自动赋默认值：数值为0，布尔值为false，对象引用为null。（不推荐，尽量避免，否则影响可读性）

### 无参构造器

如果在编写类时没有编写构造器，系统会提供一个无参数构造器，将所有实例域设置为默认值。
注：仅当类没有提供任何构造器时，系统才提供默认构造器，如果类中提供了至少一个构造器，但没有提供无参数构造器，则在构造对象时如果没有提供参数就会被视为不合法。

## 类设计技巧

1. 保证数据私有，不要破坏封装性；
2. 对数据初始化；
3. 不要在类中使用过多基本类型；
4. 将职责过多的类进行分解；
5. 类名和方法名要能够体现它们的职责。