---
title: 'Linux 与win双系统时间不统一的解决方法'
date: 2019-12-12 18:15:07
tags:
 - linux
categories:
 - linux
---
这个问题的原因是：Win和 Linux 对硬件时间的处理方法不同，一个将硬件时间作为本地时间，而另一个则处理为UTC时间。因此在中国UTC+8时区的情况下使用 Windows 和 Linux 会有八个小时的差异。

想要将两个时间统一最好的办法就是统一对硬件时间的处理办法。
通过`timedatectl set-local-rtc `命令可以硬件处理的办法设置为本地时间或UTC时间

```bash
timedatectl set-local-rtc 1 --adjust-system-clock
timedatectl set-local-rtc 0 --adjust-system-clock
```
两个命令是设置是否将硬件时间设置为本地时间。

使用
```
sudo hwclock -w
```
更新硬件时间

`sudo hwclock`可以查看硬件时间，`timedatectl`可以查看本地时间、UTC时间、时区、是否开启时间同步等信息。如果经过设置之后时间不正确了，可以通过以下命令开启同步。

```bash
sudo systemctl restart systemd-timesyncd.service #开启时间同步服务
sudo timedatectl set-ntp true #开启同步
sudo hwclock -w #更新硬件时间
```

