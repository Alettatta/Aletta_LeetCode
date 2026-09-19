# Problem
https://labuladong.online/zh/problem/leetcode/search-a-2d-matrix-ii/description/


# Problem Description
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。

数据范围：

m == matrix.length

n == matrix[i].length

1 <= m, n <= 300

-10^9 <= matrix[i][j] <= 10^9

-10^9 <= target <= 10^9

编写一个高效的算法，在每行升序、每列升序的 m x n 矩阵 matrix 中搜索目标值 target，存在则返回 true，否则返回 false。


# Key Points
经典的杨氏矩阵（Young Tableau）搜索问题。由于矩阵每行从左到右升序，每列从上到下升序，我们可以利用这个特性从右上角或左下角开始搜索。


# Solution
最优解法：从右上角 (0, n-1) 开始：

如果 当前值 == target：找到，返回 True

如果 当前值 > target：说明当前列下面的元素都更大，向左移动一列

如果 当前值 < target：说明当前行左边的元素都更小，向下移动一行

这样**每次都能排除一行或一列**，时间复杂度 O(m+n)。


# Code

## LC version

```python
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix) # 有多少行
        n = len(matrix[0]) # 每一行有多少个元素
        i = 0
        j = n-1
        while i < m and j >= 0:
            if matrix[i][j] == target:
                return 'true'
            elif matrix[i][j] < target:
                i += 1
            elif matrix[i][j] > target:
                j -= 1
        return False
```

## ACM version

**ACM 模式的注意点：**

- 需要 import 完整的类（包括 sys、typing 等）
- 数据在标准输入流 stdin 中，全部是原始的文本字符串
- 必须用 print() 手动将结果写到标准输出流 stdout
- 需要写 while 或 for line in sys.stdin 循环处理，直到文件结束（EOF）

```python
import sys
from typing import List

class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        m = len(matrix) # 有多少行
        n = len(matrix[0]) # 每一行有多少个元素
        i = 0
        j = n-1
        while i < m and j >= 0:
            if matrix[i][j] == target:
                return True
            elif matrix[i][j] < target:
                i += 1
            elif matrix[i][j] > target:
                j -= 1
        return False

data = sys.stdin.read().split()
index = 0
while index < len(data):
    m = int(data[index])
    index += 1
    n = int(data[index])
    index += 1
    target = int(data[index])
    index += 1
    
    matrix = [] # 要放在这里因为有很多示例，每次循环要重来
    for i in range(m):
        row = [int(x) for x in data[index: index+n]]
        matrix.append(row)
        index += n
        
    result = Solution().searchMatrix(matrix, target)

    if result == True:
        print('true')
    else:
        print('false')
```


# Complexity Analysis
- 时间复杂度：O(logn)
- 空间复杂度：O(1)
