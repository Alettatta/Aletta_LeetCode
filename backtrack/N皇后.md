# Problem
https://labuladong.online/zh/problem/leetcode/n-queens/description/


# Problem Description
按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

n 皇后问题研究的是如何将 n 个皇后放置在 n × n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个不同的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

为了保证答案唯一，请将所有合法的方案按以下规则排序后输出：

将每个方案表示为长度为 n 的整数序列 (c_0,c_1,…,c_n-1)，其中 c_i 表示第 i 行皇后所在的列下标（列下标从 0 开始）。所有方案按该整数序列的字典序升序排列。


# Solution
对于任意一个皇后，它所在的行、列和对角线（左上、右上、左下、右下）都没有其他皇后，所以这就是一个符合规则的解。

本质是排列问题，不可复用

# Code

## LC version

```python
class Solution:
    def __init__(self):
        self.ans = []
        
    def solveNQueens(self, n: int) -> List[List[str]]: # 输入棋盘边长 n，返回所有合法的放置
        board = ['.' * n for _ in range(n)] # '.' 表示空，'Q' 表示皇后，初始化空棋盘
        self.backtrack(board, 0)
        return self.ans

    def backtrack(self, board, row):
        # base case
        if row == len(board):
            self.ans.append(board.copy())
            return

        n = len(board)
        for col in range(n):
            if not self.isValid(board, row, col):
                continue
            # 做选择
            board[row] = board[row][:col] + 'Q' + board[row][col+1:]
            self.backtrack(board, row+1)
            # 撤销选择
            board[row] = board[row][:col] + '.' + board[row][col+1:]

    # 是否可以在 board[row][col] 放置皇后？
    def isValid(self, board, row, col):
        n = len(board)
        # 检查列是否有皇后互相冲突
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        # 检查右上方是否有皇后互相冲突
        for i, j in zip(range(row - 1, -1, -1), range(col + 1, n)):
            if board[i][j] == 'Q':
                return False
        # 检查左上方是否有皇后互相冲突
        for i, j in zip(range(row - 1, -1, -1), range(col - 1, -1, -1)):
            if board[i][j] == 'Q':
                return False
        return True
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
    def __init__(self):
        self.ans = []
        
    def solveNQueens(self, n: int) -> List[List[str]]: # 输入棋盘边长 n，返回所有合法的放置
        board = ['.' * n for _ in range(n)] # '.' 表示空，'Q' 表示皇后，初始化空棋盘
        self.backtrack(board, 0)
        return self.ans

    def backtrack(self, board, row):
        # base case
        if row == len(board):
            self.ans.append(board.copy())
            return

        n = len(board)
        for col in range(n):
            if not self.isValid(board, row, col):
                continue
            # 做选择
            board[row] = board[row][:col] + 'Q' + board[row][col+1:]
            self.backtrack(board, row+1)
            # 撤销选择
            board[row] = board[row][:col] + '.' + board[row][col+1:]

    # 是否可以在 board[row][col] 放置皇后？
    def isValid(self, board, row, col):
        n = len(board)
        # 检查列是否有皇后互相冲突
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        # 检查右上方是否有皇后互相冲突
        for i, j in zip(range(row - 1, -1, -1), range(col + 1, n)):
            if board[i][j] == 'Q':
                return False
        # 检查左上方是否有皇后互相冲突
        for i, j in zip(range(row - 1, -1, -1), range(col - 1, -1, -1)):
            if board[i][j] == 'Q':
                return False
        return True


# Convert a solution (list[str]) into the column-index sequence
# used as the lexicographic sort key.
def queen_cols(board):
    cols = []
    for row in board:
        cols.append(row.index('Q'))
    return cols


for line in sys.stdin:
    line = line.strip()
    if not line:
        continue
    n = int(line)
    result = Solution().solveNQueens(n)
    result.sort(key=queen_cols)

    print(len(result))
    for i, board in enumerate(result):
        for row in board:
            print(row)
        if i + 1 < len(result):
            print()
```


# Complexity Analysis
- 时间复杂度：O(mn)
- 空间复杂度：O(mn)
