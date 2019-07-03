---
title: sql8配置出现错误
date: 2019-05-24 21:45:36
tags:
 - hibernate
 - MySQL8
categories:
 - hibernate
 - JDBC
---
使用mysql 8的时候出现 org.hibernate.exception.GenericJDBCException: Unable to open JDBC Connection for DDL execution错误
配置文件出现了问题，与mysql 5的配置文化出现了不同
首先驱动要下载 [mysql-connector-java-8.0.16.jar](https://download.csdn.net/download/qq_40827780/11200742) 点击可直接下载，官网也可以找到
然后以下两项配置项出现区别，其中 url 的 serverTimezone=Asia/Shanghai不可少，否则会报错，而5.7以下则不需要。这是设置时区，可以更改为其它时区，zheli是GMT+8上海时。
```xml
<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/blog?useUnicode=true&amp;serverTimezone=Asia/Shanghai&amp;characterEncoding=UTF-8&amp;useSSL=false</property>
<property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>
```


以下是我的hibernate配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC   
          "-//Hibernate/Hibernate Configuration DTD 3.0//EN"   
          "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<!--表明以下的配置是针对session-factory配置的，SessionFactory是Hibernate中的一个类，这个类主要负责保存HIbernate的配置信息，以及对Session的操作 -->
	<session-factory>
		<!-- 根据hibernate维护表结构方法，这里是更新 -->
		<property name="hibernate.hbm2ddl.auto">update</property>
		<!-- mysql 5请改配置为  com.mysql.jdbc.Driver -->
		<property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver </property>
		<!--设置数据库的连接jdbc:mysql://localhost:3306/dbname?useUnicode=true&amp;serverTimezone=GMT&amp;characterEncoding=UTF-8&amp;useSSL=false,其中localhost表示mysql服务器名称，此处为本机，dbname是数据库名 若serverTimezone=GMT不写则会报错-->
			
		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/blog?useUnicode=true&amp;serverTimezone=GMT&amp;characterEncoding=UTF-8&amp;useSSL=false</property>
		<!--连接数据库是用户名 -->
		<property name="hibernate.connection.username">root </property>
		<!--连接数据库是密码 -->
		<property name="hibernate.connection.password">123456 </property>
		
		<!--hibernate.dialect 只是Hibernate使用的数据库方言,就是要用Hibernate连接那种类型的数据库服务器。 -->
		
		<!--mysql 5请改配置为 org.hibernate.dialect.MySQL5Dialect </property> -->
		<property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>
		
		<!-- jdbc.use_scrollable_resultset是否允许Hibernate用JDBC的可滚动的结果集。对分页的结果集。对分页时的设置非常有帮助 
			<property name="jdbc.use_scrollable_resultset">false </property> -->

		<!--connection.characterEncoding连接数据库时数据的传输字符集编码方式， -->
		<property name="connection.characterEncoding">utf-8 </property>
		<property name="cache.use_second_level_cache">false</property>
		<!--是否在后台显示Hibernate用到的SQL语句，开发时设置为true，便于差错，程序运行时可以在Eclipse的控制台显示Hibernate的执行Sql语句。项目部署后可以设置为false，提高运行效率 -->
		<property name="hibernate.show_sql">true </property>
	</session-factory>
</hibernate-configuration>  
```
