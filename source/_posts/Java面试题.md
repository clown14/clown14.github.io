---
title: Java面试题
date: 2018-03-11 16:53:08
tags: Java
categories: Java
---

找工作也有一周了，简单写下基础的面试题吧。
<!-- more -->

[题目来源](https://juejin.im/post/5a94a8ca6fb9a0635c049e67)

# 基本功

## 面向对象的特征：
封装：就是把过程和数据包围起来,对数据的访问只能通过特定的界面，
继承：一种联结类的层次模型,并且允许和鼓励类的重用,对象的一个新类可以从现有的类中派生，
多态：是指允许不同类的对象对同一消息做出响应，方法的重写,重载与动态链接构成多态性。

## final, finally, finalize 的区别：
final—修饰符（关键字）如果一个类被声明为final，不能再派生出子类，不能被继承。将变量或方法声明为final，可以保证它们在使用中不被改变。被声明为final的变量必须在声明时给定初值，不可修改。被声明为final的方法也同样只能使用，不能重载，
finally—再异常处理时提供 finally 块来执行任何清除操作，
finalize—方法名。Java 技术允许使用 finalize() 方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。

## int 和 Integer 有什么区别：
Ingeter是int的包装类，int的初值为0，Ingeter的初值为null，引入了自动装箱/拆箱机制，使得二者可以相互转换。

## 重载和重写的区别：
override（重写）方法名、参数、返回值相同，
overload（重载）参数类型、个数、顺序至少有一个不相同。

## 抽象类和接口有什么区别：
接口和抽象类都是继承树的上层，都是上层的抽象层，都不能被实例化，都能包含抽象的方法，
在抽象类中可以写非抽象的方法，接口中只能有抽象的方法，
一个类只能继承一个直接父类，这个父类可以是具体的类也可是抽象类；但是一个类可以实现多个接口。

## 说说反射的用途及实现：
Java反射机制主要提供了以下功能：在运行时构造一个类的对象；判断一个类所具有的成员变量和方法；调用一个对象的方法；生成动态代理。

## 说说自定义注解的场景及实现：
@Override注解用于告知编译器被注解元素是对父类的重写，
@Override注解不是必须的，更多的是用于防止重写错误，
@SuppressWarnings注解用于告知编译器忽略特定类型的告警信息，
类型注解的引入是为了提高对Java程序的分析，以确保增强对代码的类型检查，元注解是指被用于定义其他注解的注解。

## HTTP 请求的 GET 与 POST 方式的区别：
1. 最直观的就是语义上的区别，get用于获取数据，post用于提交数据，
2. get参数有长度限制（受限于url长度，具体的数值取决于浏览器和服务器的限制），而post无限制，
3. get能被缓存，post不能，
4. get数据在url可见，post不能，
5. post更安全，不会存在服务器日志中。

## session 与 cookie 区别：
1. session 在服务器端，cookie 在客户端（浏览器），
2. session 默认被存在在服务器的一个文件里（不是内存），
3. session 的运行依赖 session id，而 session id 是存在 cookie 中的，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效（但是可以通过其它方式实现，比如在 url 中传递 session_id），
4. 一般情况，登录信息等重要信息存储在session中，其他信息存储在cookie中，cookie目的可以跟踪会话，也可以保存用户喜好或者保存用户名密码，session用来跟踪会话。

## JDBC 流程：
1. 加载JDBC驱动程序
2. 提供JDBC连接的URL  
3. 创建数据库的连接  
4. 创建一个Statement    
5. 执行SQL语句 
6. 处理结果  
7. 关闭JDBC对象

## SpringMVC的原理：
1. 首先用户发出请求，请求到达SpringMVC的前端控制器（DispatcherServlet），
2. 前端控制器根据用户的url，请求处理器映射器(HandlerMapping)查找匹配该url的handler，并返回一个执行链(HandlerExecutionChain)，
3. 前端控制器再请求处理器适配器(HandlerAdapter)调用相应的handler进行处理并返回给前端控制器一modelAndView，  
4. 前端控制器再请求视图解析器(ViewResolver)对返回的逻辑视图进行解析，
5. 最后前端控制器将返回的视图进行渲染并把数据装入到request域，返回给用户。

## equals 与 == 的区别:
1. ==是判断两个变量或实例是不是指向同一个内存空间,equals是判断两个变量或实例所指向的内存空间的值是不是相同，
2. ==是指对内存地址进行比较,equals()是对字符串的内容进行比较，
3. ==指引用是否相同,equals()指的是值是否相同。

# 集合

## List Set Map 区别：
List,Set都是继承自Collection接口，Map则不是，
List特点：元素有放入顺序，元素可重复 ，
Set特点：元素无放入顺序，元素不可重复，
Map 跟 Set 一样对元素进行无序存储，但其某些实现类对元素进行了排序。比如，TreeMap 依据键对其中的元素进行升序排序而 LinkedHashMap 则按照每个元素的插入次序进行排序。

List 允许任意数量的空值，
Set 最多允许一个空值的出现，
Map 只允许出现一个空键但允许任意数量的空值。

Set 检索元素效率低下，删除和插入效率高，插入和删除不会引起元素位置改变。，
List 和数组类似，List可以动态增长，查找元素效率高，插入删除元素效率低，因为会引起其他元素位置改变。


## ArrayList 与 Vector 区别：
1. Vector是线程安全的，ArrayList不是线程安全的，
2. ArrayList在底层数组不够用时在原来的基础上扩展0.5倍，Vector是扩展1倍。

## HashMap Hashtable 区别：
Hashtable是线程安全的，HashMap非线程安全，基于哈希表，效率要高于HashTable，HashMap允许空键值，而HashTable不允许，
TreeMap：非线程安全，基于红黑树实现，适用于按自然顺序或自定义顺序遍历键(key)。

HashMap：适用于Map中插入、删除和定位元素，
HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的。

## HashSet 和 HashMap 区别：
<img src="/img/hash表.png" alt="图片名称" title="">