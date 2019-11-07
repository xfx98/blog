---
title: 'pip更新至19.3.1出现TypeError_ module object is not callable'
date: 2019-10-27 09:38:07
tags:
 - Python
 - pip
categories:
 - Python
---
我估计是19.3.1的pip版本问题，移除了直接能用`pip`命令功能，现在使用pip命令需要把pip前加上`python -m` 之后就能写`pip`命令了，搞不懂官方为啥要这样（手动狗头）
