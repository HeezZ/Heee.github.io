---
title: leetcode每日一题
date: 2020-04-14 11:39:50
tags: leetcode
description: leetcode每日一题解析和python实现
---
<!-- toc -->

#### 4.14 两数相加Ⅱ
[leetcode 445 两数相加](https://leetcode-cn.com/problems/add-two-numbers-ii/)   
给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

进阶：

如果输入链表不能修改该如何处理？换句话说，你不能对列表中的节点进行翻转。  

示例：
```
输入：(7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 8 -> 0 -> 7
```
- **分析**  
1. 若可以翻转链表则可先翻转链表，然后逐位相加，用carry记录进位
2. 不可翻转链表则使用堆栈，将两个链表逐位压入堆栈然后依次弹出，使用carry记录进位
3. 若使用python，整型数据不会溢出，则可直接将两个数存到a,b中，相加后保存为链表
4. 考虑溢出则使用堆栈，（也可先存为字符串，再使用字符串的加法，不推荐）
- **算法实现**
python实现以上分析的第三种方法
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        a = 0
        p1 = l1
        while p1:
            a = a*10 + p1.val
            p1 = p1.next
        b = 0
        p2 = l2
        while p2:
            b = b*10 + p2.val
            p2 = p2.next
        s = a+b
        if s == 0:
            return ListNode(0)
        pre = None
        while s > 0:
            cur = ListNode(s%10)
            cur.next = pre
            pre = cur
            s //=10
        return pre
```
#### 4.15