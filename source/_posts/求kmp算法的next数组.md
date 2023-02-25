---
title: 求kmp算法的next数组
date: 2020-07-23 10:56:29
tags: 
    - 算法
    - leetcode
---
<!-- toc -->
<!--more-->
#### leetcode 题目  
[leetcode 28](https://leetcode-cn.com/problems/implement-strstr/submissions/)

#### kmp实现
```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        def kmp(s):
            if not s:
                return []
            next = [0 for _ in range(len(s))]
            next[0] = -1
            i,j = 1, 0
            while i < len(s)-1:
                if j == -1 or s[i] == s[j]:
                    i += 1
                    j += 1
                    next[i] = j
                else:
                    j = next[j]
            return next
        if not needle:
            return 0
        if not haystack or len(haystack) < len(needle):
            return -1
        next = kmp(needle)
        i,j = 0, 0
        while i < len(haystack):
            if  j == -1 or haystack[i] == needle[j]:
                i += 1
                j += 1
            else:
                j = next[j]
            if j == len(needle):
                return i - len(needle)
        return -1
```

#### 思考题
- 设查询串的的长度为 nn，模式串的长度为 mm，我们需要判断模式串是否为查询串的子串。那么使用 KMP 算法处理该问题时的时间复杂度是多少？在分析时间复杂度时使用了哪一种分析方法？
    - 时间复杂度为 O(n+m)O(n+m)，用到了均摊分析（摊还分析）的方法
    - 具体地，无论在预处理过程还是查询过程中，虽然匹配失败时，指针会不断地根据 \textit{fail}fail 数组向左回退，看似时间复杂度会很高。但考虑匹配成功时，指针会向右移动一个位置，这一部分对应的时间复杂度为 O(n+m)O(n+m)。又因为向左移动的次数不会超过向右移动的次数，因此总时间复杂度仍然为 O(n+m)O(n+m)。

- 如果有多个查询串，平均长度为 nn，数量为 kk，那么总时间复杂度是多少？
    - 时间复杂度为 O(nk+m)O(nk+m)。模式串只需要预处理一次

- 在 KMP 算法中，对于模式串，我们需要预处理出一个 \textit{fail}fail 数组（有时也称为 \textit{next}next 数组、\piπ 数组等）。这个数组到底表示了什么？
    - fail[i] 等于满足下述要求的 x 的最大值：s[0:i] 具有长度为 x+1的完全相同的前缀和后缀。