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

#### 4.15 矩阵
[leetcode 542. 01 矩阵](https://leetcode-cn.com/problems/01-matrix/)  
给定一个由 0 和 1 组成的矩阵，找出每个元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

示例 1:  
输入:
```
0 0 0
0 1 0
0 0 0
```
输出:
```
0 0 0
0 1 0
0 0 0
```
示例 2:  
输入:
```
0 0 0
0 1 0
1 1 1
```
输出:
```
0 0 0
0 1 0
1 2 1
```
注意:

1. 给定矩阵的元素个数不超过 10000。
2. 给定矩阵中至少有一个元素是 0。
3. 矩阵中的元素只在四个方向上相邻: 上、下、左、右。

- **分析**
    1. 使用广度优先搜索，先将所有值为0的元素的下标放入列队，返回值可在原矩阵更改，将原矩阵值为1之外的元素修改为-1，将列队里的每个元素（i，j）从左弹出（FIFO），搜索四个方向，若值为-1，即该元素之前未被搜索到，即得到了最短路径为`1+matrix[i][j]`，该元素的下标压入列队 

        整个过程可看依次搜索最小路径为0、1、2、3……的下标 
    2. 使用动态规划，从左上角往右下角遍历一次，然后从右下角往左上角遍历一次，取最小值，[参考](https://leetcode-cn.com/problems/01-matrix/solution/01ju-zhen-by-leetcode-solution/)

- **算法实现**  
实现上述思路一：广度优先搜索
```python
class Solution:
    def updateMatrix(self, matrix: List[List[int]]) -> List[List[int]]:
        from collections import deque
        if not matrix:
            return 
        row = len(matrix)
        col = len(matrix[0])
        queue = deque()
        for i in range(row):
            for j in range(col):
                if matrix[i][j] == 0:
                    queue.append((i,j))
                else:
                    matrix[i][j] = -1
        # print(matrix)
        while queue:
            i,j = queue.popleft()
            for di,dj in [(i,j+1),(i,j-1),(i+1,j),(i-1,j)]:
                if 0<=di<row and 0<=dj<col and matrix[di][dj] == -1:
                    matrix[di][dj] = 1+matrix[i][j]
                    queue.append((di,dj))
            # print(matrix)
        return matrix
```


#### 4.16 合并区间

[leetcode 55 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

给出一个区间的集合，请合并所有重叠的区间。

示例 1:
```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```
示例 2:
```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```
- **分析**  

    考虑根据区间左端点排序，则可合并的区间一定是连续的，若可合并则合并即可，直接看代码即可理解
- **算法实现**
```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return []
        intervals.sort(key = lambda x: x[0])
        output = []
        for e in intervals:
            if not output or output[-1][-1] < e[0]:
                output.append(e)
            else:
                output[-1][-1] = max(output[-1][-1], e[-1])
        return output
```
- **复杂度分析**  

    时间复杂度：O(n\log n)O(nlogn)，其中 nn 为区间的数量。除去排序的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的 O(n\log n)O(nlogn)。

#### 4.17 跳跃游戏  
[leetcode 55 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

示例 1:
```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```
示例 2:
```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```

- **分析**  
遍历一次数组，每次更新当前可到达的最远点，若当前点不可到达，则返回False，遍历结束返回True
- **算法实现**  
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        if not nums: return False
        res = 0
        for i in range(len(nums)):
            if res < i:
                return False
            res = max(res, i + nums[i])
        return True
```
- **算法分析**
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)

#### 4.18 盛最多水的容器  
[leetcode 11 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)   
给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

![leetcode_zuiduoshuiderongqi](/assets/img/leetcode_zuiduoshuiderongqi.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

示例：
```
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```
- **分析**  
考虑使用双指针，左右指针分别指向数组头部和尾部，若左指针小于右指针，则以左指针为左边界的容器不可能大于当前的容器，故左指针往右移动，同理，若右指针小于左指针，则右指针往左移动  
- **算法实现**
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        if len(height) < 2:
            return 0
        i,j = 0, len(height)-1
        res = 0
        while i < j:
            res = max(res, (j-i) * min(height[i], height[j]))
            # print(i, j , res)
            if height[i] > height[j]:
                j -= 1
            else:
                i += 1
        return res
```
- **算法分析**
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)

#### 4.19 统计重复个数

[leetcode 466 统计重复个数](https://leetcode-cn.com/problems/count-the-repetitions/)

由 n 个连接的字符串 s 组成字符串 S，记作 S = [s,n]。例如，["abc",3]=“abcabcabc”。

如果我们可以从 s2 中删除某些字符使其变为 s1，则称字符串 s1 可以从字符串 s2 获得。例如，根据定义，"abc" 可以从 “abdbec” 获得，但不能从 “acbbe” 获得。

现在给你两个非空字符串 s1 和 s2（每个最多 100 个字符长）和两个整数 0 ≤ n1 ≤ 106 和 1 ≤ n2 ≤ 106。现在考虑字符串 S1 和 S2，其中 S1=[s1,n1] 、S2=[s2,n2] 。

请你找出一个可以满足使[S2,M] 从 S1 获得的最大整数 M 。

示例：
```
输入：
s1 ="acb",n1 = 4
s2 ="ab",n2 = 2

返回：
2
```

- **分析**  
此题的关键在于找出循环体，循环体的含义是在每xunhuan1个s1中包含了xunhuan2个s2，找出循环开始的位置。找出之后n1个s1包含s2的数量被分为了三份，循环开始前，循环和循环余下部分，得到s2的个数之后除以n2即可得到答案。循环位置的查找直接看代码。

- **算法实现**
```python
class Solution:
    def getMaxRepetitions(self, s1: str, n1: int, s2: str, n2: int) -> int:
        if not n1 or not n2:
            return 0
        s1cnt, s2cnt = 0,0
        idx = 0
        hashmap = {}
        
        # 查找循环体
        while True:
            s1cnt += 1
            for ch in s1:
                if ch == s2[idx]:
                    idx += 1
                    if idx == len(s2):
                        s2cnt += 1
                        idx = 0
            if s1cnt == n1:
                return s2cnt // n2
            # idx即为下一个s1中第一个s2中的元素的下标
            if idx not in hashmap:
                hashmap[idx] = (s1cnt, s2cnt)
            
            # 若idx已经出现过，则找到了循环
            else:
                pre = hashmap[idx]
                xunhuan = s1cnt-pre[0],s2cnt-pre[1]
                break
        # res 分为三部分，这里是第一部分循环前pre[0]和循环部分
        res = pre[1] + (n1 - pre[0]) // xunhuan[0] * xunhuan[1]

        # 在加上循环后的部分
        rest = (n1 - pre[0]) % xunhuan[0]
        for _ in range(rest):
            for ch in s1:
                if ch == s2[idx]:
                    idx += 1
                    if idx == len(s2):
                        res += 1
                        idx = 0
        return res // n2
```
