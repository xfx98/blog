---
title: '管理nodejs版本'
date: 2021-3-24 14:10:07
tags:
 - Linux
 - nodejs
categories:
 - IDE

---

### 安装下载 [nvm node版本管理工具](https://github.com/coreybutler/nvm-windows/releases) 

执行命令安装指定版本，进行版本管理

```bash
nvm install 14.16.0 #安装14.16.0
```

```bash
nvm list #查看安装所有版本
nvm list available #查看可安装的版本
nvm use 14.16.0 #使用指定版本
```

### 安装n模块

1、首先安装n模块：

```bash
npm install -g n
```
2、安装好n模块后可以选择下面其一升级：

选择一：升级node.js到最新稳定版
```bash
n stable
```
选择二：升级node.js到最新版
```bash
n latest
```
选择三：升级node.js到制定版本
```bash
n v14.16.0
```
