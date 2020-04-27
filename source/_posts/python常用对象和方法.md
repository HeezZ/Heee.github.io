---
title: python常用对象和方法
date: 2020-04-18 14:08:03
tags: python
---

<!-- toc -->
<!--more-->

#### OrderedDict
>有序词典就像常规词典一样，但有一些与排序操作相关的额外功能

>算法上， OrderedDict 可以比 dict 更好地处理频繁的重新排序操作。 这使其适用于跟踪最近的访问（例如在 LRU cache 中）。

>对于 OrderedDict ，相等操作检查匹配顺序。

>OrderedDict 类的 popitem() 方法有不同的签名。它接受一个可选参数来指定弹出哪个元素。

>OrderedDict 类有一个 move_to_end() 方法，可以有效地将元素移动到任一端

- **popitem(last=True)**  
有序字典的 popitem() 方法移除并返回一个 (key, value) 键值对。 如果 last 值为真，则按 LIFO 后进先出的顺序返回键值对，否则就按 FIFO 先进先出的顺序返回键值对。

- **move_to_end(key, last=True)**   
将现有 key 移动到有序字典的任一端。 如果 last 为真值（默认）则将元素移至末尾；如果 last 为假值则将元素移至开头。如果 key 不存在则会触发 KeyError

```
>>> d = OrderedDict.fromkeys('abcde')
>>> d.move_to_end('b')
>>> ''.join(d.keys())
'acdeb'
>>> d.move_to_end('b', last=False)
>>> ''.join(d.keys())
'bacde'
```

- 使用例子：[leetcode 146 LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/)

```python
from collections import OrderedDict
class LRUCache:

    def __init__(self, capacity: int):
        self.maxsize = capacity
        self.lrucache = OrderedDict()
    def get(self, key: int) -> int:
        if key in self.lrucache:
            self.lrucache.move_to_end(key)
        return self.lrucache.get(key,-1)
    def put(self, key: int, value: int) -> None:
        if key in self.lrucache:
            del self.lrucache[key]
        self.lrucache[key] = value 
        if len(self.lrucache)>self.maxsize:
            self.lrucache.popitem(last = False)   
```

#### set.pop()
很多资料显示是随机删除一个，其实不是，一个例子：
```
set1 = set([9,4,5,2,6,7,1,8])
print(set1)
print(set1.pop())
print(set1)
结果:
{1, 2, 4, 5, 6, 7, 8, 9}
1
{2, 4, 5, 6, 7, 8, 9}
 
set1 = set((6,3,1,7,2,9,8,0))
print(set1)
print(set1.pop())
print(set1)
结果:
{0, 1, 2, 3, 6, 7, 8, 9}
0
{1, 2, 3, 6, 7, 8, 9}
```

**按照hash值排序 然后删除排序好的最左边一个**  
时间复杂度O(1)

#### dict.pop()   
Python 字典 pop() 方法删除字典给定键 key 及对应的值，**返回值**为被删除的值。key 值必须给出。 否则，返回 default 值。  

语法
```python
pop(key[,default]) # 必须给出参数，无参数则报错
```

**注意**：del 语句和 pop() 函数作用相同，pop() 函数有返回值。

时间复杂度：O(1)

##### 与dict.popitem()的区别：
Python 字典 popitem() 方法返回并删除字典中的最后一对键和值，（最后加入），不接收参数。

如果字典已经为空，却调用了此方法，就报出 KeyError 异常。
