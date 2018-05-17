---
title: MysqlCrashCourse学习笔记（三）
date: 2018-04-14 14:53:38
tags: Mysql
categories: Mysql
---

MySQL必知必会（三）
<!-- more -->

# 使用子查询

## 利用子查询进行过滤

在SELECT语句中，子查询总是从内向外处理。

	SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems WHERE prod_id = 'TNT2');

# 联结表

## 外键

外键为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。

## 创建联结

	SELECT vend_name, prod_name, prod_price FROM vendors, products
	WHERE vendors.vend_id = products.vend_id ORDER BY vend_name, prod_name;

**完全限定名：** 在引用的列可能出现二义性时，必须使用完全限定名（一个点分隔的表名和列名）。

**笛卡儿积：** 由没有联结条件的表关系返回的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

## 内部联结

	SELECT vend_name, prod_name, prod_price FROM vendors INNER JOIN products
	on vendors.vend_id = products.vend_id ;

此语句中的SELECT与前面的SELECT语句相同，但FROM子句不同。这里，两个表之间的关系时FROM子句的组成部分，以INNER JOIN指定。
在使用这种语法时，联结条件用特定的ON子句而不是WHERE子句给出。传递给ON的实际条件与传递给WHERE的相同。

## 联结多个表

SQL对一条SELECT语句中可以联结的表的数目没有限制。

# 创建高级联结

## 自联结

	SELECT prod_id, prod_name FROM products WHERE vend_id =
	(SELECT vend_id FROM products WHERE prod_id = 'DTNTR')；

等价于

	SELECT p1.prod_id, p1.prod_name FROM products AS p1, products AS p2
	WHERE p1.vend_id = p2.vend_id AND p2.prod_id = 'DTNTR';

## 自然联结

无论何时对表进行联结，应该至少有一个列出现在不止一个表中（被联结的列）。标准的联结（内部联结）返回所有数据，甚至相同的列多次出现。自然联结排除多次出现，使每个列只返回一次。

## 外部联结

	SELECT customers.cust_id, orders.order_num FROM customers LEFT OUTER JOIN orders
	on customers.cust_id = orders.cust_id;

与内部联结关联两个表中的行不同的是，外部联结还包括没有关联行的行。在使用OUTER JOIN语法时，必须使用RIGHT和LEFT关键字指定包括其所有行的表（RIGHT指出的是OUTER JOIN右边的表，而LEFT指出的是OUTER JOIN左边的表）。

# 组合查询

## 使用UNION

	SELECT vend_id, prod_id, prod_price FROM products WHERE prod_price <= 5 
	ONION
	SELECT vend_id, prod_id, prod_price FROM products WHERE vend_id IN (1001, 1002);

UNION指示MySQL执行两条SELECT语句，并把输出组合成单个查询结果集。

UNION从查询结果集中自动去除了重复的行，如果想返回所有匹配行，可使用UNION ALL而不是UNION。

在用UNION组合查询时，只能使用一条ORDER BY子句，它必须出现在最后一天SELECT语句之后。

# 使用视图

视图是虚拟的表。与包含数据的表不一样，视图只包含使用时动态检索数据的查询。

## 为什么使用视图

1. 重用SQL语句。
2. 简化复杂的SQL操作。
3. 使用表的组成部分而不是整个表。
4. 保护数据。
5. 更改数据格式和表示。

## 视图的规则和限制

1. 视图必须唯一命名。
2. 数目没有限制。
3. 为了创建视图，必须具有足够的访问权限。
4. 视图可以嵌套。
5. 视图不能索引，也不能有关联的触发器或默认值。
6. 视图可以和表一起使用。

## 使用视图

	CREATE VIEW productcustomers AS 
	SELECT cust_name, cust_contact, prod_id FROM customers, orders, orderitems WHERE
	customers.cust_id = orders.cust_id AND orderitems.order_num = orders.order_num;

创建一个名为productcustomers的视图。

	SELECT cust_name, cust_tact FROM productcustomers WHERE prod_id = 'TNT2';

通过WHERE子句从视图中检索特定数据。

通常，视图是可更新的。更新一个视图将更新其基表。

# 使用存储过程

## 为什么要使用存储过程

1. 简化复杂的操作。
2. 保证数据的完整性，防止错误。
3. 简化对变动的管理。
4. 提高性能。

换句话说就是简单，安全，高性能。

## 使用存储过程

	CREATE PROCEDURE productpricing()
	BEGIN 
		SELECT Avg(prod_price) AS priceaverage FROM products;
	END;

如果存储过程接受参数，它们将在（）中列举出来。BEGIN和END用来先抵挡存储过程体。

	CALL productpricing();

使用。（存储过程实际上是一种函数）

	DROP PROCEDURE productpricing;

删除存储过程。

## 使用参数

一般，存储过程并不显示结果，而是把结果返回给你指定的变量。

	CREATE PROCEDURE productpricing(
		OUT pl DECIMAL(8,2),  //DECIMAL为精度函数
		OUT ph DECIMAL(8,2),
		OUT pa DECIMAL(8,2),
	)
	BEGIN 
		SELECT Min(prod_price) INTO pl FROM products;
		SELECT Max(prod_price) INTO ph FROM products;
		SELECT Avg(prod_price) INTO pa FROM products;
	END;

此存储过程接受3个参数，pl存储最低价格，ph最高价，pa平均价。关键字OUT指出相应的参数用来从存储过程传出一个值。

	CALL productpricing(@pricelow, @pricehigh, @priceaverage);

MySQL变量都必须以@开始。

在调用时，这条语句不显示任何数据。它返回以后可以显示的变量。

检索：

	SELECT @pricelow, @pricehigh, @priceaverage;