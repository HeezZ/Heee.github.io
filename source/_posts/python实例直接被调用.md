---
title: python实例直接被调用
date: 2020-07-29 17:38:34
tags:
    - python
---

<!-- toc -->
<!--more-->

看pytorch代码的时候发现看到继承了module类的实例直接被调用，不解故搜索一番

#### 函数也是对象

具体看[这里](https://www.zhihu.com/question/32002222)

Everything in Python is an object

#### 实现了 __call__ 的类也可以作为函数
```python
class Add:
    def __init__(self, n):
         self.n = n
    def __call__(self, x):
        return self.n + x

>>> add = Add(1)
>>> add(4)
>>> 5
```
