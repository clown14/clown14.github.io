---
title: MBG使用手册
date: 2017-06-11 20:09:20
tags: Mybatis
categories: Mybatis
---
Mybaits Generator 使用手册
<!-- more -->

## Generator工具简介

自动生成底层模型类、Dao接口类甚至Mapping映射文件。

## 数据库建表

<img src="/img/数据库1.png" alt="图片名称" title="">
<img src="/img/数据库2.png" alt="图片名称" title="">

## 下载对应jar包

<img src="/img/jar包.png" alt="图片名称" title="">

## generatorConfig.xml配置文件书写

	<?xml version="1.0" encoding="UTF-8"?>  
	<!DOCTYPE generatorConfiguration  
	  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"  
	  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">  
	<generatorConfiguration>  
	<!-- 数据库驱动-->  
	    <classPathEntry  location="mysql-connector-java-3.1.12-bin.jar"/>  
	    <context id="DB2Tables"  targetRuntime="MyBatis3">  
	        <commentGenerator>  
	            <property name="suppressDate" value="true"/>  
	            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->  
	            <property name="suppressAllComments" value="true"/>  
	        </commentGenerator>  
	        <!--数据库链接URL，用户名、密码 -->  
	        <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://localhost/test" userId="root" password="">  
	        </jdbcConnection>  
	        <javaTypeResolver>  
	            <property name="forceBigDecimals" value="false"/>  
	        </javaTypeResolver>  
	        <!-- 生成模型的包名和位置-->  
	        <javaModelGenerator targetPackage="model" targetProject="src">  
	            <property name="enableSubPackages" value="true"/>  
	            <property name="trimStrings" value="true"/>  
	        </javaModelGenerator>  
	        <!-- 生成映射文件的包名和位置-->  
	        <sqlMapGenerator targetPackage="mapping" targetProject="src">  
	            <property name="enableSubPackages" value="true"/>  
	        </sqlMapGenerator>  
	        <!-- 生成DAO的包名和位置-->  
	        <javaClientGenerator type="XMLMAPPER" targetPackage="dao" targetProject="src">  
	            <property name="enableSubPackages" value="true"/>  
	        </javaClientGenerator>  
	        <!-- 要生成哪些表-->  
	        <table tableName="test" domainObjectName="Test" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>  
	    </context>  
	</generatorConfiguration>  

## 运行

进入命令行界面，mybatis-generator-core-1.3.2\lib目录下运行

	Java -jar mybatis-generator-core-1.3.2.jar -configfile generatorConfig.xml -overwrite

<img src="/img/运行成功.png" alt="图片名称" title="">

## 生成文件

<img src="/img/生成文件.png" alt="图片名称" title="">

Test.java 实体类：
<img src="/img/生成1.png" alt="图片名称" title="">

TestMapper.java 映射接口：
<img src="/img/生成2.png" alt="图片名称" title="">

TestMapper.xml 具体sql语句：
<img src="/img/生成3.png" alt="图片名称" title="">