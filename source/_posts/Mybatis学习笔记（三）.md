---
title: Mybatis学习笔记（三）
date: 2017-02-06 15:49:23
tags: Mybatis
categories: Mybatis
---
Mybatis的Dao开发
<!-- more -->
使用dao开发有两种开发方式，一种是使用普通的接口和接口实现类来进行开发
另一种开发方式是使用mapper代理的开发的方式(直接使用接口)

## sqlSession的使用范围

### sessionFactoryBuidler
通过SqlSessionFactoryBuilder创建会话工厂SqlSessionFactory将SqlSessionFactoryBuilder当成一个工具类使用即可，不需要使用单例管理SqlSessionFactoryBuilder。在需要创建SqlSessionFactory时候，只需要new一次SqlSessionFactoryBuilder即可。

### sqlSessionFactory
sqlSessionFactory生产sqlSession回话，使用单列模式进行创建sqlSessionFactory(工厂一旦创建，只创建一个实例)，将mybatis和spring进行整合后使用单列模式进行管理。

### SqlSession
sqlSession是面向用户的一个回话接口
sqlSession提供很多操作数据库的方法：selectOne(查询单个对象)，selectList(返回单个或者多个对象)
sqlSession是线程不安全的，在sqlSession实现类中除了有查询数据库中的方法外还有数据域属性。
sqlSession最好是在方法内部使用。

## 传统的dao的开发方式
程序员需要编写dao接口和dao接口实现类
在dao实现类里面进行注入sqlSessionFactory,然后通过sqlSessionFactory创建sqlSession

### Dao接口

	public interface UserDao {
		public User selectUserByID(int id) throws Exception;
		
		public void insertUser(User user) throws Exception;
		
		public List<User> getUserByName(String name) throws Exception;
		
		public void deleteUser(int id) throws Exception;
	}

### Dao的实现类

	public class UserDaoImpl implements UserDao {

		//给dao层注入SessionFactory对象
		//使用构造方法进行注入
		public SqlSessionFactory sessionFactory;	
		public UserDaoImpl(SqlSessionFactory sessionFactory){
			this.sessionFactory=sessionFactory;
		}
		
		@Override
		public User selectUserByID(int id) throws Exception {
			// TODO Auto-generated method stub
			//获取sqlSession
				SqlSession sqlSession = sessionFactory.openSession();
				//根据ID进行查询
				User user = sqlSession.selectOne("test.selectUserByID", 3);
				return user;
		}

		@Override
		public void insertUser(User user) throws Exception{
			// TODO Auto-generated method stub
			SqlSession sqlSession = sessionFactory.openSession();
			//执行插入操作
			sqlSession.insert("test.insertUser", user);
		}

		@Override
		public List<User> getUserByName(String name) throws Exception{
			// TODO Auto-generated method stub
			SqlSession sqlSession = sessionFactory.openSession();
	        List<User> list = sqlSession.selectList("test.getUserByName", name);
			return list;
		}

		@Override
		public void deleteUser(int id) throws Exception {
			// TODO Auto-generated method stub
			SqlSession sqlSession = sessionFactory.openSession();
	        //执行删除操作
	        sqlSession.delete("test.deleteUser", id);
		}

	}

### 测试代码
	public class UserDaoImplTest {

		private SqlSessionFactory sqlSessionFactory;

		// 此方法是在执行testFindUserById之前执行
		@Before
		public void setUp() throws Exception {
			// 创建sqlSessionFactory

			// mybatis配置文件
			String resource = "SqlMapConfig.xml";
			// 得到配置文件流
			InputStream inputStream = Resources.getResourceAsStream(resource);

			// 创建会话工厂，传入mybatis的配置文件信息
			sqlSessionFactory = new SqlSessionFactoryBuilder()
					.build(inputStream);
		}

		@Test
		public void testFindUserById() throws Exception {
			// 创建UserDao的对象
			UserDao userDao = new UserDaoImpl(sqlSessionFactory);

			// 调用UserDao的方法
			User user = userDao.selectUserByID(1);

			System.out.println(user);
		}

	}

### 原始dao总结
* dao接口中存在着大量的模版方法，能不能提取出来，大量减轻程序员的压力
* 实现类进行操作数据库时对statement的ID进行硬编码了。
* sqlSession直接进行设置参数，由于sqlsession使用的泛型，设置参数类型不会报错，这样不利于开发。

## 代理Mapper开发模式
程序员还需要开发mapper.xml文件。
程序员编写mapper接口需要遵循一些开发规范。Mybatis会自动生成mapper接口实现类代理类。

### 开发规范

* namespace的地址和mapper.xml的地址相同
	<!--
	 namespace 命名空间，作用就是对sql进行分类化管理,理解为sql隔离
	 注意：使用mapper代理方法开发，namespace有特殊重要的作用,namespace等于mapper接口地址
	 -->
	<mapper namespace="com.mybatis.mapper.UserMapper">

* Dao接口中的方法必须和statement的Id必须一致
* Mapper接口中的方法输入参数必须和对应的Mapper.xml中的parameterType中的参数匹配
* Mapper接口返回值类型必须和对应的mapper.xml中resultType输出类型匹配。


		User getUserByName(String name);
----
		<select id="getUserByName" parameterType="string" resultType="User">
		  SELECT * FROM  user  WHERE userName LIKE #{name}
		</select>


### mapper.xml

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

	<mapper namespace="com.mybatis.mapper.UserMapper">

	    <select id="selectUserByID"    parameterType="int"      resultType="User">
	        select * from user where userID = #{userId}
	    </select>
	    
	    <insert id="addUser"  parameterType="User" >
	    	INSERT INTO `user`(userName,userAge,userAddress) 
			VALUES(#{userName},#{userAge},#{userAddress})
	    </insert>
	    
		<!-- resultType是返回结果中一条记录的类型 -->
		<select id="getUserByName" parameterType="string" resultType="User">
			SELECT * from user where userName LIKE #{name}
		</select>
		
		<update id="updateUser" parameterType="User">
			UPDATE USER
			SET userName = #{username},
			 userAge = #{age},
			 userAddress = #{address},
			WHERE
			userID = #{id}
		</update>

		<delete id="deleteUser" parameterType="int">
       		delete from user where id=#{userId}
    	</delete>
		
	</mapper>

### UserMapper.java 

	public interface UserMapper {
		User selectUserByID(int id);
		
		void insertUser(User user);
		
		User getUserByName(String name);
		
		void updateUser (User user);

		void deleteUser(int id);
	}

### Test.java
	public class UserDaoImplTest {

		private SqlSessionFactory sqlSessionFactory;

		// 此方法是在执行testFindUserById之前执行
		@Before
		public void setUp() throws Exception {
			// 创建sqlSessionFactory

			// mybatis配置文件
			String resource = "SqlMapConfig.xml";
			// 得到配置文件流
			InputStream inputStream = Resources.getResourceAsStream(resource);

			// 创建会话工厂，传入mybatis的配置文件信息
			sqlSessionFactory = new SqlSessionFactoryBuilder()
					.build(inputStream);
		}

		@Test
		public void testFindUserById() throws Exception {
			SqlSession sqlSession=sqlSessionFactory.openSession();  

	        //创建UserMapper代理对象  
	        UserMapper userMapper=sqlSession.getMapper(UserMapper.class);  

	        //调用userMapper的方法  
	        User user=userMapper.selectUserById(1);  

	        System.out.println(user.getUsername());
		}

	}

### 总结
Mapper动态代理方式底层根据返回值类型进行判断调用那个方法：
如果是返回单个对象就调用selectOne
如果是返回集合就调用selectList

Mapper接口中的参数只能是一个是否影响系统开发系统维护：
注意：dao层代码是被业务层公用的，即使是dao层代码参数只能是一个
业务层包装成不同类型pojo都可以满足不同业务需求。
持久层方法参数可以是map，基本类型，自定义对象，包装类型.