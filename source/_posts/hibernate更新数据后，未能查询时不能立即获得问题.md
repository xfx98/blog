---
title: hibernate更新数据后，未能查询时不能立即获得问题
date: 2019-05-22 21:57:39
tags:
 - hibernate
 - 缓存
 - 连接池
 - web
categories:
 - hibernate
---
在做一个项目时出现了在对一个数据删除时，相关联的数据没有及时更新问题，这是因为hibernate的缓存问题，每次读取的时候会从缓存内查询，造成数据获取的不是最新的，这里我分享我的解决方法。
首先我加入了c3p0连接池。
具体设置方法导入c3p0-0.9.5.2.jar或者其它版本的jar包以及依赖jar包
然后再hibernate配置文件加入配置

```xml
		<property name="hibernate.connection.provider_class">org.hibernate.connection.C3P0ConnectionProvider</property>
		<property name="hibernate.c3p0.max_size">20</property>
		<property name="hibernate.c3p0.min_size">5</property>
		<property name="automaticTestTable">Test</property>
		<property name="hibernate.c3p0.max_statements">100</property>
		<property name="hibernate.c3p0.idle_test_period">120</property>
		<property name="hibernate.c3p0.acquire_increment">1</property>
		<property name="c3p0.testConnectionOnCheckout">true</property>
		<property name="c3p0.idleConnectionTestPeriod">18000</property>
```

在之后就是在每次执行查询的时候删除缓存。调用session的clear方法。
