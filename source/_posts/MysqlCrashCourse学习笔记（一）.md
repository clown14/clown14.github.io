---
title: MysqlCrashCourse学习笔记（一）
date: 2018-03-25 14:14:00
tags: Mysql
categories: Mysql
---

MySQL必知必会（一）。
<!-- more -->

# 了解SQL

## 数据库基础 

**数据库**（database）保存有组织的数据的容器（通常是一个文件或一组文件）。

人们通常用数据库来代表所使用的数据库软件，这不正确，确切地说，数据库软件应称为DBMS（数据库管理系统）。数据库是通过DBMS创建和操作的容器。

**表**（table）某种特定类型数据的结构化清单。

**模式**（schema）关于数据库和表的布局及特性的信息。

**列**（column）表中的一个字段。所有表都是由一个或多个列组成的。

**数据类型**（datatype）所容许的数据的类型。每个表列都有相应的数据类型，它限制该列中存储的数据。

**行**（row）表中的一个记录。

**主键**（primary key）一列（或一组列），其值能够唯一区分表中每个行。

 * 任意两行都不具有相同的主键值；
 * 每个行都必须具有一个主键值（主键值不允许NULL值）；
 * 在使用多列作为主键时，所有列值的组合必须是唯一的。


 ## 什么是SQL

 Structured Query Language（结构化查询语言）

# MySQL简介

 MySQL是一种DBMS，即它是一种数据库软件。

# 使用MySQL

## 选择数据库

输入： ``USE crashcourse;``
输出： ``Database changes``

记住，必须先使用USE打开数据库，才能读取其中的数据。

## 了解数据库和表

数据库、表、列、用户、权限等信息被存储在数据库和表中。不过，内部的表一般不直接访问。可用SHOW命令来显示这些信息。

``SHOW DATABASES;``	返回可用数据库的一个列表。
``SHOW TABLES;``返回当前选择的数据库内可用表的列表。

所支持的其他SHOW语句：
``SHOW COLUMNS;`` 
``SHOW STATUS;`` 
``SHOW GRANTS;`` 
``SHOW ERRORS;`` 
``SHOW WARNINGS;`` 

MySQL支持用DESCRIBE作为SHOW XXX FROM的一种快捷方式。

# 检索数据

``SELECT prod_name FROM product;``

上述语句利用SELECT语句从products表中检索一个名为prod_name的列，返回所有行，数据没有过滤。

多条SQL语句必须以分号（；）分隔。MySQL如同多数DBMS一样，不需要在单条SQL语句后加分号。SQL语句不区分大小写。

``SELECT prod_id,prod_name,prod_price FROM products;``

要想从一个表中检索多个列，使用相同的SELECT语句，唯一的不同是必须在SELECT关键字后给出多个列名，列名之间必须以逗号分隔。（最后一个列名后不加）

``SELECT * FROM products;``

如果给定一个通配符（* ），则返回表中所有列。

``SELECT DISTINCT vend_id FROM products;``

SELECT DISTINCT vend_id告诉MySQL只返回不同（唯一）的vend_id行。如果使用DISTINCT关键字，它必须直接放在列名的前面。

``SELECT prod_name FROM products LIMIT 5;``

此语句使用SELECT语句检索单个列。LIMIT 5指示MySQL返回不多于5行。

``SELECT prod_name FROM products LIMIT 5,5;``

LIMIT 5指示MySQL返回从行5开始的5行。第一个数为开始位置，第二个数为要检索的行数。

# 排序检索数据

**子句（clause）** SQL语句由子句构成，子句的例子有SELECT语句的FROM子句。

``SELECT prod_name FROM products ORDER BY prod_name;``

为了明确地排序用SELECT语句检索出的数据，可使用ORDER BY子句，ORDER BY子句取一个或多个列的名字，据此对输出进行排序。这条语句指示MySQL对prod_name列以字母顺序排序数据。

``SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price, prod_name;``

对于上述例子的输出，仅在多个行具有相同的prod_price值时才对产品按prod_name进行排序。如果prod_price列中所有的值是唯一的，则不会按prod_name排序。

``SELECT prod_id,prod_price,prod_name FROM products ORDER BY prod_price DESC, prod_name;``

数据排序不限于升序排序，这是默认的的排序顺序，为了进行降序排序，必须指定DESC关键字。DESC关键字只应用到直接位于其前面的列名。在上例中，只对prod_price列指定DESC，对prod_name列不指定。




