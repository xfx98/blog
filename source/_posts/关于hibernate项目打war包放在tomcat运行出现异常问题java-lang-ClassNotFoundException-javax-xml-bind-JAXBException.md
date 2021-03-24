---
title: >-
  关于hibernate项目打war包放在tomcat运行出现异常问题
date: 2019-05-22 21:59:57
tags:
 - hibernate
 - web
 - JDK
categories:
 - Java
---
今天在将开发好的项目放到tomcat 下行测试结果出现了
> java.lang.ClassNotFoundException: javax.xml.bind.JAXBException

异常搜索发现是由于jdk 自8开始出现了模块化，javaSE中不再包含javaEE的jar包，而我使用的是jdk11出现了错误。
可以通过添加缺失的jar包，以及依赖jar包解决解决
[javax.activation-1.2.0.jar ](http://search.maven.org/remotecontent?filepath=com/sun/activation/javax.activation/1.2.0/javax.activation-1.2.0.jar)
[jaxb-api-2.3.0.jar ](http://search.maven.org/remotecontent?filepath=javax/xml/bind/jaxb-api/2.3.0/jaxb-api-2.3.0.jar)
[jaxb-core-2.3.0.jar ](http://search.maven.org/remotecontent?filepath=com/sun/xml/bind/jaxb-core/2.3.0/jaxb-core-2.3.0.jar)
[jaxb-impl-2.3.0.jar ](http://search.maven.org/remotecontent?filepath=com/sun/xml/bind/jaxb-impl/2.3.0/jaxb-impl-2.3.0.jar)

同样还可以通过修改tomcat的java运行版本解决
![tomcat java运行环境配置](https://raw.githubusercontent.com/xfx98/ms/img/tomcat-setting.png)

将jvm配置为1.8便可成功。这里在图形界面更改，也可以在配置文件中修改。