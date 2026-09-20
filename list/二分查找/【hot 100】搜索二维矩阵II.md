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

## ACM version


```python
import sys 

class Solution:
    def searchMatrix(self, matrix, target): # 注意拼接也不是全局有序，从右上角开始搜索
        m, n = len(matrix), len(matrix[0])
        i, j = 0, n - 1          # 从右上角开始
        while i < m and j >= 0:
            if matrix[i][j] == target:
                return 'true'
            elif matrix[i][j] > target:
                j -= 1           # 当前值太大，往左走
            else:
                i += 1           # 当前值太小，往下走
        return 'false'
        
data = sys.stdin.read().strip().split('\n')
idx = 0
while idx < len(data):
    m, n, target = map(int, data[idx].split()); idx += 1
    matrix = []
    for _ in range(m):
        matrix.append(list(map(int, data[idx].split())))
        idx += 1
    ans = Solution().searchMatrix(matrix, target)
    print(ans)
```


# Complexity Analysis
- 时间复杂度：O(m+n)
- 空间复杂度：O(1)
