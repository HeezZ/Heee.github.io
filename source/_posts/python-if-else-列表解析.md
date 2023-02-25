---
title: python if else 列表解析
date: 2020-07-31 17:10:29
description: python用for if else构造列表 
tags: python
---

<!-- toc -->

用for构造列表时，用if之后记不太清什么时候可以用else，如以下代码会报错：  
```python
# 报错
b = [a[i] for i in range(len(a)) if i != 1 else 8] 
```
**列表解析总共有两种形式：**
- [i for i in range(k) if condition]：此时if起条件判断作用，满足条件的，将被返回成为最终生成的列表的一员。

- [i if condition else exp for exp]：此时if...else被用来赋值，满足条件的i以及else被用来生成最终的列表。

如：
```python
print([i for i in range(10) if i%2 == 0])
print([i if i == 0 else 100 for i in range(10)])
 
# 输出[0, 2, 4, 6, 8]
# 输出[0, 100, 100, 100, 100, 100, 100, 100, 100, 100]
```