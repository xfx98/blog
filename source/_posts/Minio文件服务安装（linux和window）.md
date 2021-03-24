---
title: 'Minio文件服务安装（Linux和window）'
date: 2021-02-21 21:50:07
tags:
 - Linux
 - Minio
categories:
 - IDE
---

项目地址[https://github.com/minio/minio](https://github.com/minio/minio)

## Linux安装
下载安装介质
```bash
cd /usr/local
wget https://dl.minio.io/server/minio/release/linux-amd64/minio
```
安装（下文中ip指代的是Minio所在的服务器地址）
```bash
chmod +x minio
export MINIO_ACCESS_KEY=minioadmin
export MINIO_SECRET_KEY=minioadmin
./minio server /minio/data
```
> Endpoint: http://ip:9000 http://127.0.0.1:9000
AccessKey: minioadmin
SecretKey: minioadmin
Browser Access: http://ip:9000

开机自启动

编写脚本 minioSysInit.sh，脚本内容如下。
```bash
# !/bin/bash
/usr/local/minio server /minio/data
```
更改文件权限。
```bash
  chmod +x /usr/local/minioSysInit.sh
```
编辑 rc.local文件，加入脚本。
```bash
  vim /etc/rc.d/rc.local
  /usr/local/minioSysInit.sh
```
更新rc.local权限

```bash
  chmod +x /etc/rc.d/rc.local
```
  接口开放相关查看[MySQL安装的开放远程端口，关闭防火墙部分](https://blog.csdn.net/qq_40827780/article/details/114258020?spm=1001.2014.3001.5501)

## Windows安装

[https://dl.min.io/server/minio/release/windows-amd64/minio.exe](https://dl.min.io/server/minio/release/windows-amd64/minio.exe)
下载就可使用下面命令前台使用，要在后台运行，就要配置成服务进行使用
```sh
./minio server ./data
```
需要用到`winsw`
下载：

[https://github.com/winsw/winsw/releases](https://github.com/winsw/winsw/releases)
将下载的`WinSW.NET***.exe`复制到自定义的目录，并重命名为自己想命名的服务名称`minio-server.exe`
同目录下创建`minio-server.xml`。特别注意，`xml`和`exe`必须同名
配置`minio-server.xml`文件
使用`./minio-server.exe install`安装服务
安装完后，去服务中启动服务。启动成功就可以正常使用`minio`啦
使用`./minio-server.exe uninstall`卸载服务
当然此操作也适用于其它`exe`程序只需更改配置即可
配置
```xml
<service>
	<!-- 服务名字 -->
    <id>minio-server</id>
    <name>minio-server</name>
    <description>minio文件存储服务器</description>
    <!-- 可设置环境变量 -->
    <env name="HOME" value="%BASE%"/>
    <executable>%BASE%\minio.exe</executable>
    <!-- 文件储存地址 -->
    <arguments>server "%BASE%\data"</arguments>
    <!-- <logmode>rotate</logmode> -->
    <logpath>%BASE%\logs</logpath>
    <!-- 日志配置 -->
    <log mode="roll-by-size-time">
      <sizeThreshold>10240</sizeThreshold>
      <pattern>yyyyMMdd</pattern>
      <autoRollAtTime>00:00:00</autoRollAtTime>
      <zipOlderThanNumDays>5</zipOlderThanNumDays>
      <zipDateFormat>yyyyMMdd</zipDateFormat>
    </log>
</service>
```
