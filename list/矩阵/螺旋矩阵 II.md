# Problem

https://leetcode.cn/problems/spiral-matrix-ii/description/

# Problem Description

给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。



# Solution

同螺旋矩阵，考虑矩阵坐标的边界；注意需要填入矩阵的数字就是 1-n^2，不断+1填入就行

# Code

## LC version

```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        matrix = [[0] * n for _ in range(n)] # 首先初始化matrix，后面只要修改它
        upper, lower = 0, n - 1 # 0, 2
        left, right = 0, n - 1 # 0, 2
        num = 1 # 需要填入matrix的数字，1-n^2

        while num <= n * n:
            if upper <= lower:
                for j in range(left, right + 1):
                    matrix[upper][j] = num 
                    num += 1
                upper += 1

            if left <= right:
                for i in range(upper, lower + 1):
                    matrix[i][right] = num
                    num += 1
                right -= 1
            
            if upper <= lower:
                for j in range(right, left -1, -1):
                    matrix[lower][j] = num 
                    num += 1
                lower -= 1
            
            if left <= right:
                for i in range(lower, upper - 1, -1):
                    matrix[i][left] = num
                    num +=1
                left += 1

        return matrix
```

# Complexity Analysis
- 时间复杂度：O(n^2)
- 空间复杂度：O(n^2)
