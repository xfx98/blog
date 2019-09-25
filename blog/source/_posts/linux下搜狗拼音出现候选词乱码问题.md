---
title: 'linux下搜狗拼音出现候选词乱码问题'
date: 2019-9-24 20:50:07
tags:
 - linux
categories:
 - linux
---
搜狗输入法自我感觉非常好用，但是总是会在开机的时候随机出现乱码，目前找到两个解决该问题的方法。
## 1、删除搜狗拼音的配置文件，然后重新登录
命令
 `cd ~/.config `
`rm -rf SogouPY* sogou* `
或者
`rm -rf ~/.config/SogouPY ~/.config/sogou*`

## 2、重启fcitx
命令
 1. 找到fcitx的进程
 	`pidof fcitx `
 2. 关闭进程:pid是查到的进程号
	`kill pid `
 3. 重启fcitx
	`fcitx`

