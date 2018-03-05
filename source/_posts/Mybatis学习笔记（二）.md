---
title: Mybatis学习笔记（二）
date: 2017-02-05 14:36:12
tags: Mybatis
categories: Mybatis
---
Mybatis入门程序
<!-- more -->

## 工程搭建

第一步：创建一个java工程。（数据库自行创建）
第二步：导入jar包。包括mybatis的jar包、mybatis依赖的jar包、mysql的数据库驱动。
第三步：创建一个log4j.properties。方便查看sql语句用的。
第四步：创建一个SqlMapConfig.xml。配置数据库连接池。
第五步：创建Mapper映射文件。
第六步：编码。

### log4j.properties

	# Global logging configuration
	log4j.rootLogger=DEBUG, stdout
	# Console output...
	log4j.appender.stdout=org.apache.log4j.ConsoleAppender
	log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
	log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

### SqlMapConfig.xml

	<configuration>
	    <!-- 和spring整合后 environments配置将废除 -->
	    <environments default="development">
	        <environment id="development">
	        <transactionManager type="JDBC"/>
	            <dataSource type="POOLED">
	            <property name="driver" value="com.mysql.jdbc.Driver"/>
	            <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis" />
	            <property name="username" value="root"/>
	            <property name="password" value=""/>
	            </dataSource>
	        </environment>
	    </environments>  

	    <mappers>
	        <mapper resource="com/mybatis/po/User.xml"/>
	    </mappers>
	</configuration>

### Mapper映射文件

习惯上mapper映射文件的名称和数据库的表名一致。user.xml

	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper
	        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<!-- namespace 命名空间，作用就是对sql进行分类化管理,理解为sql隔离
	 注意：使用mapper代理方法开发，namespace有特殊重要的作用
	 -->
	<mapper namespace="test">
	    <!-- 在映射文件中配置很多sql语句 -->
	    <!--需求:通过id查询用户表的记录 -->
	    <!-- 通过select执行数据库查询
	     id:标识映射文件中的sql，称为statement的id
	     将sql语句封装到mappedStatement对象中，所以将id称为statement的id
	     parameterType:指定输入参数的类型
	     #{}标示一个占位符,
	     #{value}其中value表示接收输入参数的名称，如果输入参数是简单类型，那么#{}中的值可以任意。

	     resultType：指定sql输出结果的映射的java对象类型，select指定resultType表示将单条记录映射成java对象
	     -->

	    <select id="selectUserByID"    parameterType="int"      resultType="User">
	        select * from user where userID = #{userId}
	    </select>

	    <!-- 根据用户名称模糊查询用户信息，可能返回多条
		resultType：指定就是单条记录所映射的java对象类型
		${}:表示拼接sql串，将接收到参数的内容不加任何修饰拼接在sql中。
		使用${}拼接sql，引起 sql注入
		${value}：接收输入参数的内容，如果传入类型是简单类型，${}中只能使用value
		 -->

	    <select id="getUserByName" parameterType="string" resultType="User">
		    select * from user where userName LIKE #{name}
	    </select>

	    <!-- 添加用户
        parameterType：指定输入 参数类型是pojo（包括 用户信息）
        #{}中指定pojo的属性名，接收到pojo对象的属性值，mybatis通过OGNL获取对象的属性值
        -->

	    <insert id="insertUser"  parameterType="User" >
		    <!--
	         将插入数据的主键返回，返回到user对象中

	         SELECT LAST_INSERT_ID()：得到刚insert进去记录的主键值，只适用与自增主键

	         keyProperty：将查询到主键值设置到parameterType指定的对象的哪个属性
	         order：SELECT LAST_INSERT_ID()执行顺序，相对于insert语句来说它的执行顺序
	         resultType：指定SELECT LAST_INSERT_ID()的结果类型
	          -->
	        <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
	          SELECT LAST_INSERT_ID()
	        </selectKey>

	    	INSERT INTO `user`(userName,userAge,userAddress) 
			VALUES(#{userName},#{userAge},#{userAddress})
    	</insert>

	    <!-- 删除用户 根据id删除用户，需要输入 id值-->
	    <delete id="deleteUser" parameterType="java.lang.Integer">
	        delete from user where id=#{userId}
	    </delete>

	    <!-- 根据id更新用户
	    分析：
	    需要传入用户的id
	    需要传入用户的更新信息
	    parameterType指定user对象，包括 id和更新信息，注意：id必须存在
	    #{id}：从输入 user对象中获取id属性值
	     -->

	    <update id="updateUser" parameterType="User">
			UPDATE USER
			SET userName = #{username},
			 userAge = #{age},
			 userAddress = #{address},
			WHERE
			userID = #{id}
		</update>
	</mapper>

### po类

User.java


	public class User {
		private int userid;
	    private String userName;
	    private int userAge;
	    private String userAddress;
	 	/**
		 * @return the userid
		 */
		public int getUserid() {
			return userid;
		}
		/**
		 * @param userid the userid to set
		 */
		public void setUserid(int userid) {
			this.userid = userid;
		}
		/**
		 * @return the userName
		 */
		public String getUserName() {
			return userName;
		}

		/**
		 * @param userName the userName to set
		 */
		public void setUserName(String userName) {
			this.userName = userName;
		}
		/**
		 * @return the userAge
		 */
		public int getUserAge() {
			return userAge;
		}
		/**
		 * @param userAge the userAge to set
		 */
		public void setUserAge(int userAge) {
			this.userAge = userAge;
		}
		/**
		 * @return the userAddress
		 */
		public String getUserAddress() {
			return userAddress;
		}
		/**
		 * @param userAddress the userAddress to set
		 */
		public void setUserAddress(String userAddress) {
			this.userAddress = userAddress;
		}

		/* (non-Javadoc)
		 * @see java.lang.Object#toString()
		 */
		@Override
		public String toString() {
			return "User [userid=" + userid + ", userName=" + userName + ", userAge=" + userAge + ", userAddress="
					+ userAddress + "]";
		}
	}

### 测试代码

	public class Test1 {
		static {
			try {
				//把配置文件读取到流中
				reader = Resources.getResourceAsReader("SqlMapConfig.xml");
				// InputStream=Resources.getResourceAsStream("SqlMapConfig.xml");
				//创建SqlSessionFactory对象
				sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
			} catch (Exception e) {
				e.printStackTrace();
			}
		}

		public static SqlSessionFactory getSession() {
			return sqlSessionFactory;
		}

		public static void main(String[] args) {
			//创建sqlsession对象
			SqlSession sqlSession = sqlSessionFactory.openSession();
			try {
				//查询用户
				//查询一条记录使用selectOne方法
				//第一个参数：statementID
				//第二个参数：sql语句用到的参数
				User user =(User) sqlSession.selectOne("test.selectUserByID",2);
				System.out.println(user);	

			/*  //添加用户			
				User user = new User(); user.setUserName("张三");
				user.setUserAge(26); 
				user.setUserAddress("天津");
				sqlSession.insert("test.insertUser", user);

				//
				//删除用户
				sqlSession.delete("test.deleteUser", 2);

				//更新信息
				User user = sqlSession.selectOne("test.getUserByID", 2);
				System.out.println(user); 
				user.setUserName("武松");
				user.setUserAge(20); 
				user.setUserAddress("清河县");
				sqlSession.update("test.updateUser", user);*/

			} finally {
				//关闭sqlsession
				sqlSession.commit();
				sqlSession.close();
			}
		}
	}

### 测试结果
	User [userid=3, userName=sss, userAge=0, userAddress=null]

## 总结

### parameterType
指定传递参数类型，前台传入参数和后台获取参数类型必须匹配

### resultType
指定返回值类型，执行sql结果映射Java对象

### #和$
#{}表示一个占位符，#{}接受传入参数，类型可以是基本类型，pojo，map
如果接受的是基本类型的值，那么#{}括号中可以任意或者value
如果#{}获取是的pojo，mybatis通过ognl获取参数值。Ognl就是对象导航语言 属性.属性.属性的方式获取值。
如果传递是map值，#{}中需要的是map的key

${}表示拼接sql,会引入sql注入，不建议使用
${}接受输入参数可以是pojo，基本类型，map
${}如果接受的是基本类型，只能是value
${}接受pojo类型的参数，通过ognl对象导航进行获取

### selectOne和selectList
selectOne是查询单个对象记录，selectList查询多条记录
不能使用selectOne查询selectList的记录

### mybatis和hibernate区别
Hibernate:hibernate是一个标准的ORM框架，不需要写sql语句，维护关系比叫复杂，sql语句自动生成，对sql语句优化，修改比较困难。
Hibernate的优缺点：
优点：面向对象开发，不需要自己写sql语句。如果进行数据库迁移不需要修改sql语句，只需要修改一下方言。
缺点：hibernate维护数据表关系比较复杂。完全是有hibernate来管理数据表的关系，对于我们来说完全是透明的，不易维护。
Hibernate自动生成sql语句，生成sql语句比较复杂，比较难挑错。
Hibernate由于是面向对象开发，不能开发比较复杂的业务。
应用场景：
适合需求变化较少的项目，比如ERP，CRM等等
Mybatis框架对jdbc框架进行封装，屏蔽了jdbc的缺点，开发简单。
Mybatis只需要程序员关注sql本身，不需要过多的关注业务。对sql的优化，修改比较容易
适应场景：
适合需求变化多端的项目，比如：互联网项目