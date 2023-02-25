---
title: 获取python当前路径
date: 2020-06-23 11:10:26
tags: python
---
<!--more-->
[参考1](https://blog.csdn.net/vitaminc4/article/details/78702852)  
[参考2](https://www.cnblogs.com/mq0036/p/11417996.html)


Python脚本使用相对路径时，被另一个不同目录下的py文件中导入时，会报找不到对应文件的问题。感觉是当前工作目录变成了导入py文件当前目录。如果你有配置文件的读取操作，然后都放在一个py文件中，而你又用的是相对路径，而且这个py文件在多个不同目录下的py文件中被导入，则会报错找不到文件

先写结论，要获取脚本的绝对路径，则
```python
import os
# 获得脚本所在文件夹的目录，不包括脚本
abs_dir = os.path.split(os.path.realpath(__file__))[0]
```

另外几种方法  

```
文件目录结构

C:test
|-getpath
    |-path.py
    |-sub
        |-sub_path.py
```
在c:test下执行path.py,path.py中调用sub_path.py,则sub_path.py中各种方法对应的值为：
```python
os.getcwd()  # 取的是起始执行目录：“C:\test”

sys.path[0]或sys.argv[0] #取的是被初始执行的脚本的所在目录：“C:\test\getpath”

os.path.split(os.path.realpath(__file__))[0] # 取的是file所在文件sub_path.py的所在目录：“C:\test\getpath\sub”
```