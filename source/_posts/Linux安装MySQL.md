---
title: 'Linux安装MySQL'
date: 2021-03-01 21:50:07
tags:
 - Linux
 - MySQL 
categories:
 - IDE
---
## 下载RPM

可以前往MySQL官网进行下载，本次安装使用mysql57-community-release-el7-8.noarch.rpm。

```bash
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```
下载完成后将Yum库导入到服务器。
```bash
yum localinstall mysql57-community-release-el7-8.noarch.rpm
```
检查是否安装成功。
```bash
yum repolist enabled | grep "mysql.*-community.*"
```
## 安装MySQL

通过第一步的操作我们当前的Yum库中已经包含了MySQLServer、MySQL工作台管理工具以及ODBC驱动，现在我们就可以通过下面的命令简单的安装MySQL了。
```bash
yum install mysql-community-server
```
若出现
```
所有匹配已被模块的排除过滤过滤掉。: mysql-community-server
错误：没有任何匹配: mysql-community-server
```
执行
```bash
yum module disable mysql
yum install mysql-community-server
```
安装过程中会提示选择项，请选择y。

## 启动MySQL
执行下面的命令来启动MySQL服务。
```bash
systemctl start mysqld
```
查看MySQL的启动状态。
```bash
systemctl status mysqld
```
### 设置开机启动
执行下面的命令来配置MySQL开机启动。
```bash
systemctl enable mysqld
systemctl daemon-reload
```
### 修改MySQL root账号密码

由于MySQL安装完成之后会在/var/log/mysqld.log文件中给root生成了一个默认密码，因此我们可以通过下面的方式找到root的默认密码，然后登陆MySQL进行修改。
```bash
grep 'temporary password' /var/log/mysqld.log
```
使用上一步获取到的root默认密码来登陆MySQL。

```bash
mysql -u root -p
```
修改root账号密码
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'xxxxx';
```
密码强度问题：设置密码时密码强度可能不够，所有可以通过设置全局参数改变
首先查看密码验证相关变量
```sql
show variables like 'validate_password%';
```
验证策略是MEDIUM,就是长度，数字，大小写，特殊字符都得验证，因此出现如此所示的错误，就很正常了。我们可以修改validate_password_policy=0
```sql
set global validate_password_policy=0;
```
设置密码验证长度
```sql
set global validate_password_length=4;
```
1、修改密码的四种方式

a)shell命令行下 `mysqladmin -uroot -poldpassword password newpassword`
b)sql命令行下 `set password = password(newpassword);`
c)sql命令行下 `update user set authentication_string=password(newpassword) where user='root';`
d)sql命令行下 `alter user root@'localhost' identified by 'newpassword';`//使用随机密码登录，如果要使用数据，查看数据库等操作时mysql会推荐这种写法改变密码。
2、密码策略的三个值分别代表的含义

a)LOW(0)只检查长度。

b)MEDIUM(1)检查长度，数字，大小写，特殊字符。

c)STRONG(2)检查长度，数字，大小写，特殊字符字典文件。

### 设置远程访问
远程登录的时候需要设置权限，允许所有IP访问
```sql
use mysql;
update user set host = '%' where user = 'root';
FLUSH PRIVILEGES;
```
或创建可远程访问的用户
```sql
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' IDENTIFIED BY 'admin' WITH GRANT OPTION;
```
另外就是防火墙问题，可以先关闭防火墙，列出的两个命令可以选择使用其中一个即可，也可配置开放端口
1:查看防火状态
```bash
systemctl status firewalld
service iptables status
```

2:暂时关闭防火墙
```bash
systemctl stop firewalld
service  iptables stop
```
3:永久关闭防火墙
```bash
systemctl disable firewalld
chkconfig iptables off
```
4:重启防火墙
```bash
systemctl enable firewalld
service iptables restart  
```
#### 配置开放端口
查看防火墙已开放端口列表
```bash
firewall-cmd --list-all
```
防火墙添加端口
```bash
firewall-cmd --permanent --add-port=3306/tcp
firewall-cmd --reload
```
防火墙关闭端口
```bash
firewall-cmd --permanent --remove-port 3306/tcp
firewall-cmd --reload
```
出现连接数太多报错时
>Data source rejected establishment of connection,  message from server: "Too many connections"
	
可通过执行SQL语句设置最大连接数，此方法重启失效
```sql
show variables like "max_connections"; -- 显示最大连接数
show processlist ; --显示当前连接数
set global max_connections=1000 --设置最大连接数1000
```
也可修改my.cnf配置文件
在`[mysqld]`下面添加`max_connections=1000`
执行：`service mysql restart` 重新启动MySQL服务
win系统是my.ini的属性`max_connections`
该文件可能存在与安装目录或者`ProgramData`的MySQL目录下，我的是在`ProgramData`下的
### MySQL常用默认文件路径

下面提供一下linux的MySQL的一些常用的默认文件路径。

配置文件：`/etc/my.cnf `
日志文件：`/var/log//var/log/mysqld.log `
服务启动脚本：`/usr/lib/systemd/system/mysqld.service `
socket文件：`/var/run/mysqld/mysqld.pid`
