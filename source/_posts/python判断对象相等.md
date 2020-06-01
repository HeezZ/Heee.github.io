---
title: python判断对象相等
date: 2020-05-08 02:09:50
tags: python
---
<!-- toc -->
<!--more-->

#### python 对象的标识

- python对象标识就是python对象自身的要素，python对象主要有3要素：

- id:相当于对象在内存中的地址，相当于c的指针，可以用id(对象)来获取。

- 类型：python的基本对象有Number、String、List、Tuple、Set、Dictionary六种，当然还有对象的实例化，他们的类型就是对象的类名。可以通过type(对象)来获取。

- 值：对象的值

#### 对象相等的判断
python中的对象是否相等有两个层面，一个层面是是否是同一个对象，及在内存中是否共用一个内存区域，用is判断，另一个是对象的值是否相等，用==判断。

```python
class A:
    def __init__(self,val):
        self.a = val
        self.b = val
        self.c = val
a = A(0)
b = A(0)
print(a is b)  # False
print(a==b)  # False
```
a、b明显是两个不同的对象，所占地址内存不同，所以is判断为False  

a、b虽然属性相同但用==判断的结果也为false

#### 自定义python对象相等的条件

在实际情况中如若我们希望a、b属性相同，就认为是同一个对象，需要重写__eq__方法

```python
class A:
    def __init__(self,val):
        self.a = val
        self.b = val
        self.c = val
    def __eq__(self,other):
        return self.__dict__ == other.__dict__
a = A(0)
b = A(0)
print(a==b) # True
```
调用一个对象的dict方法可以用字典的形式输出其属性列表，由于两个对象的属性相同，所以==运算为True。

**注意**：当类方法中也定义属性时要求方法中的属性也相同

```python
class A:
    def __init__(self,val):
        self.a = val
        self.b = val
        self.c = val
    def fun(self,val):
        self.d = val
        self.f = val
    def __eq__(self,other):
        return self.__dict__ == other.__dict__
a = A(0)
b = A(0)
print(a==b) # True
a.fun(1)
print(a==b) # False
print(a.__dict__) # {'a': 0, 'b': 0, 'c': 0, 'd': 1, 'f': 1}
print(b.__dict__) # {'a': 0, 'b': 0, 'c': 0}
```

也可更灵活地定义两个对象相等的条件

```python
class student(object):
    def __init__(self,name,age,sex):
        self.name = name
        self.age = age
        self.sex = sex
    def __eq__(self,other):
        return self.name == other.name

like = student("like",25,"male")
dong = student("like",23,"female")        

print(like == dong) #True
```
#### in
list的in操作就是通过==来判断是否在list中。

```python
like = student("like",25,"male")
dong = student("like",25,"male")

list1 = []
list1.append(like)
if dong not in list1:
    list1.append(dong)
print(len(list1)) #1
```