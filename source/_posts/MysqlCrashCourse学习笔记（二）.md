---
title: MysqlCrashCourse学习笔记（二）
date: 2018-04-01 13:49:45
tags: Mysql
categories: Mysql
---

MySQL必知必会（二）
<!-- more -->

# 过滤数据

## 使用WHERE子句

在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。WHERE子句在表名(FROM子句)之后给出：

	SELECT prod_name, prod_price FROM products WHERE prod_price = 2.50;

这条语句从products表中检索两个列，但不返回所有行，只返回prod_price值为2.50的值。

## <center>WHERE子句操作符</center>

 | 操作符     | 说明| 
|:--------:|:---------:|
| =| 等于| 
| <>| 不等于| 
| !=| 不等于| 
| <| 小于| 
| <=| 小于等于| 
| >| 大于| 
| >=| 大于等于| 
| BETWEEN| 在指定的两个值之间| 

	SELECT prod_name, prod_price FROM products WHERE prod_name = 'fuses';

检查WHERE prod_name='fuses'语句，它返回prod_name的值为Fuses的一行。MySQL在执行匹配时默认不区分大小写；

	SELECT prod_name, prod_price FROM products WHERE prod_price < 10;

列出价格小于10美元的所有产品；

	SELECT vend_id, prod_name FROM products WHERE vend_id <>1003;

列出不是由供应商1003制造的所有产品；

	SELECT prod_name, prod_price FROM products WHERE prod_price BETWEEN 5 AND 10;

检索价格在5美元和10美元之间的产品。使用BETWEEN时，必须指定两个值，用AND分隔。


# 数据过滤

## AND操作符

	SELECT prod_id, prod_price, prod_name FROM products WHERE vend_id = 1003 AND prod_price <= 10;

检索有供应商1003制造且价格小于等于10美元的所有产品的名称和价格。

## OR操作符

	SELECT prod_name, prod_price FROM products WHERE vend_id = 1002 OR vend_id =1003;

检索由任一个指定供应商制造的所有产品的产品名和价格。

	SELECT prod_name, prod_price FROM products WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;

SQL在处理OR操作符前，优先处理AND操作符，可以使用圆括号明确地分组相应的操作符。

## IN操作符

	SELECT  prod_name, prod_price FROM products WHERE vend_id IN (1002, 1003) ORDER BY prod_name;

IN操作符用来指定条件范围，范围中的每个条件都可以进行匹配。


# 用通配符进行过滤

## 百分号（%）通配符

	SELECT prod_id, prod_name FROM products WHERE prod_name LIKE 'jet%';

此例子使用了搜索模式'jet%'。在执行时，将检索任意以jet起头的词。%告诉MySQL接受jet之后的任意字符，不管它有多少字符。

	SELECT prod_id, prod_name FROM products WHERE prod_name LIKE '%anvil%';

通配符可在搜索模式中任意位置使用，且可以使用多个通配符。

## 下划线（\_）通配符

	SELECT prod_id, prod_name FROM products WHERE prod_name LIKE '_ ton anvil';

下划线的用途与%一样，但下划线只匹配单个字符而不是多个字符。与%能匹配0个字符不一样，\_总是匹配一个字符，不能多也不能少。

# 创建计算字段

## 计算字段

存储在数据库表中的数据一般不是应用程序所需要的格式。计算字段并不实际存在于数据库表中，计算字段是运行时在SELECT语句内创建的。

## 拼接字段

在MySQL的SELECT语句中，可使用Concat()函数来拼接两个列。

	SELECT Concat(vend_name, '(', vend_country, ')') FROM vendors ORDER BY vend_name;

上述SELECT语句返回包含上述4个元素的单个列（计算字段）。

## 使用别名

	SELECT Concat(vend_name, '(', vend_country, ')') AS vend_title FROM vendors ORDER BY vend_name;

SQL支持列别名。别名（alias）是一个字段或值的替换名。别名用AS关键字赋予。

# 使用数据处理函数

## <center>文本处理函数</center>

 | 函数     | 说明| 
|:--------:|:---------:|
| Left()| 返回串左边的字符| 
| Length()| 返回串的长度| 
| Locate()| 找出串的一个字串| 
| Lower()| 将串转换为小写| 
| LTrim()| 去掉串左边的空格| 
| Right()| 返回串右边的字符| 
| RTrim()| 去掉串右边的空格| 
| Soundex()| 返回串的SOUNDEX值| 
| SubString()| 返回子串的字符| 
| Upper()| 将串转换为大写|

## <center>日期和时间处理函数</center>

 | 函数     | 说明| 
|:--------:|:---------:|
| AddDate()| 增加一个日期| 
| AddTime()| 增加一个时间| 
| CurDate()| 返回当前日期| 
| CurTime()| 返回当前时间| 
| Date()| 返回日期时间的日期部分| 
| DateDiff()| 计算两个日期之差| 
| Date_ADD()| 高度灵活的日期运算函数| 
| Date_Format()| 返回一个格式化的日期或时间串| 
| Day()| 返回一个日期的天数部分| 
| DayOfWeek()| 对于一个日期，返回对应的星期几|
| Hour()| 返回一个时间的小时部分| 
| Minute()| 返回一个时间的分钟部分| 
| Month()| 返回一个日期的月份部分| 
| Now()| 返回当前日期和时间| 
| Second()| 返回一个时间的秒部分| 
| Time()| 返回一个日期时间的时间部分| 
| Year()| 返回一个日期的年份部分|

## <center>数值处理函数</center>

 | 函数     | 说明| 
|:--------:|:---------:|
| Abs()| 返回一个数的绝对值| 
| Cos()| 返回一个角度的余弦| 
| Exp()| 返回一个数的指数值| 
| Mod()| 返回除操作的余数| 
| Pi()| 返回圆周率| 
| Rand()| 返回一个随机数| 
| Sin()| 返回一个角度的正弦| 
| Sqrt()| 返回一个数的平方根| 
| Tan()| 返回一个角度的正切|

# 汇总数据

## 	<center>SQL聚集函数</center>

 | 函数     | 说明| 
|:--------:|:---------:|
| AVG()| 返回某列的平均值| 
| COUNT()| 返回某列的行数| 
| MAX()| 返回某列的最大值| 
| MIN()| 返回某列的最小值| 
| SUM()| 返回某列值之和|

# 分组数据

## 创建分组

	SELECT vend_id, COUNT(\*) AS num_prods FROM products GROUP BY vend_id;

上面的SELECT语句指定了两个列，vend_id包含产品供应商的ID，num_prods为计算字段。GROUP BY子句指示MySQL按vend_id排序并分组数据。

## 过滤分组

	SELECT cust_id, COUNT(\*) AS orders FROM orders GROUP BY cust_id HAVING COUNT(\*) >= 2;

WHERE与HAVING相似，WHERE过滤行，HAVING过滤分组。

## <center>分组和排序</center>

 | ORDER BY    | GROUP BY| 
|:--------:|:---------:|
| 排序产生的输出| 分组行。但输出可能不是分组的顺序| 
| 任意列都可以使用| 只可能使用选择列或表达式列，而且必须使用每个选择列表达式| 
| 不一定需要| 如果与聚集函数一起使用列，则必须使用| 

## <center>SELECT子句顺序</center>

 | 子句 | 说明 | 是否必须使用| 
|:--------:|:---------:|
| SELECT| 要返回的列或表达式| 是| 
| FROM| 从中检索数据的表| 仅在从表选择数据时使用| 
| WHERE| 行级过滤| 否| 
| GROUP BY| 分组说明| 仅在按组计算聚集时使用| 
| HAVING| 组级过滤| 否| 
| ORDER BY| 输出排序顺序| 否| 
| LIMIT| 要检索的行数| 否| 