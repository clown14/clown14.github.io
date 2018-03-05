---
title: Core Java I 学习笔记(二)
date: 2017-03-18 19:45:08
tags: Java
categories: Java
---
Java的基本程序设计结构
<!-- more -->

## 注释

Java中，有三种标记注释的方式。

* 最常用的是//，注释内容从//开始到本行结尾。


	System.out.println("We will not use 'Hello,World'");//is this too cute?


* 第2种注释


	/* This is the first sample program in Core Java Chapter 3 */


* 第3种注释可以用来自动地生成文档。

``` bash
/**
 * This is the first sample program in Core Java Chapter 3
 * @version 1.01 1997-03-22
 * @author Gary Cornell
 */
 ``` 

 注：Java中，/**/注释不能嵌套

 ## 数据类型

 Java是强类型语言，每个变量声明一种类型。
 Java中一共有8种基本类型。

 ### 整型（4种）

 | 类型     | 存储需求| 取值范围|
|:--------:|:---------:|:-------:|
| int| 4字节| -2 147 483 648~2 147 483 647      |
| short| 2字节| -32768~32767      |
| long| 8字节| -9 223 372 036 854 775 808~9 223 372 036 854 775 807      |
| byte| 1字节| -128~127      |

### 浮点型（2种）

 | 类型     | 存储需求| 取值范围|
|:--------:|:---------:|:-------:|
| float| 4字节| 大约±3.402 823 47E+38F（有效位6~7位）      |
| double| 8字节| 大约±1.797 693 134 862 315 70E+308（有效位15位）     |

注：浮点数值不适用于无法接收舍入误差的金融计算。
浮点数值采用二进制系统表示，无法精确地表示分数1/10。

### char类型

描述了UTF-16编码中的一个代码单元。建议不要在程序中使用char类型。

### boolean类型

boolean类型有两个值：false和true，用来判定逻辑条件。整型和布尔值之间不能进行互相转换。

## 变量

* 在Java中，每个变量都有一个类型。
* 声明一个变量后，必须用赋值语句对变量进行显式初始化。
* 变量的声明尽可能靠近变量第一次使用的地方

* Java中用final指示常量，表示只能被赋值一次，不能更改。

## 运算符

Java中使用+ - * /表示加减乘除。
当参与/运算的两个操作数都是整数时，表整除，否则表浮点除法。整数的求余用%表示。

### 数值类型转换

<img src="/img/数值类型转换.png" alt="图片名称" title="">

### 强制类型转换

语法格式是在圆括号中给出想要转换的目标类型，后面紧跟待转换的变量名。

	double x= 9.997;
	int nx= (int)Math.round(x);

### 括号和运算符级别

<img src="/img/运算符.jpg" alt="图片名称" title="">

### 枚举类型（详见后）

## 字符串

### 子串

substring可以从一个较大的字符串提取出一个子串。如：

	String greeting= "Hello";
	String s= greeting.substring(0,3); //Hel

### 拼接

Java允许使用+号拼接两个字符串。

	String expletive= "Expletive";
	String PG13="deleted";
	String message= expletive + PG13; //Expletivedeleted

### 不可变

String类没有提供用于修改字符串的方法。
而是创建一个新字符串，而不是修改一个代码单元。

### 检测字符串是否相等

可以使用equals方法检测两个字符串是否相等。如：
	
	s.equals(t);

注：不要用==检测两个字符串是否相等，它只能确定两个字符串是否放在同一个位置上。

## 控制流程

### 块作用域

块（即复合语句）是指在一对大括号括起来的若干条简单的Java语句。块决定了变量的作用域，一个块可以嵌套在另一个块中。但是不能在嵌套的两个块中声明同名的变量。

	public static void main(String[] args)
	{
		int n;
		...
		{
			int k;
			int n; //Error--can't redefine n in inner block
			...
		}
	}

### 条件语句

	if (condition){
		statement
	}else if (condition){
		statement
	}else{
		statement
	}

### 循环

	while(condition)
	{
		statement
	}


	do
	{
		statement
	}
	while(condition);

### 确定循环

	for(condition)
	{
		statement
	}

### 多重选择

	switch(choice)
	{
		case 1:
			...
			break;
		case 2:
			...
			break;
		case 3:
			...
			break;
		case 4:
			...
			break;
		default:
			//bad input
			...
			break;
	}

注：switch语句将从与选项值相匹配的case标签处开始执行直到遇到break语句，或者执行到switch语句的结束处为止。如果没有相匹配的case标签，而有default字句，就执行这个子句。

case标签可以使常量表达式，枚举常量，还可以是字符串字面量。

## 数组

### 两种声明形式

	int[] a;

	或

	int a[];

### for each 循环

可以用来依次处理数组中的每个元素（其他类型的元素集合亦可）。

	for (variable : collection ) statement

	如:
	for(int element : a)
		System.out.println(element);

### 数组拷贝

在Java中。允许将一个数组变量拷贝给另一个数组变量。这时，两个变量将引用同一个数组。