---
title: 使用git提交项目到GitHub
date: 2019-7-1 16:43:17
tags:
 - Git
categories:
 - Git

---
## 一、执行将GitHub文件加载到本地
执行`git clone repositoryURL`repositoryURL如下图中的url (大家别拿我的实验哈</卑微>)
![git仓库地址](https://img-blog.csdnimg.cn/20190623192124993.png)

## 二、添加提交信息
执行`git add .`或者将`.`改成需要添加的文件
使用`git status`可查看当前文件修改的状态，红色是改变后没有进行`add`操作的文件，绿色的是提交将会修改原文文件。若都没有表示没有对文件修改，不需要提交。
## 三、提交
执行`git commit -m "提交信息"`也可改成其它如`git commit -a`提交所有改变文件这里会弹出文本窗口要求填写提交信息，其它选项可执行`git commit -help`查看。
最后执行`git push -u origin master`提交到GitHub仓库的`master`分支，如要提交其它分支可自行更改，第一次提交应该会要求输入账号和密码。

