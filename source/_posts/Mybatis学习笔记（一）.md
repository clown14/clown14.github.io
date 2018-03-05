---
title: Mybatis学习笔记（一）
date: 2017-02-01 20:33:13
tags: Mybatis
categories: Mybatis
---
jdbc问题总结 && Mybatis简介
<!-- more -->

## JDBC

### jdbc实现步骤：

* 加载数据库驱动
* 创建并获取数据源连接
* 创建jdbc的statement对象
* 设置sql语句
* 设置statement中的sql参数(statement的参数注入)
* statement执行sql语句
* 对sql执行结果进行解析处理
* 释放资源(statement,connection,resultSet)

### 存在的问题
* 数据库连接，使用时就创建，不使用立即释放，对数据库进行频繁连接开启和关闭，造成数据库资源浪费，影响数据库性能。
* 我们使用JDBC进行操作数据库时，SQL语句基本都散落在各个JAVA类中，这样有三个不足之处：
第一，可读性很差，不利于维护以及做性能调优。
第二，改动Java代码需要重新编译、打包部署。
第三，不利于取出SQL在数据库客户端执行（取出后还得删掉中间的Java代码，编写好的SQL语句写好后还得通过＋号在Java进行拼凑）。 
* 很多情况下，我们都可以通过在SQL语句中设置占位符来达到使用传入参数的目的，这种方式本身就有一定局限性，它是按照一定顺序传入参数的，要与占位符一一匹配。
* 从resutSet中遍历结果集数据时，存在硬编码，将获取表的字段进行硬编码，不利于系统维护。
* 存在sql重复问题，不利于维护。

### 代码实现

```java
public class JdbcTest {
    public static void main(String[] args) {
        //数据库连接
        Connection connection = null;
        //预编译的Statement，使用预编译的Statement提高数据库性能
        PreparedStatement preparedStatement = null;
        //结果集
        ResultSet resultSet = null;

        try {
            //加载数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            //通过驱动管理类获取数据库链接
            connection =  DriverManager.getConnection("jdbc:mysql://localhost:3306/mybatis001?characterEncoding=utf-8", "root", "mysql");
            //定义sql语句 ?表示占位符
            String sql = "select * from user where username = ?";
            //获取预处理statement
            preparedStatement = connection.prepareStatement(sql);
            //设置参数，第一个参数为sql语句中参数的序号（从1开始），第二个参数为设置的参数值
            preparedStatement.setString(1, "王五");
            //向数据库发出sql执行查询，查询出结果集
            resultSet =  preparedStatement.executeQuery();
            //遍历查询结果集
            while(resultSet.next()){
                System.out.println(resultSet.getString("id")+"  "+resultSet.getString("username"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            //释放资源
            if(resultSet!=null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
            if(preparedStatement!=null){
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
            if(connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }

        }

    }

}

   
```

## Mybatis

### 什么是Mybatis

* mybatis是一个持久层的框架，是apache下的顶级项目。
* mybatis托管到goolecode下，再后来托管到github下(https://github.com/mybatis/mybatis-3/releases)。
* mybatis让程序将主要精力放在sql上，通过mybatis提供的映射方式，自由灵活生成（半自动化，大部分需要程序员编写sql）满足需要sql语句。
* mybatis通过xml或注解的方式将要执行的各种statement（statement、preparedStatemnt、CallableStatement）配置起来，并通过java对象和statement中的sql进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射成java对象并返回。

### 框架图

<img src="/img/mybatis_框架图.png" alt="图片名称" title="">

* mybatis配置
SqlMapConfig.xml，此文件作为mybatis的全局配置文件，配置了mybatis的运行环境等信息。
mapper.xml文件即sql映射文件，文件中配置了操作数据库的sql语句。此文件需要在SqlMapConfig.xml中加载。
* 通过mybatis环境等配置信息构造SqlSessionFactory即会话工厂
* 由会话工厂创建sqlSession即会话，操作数据库需要通过sqlSession进行。
* mybatis底层自定义了Executor执行器接口操作数据库，Executor接口有两个实现，一个是基本执行器、一个是缓存执行器。
* Mapped Statement也是mybatis一个底层封装对象，它包装了mybatis配置信息及sql映射信息等。mapper.xml文件中一个sql对应一个Mapped Statement对象，sql的id即是Mapped statement的id。
* Mapped Statement对sql执行输入参数进行定义，包括HashMap、基本类型、pojo，Executor通过Mapped Statement在执行sql前将输入的java对象映射至sql中，输入参数映射就是jdbc编程中对preparedStatement设置参数。
* Mapped Statement对sql执行输出结果进行定义，包括HashMap、基本类型、pojo，Executor通过Mapped Statement在执行sql后将输出结果映射至java对象中，输出结果映射过程相当于jdbc编程中对结果的解析处理过程。