---
title: Mybatis学习笔记（四）
date: 2017-02-08 15:02:39
tags: Mybatis
categories: Mybatis
---
SqlMapConfig配置文件 && 输入&输出映射
<!-- more -->

## SqlMapConfig

SqlMapConfig.xml中配置的内容和顺序如下

* properties（属性）
* settings（全局配置参数）
* ** typeAliases（类型别名) **
* typeHandlers（类型处理器）
* _objectFactory（对象工厂）_
* _plugins（插件）_
* environments（环境集合属性对象）
	* environment（环境子属性对象）
		* transactionManager（事务管理）
		* dataSource（数据源）
* ** mappers（映射器）** 
(注：粗体是重点，斜体不常用)

### properties

将数据库连接参数单独配置在db.properties中，只需要在SqlMapConfig.xml中加载db.properties的属性值。在SqlMapConfig.xml中就不需要对数据库连接参数硬编码。
将数据库连接参数只配置在db.properties中。原因：方便对参数进行统一管理，其它xml可以引用该db.properties。

	jdbc.driver=com.mysql.jdbc.Driver
	jdbc.url=jdbc:mysql://localhost:3306/mybatis001?characterEncoding=utf-8
	jdbc.username=root
	jdbc.password=

注意： MyBatis 将按照下面的顺序(优先级)来加载属性：

* 在properties元素体内定义的属性首先被读取。
* 然后会读取properties元素中resource或url加载的属性，它会覆盖已读取的同名属性。
* 最后读取parameterType传递的属性，它会覆盖已读取的同名属性。

### settings

mybatis框架在运行时可以调整一些运行参数,比如：开启二级缓存、开启延迟加载…
全局参数将会影响mybatis的运行行为。具体参考:[mybatis-settings](http://www.mybatis.org/mybatis-3/configuration.html#settings)

### typeAliases

在mapper.xml中，定义很多的statement，statement需要parameterType指定输入参数的类型、需要resultType指定输出结果的映射类型。
如果在指定类型时输入类型全路径，不方便进行开发，可以针对parameterType或resultType指定的类型定义一些别名，在mapper.xml中通过别名定义，方便开发。
* mybatis默认支持别名参考：[typeAliases](http://www.mybatis.org/mybatis-3/configuration.html#typeAliases)
* 自定义别名：


	<!-- 别名定义 -->
	<typeAliases>

	    <!-- 针对单个别名定义
	    type：类型的路径
	    alias：别名
	     -->
	    <!--  <typeAlias alias="User"  type="com.mybatis.po.User"/>  -->
	    <!-- 批量别名定义
	    指定包名，mybatis自动扫描包中的po类，自动定义别名，别名就是类名（首字母大写或小写都可以）
	    -->
	    <package name="com.mybatis.po"/>

	</typeAliases>

### typeHandlers

mybatis中通过typeHandlers完成jdbc类型和java类型的转换。
通常情况下，mybatis提供的类型处理器满足日常的需求，不需要自定义。具体参考：[typeHandlers](http://www.mybatis.org/mybatis-3/configuration.html#typeHandlers)

### mappers

* 通过resource加载单个配置文件
		<!--通过resource方法一次加载一个映射文件 -->
		<mapper resource="com/mybatis/po/User.xml"/>
* 通过mapper接口加载单个mapper
		<!-- 通过mapper接口加载单个 映射文件
	        遵循一些规范：需要将mapper接口类名和mapper.xml映射文件名称保持一致，且在一个目录中
	        上边规范的前提是：使用的是mapper代理方法-->
		<mapper class="com.mybatis.mapper.UserMapper"/>
* package批量加载Mapper文件
		<!-- 批量加载mapper
				指定mapper接口的包名，mybatis自动扫描包下边所有mapper接口进行加载
				遵循一些规范：需要将mapper接口类名和mapper.xml映射文件名称保持一致，且在一个目录 中
				上边规范的前提是：使用的是mapper代理方法
				 -->
		<package name="com.mybatis.mapper"/>

## 输入映射

通过parameterType指定输入参数类型，参数类型可以是基本类型，pojo，map类型

### 传递pojo的包装对象

* 定义包装类型pojo
		public class UserQueryVo {
		    //在这里包装所需要的查询条件

		    //用户查询条件
		    private UserCustom userCustom;

		    public UserCustom getUserCustom() {
		        return userCustom;
		    }

		    public void setUserCustom(UserCustom userCustom) {
		        this.userCustom = userCustom;
		    }

		    //可以包装其它的查询条件，订单、商品
		    //....
		}
其中，UserCustom类继承User
		public class UserCustom extends User{
		}

* mapper.xml
		<select id="FindUserList" parameterType="QueryVo" resultType="user">
			<!-- 取属性名称使用“.”的方式来取 -->
			select * from user where id=#{user.userId}
		</select>


* mapper.java
		//用户信合查询息综
		public List<User> findUserList(UserQueryVo vo) throws Exception;

* 测试代码
		public void testFindUserList() throws Exception {

			SqlSession sqlSession = sqlSessionFactory.openSession();

			//创建UserMapper对象，mybatis自动生成mapper代理对象
			UserMapper userMapper  sqlSession.getMapper(UserMapper.class);

			//创建包装对象，设置查询条件
			UserQueryVo userQueryVo = new UserQueryVo();
			UserCustom userCustom = new UserCustom();
			//由于这里使用动态sql，如果不设置某个值，条件不会拼接在sql中
			userCustom.setAge("1");
			userCustom.setuserName("张三");
			userQueryVo.setUserCustom(userCustom);
			//调用userMapper的方法

			List<UserCustom> list = userMapper.findUserList(userQueryVo);

			System.out.println(list);
		}

## 输出映射
输出映射有两种方式：resultType，resultMap

### resultType
使用resultType进行输出映射，只有查询出来的列名和pojo中的属性名一致，该列才可以映射成功。
如果查询出来的列名和pojo中的属性名全部不一致，没有创建pojo对象。
只要查询出来的列名和pojo中的属性有一个一致，就会创建pojo对象。

* mapper.xml
		<select id="findUserCount" parameterType="com.iot.mybatis.po.UserQueryVo" resultType="int">
	        SELECT count(*) FROM user WHERE user.sex=#{userCustom.sex} AND user.username LIKE '%${userCustom.username}%'
	    </select>

* mapper.java
		public void testFindUserCount() throws Exception {

			SqlSession sqlSession = sqlSessionFactory.openSession();

			//创建UserMapper对象，mybatis自动生成mapper代理对象
			UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

			//创建包装对象，设置查询条件
			UserQueryVo userQueryVo = new UserQueryVo();
			UserCustom userCustom = new UserCustom();
			//由于这里使用动态sql，如果不设置某个值，条件不会拼接在sql中
			userCustom.setuserName("小");
			userQueryVo.setUserCustom(userCustom);
			//调用userMapper的方法

			int count = userMapper.findUserCount(userQueryVo);

			System.out.println(count);
		}
查询出来的结果集只有一行且一列，可以使用简单类型进行输出映射。

### 输出pojo

不管是输出的pojo单个对象还是一个列表（list中包括pojo），在mapper.xml中resultType指定的类型是一样的。

* 输出pojo对象
		public User findUserById(int id) throws Exception;

* 输出pojo对象list
		public List<User> findUserByName(String name) throws Exception;
生成的动态代理对象中是根据mapper方法的返回值类型确定是调用selectOne(返回单个对象调用)还是selectList （返回集合对象调用 ）。

### resultMap

mybatis中使用resultMap完成高级输出结果映射。

* 定义reusltMap
		<!-- 定义resultMap
		将SELECT id id_,username username_ FROM USER 和User类中的属性作一个映射关系

		type：resultMap最终映射的java对象类型,可以使用别名
		id：对resultMap的唯一标识
		 -->
		 <resultMap type="user"  id="userResultMap">
			<!-- 
				id:为主键列
				column:结果集中的主键列的列名
				property：对应user对象中保存主键属性。
				最终resultMap对column和property作一个映射关系 （对应关系）
			 -->
			<id column="id" property="userID"/>
			<!-- 普通列
				result：对普通名映射定义
			 	column：查询出来的列名
			 	property：type指定的pojo类型中的属性名
			 	最终resultMap对column和property作一个映射关系 （对应关系）
		 	 -->
			<result column="uname" property="userName"/>
			<result column="age" property="userAge"/>
		</resultMap>

* 使用resultMap作为statement的输出映射类型
		<!-- 使用resultMap进行输出映射
	        resultMap：指定定义的resultMap的id，如果这个resultMap在其它的mapper文件，前边需要加namespace
	        -->
	    <select id="getUserResultMap" resultMap="userResultMap">
			select userID, userName uname, userAge age from mybatis.user
		</select>

* mapper.java
		public List<User> getUserResultMap() throws Exception;

* 测试代码
		public void testFindUserByIdResultMap() throws Exception {

			SqlSession sqlSession = sqlSessionFactory.openSession();

			//创建UserMapper对象，mybatis自动生成mapper代理对象
			UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

			//调用userMapper的方法

			List<User> list = userMapper.getUserResultMap();

			System.out.println(list);


		}