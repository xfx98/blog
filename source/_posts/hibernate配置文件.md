---
title: 'hibernate配置文件'
date: 2019-04-14 19:31:18
tags:
 - web
 - hibernate
categories:
 - hibernate
---
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--表明解析本XML文件的DTD文档位置，DTD是Document Type Definition 的缩写,即文档类型的定义,XML解析器使用DTD文档来检查XML文件的合法性。hibernate.sourceforge.net/hibernate-configuration-3.0dtd可以在Hibernate3.1.3软件包中的src\org\hibernate目录中找到此文件 -->   
<!DOCTYPE hibernate-configuration PUBLIC   
          "-//Hibernate/Hibernate Configuration DTD 3.0//EN"   
          "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<!--Hibernate配置文件的根元素,其他文件要包含在其中 -->
<hibernate-configuration>
	<!--表明以下的配置是针对session-factory配置的，SessionFactory是Hibernate中的一个类，这个类主要负责保存HIbernate的配置信息，以及对Session的操作 -->
	<session-factory>
		<!-- 1，数据库连接信息： -->
		<!--配置数据库的驱动程序，Hibernate在连接数据库时，需要用到数据库的驱动程序 -->
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver </property>
		<!--设置数据库的连接url:jdbc:mysql://localhost:3306/dbname,其中localhost表示mysql服务器名称，此处为本机， 
			dbname是数据库名 -->
		<property name="hibernate.connection.url">jdbc:mysql://localhost：3306/dbname</property>
		<!--连接数据库是用户名 -->
		<property name="hibernate.connection.username">root </property>
		<!--连接数据库是密码 -->
		<property name="hibernate.connection.password">123456 </property>
		<!--hibernate.dialect 只是Hibernate使用的数据库方言,就是要用Hibernate连接那种类型的数据库服务器。 -->
		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect </property>

		<!-- 以下信息可选 -->
		<!--jdbc.use_scrollable_resultset是否允许Hibernate用JDBC的可滚动的结果集。对分页的结果集。对分页时的设置非常有帮助 -->
		<property name="jdbc.use_scrollable_resultset">false </property>
		<!--connection.useUnicode连接数据库时是否使用Unicode编码 -->
		<property name="Connection.useUnicode">true </property>
		<!--connection.characterEncoding连接数据库时数据的传输字符集编码方式， -->
		<property name="connection.characterEncoding">utf-8 </property>

		<!--2，数据库连接池信息： -->
		<property name="hibernate.connection.pool.size">20 </property>
		<!-- 指定连接池的最大连接个数，使用连接池需要加载所有的链接池的JAR文件，JAR文件在Hibernate文件夹下的“lib\optional\c3p0”中 -->
		<property name="hibernate.c3p0.max_size">30</property>
		<property name="hibernate.c3p0.min_size">10</property>
		<property name="hibernate.c3p0.timeout">5000</property><!-- 指定连接池里连接超时时长，即最大时间 -->

		<!--3，数据库一次操作时的记录数：： -->
		<!--jdbc.fetch_size是指Hibernate每次从数据库中取出并放到JDBC的Statement中的记录条数。Fetch Size设的越大，读数据库的次数越少，速度越快，Fetch 
			Size越小，读数据库的次数越多，速度越慢 -->
		<property name="jdbc.fetch_size">50 </property>
		<!--jdbc.batch_size是指Hibernate批量插入,删除和更新时每次操作的记录数。Batch Size越大，批量操作的向数据库发送Sql的次数越少，速度就越快，同样耗用内存就越大 -->
		<property name="jdbc.batch_size">23 </property>


		<!--4，是否显示sql： -->
		<!--是否在后台显示Hibernate用到的SQL语句，开发时设置为true，便于差错，程序运行时可以在Eclipse的控制台显示Hibernate的执行Sql语句。项目部署后可以设置为false，提高运行效率 -->
		<property name="hibernate.show_sql">true </property>


		<!-- create: 先删表，再建表。 create-drop: 启动时建表，退出前删表。 update: 如果表结构不一致，就创建或更新。 
			validate: 启动时验证表结构，如果不致就抛异常。 -->
		<property name="hibernate.hbm2ddl.auto">update</property>

		<!-- 5,开启二级缓存使用： -->
		<property name="hibernate.cache.use_second_level_cache">true</property>
		<property name="hibernate.cache.provider_class">org.hibernate.cache.EhCacheProvider</property><!-- 
			指定缓存产品所需的类 -->
		<property name="hibernate.cache.use_query_cache">true</property><!-- 启用查询缓存 -->


		<!--6，指定映射文件，可映射多个映射文件 -->
		<mapping resource="org/mxg/UserInfo.hbm.xml"/>
	</session-factory>
</hibernate-configuration>  
```

