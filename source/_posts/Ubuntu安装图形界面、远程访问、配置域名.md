---
title: 'Ubuntu安装图形界面、远程访问、配置域名'
date: 2019-11-7 21:44:09
tags:
 - linux
 - 图形界面
 - 配置域名
 - 远程访问
categories:
 - linux
---
 最近在华为云申请了服务器玩玩，因为linux的命令不算太熟悉，所以就想搞一下图形界面试试水。。。（主要因为win系统占用空间有点大，所有就投身到linux的怀抱，反正也能随便改系统，也不用花费毛爷爷）
 ## 1、安装Ubuntu图形界面
```shell
apt-get install xinit 
apt-get install gdm
apt-get install ubuntu-desktop
```
经过这一步已经可以在华为云远程到桌面系统啦。服务器重启后使用华为云的远程连接，就可以看到原生的Ubuntu桌面。
![华为云远程](https://raw.githubusercontent.com/xfx98/ms/img/hua-wei-yun-server.png)
![远程画面](https://raw.githubusercontent.com/xfx98/ms/img/ubuntu-shouye.png)
## 2、如果要进一步设置想要在win系统远程桌面
安装xrdp ，vnc4server 和 tightvncserver，向xsession中写入那堆，让用户能够远程访问，之后就能通过win的远程桌面访问了。
```shell
apt-get install xrdp
apt-get install vnc4server tightvncserver
echo "gnome-session --session=ubuntu-2d" > .xsession
/etc/init.d/xrdp restart
```
（据说可以在装了X-Window之后可以直接用[mobaxterm](https://mobaxterm.mobatek.net/download-home-edition.html)这个软件远程。目前正在尝试）
## 3、有的小伙伴买了域名，想要配置一番

 - 需要在域名注册里进行域名解析，创建公网域名，然后点击解析![域名解析](https://raw.githubusercontent.com/xfx98/ms/img/yuming.png)
 - 之后在域名提供商里创建一个A解析到公网IP，这样就能通过域名远程服务器啦（反正我是这样，说的要改dns服务器，结果我没改也可以，所有就这样了。）
## 3、其它服务器使用
 - 服务器还能搭建个人动态个人博客，比github那样的维护要好的多（不过以后要是一直用、就不能离开服务器了，总的来说github的github page要建的时候省事，动态blog用的舒服。。。就是得氪金）
 - 建个社团、团队网站作为宣传什么的。
 - 平时也可以用来学习linux等等，反正好处挺多的
