---
title: python实现trie（前缀树）
date: 2020-06-01 16:32:14
tags: 
    - leetcode
    - 算法
---

<!-- toc -->
<!--more-->

### 题目  

[leetcode 208 实现Trie(前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)  

实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。

**示例:**
```java
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```
**说明:**

你可以假设所有的输入都是由小写字母 a-z 构成的。
保证所有输入均为非空字符串。

### trie(前缀树)
Trie (发音为 "try") 或前缀树是一种树数据结构，用于检索字符串数据集中的键。这一高效的数据结构有多种应用：
- 自动补全
- 拼写检查
- IP 路由 (最长前缀匹配)
- 等


Trie 树的结点结构  

Trie 树是一个有根的树，其结点具有以下字段：。

- 最多R个指向子结点的链接，其中每个链接对应字母表数据集中的一个字母。
- 本文中假定R为 26，小写拉丁字母的数量。
- 布尔字段，以指定节点是对应键的结尾还是只是键前缀。

下图表示leet在Trie树中的表示

![trie](/assets/img/Trie.png)

### python实现

```python
class Trie:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.lookup = {}


    def insert(self, word: str) -> None:
        """
        Inserts a word into the trie.
        """
        tree = self.lookup
        for s in word:
            if s not in tree:
                tree[s] = {}
            tree = tree[s]
        tree['end'] = 1


    def search(self, word: str) -> bool:
        """
        Returns if the word is in the trie.
        """
        tree = self.lookup
        for s in word:
            if s not in tree:
                return False
            tree = tree[s]
        if 'end' in tree:
            return True
        return False


    def startsWith(self, prefix: str) -> bool:
        """
        Returns if there is any word in the trie that starts with the given prefix.
        """
        tree = self.lookup
        for s in prefix:
            if s not in tree:
                return False
            tree = tree[s]
        return True



# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```


