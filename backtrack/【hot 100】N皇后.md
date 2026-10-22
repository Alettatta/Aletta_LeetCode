# Problem
https://labuladong.online/zh/problem/leetcode/n-queens/description/


# Problem Description
按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

n 皇后问题研究的是如何将 n 个皇后放置在 n × n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个不同的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

为了保证答案唯一，请将所有合法的方案按以下规则排序后输出：

将每个方案表示为长度为 n 的整数序列 (c_0,c_1,…,c_n-1)，其中 c_i 表示第 i 行皇后所在的列下标（列下标从 0 开始）。所有方案按该整数序列的字典序升序排列。



# Code

## ACM version

```python
import sys 


# 排列问题，有重复元素不可以复用，同一行/列/斜线不能有Q
# 对于一个Q，首先我们选行，保证每行只有一个Q
# 接下来选列：① 不能同列（col不能被占用）
# ② 不能同主对角线（左上-右下）：row-col相同
# ③ 不能同副对角线（右上-左下）：row+col相同

class Solution:
    def solveNQueens(self, n):
        ans  = []
        board = [['.'] * n for _ in range(n)] # 建立棋盘矩阵

        # 从列出发思考问题，行只要保证不在同一行即可
        cols = set() # 已占用的列
        diag1 = set() # 已占用的主对角线 row - col
        diag2 = set() # # 已占用的副对角线 row + col

        def backtrack(row): # 从第0行开始放
            # base case：n 行都放好了，收集一种方案
            if row == n:
                ans.append([''.join(r) for r in board])
                return

            for col in range(n):
                # 剪枝，如果冲突就跳过：
                if col in cols or (row - col) in diag1 or (row + col) in diag2:
                    continue 

                # 选择皇后位置
                board[row][col] = 'Q'
                cols.add(col)
                diag1.add(row - col)
                diag2.add(row + col)
                
                # 下一行
                backtrack(row + 1)
                
                # 回溯
                board[row][col] = '.'
                cols.remove(col)
                diag1.remove(row - col)
                diag2.remove(row + col)

        backtrack(0)
        return ans


data = sys.stdin.read().strip().split('\n')
idx = 0
while idx < len(data):
    n = int(data[idx]); idx += 1
    ans = Solution().solveNQueens(n)
    print(len(ans))
    for ch in ans:
        for row in ch:
            print(row)
```

# Complexity Analysis
- 时间复杂度：O(mn)
- 空间复杂度：O(mn)
