---
title: 'pip更新至19.3.1出现TypeError_ module object is not callable'
date: 2019-10-27 09:38:07
tags:
 - Python
 - pip
categories:
 - Python
---
错误信息

```powershell
Traceback (most recent call last):
  File "c:\program files (x86)\python37-32\lib\runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "c:\program files (x86)\python37-32\lib\runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "C:\Program Files (x86)\Python37-32\Scripts\pip.exe\__main__.py", line 9, in <module>
TypeError: 'module' object is not callable
```

我估计是19.3.1的pip版本问题，移除了直接能用`pip`命令功能，现在使用pip命令需要把pip前加上`python -m` 之后就能写`pip`命令了，搞不懂官方为啥要这样（手动狗头）
<hr/>
楼下一哥们说，是安装错误导致的，emmm观察了一下，发现我这里缺了

`pip-inscript.py` 文件，执行了

```powershell
easy_install pip
```
会安装如下东西

```powershell
Searching for pip
Best match: pip 19.3.1
Adding pip 19.3.1 to easy-install.pth file
Installing pip-script.py script to c:\program files (x86)\python37-32\Scripts
Installing pip.exe script to c:\program files (x86)\python37-32\Scripts
Installing pip.exe.manifest script to c:\program files (x86)\python37-32\Scripts
Installing pip3-script.py script to c:\program files (x86)\python37-32\Scripts
Installing pip3.exe script to c:\program files (x86)\python37-32\Scripts
Installing pip3.exe.manifest script to c:\program files (x86)\python37-32\Scripts
Installing pip3.7-script.py script to c:\program files (x86)\python37-32\Scripts
Installing pip3.7.exe script to c:\program files (x86)\python37-32\Scripts
Installing pip3.7.exe.manifest script to c:\program files (x86)\python37-32\Scripts
```
确实可以使用 `pip` 命令了，emmm其它的后果有待探究。