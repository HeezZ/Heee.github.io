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

    时间复杂度：$O(nlogn)$，其中 nn 为区间的数量。除去排序的开销，我们只需要一次线性扫描，所以主要的时间开销是排序的 $O(nlogn)$。

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
此题的关键在于找出循环体，循环体的含义是在每xunhuan1个s1中包含了xunhuan2个s2，找出循环开始的位置。循环体找出之后，n1个s1包含s2的数量被分为了三份，循环开始前，循环和循环余下部分，得到s2的个数之后除以n2即可得到答案。循环位置的查找直接看代码。

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

#### 4.20 岛屿数量

[leetcode 200 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1:
```
输入:
11110
11010
11000
00000
输出: 1
```
示例 2:
```
输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```

- **分析**  
    1. 直接深搜或者宽搜即可
    2. 使用并查集
- **算法实现**
    1. dfs
    ```python
    class Solution:
        def numIslands(self, grid: List[List[str]]) -> int:
            if not grid:
                return 0
            row = len(grid)
            col = len(grid[0])
            res = 0

            def dfs(i,j):
                if i<0 or i>=row or j < 0 or j >= col or grid[i][j] == '0':
                    return
                grid[i][j] = '0'
                for m,n in [(i+1,j), (i-1,j), (i, j+1), (i,j-1)]:
                    # print(m,n)
                    dfs(m,n)
            
            for i in range(row):
                for j in range(col):
                    if grid[i][j] == '1':
                        res += 1
                        dfs(i,j)
            # print(grid)
            return res
    ```
    时间复杂度：$O(MN)$，其中 $M$ 和 $N$ 分别为行数和列数。

    空间复杂度：$O(MN)$，在最坏情况下，整个网格均为陆地，深度优先搜索的深度达到 MN。
  
    2. 并查集
    ```python
    # 并查集实现三个方法，find找到根元素，并且使用递归的方法进行路径压缩，union把两个元素并到一起，使他们有相同的根元素，getcount返回集合数量
    class Union_Find:
        def __init__(self,grid):
            row = len(grid)
            col = len(grid[0])
            self.count = 0
            self.parent = [0 for _ in range(row*col)]
            self.rank = [0 for _ in range(row*col)]
            for i in range(row):
                for j in range(col):
                    if grid[i][j] == '1':
                        self.count += 1
                        self.parent[i * col + j] = i * col + j
                        
        def find(self,i):
            if self.parent[i] != i:
                self.parent[i] = self.find(self.parent[i])
            return self.parent[i]
        
        def union(self,x,y):
            rootx = self.find(x)
            rooty = self.find(y)
            if rootx != rooty:
                # 这里的rank用来防止树过深，减小搜索时间，故每次把深度较小的树挂到更深的树上。
                if self.rank[rootx] < self.rank[rooty]:
                    rootx, rooty = rooty, rootx
                self.parent[rooty] = rootx
                if self.rank[rootx] == self.rank[rooty]:
                    self.rank[rootx] += 1
                self.count -= 1

        def getcount(self):
            return self.count

    class Solution:
        def numIslands(self, grid: List[List[str]]) -> int:
            if not grid:
                return 0
            uf = Union_Find(grid)
            row = len(grid)
            col = len(grid[0])
            for i in range(row):
                for j in range(col):
                    # print(i,j)
                    if grid[i][j] == '1':
                        grid[i][j] = '0'
                        #因为i,j都是递增的方向，故可以只考虑两个方向
                        for di,dj in ((i+1, j), (i, j+1)):
                            if 0<=di<row and 0<=dj<col and grid[di][dj] == '1':
                                # 这里不可置零
                                # grid[di][dj] = '0'
                                uf.union(i*col+j, di*col + dj)
                        # print(uf.parent)
            return uf.getcount()
    ```
    时间复杂度：$O(MN * \alpha(MN))$，其中 $M$ 和 $N$ 分别为行数和列数。注意当使用路径压缩（见 find 函数）和按秩合并（见数组 rank）实现并查集时，单次操作的时间复杂度为 $\alpha(MN)$，其中 $\alpha(x)$ 为反阿克曼函数，当自变量 x 的值在人类可观测的范围内（宇宙中粒子的数量）时，函数 $\alpha(x)$的值不会超过 5，因此也可以看成是常数时间复杂度。

    空间复杂度：$O(MN)$，这是并查集需要使用的空间。

#### 4.21 统计「优美子数组」

[leetcode 1248 统计「优美子数组」](https://leetcode-cn.com/problems/count-number-of-nice-subarrays/)

给你一个整数数组 nums 和一个整数 k。

如果某个 连续 子数组中恰好有 k 个奇数数字，我们就认为这个子数组是「优美子数组」。

请返回这个数组中「优美子数组」的数目。

 

示例 1：

输入：nums = [1,1,2,1,1], k = 3
输出：2
解释：包含 3 个奇数的子数组是 [1,1,2,1] 和 [1,2,1,1] 。
示例 2：

输入：nums = [2,4,6], k = 1
输出：0
解释：数列中不包含任何奇数，所以不存在优美子数组。
示例 3：

输入：nums = [2,2,2,1,2,2,1,2,2,2], k = 2
输出：16

- **分析**  
    1. 考虑采用滑动窗口，维护一个长为k的列队idx保存奇数元素的下标
    2. 窗口的左端点和右端点分别初始化为0
    3. 右端点右移，遇到奇数元素则加入列队，直到列队长度为k，则此时以右端点为末尾满足条件的连续子数组的数量为idx[0]-left+1
    4. 此时列队长度已经是k，若右端点遇到偶数，则以此元素为右端点的满足条件的连续子数组数量仍然为idx[0]-left+1
    5. 若右端点遇到奇数，则将idx的最左端元素弹出，且修改left为：left=idx.popleft()+1，将此奇数压入idx，则以此元素为右端点的满足条件的连续子数组数量仍然为idx[0]-left+1

- **算法实现**
```python
class Solution:
    def numberOfSubarrays(self, nums: List[int], k: int) -> int:
        from collections import deque
        left = right = 0
        res = 0
        idx = deque()
        for right in range(len(nums)):
            if nums[right] & 1 and len(idx)<k:
                idx.append(right)
                if len(idx) == k:
                    res += idx[0]-left+1

            elif nums[right] & 1:
                left = idx.popleft() + 1
                idx.append(right)
                res += idx[0]-left+1
            else:
                if len(idx) == k:
                    res += idx[0]-left+1
        return res
```
- **算法分析**
    - 时间复杂度：$O(n)$
    - 空间复杂度：$O(1)$

#### 4.22 二叉树的右视图

[leetcode 199 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

给定一棵二叉树，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

示例:
```
输入: [1,2,3,null,5,null,4]
输出: [1, 3, 4]
解释:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```

- **分析**  
二叉树的层次遍历，每层只取最后一个元素

- **算法实现**  
```python
class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        res = []
        stack = [root]
        while stack:
            cur = []
            tmp = []
            for e in stack:
                cur.append(e.val)
                if e.right:
                    tmp.append(e.right)
                if e.left:
                    tmp.append(e.left)
            res.append(cur)
            stack = tmp
        return [n[0] for n in res]
```

#### 4.23 硬币

[leetcode 面试题 08.11. 硬币](https://leetcode-cn.com/problems/coin-lcci/)

硬币。给定数量不限的硬币，币值为25分、10分、5分和1分，编写代码计算n分有几种表示法。(结果可能会很大，你需要将结果模上1000000007)

示例1:
```
 输入: n = 5
 输出：2
 解释: 有两种方式可以凑成总金额:
5=5
5=1+1+1+1+1
```
示例2:
```
 输入: n = 10
 输出：4
 解释: 有四种方式可以凑成总金额:
10=10
10=5+5
10=5+1+1+1+1+1
10=1+1+1+1+1+1+1+1+1+1
```

- **分析** 
使用动态规划，先初始化可能性为零，0的可能性为一，枚举硬币的可能性，每种硬币的可能性下，dp[i] += dp[i-coin]因为该硬币第一次出现，所以没有重复的可能。dp[i-coin]涵盖了所有dp[i]中有coin时的所有可能性故没有遗漏。
- **算法实现**
```python
class Solution:
    def waysToChange(self, n: int) -> int:
        coins = [1, 5, 10, 25]
        if n <= 0:
            return 0
        dp = [1] + [0]*n
        for coin in coins:
            for i in range(coin, n+1):
                dp[i] += dp[i-coin]
                dp[i] %= 1000000007
        return dp[-1]
```

- **算法分析**
    - 时间复杂度：$O(kn)$，其中k为硬币数量
    - 空间复杂度：$O(n)$

#### 4.24 数组中的逆序对

[leetcode 面试题51. 数组中的逆序对](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

示例 1:
```
输入: [7,5,6,4]
输出: 5
```
- **分析**  
归并排序，在并入时统计逆序对数量

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        if not nums:
            return 0
        self.res = 0
        def guibing(left, right):
            if left == right:
                return [nums[left]]

            mid = (left + right) >> 1
            # print(mid)
            return merge(guibing(left,mid), guibing(mid+1,right))

        def merge(l1, l2):
            left = 0
            right = 0
            r = []
            while left < len(l1) and right< len(l2):
                if l1[left] <= l2[right]:
                    r.append(l1[left])
                    left += 1
                else:
                    r.append(l2[right])
                    self.res += len(l1)-left
                    right += 1
            r.extend(l1[left:])
            r.extend(l2[right:])
            # print(l1,l2,self.res)
            return r
        
        r = guibing(0, len(nums)-1)
        # print(r)
        return self.res 
```

- **算法分析**
    - 时间复杂度：$O(n\log(n))$
    - 空间复杂度：$O(n)$

#### 4.25 全排列

[leetcode 46 全排列](https://leetcode-cn.com/problems/permutations/)

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:
```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

- **分析**
直接回溯

- **算法实现**
使用额外空间
```python
class Solution:
        def backtrack(cur):
            if len(cur) == len(nums):
                res.append(cur)
                return
            for i in range(len(nums)):
                if not used[i]:
                    used[i] = 1
                    backtrack(cur+[nums[i]])
                    used[i] = 0
        res = []
        used = [0 for _ in range(len(nums))]
        backtrack([])
        return res
```
不使用额外空间
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def backtrack(cur_list, first):
            if len(cur_list) == len(nums):
                output.append(cur_list)
            for i in range(first, len(nums)):
                nums[first], nums[i] = nums[i], nums[first]
                backtrack(cur_list+[nums[first]], first+1)
                nums[first], nums[i] = nums[i], nums[first]
        output = []
        backtrack([], 0)
        return output
```
- **算法分析**
 - 时间复杂度：全排列数量有$n!$种，每种排列需调用n次backtrack函数，故时间复杂度为$O(n*n!)$
 - 空间复杂度：O(n)，包括递归深度，返回数组，（和used）

 #### 4.26 合并k个排序链表

 [leetcode 23 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

 合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

示例:
```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

- **分析**
    1. 像合并两个有序链表一样，依次顺序合并
        - 时间复杂度：假设每个链表的最长长度是$n$。在第一次合并后，$ans$ 的长度为 $n$；第二次合并后，$ans$ 的长度为 $2\times n$，第 $i$ 次合并后，$ans$ 的长度为 $i\times n$。第 $i$ 次合并的时间代价是 $O(n + (i - 1) \times n) = O(i \times n)$，那么总的时间代价为 $O(\sum_{i = 1}^{k} (i \times n)) = O(\frac{(1 + k)\cdot k}{2} \times n) = O(k^2 n)$，故渐进时间复杂度为 $O(k^2 n)$。
        - 空间复杂度：除了ans之外没有额外空间
    2. 分治，和归并排序类似，两两合并
        - 时间复杂度：考虑递归「向上回升」的过程——第一轮合并 $\frac{k}{2}$组链表，每一组的时间代价是 $O(2n)$；第二轮合并 $\frac{k}{4}$组链表，每一组的时间代价是 $O(4n)$...所以总的时间代价是 $O(\sum_{i = 1}^{\log k} \frac{k}{2^i} \times 2^i n) = O(kn \times \log k)$，故渐进时间复杂度为 $O(kn \times \log k)$O
        - 空间复杂度：递归会使用到 $O(\log k)$空间代价的栈空间。

- **算法实现**
这里实现第二种思路，复杂度已经分析过了
```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        if not lists:
            return None
        if len(lists) == 1:
            return lists[0]
        left = lists[0:len(lists)//2]
        right = lists[len(lists)//2:]

        def helper(l1, l2):
            node = ListNode(0)
            p = node
            while l1 and l2:
                # print(p)
                if l1.val > l2.val:

                    node.next = l2
                    node = node.next
                    l2 = l2.next
                else:
                    node.next = l1
                    node = node.next
                    l1 = l1.next
            if l1:
                node.next = l1
            else:
                node.next = l2
            # print(p.next)
            return p.next
        return helper(self.mergeKLists(left), self.mergeKLists(right))
```

#### 4.26 搜索旋转排序数组

[leetcode 33 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 O(log n) 级别。

示例 1:
```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```
示例 2:
```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```
- **分析**  
二分法：先使用二分法得到旋转点，再使用二分法得到目标值

- **算法实现**
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if not nums:
            return -1
        def search_rotate(left, right):
            if nums[left] <= nums[right]:
                return left
            while left < right:
                mid = (left + right) >> 1
                if nums[mid] > nums[mid+1]:
                    return mid+1
                elif nums[mid] > nums[right]:
                    left = mid + 1
                else:
                    right = mid
        def search_target(left, right):
            while(left <= right):
                mid = (left + right) >> 1
                if nums[mid] == target:
                    return mid
                elif nums[mid] > target:
                    right = mid - 1
                else:
                    left = mid + 1
            return -1
        rotate_index = search_rotate(0, len(nums) -1)
        if target <= nums[-1]:
            return search_target(rotate_index, len(nums)-1)
        elif target >= nums[0]:
            return search_target(0, rotate_index)
        else:
            return -1
```
- **算法分析**
    - 时间复杂度：$O(n\log (n))$
    - 空间复杂度：$O(1)$

#### 4.29 山脉数组中查找目标值

[leetcode 1095. 山脉数组中查找目标值](https://leetcode-cn.com/problems/find-in-mountain-array/)

此题题目较长，且较为简单，故只给出链接

- **分析**
二分查找，先二分查找找出最高点，然后在最高点的左右两边分别二分查找即可，与旋转数组查找类似

- **算法实现**
```python
class Solution:
    def findInMountainArray(self, target: int, mountain_arr: 'MountainArray') -> int:
        length = mountain_arr.length()
        # left,right = 0, length - 1
        def search_top(left, right):
            while left<=right:
                mid = (left + right) >> 1
                if mountain_arr.get(mid)<mountain_arr.get(mid + 1):
                    left = mid + 1
                else:
                    right = mid - 1
            return left

        def search_target(left, right, up):
            while left <= right:
                mid = (left + right) >> 1
                if mountain_arr.get(mid) > target:
                    if up:
                        right = mid - 1
                    else:
                        left = mid + 1
                elif mountain_arr.get(mid) < target:
                    if up:
                        left = mid + 1
                    else:
                        right = mid - 1
                else:
                    return mid
            return -1

        top = search_top(0, length -1)
        res = -1
        if mountain_arr.get(top) < target:
            return -1
        if mountain_arr.get(0)<=target:
            res = search_target(0,top,True)
        if res != -1:
            return res
        if mountain_arr.get(length-1)<=target:
            res = search_target(top,length-1,False)
        if res != -1:
            return res
        return -1
```
- **算法分析**
    - 时间复杂度：$O(\log n)$，只用了三次二分查找
    - 空间复杂度：$O(1)$