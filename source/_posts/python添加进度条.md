---
title: python添加进度条
date: 2020-07-23 10:50:09
tags: python
---

- tqdm

```python
from tqdm import tqdm

for _ in tqdm(range(1000000000)):
    pass
```

- 如果不在一行显示，一直往下滚  
加上一个ncols的参数就行了，规定一下滚动条的长度，往下滚动的原因是它太长了。

```python
for i in tqdm(range(100),ncols=80)
    j = i*i 
```

